---
title: 为 Linux 上的 SQL Server 配置 RHEL FCI
description: 了解如何为 Linux 上的 SQL Server 配置 Red Hat Enterprise Linux (RHEL) 共享磁盘故障转移群集实例 (FCI)，以实现高可用性。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: dcc0a8d3-9d25-4208-8507-a5e65d2a9a15
ms.openlocfilehash: f635a10d0a1165fb3f657e41147fe023f55d2874
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346287"
---
# <a name="configure-rhel-failover-cluster-instance-fci-cluster-for-sql-server"></a>为 SQL Server 配置 RHEL 故障转移群集实例 (FCI)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本指南介绍如何在 Red Hat Enterprise Linux 上为 SQL Server 创建两节点的共享磁盘故障转移群集。 集群层基于在 [Pacemaker](https://clusterlabs.org/) 的基础上构建的 Red Hat Enterprise Linux (RHEL) [HA 加载项](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf)。 SQL Server 实例在一个节点或另一个节点上处于活动状态。

> [!NOTE] 
> 需拥有订阅才能访问 Red Hat HA 加载项和文档。 

如下图所示，存储将呈现给两个服务器。 群集组件（Corosync 和 Pacemaker）负责协调通信和资源管理。 其中一个服务器具有活动连接，可连接至存储资源以及 SQL Server。 当 Pacemaker 检测到故障时，群集组件会设法将资源移到另一个节点。  

