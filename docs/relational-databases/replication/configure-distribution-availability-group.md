---
title: 在可用性组中配置分发数据库
description: 介绍如何使用 Always On 可用性组为 SQL Server 复制配置分发数据库。
ms.custom: seo-lt-2019
ms.date: 01/16/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], distribution
- distribution configuration [SQL Server replication]
- remote Distributors [SQL Server replication]
- transactional replication, configuring distribution
- distribution databases [SQL Server replication], sizing
- Distributors [SQL Server replication], configuring
- distribution databases [SQL Server replication], about distribution databases
- distribution databases [SQL Server replication]
- merge replication [SQL Server replication], configuring distribution
ms.assetid: 94d52169-384e-4885-84eb-2304e967d9f7
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 36e2f12793e08211e285c907f26d99e0fcc9262d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348001"
---
# <a name="set-up-replication-distribution-database-in-always-on-availability-group"></a>在 Always On 可用性组中设置复制分发数据库
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本文介绍在 Always On 可用性组 (AG) 中如何设置 SQL Server 复制分发数据库。

SQL Server 2017 CU6 和 SQL Server 2016 SP2-CU3 通过以下机制，引入对 AG 中复制分发数据库的支持：

- 分发数据库 AG 需要具有侦听器。 当发布服务器添加分发服务器时，它将侦听器名称用作分发服务器名称。
- 将侦听器名称作为分发服务器名称创建复制作业。 分发服务器上创建的复制快照、日志读取器和分发代理（推送订阅）作业将在分发数据库 AG 的所有次要副本上创建。
 >[!NOTE]
 >拉取订阅的分发代理作业将在订阅服务器（而非分发服务器）上创建。 
- 新的作业监视分发数据库的状态（在 AG 中为主要或次要）并基于分发数据库的状态禁用或启用复制作业。

根据以下所述的步骤配置好 AG 中的分发数据库后，在分发数据库 AG 故障转移前后，复制配置和运行时作业都可以正确运行。

## <a name="supported-scenarios"></a>支持的方案

- 将分发数据库配置到 AG 中。
- 在 AG 故障转移前后配置复制（例如发布和订阅）。
- 复制作业在故障转移前后运行。
- 当分发数据库在 AG 中时，删除分发服务器和发布服务器上的复制。
- 为现有分发数据库 AG 添加或删除节点。
- 分发服务器可能具有多个分发数据库。 每个分发数据库都可以在各自的 AG 中，但不能在任意 AG 中。 多个分发数据库可以共享一个 AG。
- 发布服务器和分发服务器都需要在单独的 SQL Server 实例上。
- 如果托管分发数据库的可用性组的侦听器配置为使用非默认端口，则需要为侦听器和非默认端口设置别名。

## <a name="limitations-or-exclusions"></a>限制或排除

- 不支持本地分发服务器。 例如，发布服务器和分发服务器必须为不同的 SQL Server 实例。 这些实例可以托管在同一组节点上。  使用自身作为分发服务器（也称为 本地分发服务器）的发布服务器无法支持 AG 中的分发数据库。
- 不支持 Oracle 发布服务器。
- 不支持合并复制。
- 不支持使用即时或排队更新订阅服务器的事务复制。
- 不支持对等复制。
- 托管分发数据库副本的所有 SQL Server 2017 实例必须是 SQL Server 2017 CU 6 或更高版本。 
- 托管分发数据库副本的所有 SQL Server 2016 实例必须是 SQL Server 2016 SP2-CU3 或更高版本。
- 托管分发数据库副本的所有 SQL Server 实例的版本必须相同，除非进行升级的时间范围较窄。
- 分发数据库必须处于完全恢复模式。
- 要恢复和允许事务日志截断，请配置完整备份和事务日志备份。
- 分发数据库 AG 必须具有已配置的侦听器。
- 分发数据库 AG 中的次要副本可以是同步的，也可以是异步的。 建议和首选同步模式。
- 不支持双向事务复制。
- 分发数据库添加到可用性组后，SSMS 不会将分发数据库显示为同步/同步。


   >[!NOTE]
   >在次要副本上执行任何复制存储过程之前（例如 `sp_dropdistpublisher`、`sp_dropdistributiondb`、`sp_dropdistributor`、`sp_adddistributiondb`、`sp_adddistpublisher`），请确保此副本已完全同步。

