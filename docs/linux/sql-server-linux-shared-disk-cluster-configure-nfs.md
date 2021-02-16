---
title: 配置 NFS 存储 FCI - Linux 上的 SQL Server
description: 了解如何使用 NFS 存储为 Linux 上的 SQL Server 配置故障转移群集实例 (FCI)。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 90240b383e19fefd254cd5f28b6d113b40b20d98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346317"
---
# <a name="configure-failover-cluster-instance---nfs---sql-server-on-linux"></a>配置故障转移群集实例 NFS - Linux 上的 SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍如何在 Linux 上为故障转移群集实例 (FCI) 配置 NFS 存储。 

NFS 或网络文件系统是用于在 Linux 中而非 Windows 中共享磁盘的常见方法。 与 iSCSI 类似，只要满足 SQL Server 的存储要求，就可以在服务器、某种设备或存储单元上配置 NFS。

## <a name="important-nfs-server-information"></a>重要的 NFS 服务器信息

托管 NFS 的源（Linux 服务器或其他源）必须使用/符合 4.2 或更高版本。 早期版本不适用于 Linux 上的 SQL Server。

配置要在 NFS 服务器上共享的文件夹时，请确保它们遵循以下准则常规选项：
- `rw`，确保文件夹可读取和写入
- `sync`，确保一定会写入文件夹
- 不要将 `no_root_squash` 用作选项，它被认为存在安全风险
- 确保对文件夹应用完整权限 (777)

确保针对访问强制执行安全标准。 配置文件夹时，请确保只有参与 FCI 的服务器才能看到 NFS 文件夹。 下面显示了基于 Linux 的 NFS 解决方案中经过修改的 /etc/exports 的示例，其中文件夹限制为 FCIN1 和 FCIN2。

![一张屏幕截图，显示基于 Linux 的 NFS 解决方案中经过修改的 /etc/exports 的示例，其中文件夹限制为 FCIN1 和 FCIN2。][1]

## <a name="instructions"></a>Instructions

1. 选择将参与 FCI 配置的其中一个服务器。 选择任何一个均可。 

2. 检查服务器是否可以看到 NFS 服务器上的装载。

    ```bash
    sudo showmount -e <IPAddressOfNFSServer>
    ```

    \<IPAddressOfNFSServer> 是要使用的 NFS 服务器的 IP 地址。

