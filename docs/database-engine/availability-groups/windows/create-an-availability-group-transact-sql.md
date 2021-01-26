---
title: 使用 Transact-SQL (T-SQL) 创建可用性组
description: 使用 Transact-SQL 在启用了 Always On 可用性组功能的 SQL Server 2019 (15.x) 实例上创建和配置可用性组。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], creating
ms.assetid: 8b0a6301-8b79-4415-b608-b40876f30066
author: cawrites
ms.author: chadam
ms.openlocfilehash: 590b7634a1dfb903d522c394f3d52ad9dc8ba1df
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765612"
---
# <a name="create-an-always-on-availability-group-using-transact-sql-t-sql"></a>使用 Transact-SQL (T-SQL) 创建 Always On 可用性组
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 在启用了 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 功能的 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 实例上创建和配置可用性组。 “可用性组”  定义一组用户数据库，这些用户数据库将以支持故障转移的单个单元和一组故障转移伙伴（称作“可用性副本”  ）的形式进行故障转移。  
  
> [!NOTE]  
>  有关可用性组的简介，请参阅 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)中通过 PowerShell 创建和配置 AlwaysOn 可用性组。  
  
> [!NOTE]  
>  除了使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)]之外，您还可以使用“创建可用性组”向导或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell cmdlet。 有关详细信息，请参阅 [使用可用性组向导 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)、 [使用“新建可用性组”对话框 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)或 [创建可用性组 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)。  

  
## <a name="prerequisites-restrictions-and-recommendations"></a><a name="PrerequisitesRestrictions"></a> 先决条件、限制和建议  
  
-   创建可用性组之前，请先验证承载可用性副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例位于同一 WSFC 故障转移群集内的不同 Windows Server 故障转移群集 (WSFC) 节点上。 此外，还请验证每个服务器实例是否都满足所有其他 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 先决条件。 有关详细信息，我们强烈建议你参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要 **sysadmin** 固定服务器角色的成员资格，以及 CREATE AVAILABILITY GROUP 服务器权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
##  <a name="using-transact-sql-to-create-and-configure-an-availability-group"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL 创建和配置可用性组 

###  <a name="summary-of-tasks-and-corresponding-transact-sql-statements"></a><a name="SummaryTsqlStatements"></a> 任务和相应 Transact-SQL 语句摘要  
 下表列出了涉及创建和配置可用性组的基本任务，并且指出了要用于这些任务的 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句。 必须按照任务在表中出现的顺序执行 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 任务。  
  
|任务|Transact-SQL 语句|执行任务的位置&#42; |  
|----------|----------------------------------|---------------------------------|  
|创建数据库镜像端点（每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例一次）|[CREATE ENDPOINT](../../../t-sql/statements/create-endpoint-transact-sql.md) *endpointName* ...FOR DATABASE_MIRRORING|在缺少数据库镜像端点的每个服务器实例上执行。|  
|创建可用性组|[CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)|在要承载初始主副本的服务器实例上执行。|  
|将辅助副本联接到可用性组|[ALTER AVAILABILITY GROUP](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md) *group_name* JOIN|在承载辅助副本的各服务器实例上执行。|  
|准备辅助数据库|[BACKUP](../../../t-sql/statements/backup-transact-sql.md) 和 [RESTORE](../../../t-sql/statements/restore-statements-transact-sql.md)。|在承载主副本的服务器实例上创建备份。<br /><br /> 使用 RESTORE WITH NORECOVERY 在承载辅助副本的各服务器实例上还原备份。|  
|通过将各辅助数据库联接到可用性组，开始数据同步|[ALTER DATABASE](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) *database_name* SET HADR AVAILABILITY GROUP = *group_name*|在承载辅助副本的各服务器实例上执行。|  
  
 *若要执行某个给定任务，请连接到指示的服务器实例。   
 
