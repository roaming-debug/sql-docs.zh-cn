---
title: 配置 FCI - Linux 上的 SQL Server (RHEL)
description: 了解如何在 Red Hat Enterprise Linux (RHEL) 上为 SQL Server 配置故障转移群集实例 (FCI)。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 31c8c92e-12fe-4728-9b95-4bc028250d85
ms.openlocfilehash: b2b97fa250b5af1a0030b51b0ea29a89324db9c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346303"
---
# <a name="configure-failover-cluster-instance---sql-server-on-linux-rhel"></a>配置故障转移群集实例 - Linux 上的 SQL Server (RHEL)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

SQL Server 双节点共享磁盘故障转移群集实例为实现高可用性提供服务器级别的冗余。 本教程介绍如何创建 Linux 上的 SQL Server 的双节点故障转移群集实例。 你将完成的特定步骤包括：

> [!div class="checklist"]
> * 设置和配置 Linux
> * 安装和配置 SQL Server
> * 配置主机文件
> * 配置共享存储并移动数据库文件
> * 在每个群集节点上安装和配置 Pacemaker
> * 配置故障转移群集实例

本文介绍如何为 SQL Server 创建双节点共享磁盘故障转移群集实例 (FCI)。 本文提供 Red Hat Enterprise Linux (RHEL) 的说明和脚本示例。 Ubuntu 分发版与 RHEL 相似，因此脚本示例通常也适用于 Ubuntu。 

有关概念性信息，请参阅[故障转移群集实例 - Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-concepts.md)。

## <a name="prerequisites"></a>先决条件

若要完成接下来的端到端方案，需要两台计算机来部署双节点群集，还需要一台服务器进行存储。 以下步骤概述了如何配置这些服务器。

## <a name="set-up-and-configure-linux"></a>设置和配置 Linux

第一步是在群集节点上配置操作系统。 在群集中的每个节点上，配置 linux 分发版。 在两个节点上使用相同的分发版和版本。 使用以下任一分发版：
    
* 具有适用于 HA 加载项的有效订阅的 RHEL

## <a name="install-and-configure-sql-server"></a>安装和配置 SQL Server