3. 对于系统数据库或存储在默认数据位置的任何内容，请执行以下步骤。 否则，请跳至步骤 4。
 
   * 确保正在使用的服务器上的 SQL Server 已停止运行。

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```
   * 彻底切换为超级用户。 如果成功，不会收到任何确认信息。

    ```bash
    sudo -i
    ```

   * 切换为 mssql 用户。 如果成功，不会收到任何确认信息。

    ```bash
    su mssql
    ```

   * 创建一个临时目录来存储 SQL Server 数据和日志文件。 如果成功，不会收到任何确认信息。

    ```bash
    mkdir <TempDir>
    ```

    \<TempDir> 是文件夹的名称。 以下示例创建名为 /var/opt/mssql/tmp 的文件夹。

    ```bash
    mkdir /var/opt/mssql/tmp
    ```

   * 将 SQL Server 的数据和日志文件复制到临时目录。 如果成功，不会收到任何确认信息。
    
    ```bash
    cp /var/opt/mssql/data/* <TempDir>
    ```

    \<TempDir> 是上一步中的文件夹的名称。

   * 验证文件是否位于目录中。

    ```bash
    ls TempDir
    ```

    \<TempDir> 是步骤 d 中文件夹的名称。

   * 删除现有 SQL Server 数据目录中的文件。 如果成功，不会收到任何确认信息。

    ```bash
    rm - f /var/opt/mssql/data/*
    ```

   * 验证文件是否已删除。 

    ```bash
    ls /var/opt/mssql/data
    ```
    
   * 键入 exit 切换回根用户。

   * 将 NFS 共享装载到 SQL Server 数据文件夹中。 如果成功，不会收到任何确认信息。

    ```bash
    mount -t nfs4 <IPAddressOfNFSServer>:<FolderOnNFSServer> /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

    \<IPAddressOfNFSServer> 是要使用的 NFS 服务器的 IP 地址 

    \<FolderOnNFSServer> 是 NFS 共享的名称。 以下示例语法与步骤 2 中的 NFS 信息匹配。

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci1 /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

   * 在不切换的情况下，发布装载，以检查装载是否成功。

    ```bash
    mount
    ```

    ![mount 命令及对该命令的响应的屏幕截图，其中显示没有开关。][2]

   * 切换为 mssql 用户。 如果成功，不会收到任何确认信息。

    ```bash
    su mssql
    ```

   * 从临时目录 /var/opt/mssql/data 复制文件。 如果成功，不会收到任何确认信息。

    ```bash
    cp /var/opt/mssql/tmp/* /var/opt/mssqldata
    ```

   * 验证文件是否在那里。

    ```bash
    ls /var/opt/mssql/data
    ```

   * 输入 exit，退出 mssql 身份 
    
   * 输入 exit，退出 root 身份

   * 启动 SQL Server。 如果正确复制了所有内容并正确应用了安全性，SQL Server 应显示为已启动。

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    ```
    
   * 创建数据库以测试是否正确设置了安全性。 以下示例显示如何通过 Transact-SQL 实现此操作；还可以通过 SSMS 实现。
 
    ![CreateTestdatabase][3]

   * 停止 SQL Server 并验证它是否已关闭。

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```

   * 如果不再创建任何其他 NFS 装载，请卸载共享。 如果还要创建，请不要卸载。

    ```bash
    sudo umount <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn>
    ```

    \<IPAddressOfNFSServer> 是要使用的 NFS 服务器的 IP 地址

    \<FolderOnNFSServer> 是 NFS 共享的名称

    \<FolderMountedIn> 是在上一步中创建的文件夹。 

4. 对于系统数据库以外的其他内容，例如用户数据库或备份，请按照以下步骤操作。 如果仅使用默认位置，请跳至步骤 5。

   * 切换为超级用户。 如果成功，不会收到任何确认信息。

    ```bash
    sudo -i
    ```

   * 创建将由 SQL Server 使用的文件夹。 

    ```bash
    mkdir <FolderName>
    ```

    \<FolderName> 是文件夹的名称。 如果文件夹不在正确的位置，需要指定文件夹的完整路径。 以下示例将创建名为 /var/opt/mssql/userdata 的文件夹。

    ```bash
    mkdir /var/opt/mssql/userdata
    ```

   * 将 NFS 共享装载到上一步中创建的文件夹中。 如果成功，不会收到任何确认信息。

    ```bash
    Mount -t nfs4 <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn> -o nfsvers=4.2,timeo=14,intr
    ```

    \<IPAddressOfNFSServer> 是要使用的 NFS 服务器的 IP 地址

    \<FolderOnNFSServer> 是 NFS 共享的名称

    \<FolderToMountIn> 是在上一步中创建的文件夹。 下面是一个示例。 

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci2 /var/opt/mssql/userdata -o nfsvers=4.2,timeo=14,intr
    ```

   * 在不切换的情况下，发布装载，以检查装载是否成功。
  
   * 键入 exit，退出超级用户身份。

   * 若要进行测试，请在该文件夹中创建数据库。 以下示例使用 sqlcmd 创建数据库、相应切换上下文、验证文件是否存在于操作系统级别，然后删除临时位置。 可以使用 SSMS。

    ![sqlcmd 命令及对该命令的响应的屏幕截图。][4]
 
   * 卸载共享 

    ```bash
    sudo umount <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn>
    ```

    \<IPAddressOfNFSServer> 是要使用的 NFS 服务器的 IP 地址
    
    \<FolderOnNFSServer> 是 NFS 共享的名称

    \<FolderMountedIn> 是在上一步中创建的文件夹。 下面是一个示例。 
 
5. 在其他节点上重复这些步骤。


## <a name="next-steps"></a>后续步骤

[配置故障转移群集实例 - Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/05-nfsacl.png
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/10-mountnoswitches.png
[3]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/20-createtestdatabase.png
[4]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/15-createtestdatabase.png
