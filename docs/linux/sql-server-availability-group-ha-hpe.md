---
title: 使用 HPE Serviceguard 部署可用性组 - Linux 上的 SQL Server
description: 将 HPE Serviceguard 用作群集管理器，以在 Linux 上的 SQL Server 上通过可用性组实现高可用性
ms.date: 01/15/2021
ms.prod: sql
ms.technology: linux
ms.topic: tutorial
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.openlocfilehash: 3956c0470ac9f4b3ac2f2a35ed057015db6ea0e0
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534865"
---
# <a name="tutorial---setup-a-three-node-always-on-availability-group-with-hpe-serviceguard-for-linux"></a>教程 - 使用适用于 Linux 的 HPE Serviceguard 设置三节点 Always On 可用性组 

本教程介绍如何使用在本地虚拟机 (VM) 上运行的适用于 Linux 的 HPE Serviceguard 配置 SQL Server Always On 可用性组。

有关 HPE Serviceguard 群集的概述，请参阅 [HPE Serviceguard 群集](https://h20195.www2.hpe.com/v2/GetPDF.aspx/c04154488.pdf)。

> [!NOTE]
> Microsoft 支持数据移动、可用性组和 SQL Server 组件。 请联系 HPE 以获取与 HPE Serviceguard 群集和仲裁管理文档相关的支持。

本教程包含以下任务：

> [!div class="checklist"]
> * 在三个 VM（将构成可用性组的一部分）上都安装 SQL Server
> * 在 VM 上安装 HPE Serviceguard
> * 创建 HPE Serviceguard 群集
> * 创建可用性组，并将示例数据库添加到可用性组
> * 通过 Serviceguard 群集管理器在可用性组上部署 SQL Server 工作负载
> * 执行自动故障转移，并将节点加入回群集

## <a name="prerequisites"></a>先决条件

* 本地环境中有三个 VM，它们在受支持的 Linux 分发版上运行 HPE Serviceguard。

   如需受支持的分发的示例，请参阅[适用于 Linux 的 HPE Serviceguard](https://h20195.www2.hpe.com/v2/gethtml.aspx?docname=c04154488)。

   请参阅 HPE，以获取有关对公有云环境的支持的信息。

   本教程中的说明已针对适用于 Linux 的 HPE Serviceguard 进行验证。 可从 [HPE](https://www.hpe.com/us/en/resources/servers/serviceguard-linux-trial.html) 下载试用版。

* 三个虚拟机的逻辑卷装载 (LVM) 上的 SQL Server 数据库文件。 请参阅 [Serviceguard Linux (HPE) 快速入门指南](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us)

* 确保 VM 上已安装 OpenJDK java 运行时。

   > [!NOTE]
   > 不支持 IBM java sdk。

## <a name="install-sql-server"></a>安装 SQL Server

在这三个 VM 上，都请根据你在本教程中选择的 Linux 分发版，按照以下步骤之一安装 SQL Server 和工具。

### <a name="rhel"></a>RHEL

* [在 Red Hat 上安装 SQL Server](quickstart-install-connect-red-hat.md#install)
* [工具](quickstart-install-connect-red-hat.md#tools)

### <a name="sles"></a>SLES

* [在 SLES 上安装 SQL Server](quickstart-install-connect-suse.md#install)
* [工具](quickstart-install-connect-suse.md#tools)

完成此步骤后，应在要加入可用性组的三个 VM 上都安装 SQL Server 服务和工具。

## <a name="install-hpe-serviceguard-on-the-vms"></a>在 VM 上安装 HPE Serviceguard

在此步骤中，在三个 VM 上都安装适用于 Linux 的 HPE Serviceguard。 下表描述了每个服务器在群集中扮演的角色。

|VM 数量 | HPE Servicguard 角色 | Microsoft SQL Server 可用性组副本角色|
|--------------|-----------------|------------|
|1 | HPE Serviceguard 群集节点 | 主副本 |
|大于等于 1 | HPE Serviceguard 群集节点 | 辅助副本 |
|1 | HPE Serviceguard 仲裁服务器 | 仅配置副本 |

若要安装 Serviceguard，请使用 `cminstaller` 方法。 以下链接提供了具体说明：

Serviceguard 群集和 Serviceguard 仲裁服务器

* [在两个节点上安装适用于 Linux 的 Serviceguard](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Install_serviceguard_using_cminstaller)。 请参阅“Install_serviceguard_using_cminstaller”部分。
* [在第三个节点上安装 Serviceguard 仲裁服务器](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Install_QS_from_the_ISO)。 请参阅“Install_QS_from_the_ISO”部分。

完成 HPE Serviceguard 群集的安装后，可以在主要副本节点的 TCP 端口 5522 上启用群集管理门户。 以下步骤会向防火墙添加一条规则来允许 5522，下面的命令适用于 RHEL 分发，你需要为其他分发运行类似的命令：

```console
sudo firewall-cmd --zone=public --add-port=5522/tcp --permanent

sudo firewall-cmd --reload 
```

## <a name="create-hpe-serviceguard-cluster"></a>创建 HPE Serviceguard 群集

按照以下说明配置和创建 HPE Serviceguard 群集。 在此步骤中，我们还将配置仲裁服务器。

1. [在第三个节点上配置 Serviceguard 仲裁服务器](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Configure_QS)。 请参阅“Configure_QS”部分。
2. [在其他两个节点上配置和创建 Serviceguard 群集](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Configure_and_create_cluster)。 请参阅“Configure_and_create_Cluster”部分。

## <a name="create-the-availability-group-and-add-a-sample-database"></a>创建可用性组并添加示例数据库

在此步骤中，创建具有两个（或更多）同步副本和一个仅配置副本的可用性组，该可用性组除了提供数据保护，还可以提供高可用性。 下图呈现了此体系结构：

:::image type="content" source="media/sql-server-linux-availability-group-ha/2-configuration-only.png" alt-text="主要副本将用户数据和配置数据与次要副本进行同步。主要副本只会将配置数据与仅配置副本进行同步。仅配置副本没有用户数据副本。":::

1. 用户数据到次要副本的同步复制。 它还包括可用性组配置元数据。

2. 可用性组配置元数据的同步复制。 它不包括用户数据。

有关更多详细信息，请参阅[两个同步副本和一个仅配置副本](sql-server-linux-availability-group-ha.md)。

若要创建可用性组，请执行以下步骤：

1. 在所有 VM（包括仅配置副本）上[启用 AlwaysOn 可用性组并重启 mssql-server](#enable-always-on-availability-groups-and-restart-mssql-server)。
2. [启用 `AlwaysOn_health` 事件会话（可选）](#enable-an-alwayson_health-event-session---optional)
3. [在主 VM 上创建证书](#create-a-certificate-on-the-primary-vm)
4. [在辅助服务器上创建证书](#create-the-certificate-on-secondary-servers)
5. [在副本上创建数据库镜像终结点](#create-the-database-mirroring-endpoints-on-the-replicas)
6. [创建可用性组](#create-availability-group)
7. [联接次要副本](#join-the-secondary-replicas)
8. [将数据库添加到上面创建的可用性组](#add-a-database-to-the-availability-group-created-above)

### <a name="enable-always-on-availability-groups-and-restart-mssql-server"></a>启用 AlwaysOn 可用性组，并重启 mssql-server

在托管 SQL Server 实例的所有节点上启用 Always On 功能。 然后重启 mssql-server。 在三个节点上都运行以下脚本：

```console
sudo /opt/mssql/bin/mssql-conf
set hadr.hadrenabled 1 sudo systemctl restart mssql-serve
```

### <a name="enable-an-alwayson_health-event-session---optional"></a>启用 `AlwaysOn_health` 事件会话（可选）

可选择性地启用 AlwaysOn 可用性组的扩展事件，以便在对可用性组进行故障排除时帮助诊断根本原因。 在每个 SQL Server 实例上运行以下命令：

```tsql
ALTER EVENT SESSION AlwaysOn_health ON SERVER WITH (STARTUP_STATE=ON);
GO
```

### <a name="create-a-certificate-on-the-primary-vm"></a>在主 VM 上创建证书

以下 Transact-SQL 脚本创建主密钥和证书。 然后备份证书，并使用私钥保护文件。 使用强密码更新脚本。 连接到主 SQL Server 实例，并运行以下 Transact-SQL 脚本：

```tsql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Master_Key_Password';

CREATE CERTIFICATE dbm_certificate WITH SUBJECT = 'dbm';

BACKUP CERTIFICATE dbm_certificate TO FILE = '/var/opt/mssql/data/dbm_certificate.cer'
WITH PRIVATE KEY 
    ( FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
      ENCRYPTION BY PASSWORD = '<Private_Key_Password>' );

```

现在，主 SQL Server 副本的证书位于 `/var/opt/mssql/data/dbm_certificate.cer`，私钥位于 `var/opt/mssql/data/dbm_certificate.pvk`。 将这两个文件复制到所有要托管可用性副本的服务器上的同一位置。 使用 mssql 用户或为 mssql 用户授予访问这些文件的权限。

例如，在源服务器上，以下命令可将文件复制到目标计算机。 将 `'node2'` 值替换为运行辅助 SQL Server 实例的主机的名称。 同时将证书复制到仅配置副本上，并在该节点上运行以下命令。

```console
cd /var/opt/mssql/data
scp dbm_certificate.* root@<node2>:/var/opt/mssql/data/
```

现在，在运行辅助实例的辅助 VM 和 SQL Server 仅配置副本上运行以下命令，以便 mssql 用户可以拥有复制的证书：

```console
cd /var/opt/mssql/data

chown mssql:mssql dbm_certificate.*
```

### <a name="create-the-certificate-on-secondary-servers"></a>在辅助服务器上创建证书

以下 Transact-SQL 脚本根据在主 SQL Server 副本上创建的备份创建主密钥和证书。 使用强密码更新脚本。 解密密码与在此前的步骤中创建 .pvk 文件使用的密码相同。 若要创建证书，请在除仅配置副本之外的所有辅助服务器上运行以下脚本：

```tsql
CREATE MASTER KEY ENCRYPTION BY PASSWORD =
'<Master_Key_Password>';

CREATE CERTIFICATE dbm_certificate FROM FILE =
'/var/opt/mssql/data/dbm_certificate.cer'
WITH PRIVATE KEY ( FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
DECRYPTION BY PASSWORD = '<Private_Key_Password>' );
```

### <a name="create-the-database-mirroring-endpoints-on-the-replicas"></a>在副本上创建数据库镜像终结点

在主要副本和次要副本上运行以下命令以创建数据库镜像终结点：

```tsql
CREATE ENDPOINT [hadr_endpoint] AS TCP (LISTENER_PORT = 5022)
    FOR DATABASE_MIRRORING 
        (
        ROLE = WITNESS,
        AUTHENTICATION = CERTIFICATE dbm_certificate,
        ENCRYPTION = REQUIRED ALGORITHM AES
        );

ALTER ENDPOINT [hadr_endpoint] STATE = STARTED;
```

> [!NOTE]
> 5022 是用于数据库镜像终结点的标准端口，但你可以将其更改为任意可用端口。

在仅配置副本上，使用以下命令创建数据库镜像终结点，请注意此处“角色”的值设置为 `WITNESS`，这是仅配置副本所需的值。

```tsql
CREATE ENDPOINT [hadr_endpoint] AS TCP (LISTENER_PORT = 5022)
    FOR DATABASE_MIRRORING (
        ROLE = WITNESS,
        AUTHENTICATION = CERTIFICATE dbm_certificate,
        ENCRYPTION = REQUIRED ALGORITHM AES
        );

ALTER ENDPOINT [hadr_endpoint] STATE = STARTED;
```

### <a name="create-availability-group"></a>创建可用性组

在主要副本实例上，运行以下命令。 这些命令将创建一个名为 ag1 的具有外部 `cluster_type` 的可用性组，并将向该可用性组授予创建数据库的权限 。

在运行以下脚本之前，请将 `<node1>`、`<node2>` 和 `<node3>`（仅配置副本）占位符替换为在之前步骤中创建的 VM 的名称。

```tsql
CREATE AVAILABILITY GROUP [ag1]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR REPLICA ON
    N'<node1>' WITH (
        ENDPOINT_URL = N'tcp://<node1>:<5022>',
        AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
        FAILOVER_MODE = EXTERNAL,
        SEEDING_MODE = AUTOMATIC
        ),

    N'<node2>' WITH (
        ENDPOINT_URL = N'tcp://<node2>:\<5022>',
        AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
        FAILOVER_MODE = EXTERNAL,
        SEEDING_MODE = AUTOMATIC
        ),
    
    N'<node3>' WITH (
        ENDPOINT_URL = N'tcp://<node3>:<5022>',
        AVAILABILITY_MODE = CONFIGURATION_ONLY
        );
          
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

### <a name="join-the-secondary-replicas"></a>联接次要副本

在所有次要副本上运行以下命令。 这些命令将次要副本与主要副本联接到“ag1”可用性组，并提供对 ag1 可用性组的“创建数据库”访问权限。

```tsql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
GO
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
GO
```

### <a name="add-a-database-to-the-availability-group-created-above"></a>将数据库添加到上面创建的可用性组

连接到主要副本，并运行以下 T-SQL 命令来执行以下操作：

1. 创建一个名为 db1 的示例数据库，该数据库将添加到可用性组。
2. 将数据库的恢复模式设置为“完整”。 可用性组中的所有数据库都需处于完整恢复模式。
3. 备份数据库。 数据库至少需要一个完整备份，然后才能将其添加到可用性组。

```tsql
CREATE DATABASE [db1]; -- creates a database named db1
GO
ALTER DATABASE [db1] SET RECOVERY FULL; -- set the database in full recovery mode
GO
BACKUP DATABASE [db1] -- backs up the database to disk TO DISK = N'/var/opt/mssql/data/db1.bak';
GO
ALTER AVAILABILITY GROUP [ag1] ADD DATABASE [db1]; -- adds the database db1 to the AG
GO
```

成功完成上述步骤后，可以看到创建了 ag1 可用性组，并且添加了三个 VM 作为副本，其中包含一个主要副本、一个次要副本和 一个仅配置副本。 ag1 包含一个数据库。

## <a name="deploy-the-sql-server-availability-group-workload-hpe-cluster-manager"></a>部署 SQL Server 可用性组工作负载（HPE 群集管理器）

在 HPE Serviceguard 中，通过 Serviceguard 群集管理器 UI 在可用性组上部署 SQL Server 工作负载。
   
使用 [Serviceguard 管理器图形用户界面](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Protect_your_alwayson_availability_group)，通过 Serviceguard 群集部署可用性组工作负载并启用高可用性 (HA)、灾难恢复 (DR)。 请参阅“保护 Always On 可用性组的 Linux 上的 Microsoft SQL Server”部分。

## <a name="perform-automatic-failover-and-join-the-node-back-to-cluster"></a>执行自动故障转移，并将节点加入回群集

对于自动故障转移测试，可以继续操作并关闭主要副本（关闭电源），这将复制主节点突然不可用的情况。 预计的行为如下：

1. 群集管理器会将可用性组中的其中一个次要副本提升为主要副本。
2. 发生故障的主要副本在备份后会自动加入群集。 群集管理器会将其提升为次要副本。

对于 HPE Serviceguard，请参阅[测试设置以实现故障转移就绪](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Test_the_setup_preparedness)部分

## <a name="next-steps"></a>后续步骤

详细了解 [Linux 上的 Always On 可用性组](sql-server-linux-availability-group-overview.md)。