### <a name="using-transact-sql"></a>“使用 Transact-SQL” 
> [!NOTE]  
>  对于包含每个这些 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句代码示例的示例配置过程，请参阅[示例：配置使用 Windows 身份验证的可用性组](#ExampleConfigAGWinAuth)。  
  
1.  连接到要承载主副本的服务器实例。  
  
2.  通过使用 [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句创建可用性组。  
  
3.  将新的辅助副本联接到可用性组。 有关详细信息，请参阅 [将辅助副本联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)或 PowerShell 将辅助数据库联接到 Always On 可用性组。  
  
4.  对于可用性组中的每个数据库，通过使用 RESTORE WITH NORECOVERY 还原主数据库的最近的备份，创建辅助数据库。 有关详细信息，请参阅[示例：使用 Windows 身份验证设置可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)，从还原数据库备份的步骤开始。  
  
5.  将每个新的辅助数据库联接到可用性组。 有关详细信息，请参阅 [将辅助副本联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)或 PowerShell 将辅助数据库联接到 Always On 可用性组。  
  
##  <a name="example-configuring-an-availability-group-that-uses-windows-authentication"></a><a name="ExampleConfigAGWinAuth"></a> 示例：配置使用 Windows 身份验证的可用性组  
 此示例创建一个示例 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 配置过程，该过程使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 设置使用 Windows 身份验证的数据库镜像端点并且创建和配置一个可用性组及其辅助数据库。  
  
 此示例包含以下部分：  
  
-   [使用示例配置过程的先决条件](#PrerequisitesForExample)  
  
-   [示例配置过程](#SampleProcedure)  
  
-   [示例配置过程的完整代码示例](#CompleteCodeExample)  
  
###  <a name="prerequisites-for-using-the-sample-configuration-procedure"></a><a name="PrerequisitesForExample"></a> 使用示例配置过程的先决条件  
 此示例过程具有以下要求：  
  
-   服务器实例必须支持 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]。 有关详细信息，请参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)配置服务器实例时遇到的典型问题。  
  
-   两个示例数据库 *MyDb1* 和 *MyDb2* 必须在将承载主副本的服务器实例上存在。 下面的代码示例创建和配置这两个数据库，并且创建这两个数据库的完整备份。 在您想要创建示例可用性组的服务器实例上执行这些代码示例。 此服务器实例将承载示例可用性组的初始主副本。  
  
    1.  下面的 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 示例创建这些数据库并且将它们更改为使用完整恢复模式：  
  
        ```sql  
        -- Create sample databases:  
        CREATE DATABASE MyDb1;  
        GO  
        ALTER DATABASE MyDb1 SET RECOVERY FULL;  
        GO  
  
        CREATE DATABASE MyDb2;  
        GO  
        ALTER DATABASE MyDb2 SET RECOVERY FULL;  
        GO  
        ```  
  
    2.  下面的代码示例创建 *MyDb1* 和 *MyDb2* 的完整数据库备份。 此代码示例使用虚构的备份共享 \\\\*FILESERVER*\\*SQLbackups*。  
  
        ```sql  
        -- Backup sample databases:  
        BACKUP DATABASE MyDb1   
        TO DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
            WITH FORMAT;  
        GO  
  
        BACKUP DATABASE MyDb2   
        TO DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
            WITH FORMAT;  
        GO  
        ```  
  
 [[示例顶部]](#ExampleConfigAGWinAuth)  
  
###  <a name="sample-configuration-procedure"></a><a name="SampleProcedure"></a> 示例配置过程  
 在这个示例配置中，将在两个独立服务器实例上创建可用性副本，这些服务器实例的服务帐户在不同的可信域（`DOMAIN1` 和 `DOMAIN2`）下运行。  
  
 下表总结了此示例配置中使用的值。  
  
|初始角色|系统|主机 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例|  
|------------------|------------|---------------------------------------------|  
|主|`COMPUTER01`|`AgHostInstance`|  
|辅助副本|`COMPUTER02`|默认实例。|  
  
1.  在你计划创建可用性组的服务器实例（这是 *上名为* 的实例）上创建名为 `AgHostInstance` dbm_endpoint `COMPUTER01`的数据库镜像端点。 此端点使用端口 7022。 请注意，您要在其上创建可用性组的服务器实例将承载主副本。  
  
    ```sql  
    -- Create endpoint on server instance that hosts the primary replica:  
    CREATE ENDPOINT dbm_endpoint  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=7022)   
        FOR DATABASE_MIRRORING (ROLE=ALL);  
    GO  
    ```  
  
2.  在将承载辅助副本的服务器实例（这是 *上的默认服务器实例）上创建端点* dbm_endpoint `COMPUTER02`。 此端点使用端口 5022。  
  
    ```sql  
    -- Create endpoint on server instance that hosts the secondary replica:   
    CREATE ENDPOINT dbm_endpoint  
        STATE=STARTED   
        AS TCP (LISTENER_PORT=5022)   
        FOR DATABASE_MIRRORING (ROLE=ALL);  
    GO  
    ```  
  
3.  > [!NOTE]  
    >  如果要承载您的可用性副本的服务器实例的服务帐户基于相同的域帐户运行，则不需要执行此步骤。 跳过它，直接进入下一步。  
  
     如果该服务器实例的服务帐户基于不同的域用户运行，则在各服务器实例上，为其他服务器实例创建一个登录名，并且授予此登录名访问本地数据库镜像端点的权限。  
  
     下面的代码示例说明用于创建一个登录名并且向该登录名授予针对某一端点的权限的 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句。 远程服务器实例的域帐户在此处表示为 *domain_name*\\*user_name*。  
  
    ```sql  
    -- If necessary, create a login for the service account, domain_name\user_name  
    -- of the server instance that will host the other replica:  
    USE master;  
    GO  
    CREATE LOGIN [domain_name\user_name] FROM WINDOWS;  
    GO  
    -- And Grant this login connect permissions on the endpoint:  
    GRANT CONNECT ON ENDPOINT::dbm_endpoint   
       TO [domain_name\user_name];  
    GO  
    ```  
  
4.  在用户数据库驻留的服务器实例上，创建可用性组。  
  
     下面的代码示例在创建了示例数据库 *MyDb1* 和 *MyDb2* 的服务器实例上创建名为 *MyAG* 的可用性组。 首先指定 `AgHostInstance`COMPUTER01 *上的本地服务器实例* 。 该实例将承载初始的主副本。 指定远程服务器实例（ *COMPUTER02* 上的默认服务器实例）承载辅助副本。 将两个可用性副本配置为使用异步提交模式和手动故障转移（对于异步提交副本，手动故障转移意味着强制故障转移可能造成数据丢失）。  
  
    ```sql
    -- Create the availability group, MyAG:   
    CREATE AVAILABILITY GROUP MyAG   
       FOR   
          DATABASE MyDB1, MyDB2   
       REPLICA ON   
          'COMPUTER01\AgHostInstance' WITH   
             (  
             ENDPOINT_URL = 'TCP://COMPUTER01.Adventure-Works.com:7022',   
             AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
             FAILOVER_MODE = MANUAL  
             ),  
          'COMPUTER02' WITH   
             (  
             ENDPOINT_URL = 'TCP://COMPUTER02.Adventure-Works.com:5022',  
             AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
             FAILOVER_MODE = MANUAL  
             );   
    GO  
    ```  
  
     有关创建可用性组的其他 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 代码示例，请参阅 [CREATE AVAILABILITY GROUP (Transact-SQL);](../../../t-sql/statements/create-availability-group-transact-sql.md)  
  
5.  在承载辅助副本的服务器实例上，将辅助副本联接到可用性组。  
  
     下面的代码示例将 `COMPUTER02` 上的辅助副本联接到 `MyAG` 可用性组。  
  
    ```sql 
    -- On the server instance that hosts the secondary replica,   
    -- join the secondary replica to the availability group:  
    ALTER AVAILABILITY GROUP MyAG JOIN;  
    GO  
    ```  
  
6.  在承载辅助副本的服务器实例上，创建辅助数据库。  
  
     以下代码示例通过使用 RESTORE WITH NORECOVERY 还原数据库备份来创建 *MyDb1* 和 *MyDb2* 辅助数据库。  
  
    ```sql 
    -- On the server instance that hosts the secondary replica,   
    -- Restore database backups using the WITH NORECOVERY option:  
    RESTORE DATABASE MyDb1   
        FROM DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
        WITH NORECOVERY;  
    GO  
  
    RESTORE DATABASE MyDb2   
        FROM DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
        WITH NORECOVERY;  
    GO 
    ```  
  
7.  在承载主副本的服务器实例上，备份每个主数据库上的事务日志。  
  
    > [!IMPORTANT]  
    >  在您配置真实的可用性组时，我们建议在进行此日志备份之前，挂起针对您的主数据库的日志备份任务，直到您将相应的辅助数据库联接到该可用性组。  
  
     下面的代码示例在 MyDb1 和 MyDb2 上创建一个事务日志备份。  
  
    ```sql  
    -- On the server instance that hosts the primary replica,   
    -- Backup the transaction log on each primary database:  
    BACKUP LOG MyDb1   
    TO DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
        WITH NOFORMAT;  
    GO  
  
    BACKUP LOG MyDb2   
    TO DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
        WITHNOFORMAT;  
    GO
    ```  
  
    > [!TIP]  
    >  通常，日志备份必须在每个主数据库上进行，然后在相应的辅助数据库上还原（使用 WITH NORECOVERY）。 但是，如果数据库刚刚创建而尚未进行日志备份，或者如果恢复模式刚刚从 SIMPLE 更改为 FULL，则不必进行此日志备份。  
  
8.  在承载辅助副本的服务器实例上，将日志备份应用于辅助数据库。  
  
     以下代码示例通过使用 RESTORE WITH NORECOVERY 还原数据库备份来将备份应用于 *MyDb1* 和 *MyDb2* 辅助数据库。  
  
    > [!IMPORTANT]  
    >  在您准备真实的辅助数据库时，需要应用您从其创建了辅助数据库的数据库备份之后发生的每个日志备份，从最早的开始并且始终使用 RESTORE WITH NORECOVERY。 当然，如果您还原完整和差异数据库备份，则只需应用在差异备份后发生的日志备份。  
  
    ```sql  
    -- Restore the transaction log on each secondary database,  
    -- using the WITH NORECOVERY option:  
    RESTORE LOG MyDb1   
        FROM DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
        WITH FILE=1, NORECOVERY;  
    GO  
    RESTORE LOG MyDb2   
        FROM DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
        WITH FILE=1, NORECOVERY;  
    GO  
    ```  
  
9. 在承载辅助副本的服务器实例上，将新的辅助数据库联接到可用性组。  
  
     下面的代码示例依次将 *MyDb1* 辅助数据库和 *MyDb2* 辅助数据库联接到 *MyAG* 可用性组。  
  
    ```sql  
    -- On the server instance that hosts the secondary replica,   
    -- join each secondary database to the availability group:  
    ALTER DATABASE MyDb1 SET HADR AVAILABILITY GROUP = MyAG;  
    GO  
  
    ALTER DATABASE MyDb2 SET HADR AVAILABILITY GROUP = MyAG;  
    GO  
    ```  
  
###  <a name="complete-code-example-for-sample-configuration-procedure"></a><a name="CompleteCodeExample"></a> 示例配置过程的完整代码示例  
 下面的示例合并了示例配置过程的所有步骤中的代码示例。 下表总结了此代码示例中使用的占位符值。 有关此代码示例中的步骤的详细信息，请参阅本主题中前面的 [使用示例配置过程的先决条件](#PrerequisitesForExample) 和 [示例配置过程](#SampleProcedure)。  
  
|占位符|说明|  
|-----------------|-----------------|  
|\\\\*FILESERVER*\\*SQLbackups*|虚构的备份共享。|  
|\\\\*FILESERVER*\\*SQLbackups\MyDb1.bak*|MyDb1 的备份文件。|  
|\\\\*FILESERVER*\\*SQLbackups\MyDb2.bak*|MyDb2 的备份文件。|  
|*7022*|分配给各数据库镜像端点的端口号。|  
|*COMPUTER01\AgHostInstance*|承载初始主副本的服务器实例。|  
|*COMPUTER02*|承载初始辅助副本的服务器实例。 这是 `COMPUTER02`上的默认服务器实例。|  
|*上名为*|为每个数据库镜像端点指定的名称。|  
|*MyDb1*|示例可用性组的名称。|  
|*MyDb1*|第一个示例数据库的名称。|  
|*MyDb2*|第二个示例数据库的名称。|  
|*DOMAIN1\user1*|要承载初始主副本的服务器实例的服务帐户。|  
|*DOMAIN2\user2*|要承载初始辅助副本的服务器实例的服务帐户。|  
|TCP://*COMPUTER01.Adventure-Works.com*:*7022*|COMPUTER01 上 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的 AgHostInstance 实例的端点 URL。|  
|TCP://*COMPUTER02.Adventure-Works.com*:*5022*|COMPUTER02 上 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的默认实例的端点 URL。|  
  
> [!NOTE]  
>  有关创建可用性组的其他 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 代码示例，请参阅 [CREATE AVAILABILITY GROUP (Transact-SQL);](../../../t-sql/statements/create-availability-group-transact-sql.md)  
  
```sql  
-- on the server instance that will host the primary replica,   
-- create sample databases:  
CREATE DATABASE MyDb1;  
GO  
ALTER DATABASE MyDb1 SET RECOVERY FULL;  
GO  
  
CREATE DATABASE MyDb2;  
GO  
ALTER DATABASE MyDb2 SET RECOVERY FULL;  
GO  
  
-- Backup sample databases:  
BACKUP DATABASE MyDb1   
TO DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
    WITH FORMAT;  
GO  
  
BACKUP DATABASE MyDb2   
TO DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
    WITH FORMAT;  
GO  
  
-- Create the endpoint on the server instance that will host the primary replica:  
CREATE ENDPOINT dbm_endpoint  
    STATE=STARTED   
    AS TCP (LISTENER_PORT=7022)   
    FOR DATABASE_MIRRORING (ROLE=ALL);  
GO  
  
-- Create the endpoint on the server instance that will host the secondary replica:   
CREATE ENDPOINT dbm_endpoint  
    STATE=STARTED   
    AS TCP (LISTENER_PORT=7022)   
    FOR DATABASE_MIRRORING (ROLE=ALL);  
GO  
  
-- If both service accounts run under the same domain account, skip this step. Otherwise,   
-- On the server instance that will host the primary replica,   
-- create a login for the service account   
-- of the server instance that will host the secondary replica, DOMAIN2\user2,   
-- and grant this login connect permissions on the endpoint:  
USE master;  
GO  
CREATE LOGIN [DOMAIN2\user2] FROM WINDOWS;  
GO  
GRANT CONNECT ON ENDPOINT::dbm_endpoint   
   TO [DOMAIN2\user2];  
GO  
  
-- If both service accounts run under the same domain account, skip this step. Otherwise,   
-- On the server instance that will host the secondary replica,  
-- create a login for the service account   
-- of the server instance that will host the primary replica, DOMAIN1\user1,   
-- and grant this login connect permissions on the endpoint:  
USE master;  
GO  
  
CREATE LOGIN [DOMAIN1\user1] FROM WINDOWS;  
GO  
GRANT CONNECT ON ENDPOINT::dbm_endpoint   
   TO [DOMAIN1\user1];  
GO  
  
-- On the server instance that will host the primary replica,   
-- create the availability group, MyAG:  
CREATE AVAILABILITY GROUP MyAG   
   FOR   
      DATABASE MyDB1, MyDB2   
   REPLICA ON   
      'COMPUTER01\AgHostInstance' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER01.Adventure-Works.com:7022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         ),  
      'COMPUTER02' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER02.Adventure-Works.com:7022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         );   
GO  
  
-- On the server instance that hosts the secondary replica,   
-- join the secondary replica to the availability group:  
ALTER AVAILABILITY GROUP MyAG JOIN;  
GO  
  
-- Restore database backups onto this server instance, using RESTORE WITH NORECOVERY:  
RESTORE DATABASE MyDb1   
    FROM DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
    WITH NORECOVERY;  
GO  
  
RESTORE DATABASE MyDb2   
    FROM DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
    WITH NORECOVERY;  
GO  
  
-- Back up the transaction log on each primary database:  
BACKUP LOG MyDb1   
TO DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
    WITH NOFORMAT;  
GO  
  
BACKUP LOG MyDb2   
TO DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
    WITHNOFORMAT  
GO  
  
-- Restore the transaction log on each secondary database,  
-- using the WITH NORECOVERY option:  
RESTORE LOG MyDb1   
    FROM DISK = N'\\FILESERVER\SQLbackups\MyDb1.bak'   
    WITH FILE=1, NORECOVERY;  
GO  
RESTORE LOG MyDb2   
    FROM DISK = N'\\FILESERVER\SQLbackups\MyDb2.bak'   
    WITH FILE=1, NORECOVERY;  
GO  
  
-- On the server instance that hosts the secondary replica,   
-- join each secondary database to the availability group:  
ALTER DATABASE MyDb1 SET HADR AVAILABILITY GROUP = MyAG;  
GO  
  
ALTER DATABASE MyDb2 SET HADR AVAILABILITY GROUP = MyAG;  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **配置可用性组和副本属性**  
  
-   [更改可用性副本的可用性模式 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [更改可用性副本的故障转移模式 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [配置灵活的故障转移策略以控制自动故障转移的条件（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
-   [在添加或修改可用性副本时指定终结点 URL (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [配置可用性副本备份 (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [配置对可用性副本的只读访问 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [为可用性组配置只读路由 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [更改可用性副本的会话超时期限 (SQL Server)](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **完成可用性组配置**  
  
-   [将辅助副本联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [将辅助数据库联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
 **用于创建可用性组的其他方法**  
  
-   [使用可用性组向导 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [使用“新建可用性组”对话框 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [创建可用性组 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
 **启用 AlwaysOn 可用性组**  
  
-   [启用和禁用 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
 **配置数据库镜像端点**  
  
-   [为 AlwaysOn 可用性组创建数据库镜像终结点 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [使用数据库镜像终结点证书 (Transact-SQL)](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [在添加或修改可用性副本时指定终结点 URL (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **解决 AlwaysOn 可用性组配置问题**  
  
-   [AlwaysOn 可用性组配置故障排除 (SQL Server)](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
-   [添加文件操作失败的故障排除（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   **博客：**  
  
     [Always On - HADRON 学习系列：启用了 HADRON 的数据库的工作线程池用法](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
     [SQL Server Always On 团队博客：SQL Server Always On 团队官方博客](/archive/blogs/sqlalwayson/)  
  
     [CSS SQL Server 工程师博客](/archive/blogs/psssql/)  
  
-   **视频：**  
  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 1:Introducing the Next Generation High Availability Solution](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)（Microsoft SQL Server Code-Named "Denali" Always On 系列，第 1 部分：介绍下一代高可用性解决方案）  
  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 2:Building a Mission-Critical High Availability Solution Using Always On](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)（Microsoft SQL Server Code-Named "Denali" Always On 系列，第 2 部分：使用 Always On 生成关键任务高可用性解决方案）  
  
-   **白皮书：**  
  
     [用于高可用性和灾难恢复的 Microsoft SQL Server AlwaysOn 解决方案指南](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [针对 SQL Server 2012 的 Microsoft 白皮书](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [SQL Server 客户咨询团队白皮书](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像终结点 (SQL Server)](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [可用性组侦听程序、客户端连接和应用程序故障转移 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
