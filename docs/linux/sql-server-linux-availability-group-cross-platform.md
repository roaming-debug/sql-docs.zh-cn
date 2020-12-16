---
title: 在 Windows 和 Linux 上配置 SQL Server AlwaysOn 可用性组
description: 了解如何创建 SQL Server“Always On 可用性组 (AG)”，且一个副本在 Windows 服务器上，另一个副本在 Linux 服务器上。
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 01/31/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
monikerRange: '>= sql-server-2017'
ms.openlocfilehash: d19971ffb5a8be19448212157c46a969e25af557
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471588"
---
# <a name="configure-sql-server-always-on-availability-group-on-windows-and-linux-cross-platform"></a>在 Windows 和 Linux 上配置 SQL Server AlwaysOn 可用性组（跨平台）

[!INCLUDE[tsql-appliesto-sslinux-only](../includes/applies-to-version/sqlserver2017.md)]

本文介绍后述操作的步骤：创建“Always On 可用性组 (AG)”，且一个副本在 Windows 服务器上，另一个副本在 Linux 服务器上。 此配置是跨平台的，因为副本在不同的操作系统上。 使用此配置从一个平台迁移到另一个平台或实现灾难恢复 (DR)。 此配置不支持高可用性，因为没有可用于管理跨平台配置的群集解决方案。 

![混合 None](./media/sql-server-linux-availability-group-overview/image1.png)

在继续之前，应熟悉 Windows 和 Linux 上 SQL Server 实例的安装和配置。 

## <a name="scenario"></a>场景

在此方案中，两台服务器使用不同的操作系统。 名为 `WinSQLInstance` 的 Windows Server 2016 承载主副本。 名为 `LinuxSQLInstance` 的 Linux 服务器承载次要副本。

## <a name="configure-the-ag"></a>配置 AG 

创建 AG 的步骤与为“读取缩放”工作负荷创建 AG 的步骤相同。 AG 群集类型为 NONE，因为没有群集管理器。 

   >[!NOTE]
   >对于本文中的脚本，尖括号 `<` 和 `>` 用于标识需要为环境替换的值。 脚本不需要尖括号本身。 