- 分发数据库 AG 中的所有次要副本都应可读。 如果次要副本不可读，则无法在特定次要副本上访问 SQL Server Management Studio 上的分发服务器属性，但复制将继续正常工作。 
- 分发数据库 AG 中的所有节点都需要使用相同的域帐户来运行 SQL Server 代理，并且该域帐户需要对每个节点都具有相同的特权。
- 如果任何复制代理在代理帐户下运行，则代理帐户需要存在于分发数据库 AG 的每个节点中，并且对每个节点都具有相同的特权。
- 在参与分发数据库 AG 的所有副本中，更改分发服务器或分发数据库属性。
- 在参与分发数据库 AG 的所有副本中，通过 msdb 存储过程或 SQL Server Management Studio 更改复制作业。
- 需要使用脚本完成在发布服务器上分配分发服务器。 无法使用复制向导。 支持将复制向导和属性表用于其他用途。
- 只能通过脚本完成为分发数据库配置 AG。
- 在 AG 中设置分发数据库需要是新的复制配置。 不支持将现有分发数据库切换到 AG。 同样，一旦将分发数据库从 AG 中移除，它就不能再作为有效的分发数据库运行，应该将其弃用。

## <a name="configuration-architecture"></a>配置体系结构

本文中的示例使用了以下服务器名称和设置。

- DIST1、DIST2、DIST3 为分发服务器；
- PUB 为发布服务器；
- 分发数据库 AG 形成后，侦听器的名称为 DISTLISTENER；
- DIST1 是用于分发数据库 AG 的初始主要副本。

## <a name="configure-distributor-distribution-database-and-publisher"></a>配置分发服务器、分发数据库和发布服务器

此例配置新的分发服务器和发布服务器并将分发数据库置于 AG。

### <a name="distributors-workflow"></a>分发服务器工作流

1. 通过 `sp_adddistributor @@servername` 将 DIST1、DIST2、DIST3 配置为分发服务器。 通过 `@password` 为 `distributor_admin` 指定密码。 DIST1、DIST2、DIST3 的 `@password` 应完全相同。
2. 通过 `sp_adddistributiondb` 在 DIST1 上创建分发数据库。 分发数据库的名称为 `distribution`。 将 `distribution` 数据库的恢复模式从简单更改为完全。
3. 使用 DIST1、DIST2 和 DIST3 上的副本，为 `distribution` 数据库创建 AG。 首选将所有的副本都设为同步。 将次要副本配置为可读取或允许读取。 此时，分发数据库是 AG，DIST1 是主要副本，DIST2 和 DIST3 是次要副本。
4. 为 AG 配置名为 `DISTLISTENER` 的侦听器。
5. 要恢复和允许事务日志截断，请配置完整备份和事务日志备份。
6. 在 DIST2 和 DIST3 上，运行：

   ```sql
   sp_adddistributiondb 'distribution'
   ```

1. 若要在 DIST1 上将 `PUB` 添加为发布服务器，请运行：
   
   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   `@working_directory` 的值应为独立于 DIST1、DIST2 和 DIST3 的网路路径。