1. 在两个节点上安装和设置 SQL Server。  有关详细说明，请参阅[安装 Linux 上的 SQL Server](sql-server-linux-setup.md)。
1. 出于配置目的，请将一个节点指定为主节点，而将另一个指定为辅助节点。 本指南使用这些术语。  
1. 在辅助节点上，停止并禁用 SQL Server。
    以下示例会停止并禁用 SQL Server： 
    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl disable mssql-server
    ```

    > [!NOTE] 
    > 在设置时，将为 SQL Server 实例生成一个服务器主密钥并放置在 `var/opt/mssql/secrets/machine-key`。 在 Linux 上，SQL Server 始终以名为 mssql 的本地帐户身份运行。 因为它是本地帐户，所以其标识不会在节点之间共享。 因此，需要将加密密钥从主节点复制到每个辅助节点，以便每个本地 mssql 帐户均可访问它，从而解密服务器主密钥。 

1.  在主节点上，为 Pacemaker 创建 SQL Server 登录名并授予登录权限以运行 `sp_server_diagnostics`。 Pacemaker 使用此帐户验证正在运行 SQL Server 的节点。 

    ```bash
    sudo systemctl start mssql-server
    ```
   
   使用 sa 帐户连接到 SQL Server `master` 数据库并运行以下命令：

   ```sql
   USE [master]
   GO
   CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'
   ALTER SERVER ROLE [sysadmin] ADD MEMBER [<loginName>]
   ```

   此外，也可以在更精细的级别设置权限。 Pacemaker 登录需要 `VIEW SERVER STATE` 使用 sp_server_diagnostics 查询运行状况状态，需要 `setupadmin` 和 `ALTER ANY LINKED SERVER` 通过运行 sp_dropserver 和 sp_addserver 来更新 FCI 实例名称和资源名称。 

1. 在主节点上，停止并禁用 SQL Server。 

## <a name="configure-the-hosts-file"></a>配置主机文件

在每个群集节点上，配置主机文件。 “主机”文件必须包含每个群集节点的 IP 地址和名称。

1. 检查每个节点的 IP 地址。 以下脚本显示当前节点的 IP 地址。 

    ```bash
    sudo ip addr show
    ```

1. 在每个节点上设置计算机名。 为每个节点提供长度不超过 15 个字符的唯一名称。 通过将计算机名添加到 `/etc/hosts` 来设置该名称。 以下脚本可使用 `vi` 编辑 `/etc/hosts`。 

   ```bash
   sudo vi /etc/hosts
   ```
   以下示例显示了 `/etc/hosts` 以及名为 `sqlfcivm1` 和 `sqlfcivm2` 的两个节点。

   ```bash
   127.0.0.1   localhost localhost4 localhost4.localdomain4
   ::1       localhost localhost6 localhost6.localdomain6
   10.128.18.128 sqlfcivm1
   10.128.16.77 sqlfcivm2
   ```

## <a name="configure-storage--move-database-files"></a>配置存储和移动数据库文件  

需要提供两个节点都可以访问的存储。 可以使用 iSCSI、NFS 或 SMB。 配置存储，将存储提供给群集节点，然后将数据库文件移到新存储。 以下文章介绍了每种存储类型的步骤：

- [配置故障转移群集实例 - iSCSI - Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-configure-iscsi.md)
- [配置故障转移群集实例 - NFS - Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-configure-nfs.md)
- [配置故障转移群集实例 - SMB- Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-configure-smb.md)

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>在每个群集节点上安装和配置 Pacemaker

1. 在两个群集节点上，创建一个文件来存储用于 Pacemaker 登录的 SQL Server 用户名和密码。 

    以下命令创建并填充此文件：

    ```bash
    sudo touch /var/opt/mssql/secrets/passwd
    sudo echo '<loginName>' >> /var/opt/mssql/secrets/passwd
    sudo echo '<loginPassword>' >> /var/opt/mssql/secrets/passwd
    sudo chown root:root /var/opt/mssql/secrets/passwd 
    sudo chmod 600 /var/opt/mssql/secrets/passwd    
    ```

1. 在两个群集节点上，打开 Pacemaker 防火墙端口。 若要使用 `firewalld` 打开这些端口，请运行以下命令：

   ```bash
   sudo firewall-cmd --permanent --add-service=high-availability
   sudo firewall-cmd --reload
   ```

   > 如果使用的是没有内置高可用性配置的其他防火墙，则需要打开以下端口，Pacemaker 才能与群集中的其他节点通信
   >
   > * TCP：端口 2224、3121、21064
   > * UDP：端口 5405

1. 在每个节点上安装 Pacemaker 包。

   ```bash
   sudo yum install pacemaker pcs fence-agents-all resource-agents
   ```
1. 为安装 Pacemaker 和 Corosync 包时创建的默认用户设置密码。 在两个节点上使用相同的密码。 

   ```bash
   sudo passwd hacluster
   ```
1. 启用并启动 `pcsd` 服务和 Pacemaker。 这样，节点将可以在重新启动后重新加入群集。 在这两个节点上运行以下命令。

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

1. 为 SQL Server 安装 FCI 资源代理。 在这两个节点上运行以下命令。 

   ```bash
   sudo yum install mssql-server-ha
   ```

## <a name="configure-the-failover-cluster-instance"></a>配置故障转移群集实例

将在资源组中创建 FCI。 这更简单一点，因为资源组减少了对约束的需求。 不过，请按照资源组本身的开始顺序将资源添加到资源组中。 资源组本身的开始顺序为： 

1. 存储资源
2. 网络资源
3. 应用程序资源

此示例将在 NewLinFCIGrp 组中创建一个 FCI。 资源组的名称必须是在 Pacemaker 中创建的任何资源的唯一名称。

1.  创建磁盘资源。 如果没有问题，你将不会收到回应。 创建磁盘资源的方式取决于存储类型。 下面是每个存储类型的示例。 使用适用于群集存储的存储类型的示例。

    **iSCSI**

    ```bash
    sudo pcs resource create <iSCSIDiskResourceName> Filesystem device="/dev/<VolumeGroupName>/<LogicalVolumeName>" directory="<FolderToMountiSCSIDisk>" fstype="<FileSystemType>" --group RGName
    ```

    \<iSCSIDIskResourceName> 是与 iSCSI 磁盘关联的资源的名称

    \<VolumeGroupName> 是卷组的名称  

    \<LogicalVolumeName> 是创建的逻辑卷的名称  

    \<FolderToMountiSCSIDIsk> 是要装入磁盘的文件夹（对于系统数据库和默认位置，它是 /var/opt/mssql/data）

    \<FileSystemType> 是 EXT4 或 XFS，具体取决于内容的格式和分发版支持的类型。 

    **NFS**

    ```bash
    sudo pcs resource create <NFSDiskResourceName> Filesystem device="<IPAddressOfNFSServer>:<FolderOnNFSServer>" directory="<FolderToMountNFSShare>" fstype=nfs4 options=" nfsvers=4.2,timeo=14,intr" --group RGName
    mount -t nfs4 IPAddressOfNFSServer:FolderOnNFSServer /var/opt/mssql/data -o 
    ```

    \<NFSDIskResourceName> 是与 NFS 共享关联的资源的名称

    \<IPAddressOfNFSServer> 是要使用的 NFS 服务器的 IP 地址

    \<FolderOnNFSServer> 是 NFS 共享的名称

    \<FolderToMountNFSShare> 是要装入磁盘的文件夹（对于系统数据库和默认位置，它是 /var/opt/mssql/data）

    下面显示了一个示例：

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci1 /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

    **SMB**

    ```bash
    sudo pcs resource create SMBDiskResourceName Filesystem device="//<ServerName>/<ShareName>" directory="<FolderName>" fstype=cifs options="vers=3.0,username=<UserName>,password=<Password>,domain=<ADDomain>,uid=<mssqlUID>,gid=<mssqlGID>,file_mode=0777,dir_mode=0777" --group <RGName>
    ```

    \<ServerName> 是具有 SMB 共享的服务器的名称

    \<ShareName> 是共享的名称

    \<FolderName> 是在上一步中创建的文件夹的名称
    
    \<UserName> 是访问共享的用户的名称

    \<Password> 是用户的密码

    \<ADDomain> 是 AD DS 域（使用基于 Windows 服务器的 SMB 时适用）

    \<mssqlUID> 是 mssql 用户的 UID

    \<mssqlGID> 是 mssql 用户的 GID

    \<RGName> 是资源组的名称
 
2.  创建 FCI 将使用的 IP 地址。 如果没有问题，你将不会收到回应。

    ```bash
    sudo pcs resource create <IPResourceName> ocf:heartbeat:IPaddr2 ip=<IPAddress> nic=<NetworkCard> cidr_netmask=<NetMask> --group <RGName>
    ```

    \<IPResourceName> 是与 IP 地址关联的资源的名称

    \<IPAddress> 是 FCI 的 IP 地址

    \<NetworkCard> 是与子网（即 eth0）关联的网卡

    \<NetMask> 是子网的网络掩码（即 24）

    \<RGName> 是资源组的名称
 
3.  创建 FCI 资源。 如果没有问题，你将不会收到回应。

    ```bash
    sudo pcs resource create FCIResourceName ocf:mssql:fci op defaults timeout=60s --group RGName
    ```

    \<FCIResourceName> 不仅是资源的名称，而且还是与 FCI 关联的易记名称。 这就是用户和应用程序用于连接的内容。 

    \<RGName> 是资源组的名称。
 
4.  运行命令 `sudo pcs resource`。 FCI 应为联机状态。
 
5.  使用 FCI 的 DNS/资源名称，通过 SSMS 或 sqlcmd 连接到 FCI。

6.  发出语句 `SELECT @@SERVERNAME`。 它应返回 FCI 的名称。

7.  发出语句 `SELECT SERVERPROPERTY('ComputerNamePhysicalNetBIOS')`。 它应返回 FCI 运行的节点的名称。

8.  将 FCI 手动故障转移到其他节点。 请参阅[运行故障转移群集实例 - Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-operate.md)。

9.  最后，将 FCI 故障转移回原始节点并删除托管约束。

<!---

|Distribution |Topic 
|----- |-----
|**Red Hat Enterprise Linux with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)<br/>[Operate](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
|**SUSE Linux Enterprise Server with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-sles-configure.md)

-->
## <a name="summary"></a>总结

在本教程中，已完成以下任务：

> [!div class="checklist"]
> * 设置和配置 Linux
> * 安装和配置 SQL Server
> * 配置主机文件
> * 配置共享存储并移动数据库文件
> * 在每个群集节点上安装和配置 Pacemaker
> * 配置故障转移群集实例

## <a name="next-steps"></a>后续步骤

- [运行故障转移群集实例 - Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-operate.md)

<!--Image references-->
