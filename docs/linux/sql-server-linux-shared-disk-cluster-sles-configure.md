---
title: 配置适用于 SQL Server 的 SLES 共享磁盘群集
description: 通过配置适用于 SQL Server 的 SUSE Linux Enterprise Server (SLES) 共享磁盘群集来实现高可用性。
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: e5ad1bdd-c054-4999-a5aa-00e74770b481
ms.openlocfilehash: 87a4fd9edf52c3590fb19781fc5bf76c9bb0168b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346264"
---
# <a name="configure-sles-shared-disk-cluster-for-sql-server"></a>配置适用于 SQL Server 的 SLES 共享磁盘群集

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本指南介绍如何在 SUSE Linux Enterprise Server (SLES) 上为 SQL Server 创建双节点共享磁盘群集。 此群集层基于在 [Pacemaker](https://clusterlabs.org/) 之上生成的 SUSE [High Availability Extension (HAE)](https://www.suse.com/products/highavailability)。 

有关群集配置、资源代理选项、管理、最佳实践和建议的详细信息，请参阅 [SUSE Linux Enterprise High Availability Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html)（SUSE Linux Enterprise 高可用性扩展 12 SP2）。

## <a name="prerequisites"></a>先决条件

若要完成下面的端到端方案，需要两台计算机来部署两个节点群集，还需要一台服务器来配置 NFS 共享。 以下步骤概述了如何配置这些服务器。

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>在每个群集节点上安装和配置操作系统

第一步是在群集节点上配置操作系统。 对于此演练，将通过 HA 附加产品的有效订阅使用 SLES。

## <a name="install-and-configure-sql-server-on-each-cluster-node"></a>在每个群集节点上安装和配置 SQL Server

1. 在两个节点上安装并设置 SQL Server。 有关详细说明，请参阅[安装 Linux 上的 SQL Server](sql-server-linux-setup.md)。
2. 出于配置目的，请将一个节点指定为主节点，而将另一个指定为辅助节点。 本指南使用这些术语。 
3. 在辅助节点上，停止并禁用 SQL Server。 以下示例会停止并禁用 SQL Server：

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl disable mssql-server
    ```

    > [!NOTE]
    > 在设置时，为 SQL Server 实例生成服务器主密钥并将其置于 `/var/opt/mssql/secrets/machine-key`。 在 Linux 上，SQL Server 始终以名为 mssql 的本地帐户身份运行。 因为它是本地帐户，所以其标识不会在节点之间共享。 因此，需要将加密密钥从主节点复制到每个辅助节点，以便每个本地 mssql 帐户均可访问它，从而解密服务器主密钥。
4. 在主节点上，为 Pacemaker 创建 SQL Server 登录名并授予登录权限以运行 `sp_server_diagnostics`。 Pacemaker 使用此帐户验证正在运行 SQL Server 的节点。

    ```bash
    sudo systemctl start mssql-server
    ```
    使用“sa”帐户连接到 SQL Server 主数据库并运行以下命令：

    ```sql
    USE [master]
    GO
    CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'
    GRANT VIEW SERVER STATE TO <loginName>
    ```
5. 在主节点上，停止并禁用 SQL Server。
6. 按照 [SUSE 文档中](https://www.suse.com/documentation/sles11/book_sle_admin/data/sec_basicnet_yast.html)的说明进行操作，配置并更新每个群集节点的主机文件。 “主机”文件必须包含每个群集节点的 IP 地址和名称。

    若要检查当前节点的 IP 地址，请运行：

    ```bash
    sudo ip addr show
    ```

    在每个节点上设置计算机名。 为每个节点提供长度不超过 15 个字符的唯一名称。 通过使用 [yast](https://www.suse.com/documentation/sles11/book_sle_admin/data/sec_basicnet_yast.html) 或[手动](https://www.suse.com/documentation/sled11/book_sle_admin/data/sec_basicnet_manconf.html)将计算机名添加到 `/etc/hostname` 来进行设置。

    以下示例显示了 `/etc/hosts` 以及名为 `SLES1` 和 `SLES2` 的两个节点。

    ```
    127.0.0.1   localhost
    10.128.18.128 SLES1
    10.128.16.77 SLES2
    ```

    > [!NOTE]
    > 所有群集节点都必须能够通过 SSH 相互访问。 hb_report 或 crm_report（用于故障排除）和 Hawk 的历史记录浏览器等工具需要在节点之间进行无密码的 SSH 访问，否则它们只能收集当前节点的数据。 如果使用非标准 SSH 端口，请使用 -X 选项（[请参阅手册页](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_requirements_other.html)）。 例如，如果 SSH 端口为 3479，请使用以下命令调用 crm_report：
    >
    >```bash
    >crm_report -X "-p 3479" [...]
    >```
    >有关详细信息，请参阅[管理指南]。(https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.misc)

在下一部分中，将配置共享存储并将数据库文件移到该存储。  

## <a name="configure-shared-storage-and-move-database-files"></a>配置共享存储并移动数据库文件

有多种解决方案可用于提供共享存储。 本演练演示如何使用 NFS 配置共享存储。 建议按照最佳做法进行操作，并使用 Kerberos 保护 NFS： 

- [使用 NFS 共享文件系统](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#cha.nfs)

如果不按照此指南进行操作，则可访问网络和假冒 SQL 节点的 IP 地址的任何人均能访问你的数据文件。 与往常一样，请确保在将系统投入生产前，针对系统建立威胁模型。 

另一个存储选项是使用 SMB 文件共享：

- [SUSE 文档的 Samba 部分](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#cha.samba)

### <a name="configure-an-nfs-server"></a>配置 NFS 服务器

若要配置 NFS 服务器，请参阅 SUSE 文档中的以下步骤：[配置 NFS 服务器](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#sec.nfs.configuring-nfs-server)。

### <a name="configure-all-cluster-nodes-to-connect-to-the-nfs-shared-storage"></a>配置所有群集节点，以连接到 NFS 共享存储

在配置客户端 NFS 以装载 SQL Server 数据库文件路径，从而指向共享存储位置前，请确保将数据库文件保存到临时位置，以便稍后能够在共享上复制它们：

1. **仅在主节点上** 将数据库文件保存到临时位置。 以下脚本会创建新的临时目录、将数据库文件复制到新目录并删除旧的数据库文件。 SQL Server 以本地用户 mssql 的身份运行时，需要确保在将数据传输到已装载的共享后，本地用户对共享具有读写访问权限。 

    ```bash
    su mssql
    mkdir /var/opt/mssql/tmp
    cp /var/opt/mssql/data/* /var/opt/mssql/tmp
    rm /var/opt/mssql/data/*
    exit
    ```

    在所有群集节点上配置 NFS 客户端：

    - [配置客户端](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#sec.nfs.configuring-nfs-clients)

    > [!NOTE]
    > 对于高度可用的 NFS 存储，建议按照 SUSE 最佳做法和建议进行操作：[具有 DRBD 和 Pacemaker 的高度可用的 NFS 存储](https://www.suse.com/documentation/sle-ha-12/book_sleha_techguides/data/art_ha_quick_nfs.html)。

2. 验证 SQL Server 是否已成功通过新文件路径启动。 对每个节点均执行此操作。 此时，一次应只有一个节点运行 SQL Server。 它们不能同时运行，因为两者将同时尝试访问数据文件（要避免同时在两个节点上意外启动 SQL Server，请使用文件系统群集资源来确保共享未由不同的节点装载两次）。 以下命令可启动 SQL Server、检查状态及随后停止 SQL Server。

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    sudo systemctl stop mssql-server
    ```

此时，SQL Server 的两个实例均已配置为使用共享存储上的数据库文件运行。 接下来，为 Pacemaker 配置 SQL Server。 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>在每个群集节点上安装和配置 Pacemaker

1. **在两个群集节点上，创建一个文件来存储用于 Pacemaker 登录的 SQL Server 用户名和密码**。 以下命令创建并填充此文件：

    ```bash
    sudo touch /var/opt/mssql/secrets/passwd
    sudo echo '<loginName>' >> /var/opt/mssql/secrets/passwd
    sudo echo '<loginPassword>' >> /var/opt/mssql/secrets/passwd
    sudo chown root:root /var/opt/mssql/secrets/passwd 
    sudo chmod 600 /var/opt/mssql/secrets/passwd
    ```
2. **所有群集节点都必须能够通过 SSH 相互访问**。 hb_report 或 crm_report（用于故障排除）和 Hawk 的历史记录浏览器等工具需要在节点之间进行无密码的 SSH 访问，否则它们只能收集当前节点的数据。 如果使用非标准 SSH 端口，请使用 -X 选项（请参见手册页）。 例如，如果 SSH 端口为 3479，请使用以下命令调用 hb_report：

    ```bash
    crm_report -X "-p 3479" [...]
    ```

    有关详细信息，请参阅 [SUSE 文档中的系统要求和建议](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_requirements_other.html)。

3. **安装高可用性扩展**。 若要安装该扩展，请按照以下 SUSE 主题中的步骤操作：
    
    [安装和设置快速入门](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html)

4. **为 SQL Server 安装 FCI 资源代理**。 在两个节点上运行以下命令：

    ```bash
    sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
    sudo zypper --gpg-auto-import-keys refresh
    sudo zypper install mssql-server-ha
    ```

5. **自动设置第一个节点**。 下一步是通过配置第一个节点（SLES1）设置一个正在运行的单节点群集。 按照 SUSE 主题中[设置第一个节点](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.1st-node)的说明进行操作。

    完成后，使用 `crm status` 检查群集状态：
    ```bash
    crm status
    ```

    它应显示一个节点 (SLES1) 已配置。

6. **将节点添加到现有群集**。 接下来将 SLES2 节点加入群集。 按照 SUSE 主题中[添加第二个节点](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.2nd-node)的说明进行操作。
    
    完成后，使用 **crm status** 检查群集状态。 如果已成功添加第二个节点，则输出将如下所示：
        
    ```
    2 nodes configured
    1 resource configured
    Online: [ SLES1 SLES2 ]
    Full list of resources:
    admin_addr     (ocf::heartbeat:IPaddr2):       Started SLES1
    ```

    > [!NOTE]
    > **admin_addr** 是最初单节点群集安装期间配置的虚拟 IP 群集资源。

7.  **删除过程**。 如果需要从群集中删除节点，请使用 **ha-cluster-remove** 启动脚本。 有关详细信息，请参阅[启动脚本概述](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.bootstrap)。  

## <a name="configure-the-cluster-resources-for-sql-server"></a>为 SQL Server 配置群集资源

以下步骤介绍如何为 SQL Server 配置群集资源。 有两个需要自定义的设置。

- **SQL Server 资源名称**：群集 SQL Server 资源的名称。 
- **超时值**：超时值是将资源联机时群集所等待的时间长度。 对于 SQL Server，这是预期 SQL Server 将 `master` 数据库联机所需花费的时间。 

为环境更新以下脚本的值。 在一个节点上运行，以配置并启动群集服务。

```bash
sudo crm configure
primitive <sqlServerResourceName> ocf:mssql:fci op start timeout=<timeout_in_seconds>
colocation <constraintName> inf: <virtualIPResourceName> <sqlServerResourceName>
show
commit
exit
```

例如，以下脚本可创建名为 mssqlha 的 SQL Server 群集资源。 

```bash
sudo crm configure
primitive mssqlha ocf:mssql:fci op start timeout=60s
colocation admin_addr_mssqlha inf: admin_addr mssqlha
show
commit
exit
```

提交配置后，SQL Server 将在虚拟 IP 资源所在的同一节点上启动。 

有关详细信息，请参阅[配置并管理群集资源（命令行）](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.manual_config)。 

### <a name="verify-that-sql-server-is-started"></a>验证 SQL Server 是否已启动。 

若要验证 SQL Server 是否已启动，请运行 **crm status** 命令：

```bash
crm status
```

以下示例显示 Pacemaker 已成功作为群集资源启动时的结果。 
```
2 nodes configured
2 resources configured

Online: [ SLES1 SLES2 ]

Full list of resources:

 admin_addr     (ocf::heartbeat:IPaddr2):       Started SLES1
 mssqlha        (ocf::mssql:fci):       Started SLES1
```

## <a name="managing-cluster-resources"></a>管理群集资源

若要管理群集资源，请参阅以下 SUSE 主题：[管理群集资源](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.config.crm )

### <a name="manual-failover"></a>手动故障转移

虽然已将资源配置为在出现硬件或软件故障时自动故障转移（或迁移）到其他节点，但还可以使用 Pacemaker GUI 或命令行将资源手动移到群集中的其他节点。 

使用迁移命令来完成此任务。 例如，若要将 SQL 资源迁移到名为 SLES2 的群集节点，请执行以下命令： 

```bash
crm resource
migrate mssqlha SLES2
```

## <a name="additional-resources"></a>其他资源

[SUSE Linux Enterprise High Availability Extension - Administration Guide](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html)（SUSE Linux Enterprise High Availability Extension - 管理指南） 