1. 在 DIST2 和 DIST3 上，如果副本可作为次要副本读取，请运行以下代码：  

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   如果副本不可作为次要副本读取，请执行故障转移，使副本成为主要副本，然后运行以下代码 

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```
   
   `@working_directory` 的值应与上一步相同。

### <a name="publisher-workflow"></a>发布服务器工作流

若要在 PUB 上将 `distribution` 数据库 AG 侦听器添加为分发服务器，请运行： 

   ```sql
   sp_adddistributor @distributor = 'DISTLISTENER', @password = <distributor_admin password> 
   ```

   @password 的值应该为在分发服务器工作流中配置分发服务器时指定的值。

## <a name="remove-distributor-and-publisher"></a>删除分发服务器和发布服务器

此示例在分发数据库位于 AG 时，删除发布服务器和分发服务器。

### <a name="publisher-workflow"></a>发布服务器工作流

在 PUB 上，删除此发布服务器的所有订阅和发布，然后调用 `sp_dropdistributor`。

### <a name="distributors-workflow"></a>分发服务器工作流

在此示例中，DIST1 是 `distribution` 数据库 AG 当前的主要副本。 DIST2 和 DIST3 是次要副本。

1. 在 DIST2 和 DIST3 上，运行：

   ```sql
   sp_dropdistpublisher 'PUB',  @no_checks = 1
   ```

1. 在 DIST1 上，运行：

   ```sql
   sp_dropdistpublisher 'PUB'
   ```

1. 删除 AG。
2. 在 DIST2 和 DIST3 上，通过恢复使数据库还原，将 `distribution` 数据库更改为 read_write 模式。
   
   ```sql
   RESTORE DATABASE distribution WITH RECOVERY, KEEP_REPLICATION
   ```

1. 若要删除 `distribution` 数据库并保留快照目录，请运行： 

   ```sql
   sp_dropdistributiondb 'distribution' , @former_ag_secondary=1
   ```

  此过程将删除此副本的所有无关联作业。

1. 若要在 DIST1 上删除 `distribution` 数据库，请运行

   ```sql
   sp_dropdistributiondb 'distribution'
   ``` 

1. 如果 AG 中没有其他分发数据库，则在 DIST1、DIST2 和 DIST3 上运行 `sp_dropdistributor`。

## <a name="add-a-replica-to-distribution-database-ag"></a>向分发数据库 AG 添加副本

此例通过 AG 中的分发数据库向现有复制配置添加新的分发服务器。 在此例中，现有分发数据库在 AG 中。 DIST1 和 DIST2 是分发服务器，`distribution` 是 AG 中的分发数据库，PUB 是发布服务器。 将 DIST3 添加为 AG 中的副本。

### <a name="distributors-workflow"></a>分发服务器工作流

1. 应通过 `sp_adddistributor @@servername` 将 DIST3 配置为分发服务器。 应通过 @password 参数指定 `distributor_admin` 的密码。 此密码应与为 DIST1 和 DIST2 指定的密码相同。
2. 向当前分发数据库的 AG 添加 DIST3。
3. 在 DIST3 上，运行：

   ```sql
   sp_adddistributiondb 'distribution'
   ```

4. 在 DIST3 上，如果副本可作为次要副本读取，请运行以下代码： 

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```

   如果副本不可作为次要副本读取，请执行故障转移，使副本成为主要副本，然后运行以下代码：

   ```sql
   sp_adddistpublisher @publisher= 'PUB', @distribution_db= 'distribution', @working_directory= '<network path>'
   ```
   
   此 `@working_directory` 值应与为 DIST1 和 DIST2 指定的值相同。

4. 在 DIST3 中，必须重新创建指向订阅服务器的链接服务器。

## <a name="remove-a-replica-from-distribution-database-ag"></a>从分发数据库 AG 删除副本

此例从当前分发服务器 AG 删除分发服务器，但不会影响分发数据库 AG 中的其他副本。 在此例中，分发数据库在 AG 中。 DIST1、DIST2 和 DIST3 是分发服务器，`distribution` 是 AG 中的分发数据库，PUB 是发布服务器。 从 AG 删除 DIST3。

### <a name="distributors-workflow"></a>分发服务器工作流

1. 请确保 DIST3 是 `distribution` 数据库 AG 的次要副本。
2. 从 `distribution` 数据库 AG 删除 DIST3。
3. 在 DIST3 上，通过恢复使数据库还原，将 `distribution` 数据库更改为 read_write 模式。 例如，运行以下命令：  
   
   ```sql
   RESTORE DATABASE distribution WITH RECOVERY, KEEP_REPLICATION.
   ```
   
1. 若要删除在 DIST3 上的所有孤立作业，请运行： 

   ```sql
   sp_dropdistpublisher 'PUB', @no_checks = 1
   ```

1. 在 DIST3 上，运行：

   ```sql
   sp_dropdistributiondb 'distribution', @former_ag_secondary=1
   ```

1. 在 DIST3 上，运行： 

   ```sql
   sp_dropdistributor
   ```