![Red Hat Enterprise Linux 7 共享磁盘 SQL 群集](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 

有关群集配置、资源代理选项和管理的详细信息，请访问 [RHEL 参考文档](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html)。


> [!NOTE] 
> 此时，SQL Server 与 Pacemaker 的集成不及与在 Windows 上与 WSFC 集成的耦合性高。 在 SQL 内部，无法了解到群集是否存在，所有业务流程都是由外至内，并且 Pacemaker.将该服务作为一个独立实例控制。 另外，例如，群集 dmvs sys.dm_os_cluster_nodes 和 sys.dm_os_cluster_properties 不会有记录。
为了使用指向字符串服务器名称的连接字符串，且不使用 IP，它们需要在其 DNS 服务器中注册用于创建使用选定服务器名称的虚拟 IP 资源的 IP（如以下部分所述）。

以下部分介绍了设置故障转移群集解决方案的步骤。 

## <a name="prerequisites"></a>先决条件

若要完成下面的端到端方案，需要两台计算机来部署两个节点群集，还需要一台服务器来配置 NFS 服务器。 以下步骤概述了如何配置这些服务器。

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>在每个群集节点上安装和配置操作系统

第一步是在群集节点上配置操作系统。 对于此演练，将通过 HA 加载项的有效订阅使用 RHEL。 

## <a name="install-and-configure-sql-server-on-each-cluster-node"></a>在每个群集节点上安装和配置 SQL Server

1. 在两个节点上安装并设置 SQL Server。  有关详细说明，请参阅[安装 Linux 上的 SQL Server](sql-server-linux-setup.md)。

1. 出于配置目的，请将一个节点指定为主节点，而将另一个指定为辅助节点。 本指南使用这些术语。  

1. 在辅助节点上，停止并禁用 SQL Server。

   以下示例会停止并禁用 SQL Server： 

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl disable mssql-server
   ```
> [!NOTE] 
> 在设置时，为 SQL Server 实例生成服务器主密钥并将其置于 `/var/opt/mssql/secrets/machine-key`。 在 Linux 上，SQL Server 始终以名为 mssql 的本地帐户身份运行。 因为它是本地帐户，所以其标识不会在节点之间共享。 因此，需要将加密密钥从主节点复制到每个辅助节点，以便每个本地 mssql 帐户均可访问它，从而解密服务器主密钥。 

1. 在主节点上，为 Pacemaker 创建 SQL Server 登录名并授予登录权限以运行 `sp_server_diagnostics`。 Pacemaker 使用此帐户验证正在运行 SQL Server 的节点。 

   ```bash
   sudo systemctl start mssql-server
   ```

   使用 sa 帐户连接到 SQL Server `master` 数据库并运行以下命令：

   ```bashsql
   USE [master]
   GO
   CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'

   ALTER SERVER ROLE [sysadmin] ADD MEMBER [<loginName>]
   ```
   此外，也可以在更精细的级别设置权限。 Pacemaker 登录要求 `VIEW SERVER STATE` 通过 sp_server_diagnostics、`setupadmin` 和 `ALTER ANY LINKED SERVER` 查询运行状况，以便通过运行 sp_dropserver 和 sp_addserver，使用资源名称来更新 FCI 实例名称。 

1. 在主节点上，停止并禁用 SQL Server。 

1. 为每个群集节点配置主机文件。 主机文件必须包含每个群集节点的 IP 地址和名称。 

    检查每个节点的 IP 地址。 以下脚本显示当前节点的 IP 地址。 

   ```bash
   sudo ip addr show
   ```

   在每个节点上设置计算机名。 为每个节点提供长度不超过 15 个字符的唯一名称。 通过将计算机名添加到 `/etc/hosts` 来设置该名称。 以下脚本可使用 `vi` 编辑 `/etc/hosts`。 

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

在下一部分中，将配置共享存储并将数据库文件移到该存储。 

## <a name="configure-shared-storage-and-move-database-files"></a>配置共享存储并移动数据库文件 

有多种解决方案可用于提供共享存储。 本演练演示如何使用 NFS 配置共享存储。 建议按照最佳做法进行操作，并使用 Kerberos 保护 NFS（可在此处找到示例： https://www.certdepot.net/rhel7-use-kerberos-control-access-nfs-network-shares/) ）。 

>[!Warning]
>如果不保护 NFS，则可访问网络以及能够欺骗 SQL 节点 IP 地址的任何人均能访问你的数据文件。 与往常一样，请确保在将系统投入生产前，针对系统建立威胁模型。 另一种存储方法是使用 SMB 文件共享。

### <a name="configure-shared-storage-with-nfs"></a>使用 NFS 配置共享存储

> [!IMPORTANT] 
> 此版本不支持在版本 <4 的 NFS 服务器上托管数据库文件。 这包括将 NFS 用于共享磁盘故障转移群集，以及非聚集实例上的数据库。 我们正在设法使后续版本能够利用其他版本的 NFS 服务器。 

在 NFS 服务器上执行以下操作：

1. 安装 `nfs-utils`

   ```bash
   sudo yum -y install nfs-utils
   ```

1. 启用并启动 `rpcbind`

   ```bash
   sudo systemctl enable rpcbind && sudo systemctl start rpcbind
   ```

1. 启用并启动 `nfs-server`
 
   ```bash
   sudo systemctl enable nfs-server && sudo systemctl start nfs-server
   ```
 
1.  编辑 `/etc/exports` 以导出要共享的目录。 每个所需共享需要 1 行。 例如： 

   ```bash
   /mnt/nfs  10.8.8.0/24(rw,sync,no_subtree_check,no_root_squash)
   ```

1. 导出共享

   ```bash
   sudo exportfs -rav
   ```

1. 验证路径是否是从 NFS 服务器共享/导出、运行

   ```bash
   sudo showmount -e
   ```

1. 在 SELinux 中添加异常

   ```bash
   sudo setsebool -P nfs_export_all_rw 1
   ```
   
1. 打开服务器防火墙。

   ```bash 
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

### <a name="configure-all-cluster-nodes-to-connect-to-the-nfs-shared-storage"></a>配置所有群集节点，以连接到 NFS 共享存储

在所有群集节点上执行以下步骤。

1.  安装 `nfs-utils`

   ```bash
   sudo yum -y install nfs-utils
   ```

1. 打开客户端和 NFS 服务器上的防火墙

   ```bash
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

1. 验证是否可以在客户端计算机上看到 NFS 共享

   ```bash
   sudo showmount -e <IP OF NFS SERVER>
   ```

1. 对所有群集节点重复这些步骤。

有关如何使用 NFS 的详细信息，请参阅以下资源：

* [NFS 服务器和 firewalld | Stack Exchange](https://unix.stackexchange.com/questions/243756/nfs-servers-and-firewalld)
* [装载 NFS 卷 | Linux 网络管理员指南](https://www.tldp.org/LDP/nag2/x-087-2-nfs.mountd.html)
* [NFS 服务器配置 | Red Hat 客户门户](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/nfs-serverconfig)

### <a name="mount-database-files-directory-to-point-to-the-shared-storage"></a>装载数据库文件目录，使其指向共享存储

1.  仅在主节点上，将数据库文件保存到一个临时位置。以下脚本将创建一个新的临时目录，将数据库文件复制到新的目录中并删除旧的数据库文件  。 SQL Server 以本地用户 mssql 的身份运行时，需要确保在将数据传输到已装载的共享后，本地用户对共享具有读写访问权限。 

   ``` 
   $ sudo su mssql
   $ mkdir /var/opt/mssql/tmp
   $ cp /var/opt/mssql/data/* /var/opt/mssql/tmp
   $ rm /var/opt/mssql/data/*
   $ exit
   ``` 

1.  在所有群集节点上编辑 `/etc/fstab` 文件以包含装载命令。  

   ```bash
   <IP OF NFS SERVER>:<shared_storage_path> <database_files_directory_path> nfs timeo=14,intr 
   ```
   
   以下脚本显示了一个编辑示例。  

   ``` 
   10.8.8.0:/mnt/nfs /var/opt/mssql/data nfs timeo=14,intr 
   ``` 
> [!NOTE] 
>如果按照此处的建议使用文件系统 (FS) 资源，则无需保留 /etc/fstab 中的装载命令。 Pacemaker 将负责在启动 FS 群集资源时装载文件夹。 借助隔离，可确保永远不会重复装载 FS。 

1.  对系统运行 `mount -a` 命令以更新装载路径。  

1.  将保存到 `/var/opt/mssql/tmp` 的数据库和日志文件复制到新装载的共享 `/var/opt/mssql/data`。 此操作仅需在主节点上执行  。 请确保为“mssql”本地用户提供读写权限。

   ``` 
   $ sudo chown mssql /var/opt/mssql/data
   $ sudo chgrp mssql /var/opt/mssql/data
   $ sudo su mssql
   $ cp /var/opt/mssql/tmp/* /var/opt/mssql/data/
   $ rm /var/opt/mssql/tmp/*
   $ exit
   ``` 
 
1.  验证 SQL Server 是否已成功通过新文件路径启动。 对每个节点均执行此操作。 此时，一次应只有一个节点运行 SQL Server。 它们不能同时运行，因为两者将同时尝试访问数据文件（要避免同时在两个节点上意外启动 SQL Server，请使用文件系统群集资源来确保共享未由不同的节点装载两次）。 以下命令可启动 SQL Server、检查状态及随后停止 SQL Server。
 
   ```bash
   sudo systemctl start mssql-server
   sudo systemctl status mssql-server
   sudo systemctl stop mssql-server
   ```
 
此时，SQL Server 的两个实例均已配置为使用共享存储上的数据库文件运行。 接下来，为 Pacemaker 配置 SQL Server。 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>在每个群集节点上安装和配置 Pacemaker


2. 在两个群集节点上，创建一个文件来存储用于 Pacemaker 登录的 SQL Server 用户名和密码。 以下命令创建并填充此文件：

   ```bash
   sudo touch /var/opt/mssql/secrets/passwd
   echo '<loginName>' | sudo tee -a /var/opt/mssql/secrets/passwd
   echo '<loginPassword>' | sudo tee -a /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/secrets/passwd 
   sudo chmod 600 /var/opt/mssql/secrets/passwd    
   ```

3. 在两个群集节点上，打开 Pacemaker 防火墙端口。 若要使用 `firewalld` 打开这些端口，请运行以下命令：

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

    

2. 为安装 Pacemaker 和 Corosync 包时创建的默认用户设置密码。 在两个节点上使用相同的密码。 

   ```bash
   sudo passwd hacluster
   ```

    

3. 启用并启动 `pcsd` 服务和 Pacemaker。 这样，节点将可以在重新启动后重新加入群集。 在这两个节点上运行以下命令。

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

4. 为 SQL Server 安装 FCI 资源代理。 在这两个节点上运行以下命令。 

   ```bash
   sudo yum install mssql-server-ha
   ```

## <a name="configure-fencing-agent"></a>配置隔离代理

STONITH 设备提供隔离代理。 有关如何在 Azure 中为此群集创建 STONITH 设备的示例，请参阅[在 Azure 中的 Red Hat Enterprise Linux 上设置 Pacemaker](/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker/#1-create-the-stonith-devices)。 修改环境的说明。

## <a name="create-the-cluster"></a>创建群集 

1. 在其中一个节点上创建群集。

   ```bash
   sudo pcs cluster auth <nodeName1 nodeName2 ...> -u hacluster
   sudo pcs cluster setup --name <clusterName> <nodeName1 nodeName2 ...>
   sudo pcs cluster start --all
   ```

2. 为 SQL Server、文件系统和虚拟 IP 资源配置群集资源，并将配置推送到群集。 需要以下信息：

   - **SQL Server 资源名称**：群集 SQL Server 资源的名称。 
   - **浮动 IP 资源名称**：虚拟 IP 地址资源的名称。
   - **IP 地址**：客户端用于连接到 SQL Server 群集实例的 IP 地址。 
   - **文件系统资源名称**：文件系统资源的名称。
   - **设备**：NFS 共享路径
   - **设备**：装载到共享的本地路径
   - **fstype**：文件共享类型（即 nfs）

   为环境更新以下脚本的值。 在一个节点上运行，以配置并启动群集服务。  

   ```bash
   sudo pcs cluster cib cfg 
   sudo pcs -f cfg resource create <sqlServerResourceName> ocf:mssql:fci
   sudo pcs -f cfg resource create <floatingIPResourceName> ocf:heartbeat:IPaddr2 ip=<ip Address>
   sudo pcs -f cfg resource create <fileShareResourceName> Filesystem device=<networkPath> directory=<localPath>         fstype=<fileShareType>
   sudo pcs -f cfg constraint colocation add <virtualIPResourceName> <sqlResourceName>
   sudo pcs -f cfg constraint colocation add <fileShareResourceName> <sqlResourceName> 
   sudo pcs cluster cib-push cfg
   ```

   例如，以下脚本可创建一个名为 `mssqlha` 的 SQL Server 群集资源以及 IP 地址为 `10.0.0.99` 的浮动 IP 资源。 它还可创建一个文件系统资源并添加约束，将所有资源并置到同一节点上用作 SQL 资源。 

   ```bash
   sudo pcs cluster cib cfg
   sudo pcs -f cfg resource create mssqlha ocf:mssql:fci
   sudo pcs -f cfg resource create virtualip ocf:heartbeat:IPaddr2 ip=10.0.0.99
   sudo pcs -f cfg resource create fs Filesystem device="10.8.8.0:/mnt/nfs" directory="/var/opt/mssql/data" fstype="nfs"
   sudo pcs -f cfg constraint colocation add virtualip mssqlha
   sudo pcs -f cfg constraint colocation add fs mssqlha
   sudo pcs cluster cib-push cfg
   ```

   推送配置后，SQL Server 将在一个节点上启动。 

3. 验证 SQL Server 是否已启动。 

   ```bash
   sudo pcs status 
   ```

   以下示例展示了 Pacemaker 已成功启动 SQL Server 的群集实例时的结果。 

   ```
   fs     (ocf::heartbeat:Filesystem):    Started sqlfcivm1
   virtualip     (ocf::heartbeat:IPaddr2):      Started sqlfcivm1
   mssqlha  (ocf::mssql:fci): Started sqlfcivm1
   
   PCSD Status:
    sqlfcivm1: Online
    sqlfcivm2: Online
   
   Daemon Status:
    corosync: active/disabled
    pacemaker: active/enabled
    pcsd: active/enabled
   ```

## <a name="additional-resources"></a>其他资源

* Pacemaker [从头开始构建群集](https://clusterlabs.org/doc/Cluster_from_Scratch.pdf)指南

## <a name="next-steps"></a>后续步骤

[在 Red Hat Enterprise Linux 共享磁盘群集上操作 SQL Server](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
