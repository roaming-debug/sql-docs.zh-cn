---
title: 教程：配置复制 (T-SQL)
description: 你将使用 Transact-SQL (T-SQL) 在包含两个 SQL Server 实例的 Linux 上配置 SQL Server 快照复制。
ms.custom: seo-dt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/09/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
titleSuffix: SQL Server on Linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: 2e423c615d026c21c3a5cc3ecaec04aaf5a44a3c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471478"
---
# <a name="configure-replication-with-t-sql"></a>使用 T-SQL 配置复制

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)] 

在本教程中，你将使用 Transact-SQL 在包含两个 SQL Server 实例的 Linux 上配置 SQL Server 快照复制。 发布服务器和分发服务器将使用同一个实例，订阅服务器将位于单独的实例。

> [!div class="checklist"]
> * 启用 Linux 上的 SQL Server 复制代理
> * 创建示例数据库
> * 配置用于 SQL Server 代理访问的快照文件夹
> * 配置分发服务器
> * 配置发布服务器
> * 创建发布和项目
> * 配置订阅服务器 
> * 运行复制作业

通过[复制存储过程](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)配置所有复制配置。

## <a name="prerequisites"></a>先决条件
若要完成本教程，需要：

- 两个 SQL Server 实例和 Linux 上最新版本的 SQL Server
- 用于发起 T-SQL 查询以设置复制的工具（例如 SQLCMD 或 SSMS）

   请参阅[使用 SSMS 管理 Linux 上的 SQL Server](./sql-server-linux-manage-ssms.md)。

   >[!NOTE]
   >[!INCLUDE[SQL Server 2017](../includes/sssqlv14-md.md)] ([CU18](https://support.microsoft.com/help/4527377)) 及更高版本支持对 Linux 上的 SQL Server 实例进行 SQL Server 复制。

## <a name="detailed-steps"></a>详细步骤

1. 在 Linux 上启用 SQL Server 复制代理。 在两台主机上，在终端中运行以下命令。 

   ```bash
   sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
   sudo systemctl restart mssql-server
   ```

1. 创建示例数据库和表。 在发布服务器上，创建示例数据库和表，将其作为发布项目。

   ```sql
   CREATE DATABASE Sales
   GO
   USE [SALES]
   GO 
   CREATE TABLE CUSTOMER([CustomerID] [int] NOT NULL, [SalesAmount] [decimal] NOT NULL)
   GO 
   INSERT INTO CUSTOMER (CustomerID, SalesAmount) VALUES (1,100),(2,200),(3,300)
   ```

   在另一个 SQL Server 实例，即订阅服务器上，创建用于接收这些项目的数据库。

   ```sql
   CREATE DATABASE Sales
   GO
   ```

1. 为 SQL Server 代理创建用于读取/写入的快照文件夹，在分发服务器上，创建快照文件夹并向“mssql”用户授予访问权限 

   ```bash
   sudo mkdir /var/opt/mssql/data/ReplData/
   sudo chown mssql /var/opt/mssql/data/ReplData/
   sudo chgrp mssql /var/opt/mssql/data/ReplData/
   ```

1. 配置分发服务器。 在本示例中，发布服务器也是分发服务器。 在发布服务器上运行以下命令，也为分发配置实例。

   ```sql
   DECLARE @distributor AS sysname
   DECLARE @distributorlogin AS sysname
   DECLARE @distributorpassword AS sysname
   -- Specify the distributor name. Use 'hostname' command on in terminal to find the hostname
   SET @distributor = N'<distributor instance name>'--in this example, it will be the name of the publisher
   SET @distributorlogin = N'<distributor login>'
   SET @distributorpassword = N'<distributor password>'
   -- Specify the distribution database. 
   
   use master
   exec sp_adddistributor @distributor = @distributor -- this should be the hostname

   -- Log into distributor and create Distribution Database. In this example, our publisher and distributor is on the same host
   exec sp_adddistributiondb @database = N'distribution', @log_file_size = 2, @deletebatchsize_xact = 5000, @deletebatchsize_cmd = 2000, @security_mode = 0, @login = @distributorlogin, @password = @distributorpassword
   GO

   DECLARE @snapshotdirectory AS nvarchar(500)
   SET @snapshotdirectory = N'/var/opt/mssql/data/ReplData/'

   -- Log into distributor and create Distribution Database. In this example, our publisher and distributor is on the same host
   use [distribution] 
   if (not exists (select * from sysobjects where name = 'UIProperties' and type = 'U ')) 
          create table UIProperties(id int) 
   if (exists (select * from ::fn_listextendedproperty('SnapshotFolder', 'user', 'dbo', 'table', 'UIProperties', null, null))) 
          EXEC sp_updateextendedproperty N'SnapshotFolder', @snapshotdirectory, 'user', dbo, 'table', 'UIProperties' 
   else 
         EXEC sp_addextendedproperty N'SnapshotFolder', @snapshotdirectory, 'user', dbo, 'table', 'UIProperties'
   GO
   ```

1. 配置发布服务器。 在发布服务器上运行以下 TSQL 命令。

   ```sql
   DECLARE @publisher AS sysname
   DECLARE @distributorlogin AS sysname
   DECLARE @distributorpassword AS sysname
   -- Specify the distributor name. Use 'hostname' command on in terminal to find the hostname
   SET @publisher = N'<instance name>' 
   SET @distributorlogin = N'<distributor login>'
   SET @distributorpassword = N'<distributor password>'
   -- Specify the distribution database. 

   -- Adding the distribution publishers
   exec sp_adddistpublisher @publisher = @publisher, 
   @distribution_db = N'distribution', 
   @security_mode = 0, 
   @login = @distributorlogin, 
   @password = @distributorpassword, 
   @working_directory = N'/var/opt/mssql/data/ReplData', 
   @trusted = N'false', 
   @thirdparty_flag = 0, 
   @publisher_type = N'MSSQLSERVER'
   GO
   ```

1. 创建发布作业。 在发布服务器上运行以下 TSQL 命令。

   ```sql
   DECLARE @replicationdb AS sysname
   DECLARE @publisherlogin AS sysname
   DECLARE @publisherpassword AS sysname
   SET @replicationdb = N'Sales'
   SET @publisherlogin = N'<Publisher login>'
   SET @publisherpassword = N'<Publisher Password>'

   use [Sales]
   exec sp_replicationdboption @dbname = N'Sales', @optname = N'publish', @value = N'true'
   
   -- Add the snapshot publication
   exec sp_addpublication 
   @publication = N'SnapshotRepl', 
   @description = N'Snapshot publication of database ''Sales'' from Publisher ''<PUBLISHER HOSTNAME>''.',
   @retention = 0, 
   @allow_push = N'true', 
   @repl_freq = N'snapshot', 
   @status = N'active', 
   @independent_agent = N'true'

   exec sp_addpublication_snapshot @publication = N'SnapshotRepl', 
   @frequency_type = 1, 
   @frequency_interval = 1, 
   @frequency_relative_interval = 1, 
   @frequency_recurrence_factor = 0, 
   @frequency_subday = 8, 
   @frequency_subday_interval = 1, 
   @active_start_time_of_day = 0,
   @active_end_time_of_day = 235959, 
   @active_start_date = 0, 
   @active_end_date = 0, 
   @publisher_security_mode = 0, 
   @publisher_login = @publisherlogin, 
   @publisher_password = @publisherpassword
   ```

1. 通过“销售”表创建项目，在发布服务器上运行以下 TSQL 命令。

   ```sql
   use [Sales]
   exec sp_addarticle 
   @publication = N'SnapshotRepl', 
   @article = N'customer', 
   @source_owner = N'dbo', 
   @source_object = N'customer', 
   @type = N'logbased', 
   @description = null, 
   @creation_script = null, 
   @pre_creation_cmd = N'drop', 
   @schema_option = 0x000000000803509D,
   @identityrangemanagementoption = N'manual', 
   @destination_table = N'customer', 
   @destination_owner = N'dbo', 
   @vertical_partition = N'false'
   ```

1. 配置订阅。 在发布服务器上运行以下 TSQL 命令。

   ```sql
   DECLARE @subscriber AS sysname
   DECLARE @subscriber_db AS sysname
   DECLARE @subscriberLogin AS sysname
   DECLARE @subscriberPassword AS sysname
   SET @subscriber = N'<Instance Name>' -- for example, MSSQLSERVER
   SET @subscriber_db = N'Sales'
   SET @subscriberLogin = N'<Subscriber Login>'
   SET @subscriberPassword = N'<Subscriber Password>'

   use [Sales]
   exec sp_addsubscription 
   @publication = N'SnapshotRepl', 
   @subscriber = @subscriber,
   @destination_db = @subscriber_db, 
   @subscription_type = N'Push', 
   @sync_type = N'automatic', 
   @article = N'all', 
   @update_mode = N'read only', 
   @subscriber_type = 0

   exec sp_addpushsubscription_agent 
   @publication = N'SnapshotRepl', 
   @subscriber = @subscriber,
   @subscriber_db = @subscriber_db, 
   @subscriber_security_mode = 0, 
   @subscriber_login = @subscriberLogin,
   @subscriber_password = @subscriberPassword,
   @frequency_type = 1,
   @frequency_interval = 0, 
   @frequency_relative_interval = 0, 
   @frequency_recurrence_factor = 0, 
   @frequency_subday = 0, 
   @frequency_subday_interval = 0, 
   @active_start_time_of_day = 0, 
   @active_end_time_of_day = 0, 
   @active_start_date = 0, 
   @active_end_date = 19950101
   GO
   ```

1. 运行复制代理作业。 运行以下查询以获取作业列表：

   ```sql
   SELECT name, date_modified FROM msdb.dbo.sysjobs order by date_modified desc
   ```

   运行快照复制作业以生成快照：

   ```sql
   USE msdb;   
   --generate snapshot of publications, for example
   EXEC dbo.sp_start_job N'PUBLISHER-PUBLICATION-SnapshotRepl-1'
   GO
   ```

   运行快照复制作业以生成快照：

   ```sql
   USE msdb;
   --distribute the publication to subscriber, for example
   EXEC dbo.sp_start_job N'DISTRIBUTOR-PUBLICATION-SnapshotRepl-SUBSCRIBER'
   GO
   ```

1. 连接订阅服务器并查询复制数据。    

   在订阅服务器上，通过运行以下查询检查复制是否正常工作：

   ```sql
   SELECT * from [Sales].[dbo].[CUSTOMER]
   ```

在本教程中，将使用 Transact-SQL 通过两个 SQL Server 实例配置 Linux 上的 SQL Server 快照复制。

> [!div class="checklist"]
> * 启用 Linux 上的 SQL Server 复制代理
> * 创建示例数据库
> * 配置用于 SQL Server 代理访问的快照文件夹
> * 配置分发服务器
> * 配置发布服务器
> * 创建发布和项目
> * 配置订阅服务器 
> * 运行复制作业

## <a name="see-also"></a>另请参阅

有关复制的详细信息，请参阅 [SQL Server 复制文档](../relational-databases/replication/sql-server-replication.md)。

## <a name="next-steps"></a>后续步骤

[概念：Linux 上的 SQL Server 复制](sql-server-linux-replication.md)

[复制存储过程](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)。