## <a name="remove-a-publisher-from-distribution-database-ag"></a>从分发数据库 AG 删除发布服务器

此例从分发服务器的当前分发数据库 AG 中删除发布服务器，但不会影响此分发数据库 AG 服务的其他发布服务器。 在此例中，现有配置具有 AG 中的分发数据库。 DIST1、DIST2 和 DIST3 是分发服务器，`distribution` 是 AG 中的分发数据库，PUB1 和 PUB2 是 `distribution` 数据库服务的发布服务器。 此示例从这些分发服务器中删除 PUB1。

### <a name="publisher-workflow"></a>发布服务器工作流

在 PUB1 上，删除此发布服务器的所有订阅和发布，然后调用 `sp_dropdistributor`。

### <a name="distributor-workflow"></a>分发服务器工作流

DIST1 是 `distribution` 数据库 AG 当前的主要副本。

1. 在 DIST2 和 DIST3 上，运行：

   ```sql
   sp_dropdistpublisher 'PUB1',  @no_checks = 1
   ```

1. 在 DIST1 上，运行：

   ```sql
   sp_dropdistpublisher 'PUB1'
   ```

1. 此时，可能有与 DIST2 或 DIST3 上的 PUB1 相关的孤立作业。 每当 DIST2 和 DIST3 发生故障转移时，`Monitor and sync replication agent jobs` 作业都将删除与 PUB1 的所有发布相关的孤立作业。

## <a name="add-subscription"></a>添加订阅

此示例介绍在分发服务器之间正确配置订阅服务器的信息。 此示例添加订阅服务器。 DIST1 是 AG 中分发数据库的主要副本，DIST2 和 DIST3 是 AG 中分发数据库的次要副本。 订阅服务器的名称为 SUB。

### <a name="publisher-workflow"></a>发布服务器工作流

在 PUB 上，类似于通常对订阅服务器 `SUB` 执行的操作一样，添加订阅。

### <a name="distributor-workflow"></a>分发服务器工作流

在 DIST2 和 DIST3 上，如果之前没有使用 DIST2 和 DIST3 注册“SUB”，则为其添加已链接的服务器。 以下是为链接服务器创建的示例 TSQL -

   ```sql 
   EXEC master.dbo.sp_addlinkedserver@server =N'SUB', @srvproduct=N'SQL Server'
   /* For security reasons the linked server remote logins password is changed with ######## */
   EXEC master.dbo.sp_addlinkedsrvlogin@rmtsrvname=N'SUB', @useself=N'True',@locallogin=NULL,@rmtuser=NULL,@rmtpassword=NULL
   ```

## <a name="add-a-pull-subscription"></a>添加请求订阅

### <a name="subscriber-workflow"></a>订阅服务器工作流

若要使用 AG 中的分发数据库添加请求订阅，请使用 `sp_addpullsubscription_agent` 的 `@distributor` 参数中的 AG 侦听器名称。

## <a name="sample-t-sql-create-distribution-db-in-ag"></a>示例 T-SQL 创建 AG 中的分发数据库

以下脚本启用可用性组中的分发数据库。 

