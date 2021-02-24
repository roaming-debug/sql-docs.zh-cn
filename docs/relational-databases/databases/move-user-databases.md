---
description: 移动用户数据库
title: 移动用户数据库 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- disaster recovery [SQL Server], moving database files
- database files [SQL Server], moving
- data files [SQL Server], moving
- editions [SQL Server], moving databases between
- moving full-text catalogs
- scheduled disk maintenace [SQL Server]
- moving databases
- full-text catalogs [SQL Server], moving
- moving database files
- moving user databases
- relocating database files
- planned database relocations [SQL Server]
- databases [SQL Server], moving
ms.assetid: ad9a4e92-13fb-457d-996a-66ffc2d55b79
author: stevestein
ms.author: sstein
ms.openlocfilehash: 64b8416375cdc61c43e8799b33891096858a79f7
ms.sourcegitcommit: 059722ff78a6061b801807416b312ae9f721ec7d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/17/2021
ms.locfileid: "100636708"
---
# <a name="move-user-databases"></a>移动用户数据库
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，通过在 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) 语句的 FILENAME 子句中指定新的文件位置，可以将用户数据库中的数据、日志和全文目录文件移动到新位置。 此方法适用于在同一 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例中移动数据库文件。 若要将数据库移动到另一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例或另一台服务器上，请使用 [备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md) 或 [分离和附加操作](../../relational-databases/databases/move-a-database-using-detach-and-attach-transact-sql.md)。  
  
## <a name="considerations"></a>注意事项  
 将数据库移动到另一个服务器实例上时，若要为用户和应用程序提供一致的体验，您可能需要为数据库重新创建部分或全部元数据。 有关详细信息，请参阅 [当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)。  
  
 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的某些功能改变了[!INCLUDE[ssDE](../../includes/ssde-md.md)]在数据库文件中存储信息的方式。 这些功能仅限于特定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]版本。 不能将包含这些功能的数据库移到不支持这些功能的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本。 使用 sys.dm_db_persisted_sku_features 动态管理视图可列出当前数据库中启用的所有特定于版本的功能。  
  
 本主题中的过程需要数据库文件的逻辑名称。 若要获取该名称，请在 [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md) 目录视图中查询名称列。  
  
 从 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]开始，全文目录已集成到数据库中，而不是存储在文件系统中。 现在移动数据库时将自动移动全文目录。  
  
## <a name="planned-relocation-procedure"></a>计划的重定位过程  
 若要将移动数据或日志文件作为计划的重定位的一部分，请执行下列步骤：  
  
1.  对于要移动的每个文件，请运行以下语句。  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name, FILENAME = 'new_path\os_file_name' );  
    ```  
  
2.  运行以下语句。  
  
    ```  
    ALTER DATABASE database_name SET OFFLINE;  
    ```  
  
3.  将文件移动到新位置。  
  
4.  运行以下语句。  
  
    ```  
    ALTER DATABASE database_name SET ONLINE;  
    ```  
  
5.  通过运行以下查询来验证文件更改。  

    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="relocation-for-scheduled-disk-maintenance"></a>计划的磁盘维护的重定位  
 若要将重定位文件作为计划的磁盘维护过程的一部分，请执行下列步骤：  
  
1.  对于要移动的每个文件，请运行以下语句。  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name , FILENAME = 'new_path\os_file_name' );  
    ```  
  
2.  停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例或关闭系统以执行维护。 有关详细信息，请参阅 [启动、停止、暂停、继续、重启 SQL Server 服务](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)。  
  
3.  将文件移动到新位置。  
  
4.  重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例或服务器。 有关详细信息，请参阅 [启动、停止、暂停、继续、重新启动数据库引擎、SQL Server 代理或 SQL Server Browser 服务](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
5.  通过运行以下查询来验证文件更改。  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="failure-recovery-procedure"></a>故障恢复过程  
 如果由于硬件故障而必须移动文件，则请执行下列步骤，将文件重新定位到一个新位置。  
  
> [!IMPORTANT]  
>  如果数据库无法启动，即处于可疑模式下或处于未恢复状态，则只有 sysadmin 固定角色的成员才可以移动该文件。  
  
1.  如果启动了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，则将其停止。  
  
2.  通过在命令提示符下输入下列命令之一，在仅 master 恢复模式下启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
    -   对于默认的 (MSSQLSERVER) 实例，请运行以下命令。  
  
        ```  
        NET START MSSQLSERVER /f /T3608  
        ```  
  
    -   对于命名实例，请运行以下命令。  
  
        ```  
        NET START MSSQL$instancename /f /T3608  
        ```  
  
     有关详细信息，请参阅 [启动、停止、暂停、继续、重启 SQL Server 服务](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)。  
  
3.  对于要移动的每个文件，请使用 **sqlcmd** 命令或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 运行以下语句。  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE( NAME = logical_name , FILENAME = 'new_path\os_file_name' );  
    ```  
  
     有关如何使用 **sqlcmd** 实用工具的详细信息，请参阅 [使用 sqlcmd 实用工具](../../ssms/scripting/sqlcmd-use-the-utility.md)。  
  
4.  退出 **sqlcmd** 实用工具或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。  
  
5.  停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。  
  
6.  将文件移动到新位置。  
  
7.  启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。 例如，运行 `NET START MSSQLSERVER`。  
  
8.  通过运行以下查询来验证文件更改。  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="examples"></a>示例  
 下面的示例将 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 日志文件移动到一个新位置，作为计划的重定位的一部分。  
  
```  
USE master;  
GO  
-- Return the logical file name.  
SELECT name, physical_name AS CurrentLocation, state_desc  
FROM sys.master_files  
WHERE database_id = DB_ID(N'AdventureWorks2012')  
    AND type_desc = N'LOG';  
GO  
ALTER DATABASE AdventureWorks2012 SET OFFLINE;  
GO  
-- Physically move the file to a new location.  
-- In the following statement, modify the path specified in FILENAME to  
-- the new location of the file on your server.  
ALTER DATABASE AdventureWorks2012   
    MODIFY FILE ( NAME = AdventureWorks2012_Log,   
                  FILENAME = 'C:\NewLoc\AdventureWorks2012_Log.ldf');  
GO  
ALTER DATABASE AdventureWorks2012 SET ONLINE;  
GO  
--Verify the new location.  
SELECT name, physical_name AS CurrentLocation, state_desc  
FROM sys.master_files  
WHERE database_id = DB_ID(N'AdventureWorks2012')  
    AND type_desc = N'LOG';  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)   
 [数据库分离和附加 (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [移动系统数据库](../../relational-databases/databases/move-system-databases.md)   
 [移动数据库文件](../../relational-databases/databases/move-database-files.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [启动、停止、暂停、继续、重新启动数据库引擎、SQL Server 代理或 SQL Server Browser 服务](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
