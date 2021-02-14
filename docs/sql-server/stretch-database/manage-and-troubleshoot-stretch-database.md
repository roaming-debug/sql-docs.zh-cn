---
description: Stretch Database 的管理和故障排除
title: 管理和故障排除
ms.date: 06/27/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, managing
- Stretch Database, troubleshooting
- managing Stretch Database
- troubleshooting Stretch Database
ms.assetid: 6334db3e-9297-44df-8d53-211187a95520
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 346bc660bc1fa1fd9a6e75a91caa182ff25e6c53
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081019"
---
# <a name="manage-and-troubleshoot-stretch-database"></a>Stretch Database 的管理和故障排除
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  要对 Stretch Database 进行管理和故障排除，请使用本文中所述的工具和方法。  
## <a name="manage-local-data"></a>管理本地数据  
  
###  <a name="get-info-about-local-databases-and-tables-enabled-for-stretch-database"></a><a name="LocalInfo"></a> 获取对 Stretch Database 启用的本地数据库和表的相关信息。  
 打开目录视图 **sys.databases** 和 **sys.tables** ，查看有关已启用延伸的 SQL Server 数据库和表的信息。 有关详细信息，请参阅 [sys.databases (Transact SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 和 [sys.tables (Transact SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)。  
 
 若要查看 SQL Server 中已启用延伸的表使用的空间量，请运行以下语句。
 
 ```sql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
   
## <a name="manage-data-migration"></a>管理数据迁移  
  
### <a name="check-the-filter-function-applied-to-a-table"></a>检查应用到表的筛选器函数  
 打开目录视图 **sys.remote_data_archive_tables** ，并检查 **filter_predicate** 列的值，以标识 Stretch Database 用于选择要迁移的行的函数。 如果值为 null，则整个表符合迁移条件。 有关详细信息，请参阅 [sys.remote_data_archive_tables (Transact SQL)](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md) 和 [通过使用筛选器函数选择要迁移的行](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md)。  
  
###  <a name="check-the-status-of-data-migration"></a><a name="Migration"></a> 检查数据迁移的状态  
 在 SQL Server Management Studio 中选择数据库的“任务 | 延伸 | 监视”以便在 Stretch Database 监视器中监视数据迁移。 有关详细信息，请参阅 [数据迁移的监视与故障排除 (Stretch Database)](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md)。  
  
 或者，打开动态管理视图 **sys.dm_db_rda_migration_status** 以查看有多少批数据和数据行已迁移。  
  
###  <a name="troubleshoot-data-migration"></a><a name="Firewall"></a> 数据迁移故障排除  
 有关故障排除建议，请参阅 [数据迁移的监视与故障排除 (Stretch Database)](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md)。  
  
## <a name="manage-remote-data"></a>管理远程数据  
  
###  <a name="get-info-about-remote-databases-and-tables-used-by-stretch-database"></a><a name="RemoteInfo"></a> 获取 Stretch Database 使用的远程数据库和表的相关信息。  
 打开目录视图 **sys.remote_data_archive_databases** 和 **sys.remote_data_archive_tables** ，以了解有关存储已迁移数据的远程数据库和表的信息。 有关详细信息，请参阅 [sys.remote_data_archive_databases (Transact SQL)](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-databases.md) 和 [sys.remote_data_archive_tables (Transact SQL)](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md)。  
 
若要查看 Azure 中已启用延伸的表使用的空间量，请运行以下语句。
 
 ```sql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>删除已迁移数据  
若要删除已迁移到 Azure 的数据，请按照 [sys.sp_rda_reconcile_batch](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-batch-transact-sql.md)中所述的步骤进行操作。  
  
## <a name="manage-table-schema"></a>管理表架构

### <a name="dont-change-the-schema-of-the-remote-table"></a>不要更改远程表的架构  
 不要更改为 Stretch Database 配置的 SQL Server 表相关联的远程 Azure 表的架构。 特别是，不要修改列的名称或数据类型。 Stretch Database 功能对与 SQL Server 表的架构相关的远程表的架构作出各种假设。 如果更改了远程架构，所更改表的 Stretch Database 将停止工作。  

### <a name="reconcile-table-columns"></a>协调时间表列  
如果意外删除了远程表中的列，运行 **sp_rda_reconcile_columns** 可将存在于已启用延伸的 SQL Server 表但不存在于远程表中的列添加到远程表。 有关详细信息，请参阅 [sys.sp_rda_reconcile_columns](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-columns-transact-sql.md)。  
  
  > [!IMPORTANT]
  > 当 **sp_rda_reconcile_columns** 重新创建你从远程表中意外删除的列时，不会还原之前位于已删除列中的数据。
  
**sp_rda_reconcile_columns** 不会删除存在于远程表中而不存在于已启用延伸的 SQL Server 表中的列。 如果远程 Azure 表中存在已启用延伸的 SQL Server 表中不复存在的列，这些额外的列不会阻止 Stretch Database 正常运行。 你可以选择手动删除额外列。  
 
## <a name="manage-performance-and-costs"></a>管理性能和成本  
  
### <a name="troubleshoot-query-performance"></a>查询性能故障排除  
  包括 Stretch 启用的表的查询应该比在为 Stretch 启用表之前执行得更慢。 如果查询性能显著下降，请查看以下可能的问题。  
  
-   你的 Azure 服务器是否处在与你的 SQL Server 不同的地理区域？ 请将你的 Azure 服务器配置为处于与你的 SQL Server 相同的地理区域以减少网络延迟。  
  
-   你的网络条件可能已退化。 有关最新问题或中断的信息，请与你的网络管理员联系。  
  
### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>提高资源密集型操作（如索引）的 Azure 性能级别  
 在生成、重新生成或重新组织为 Stretch Database 配置的大型表上的索引，并预计在此期间大量查询 Azure 中的已迁移数据时，请考虑提高响应远程 Azure 数据库的性能级别以保证操作的持续时间。 有关性能级别和定价的详细信息，请参阅 [SQL Server Stretch Database 定价](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)。  
  
### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>不能暂停 Azure 上的 SQL Server Stretch Database 服务  
 请确保选择适当的性能和定价级别。 如果为需占用大量资源的操作而临时增加性能级别，请在操作完成后将其还原到以前的级别。 有关性能级别和定价的详细信息，请参阅 [SQL Server Stretch Database 定价](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/)。  
   
 ## <a name="change-the-scope-of-queries"></a>更改查询作用域  
 针对已启用延伸的表的查询默认返回本地和远程数据。 可以更改所有用户所有查询的作用域，或只更改管理员单个查询的作用域。  
   
 ### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>更改所有用户所有查询的查询作用域  
 若要更改所有用户所有查询的作用域，请运行存储的过程 **sys.sp_rda_set_query_mode**。 可缩小作用域以便仅查询本地数据、禁用所有查询或还原默认设置。 有关详细信息，请参阅 [sys.sp_rda_set_query_mode](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md)。  
   
 ### <a name="change-the-scope-of-queries-for-a-single-query-by-an-administrator"></a><a name="queryHints"></a>更改管理员单个查询的查询作用域  
 若要更改 db_owner 角色成员单个查询的作用域，请将 **WITH ( REMOTE_DATA_ARCHIVE_OVERRIDE = *value* )** 查询提示添加到 SELECT 语句。 REMOTE_DATA_ARCHIVE_OVERRIDE 查询提示可以具有下列值。  
 -   **LOCAL_ONLY**。 仅查询本地数据。  
   
 -   **REMOTE_ONLY**。 仅查询远程数据。  
   
 -   **STAGE_ONLY**。 仅查询这样的表中的数据，Stretch Database 在其中暂存符合迁移条件的行并在迁移后指定时段内保留已迁移的行。 此查询提示是查询临时表的唯一方法。  
  
例如，下面的查询将仅返回本地结果。  
  
 ```sql  
USE <Stretch-enabled database name>;
GO
SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
GO
```  
   
 ## <a name="make-administrative-updates-and-deletes"></a><a name="adminHints"></a>进行管理更新和删除  
 默认情况下，无法更新或删除已启用延伸的表中符合迁移条件的行或已迁移的行。 如果必须解决此问题，db_owner 角色的成员可在语句中添加 **WITH ( REMOTE_DATA_ARCHIVE_OVERRIDE = *value* )** 查询提示以运行 UPDATE 或 DELETE 操作。 REMOTE_DATA_ARCHIVE_OVERRIDE 查询提示可以具有下列值。  
 -   **LOCAL_ONLY**。 仅更新或删除本地数据。  
   
 -   **REMOTE_ONLY**。 仅更新或删除远程数据。  
   
 -   **STAGE_ONLY**。 仅更新或删除这样的表中的数据，Stretch Database 在其中暂存符合迁移条件的行并在迁移后指定时段内保留已迁移的行。  
  
## <a name="see-also"></a>另请参阅  
 [数据迁移的监视与故障排除 (Stretch Database)](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md)   
[备份已启用延伸的数据库 (Stretch Database)](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
[还原已启用延伸的数据库 (Stretch Database)](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
  
  