```sql
--- WorkFlow to Enable Distribution Database In AG.

-- SECTION 1 ---- CONFIGURE THE DISTRIBUTOR SERVERS

-- Step1 - Configure the Distribution DB nodes (AG Replicas) to act as a distributor
:Connect SQLNode1
sp_adddistributor @distributor = @@ServerName, @password = 'Pass@word1'
Go 
:Connect SQLNode2
sp_adddistributor @distributor = @@ServerName, @password = 'Pass@word1'
Go

-- Step2 - Configure the Distribution Database
:Connect SQLNode1
USE master
EXEC sp_adddistributiondb @database = 'DistributionDB', @security_mode = 1;
GO
Alter Database [DistributionDB] Set Recovery Full
Go
Backup Database [DistributionDB] to Disk = 'Nul'
Go
-- Step 3 - Create AG for the Distribution DB.
:Connect SQLNode1
USE [master]
GO
CREATE ENDPOINT [Hadr_endpoint] 
    STATE=STARTED
    AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)   
    FOR DATA_MIRRORING (ROLE = ALL, AUTHENTICATION = WINDOWS NEGOTIATE
, ENCRYPTION = REQUIRED ALGORITHM AES)
GO

:Connect SQLNode2
USE [master]
GO
CREATE ENDPOINT [Hadr_endpoint] 
    STATE=STARTED
    AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)   
    FOR DATA_MIRRORING (ROLE = ALL, AUTHENTICATION = WINDOWS NEGOTIATE
, ENCRYPTION = REQUIRED ALGORITHM AES)
GO

:Connect SQLNode1
-- Create the Availability Group
CREATE AVAILABILITY GROUP [DistributionDB_AG]
FOR DATABASE [DistributionDB]
REPLICA ON 'SQLNode1'
WITH (ENDPOINT_URL = N'TCP://SQLNode1.contoso.com:5022', 
         FAILOVER_MODE = AUTOMATIC, 
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
         BACKUP_PRIORITY = 50, 
         SECONDARY_ROLE(ALLOW_CONNECTIONS = ALL), 
         SEEDING_MODE = AUTOMATIC),
N'SQLNode2' WITH (ENDPOINT_URL = N'TCP://SQLNode2.contoso.com:5022', 
     FAILOVER_MODE = AUTOMATIC, 
     AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
     BACKUP_PRIORITY = 50, 
     SECONDARY_ROLE(ALLOW_CONNECTIONS = ALL), 
     SEEDING_MODE = AUTOMATIC);
 GO


:Connect SQLNode2
ALTER AVAILABILITY GROUP [DistributionDB_AG] JOIN
GO  
ALTER AVAILABILITY GROUP [DistributionDB_AG] GRANT CREATE ANY DATABASE
Go

--STEP4 - Create the Listener for the Availability Group. This is very important.
:Connect SQLNode1

USE [master]
GO
ALTER AVAILABILITY GROUP [DistributionDB_AG]
ADD LISTENER N'DistributionDBList' (
WITH IP
((N'10.0.0.8', N'255.255.255.0')) , PORT=1500);
GO

-- STEP 5 - Enable SQLNode2 also as a Distributor
:CONNECT SQLNODE2
EXEC sp_adddistributiondb @database = 'DistributionDB', @security_mode = 1;
GO

--STEP 6 - On all Distributor Nodes Configure the Publisher Details 
:CONNECT SQLNODE1
EXEC sp_addDistPublisher @publisher = 'SQLNode4', @distribution_db = 'DistributionDB', 
    @working_directory = '\\sqlfileshare\Dist_Work_Directory\'
GO
:CONNECT SQLNODE2
EXEC sp_addDistPublisher @publisher = 'SQLNode4', @distribution_db = 'DistributionDB', 
    @working_directory = '\\sqlfileshare\Dist_Work_Directory\'
GO

-- SECTION 2 ---- CONFIGURE THE PUBLISHER SERVER
:CONNECT SQLNODE4
EXEC sp_addDistributor @distributor = 'DistributionDBList', -- Listener for the Distribution DB.    
    @password = 'Pass@word1'
Go

-- SECTION 3 ---- CONFIGURE THE SUBSCRIBERS 
-- On Publisher, create the publication as one would normally do.
-- On the Secondary replicas of the Distribution DB, add the Subscriber as a linked server.
:CONNECT SQLNODE2
EXEC master.dbo.sp_addlinkedserver @server = N'SQLNODE5', @srvproduct=N'SQL Server'
 /* For security reasons the linked server remote logins password is changed with ######## */
EXEC master.dbo.sp_addlinkedsrvlogin @rmtsrvname=N'SQLNODE5',@useself=N'True',@locallogin=NULL,@rmtuser=NULL,@rmtpassword=NULL 
```

## <a name="see-also"></a>另请参阅  
 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [保护分发服务器](../../relational-databases/replication/security/secure-the-distributor.md)  
  
## <a name="next-steps"></a>后续步骤
 [查看和修改分发服务器和发布服务器属性](view-and-modify-distributor-and-publisher-properties.md)  
 [禁用发布和分发](disable-publishing-and-distribution.md)  
 [为复制启用数据库 (SQL Server Management Studio)](enable-a-database-for-replication-sql-server-management-studio.md) 