1. 在 Windows Server 2016 上安装 SQL Server 2017，从 SQL Server 配置管理器启用 AlwaysOn 可用性组，并设置混合模式身份验证。 

   >[!TIP]
   >如果要在 Azure 中验证此解决方案，请将两个服务器放在同一可用性集中，以确保它们在数据中心中相互独立。 

   **启用可用性组**

   有关详细信息，请参阅[启用和禁用 AlwaysOn 可用性组 (SQL Server)](../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)。

   ![启用可用性组](./media/sql-server-linux-availability-group-cross-platform/1-sqlserver-configuration-manager.png)

   SQL Server 配置管理器指出计算机不是故障转移群集中的节点。 

   启用可用性组后，重新启动 SQL Server。

   **设置混合模式身份验证**

   如需相关说明，请参阅[更改服务器身份验证模式](../database-engine/configure-windows/change-server-authentication-mode.md#change-authentication-mode-with-ssms)。

1. 在 Linux 上安装 SQL Server 2017。 有关说明，请参阅[安装 SQL Server](sql-server-linux-setup.md)。 通过 mssql-conf 启用 `hadr`。

   要从 shell 提示符通过 mssql-conf 启用 `hadr`，请发出以下命令：

   ```bash
   sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled 1
   ```

   启用 `hadr` 后，重新启动 SQL Server 实例。  

   下图显示了此完整步骤。

   ![显示命令的 Git Bash 窗口的屏幕截图。](./media/sql-server-linux-availability-group-cross-platform/2-sqlserver-linux-set-hadr.png)

1. 同时在两台服务器上配置 hosts 文件或向 DNS 注册服务器名称。

1. 同时在 Windows 和 Linux 上为 TPC 1433 和 5022 打开防火墙端口。

1. 在主副本上，创建数据库登录名和密码。

   ```sql
   CREATE LOGIN dbm_login WITH PASSWORD = '<C0m9L3xP@55w0rd!>';
   CREATE USER dbm_user FOR LOGIN dbm_login;
   GO
   ```

1. 在主副本上，创建主密钥和证书，然后使用私钥备份证书。

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<C0m9L3xP@55w0rd!>';
   CREATE CERTIFICATE dbm_certificate WITH SUBJECT = 'dbm';
   BACKUP CERTIFICATE dbm_certificate
   TO FILE = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\DATA\dbm_certificate.cer'
   WITH PRIVATE KEY (
           FILE = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\DATA\dbm_certificate.pvk',
           ENCRYPTION BY PASSWORD = '<C0m9L3xP@55w0rd!>'
       );
   GO
   ```

1. 将证书和私钥复制到 Linux 服务器（次要副本）的 `/var/opt/mssql/data` 处。 可以使用 `pscp` 将文件复制到 Linux 服务器。 

1. 将私钥和证书的组和所有权设置为 `mssql:mssql`。

   以下脚本设置文件的组和所有权。 

   ```bash
   sudo chown mssql:mssql /var/opt/mssql/data/dbm_certificate.pvk
   sudo chown mssql:mssql /var/opt/mssql/data/dbm_certificate.cer
   ```

   下图中为证书和密钥正确设置了所有权和组。

   ![Git Bash 窗口的屏幕截图，其中显示 /var/opt/mssql/data 文件夹中的 .cer 和 .pvk。](./media/sql-server-linux-availability-group-cross-platform/3-cert-key-owner-group.png)


1. 在次要副本上，创建数据库登录名和密码并创建主密钥。

   ```sql
   CREATE LOGIN dbm_login WITH PASSWORD = '<C0m9L3xP@55w0rd!>';
   CREATE USER dbm_user FOR LOGIN dbm_login;
   GO
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<M@st3rKeyP@55w0rD!>'
   GO
   ```

1. 在次要副本上，还原复制到 `/var/opt/mssql/data` 的证书。 

   ```sql
   CREATE CERTIFICATE dbm_certificate   
       AUTHORIZATION dbm_user
       FROM FILE = '/var/opt/mssql/data/dbm_certificate.cer'
       WITH PRIVATE KEY (
       FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
       DECRYPTION BY PASSWORD = '<C0m9L3xP@55w0rd!>'
   )
   GO
   ```

1. 在主副本上，创建终结点。

   ```sql
   CREATE ENDPOINT [Hadr_endpoint]
       AS TCP (LISTENER_IP = (0.0.0.0), LISTENER_PORT = 5022)
       FOR DATA_MIRRORING (
           ROLE = ALL,
           AUTHENTICATION = CERTIFICATE dbm_certificate,
           ENCRYPTION = REQUIRED ALGORITHM AES
           );
   ALTER ENDPOINT [Hadr_endpoint] STATE = STARTED;
   GRANT CONNECT ON ENDPOINT::[Hadr_endpoint] TO [dbm_login]
   GO
   ```

   >[!IMPORTANT]
   >必须为侦听器 TCP 端口打开防火墙。 在上面的脚本中，端口是 5022。 使用任何可用的 TCP 端口。 

1. 在次要副本上，创建终结点。 在次要副本上重复上述脚本以创建端点。 

1. 在主副本上，使用 `CLUSTER_TYPE = NONE` 创建 AG。 示例脚本使用 `SEEDING_MODE = AUTOMATIC` 创建 AG。 

   >[!NOTE]
   >当 SQL Server 的 Windows 实例对数据和日志文件使用不同的路径时，SQL Server 的 Linux 实例的自动种子设定将失败，因为这些路径在次要副本上不存在。 若要对跨平台 AG 使用以下脚本，数据库要求数据和日志文件在 Windows 服务器上具有相同的路径。 或者，还可以更新脚本以设置 `SEEDING_MODE = MANUAL`，然后使用 `NORECOVERY` 备份和还原数据库，从而为数据库设定种子。 
   >
   >此行为适用于 Azure Marketplace 映像。 
   >
   >有关自动种子设定的详细信息，请参阅[自动种子设定 - Disk Layout](../database-engine/availability-groups/windows/automatic-seeding-secondary-replicas.md#disklayout)。 

   在运行脚本之前，请更新 AG 的值。

      * 将 `<WinSQLInstance>` 替换为主副本 SQL Server 实例的服务器名称。

      * 将 `<LinuxSQLInstance>` 替换为次要副本 SQL Server 实例的服务器名称。 

   若要创建 AG，请更新值并在主副本上运行脚本。  

   ```sql
   CREATE AVAILABILITY GROUP [ag1]
       WITH (CLUSTER_TYPE = NONE)
       FOR REPLICA ON
           N'<WinSQLInstance>' 
        WITH (
           ENDPOINT_URL = N'tcp://<WinSQLInstance>:5022',
           AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            SEEDING_MODE = AUTOMATIC,
            FAILOVER_MODE = MANUAL,
           SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            ),
           N'<LinuxSQLInstance>' 
       WITH (
            ENDPOINT_URL = N'tcp://<LinuxSQLInstance>:5022',
           AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
           SEEDING_MODE = AUTOMATIC,
           FAILOVER_MODE = MANUAL,
           SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
           )
   GO
   ```
   
   有关详细信息，请参阅 [CREATE AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/create-availability-group-transact-sql.md)。

1. 在次要副本上，联接 AG。

   ```sql
   ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE)
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE
   GO
   ```

1. 为 AG 创建数据库。 示例步骤使用名为 `<TestDB>` 的数据库。 如果使用自动种子设定，请为数据和日志文件设置相同的路径。 

   在运行脚本之前，请更新数据库的值。

      * 将 `<TestDB>` 替换为数据库的名称。

      * 将 `<F:\Path>` 替换为数据库和日志文件的路径。 为数据库和日志文件使用相同的路径。 

      也可以使用默认路径。 

    若要创建数据库，请运行该脚本。 

   ```sql
   CREATE DATABASE [<TestDB>]
      CONTAINMENT = NONE
     ON  PRIMARY ( NAME = N'<TestDB>', FILENAME = N'<F:\Path>\<TestDB>.mdf')
     LOG ON ( NAME = N'<TestDB>_log', FILENAME = N'<F:\Path>\<TestDB>_log.ldf')
   GO
   ```

1. 完整备份数据库。 

1. 如果不使用自动种子设定，请在次要副本 (Linux) 服务器上还原数据库。 [使用备份和还原将 SQL Server 数据库从 Windows 迁移到 Linux](sql-server-linux-migrate-restore-database.md)。 在次要副本上还原数据库 `WITH NORECOVERY`。 

1. 将数据库添加到 AG。 更新示例脚本。 将 `<TestDB>` 替换为数据库的名称。 在主副本上，运行 SQL 查询以将数据库添加到 AG。

   ```sql
   ALTER AG [ag1] ADD DATABASE <TestDB>
   GO
   ```

1. 验证是否在次要副本上填充了数据库。 

## <a name="fail-over-the-primary-replica"></a>故障转移主副本

[!INCLUDE[Force failover](../includes/ss-force-failover-read-scale-out.md)]

本文回顾了创建跨平台 AG 以支持迁移或“读取缩放”工作负荷的步骤。 它可用于手动灾难恢复。 还介绍了如何对 AG 进行故障转移。 跨平台 AG 使用群集类型 `NONE`，并且不支持高可用性，因为没有跨平台的群集工具。 

## <a name="next-steps"></a>后续步骤

[AlwaysOn 可用性组概述](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)

[有关 Linux 部署的 SQL Server 可用性基础知识](sql-server-linux-ha-basics.md)
