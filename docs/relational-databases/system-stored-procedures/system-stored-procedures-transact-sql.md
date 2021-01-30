---
description: 系统存储过程 (Transact-SQL)
title: 系统存储过程 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 02/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sql13.TSQLSysNoExpandPortal.f1
- sql13.TSQLSysNoExpandPortal.f1_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- stored procedures [SQL Server]
- APIs [SQL Server]
- stored procedures [SQL Server], categories
- system stored procedures [SQL Server], categories
- system stored procedures [SQL Server]
ms.assetid: a5c4d5b8-5a24-4a2d-99b4-d003b546ee3a
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 603a16d94c185783b3b90566d71b90e195949254
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203651"
---
# <a name="system-stored-procedures-transact-sql"></a>系统存储过程 (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中，可以使用系统存储过程来执行许多管理和信息活动。 系统存储过程可划分为下表所示的类别。  
  
## <a name="in-this-section"></a>本节内容  
  
|类别|说明|  
|--------------|-----------------|  
|[活动 Geo-Replication 存储过程]()|用于管理以管理 Azure SQL 数据库中的 Active Geo-Replication 配置|  
|[目录存储过程](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)|用于实现 ODBC 数据字典功能，并隔离 ODBC 应用程序以使其不受基础系统表更改的影响。|  
|[变更数据捕获存储过程](../../relational-databases/system-stored-procedures/change-data-capture-stored-procedures-transact-sql.md)|用于启用、禁用、或报告变更数据捕获对象。|  
|[游标存储过程](../../relational-databases/system-stored-procedures/cursor-stored-procedures-transact-sql.md)|用于实现游标变量功能。|  
|[数据收集器存储过程](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)|用于处理数据收集器和以下组件：收集组、收集项和收集类型。|  
|[数据库引擎存储过程](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)|用于 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的常规维护。|  
|[数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)|用于从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例内执行电子邮件操作。|  
|[数据库维护计划存储过程](../../relational-databases/system-stored-procedures/database-maintenance-plan-stored-procedures-transact-sql.md)|用于设置管理数据库性能所需的核心维护任务。|  
|[分布式查询存储过程](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)|用于实现和管理分布式查询。|  
|[Filestream 和 FileTable 存储过程 &#40;Transact-sql&#41;](./filestream-and-filetable-sp-filestream-force-garbage-collection.md)|用于配置和管理 FILESTREAM 和 FileTable 功能。|  
|[&#40;Azure SQL 数据库的防火墙规则存储过程&#41;](../../relational-databases/system-stored-procedures/firewall-rules-stored-procedures-azure-sql-database.md)|用于配置 Azure SQL 数据库防火墙。|  
|[全文搜索存储过程](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)|用于实现和查询全文索引。|  
|[常规扩展存储过程](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)|用于提供从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例到外部程序的接口，以便进行各种维护活动。|  
|[日志传送存储过程](../../relational-databases/system-stored-procedures/log-shipping-stored-procedures-transact-sql.md)|用于配置、修改和监视日志传送配置。|  
|[管理数据仓库存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/management-data-warehouse-stored-procedures-transact-sql.md)|用于配置管理数据仓库。|  
|[OLE 自动化存储过程](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)|用于使标准自动化对象能够在标准 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批次中使用。|  
|[基于策略的管理存储过程](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)|用于基于策略的管理。|  
|[PolyBase 存储过程](./polybase-stored-procedures-sp-polybase-join-group.md)|在 PolyBase 横向扩展组中添加或删除计算机。|  
|[查询存储存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)|用于优化性能。|  
|[复制存储过程](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)|用于管理复制。|  
|[安全性存储过程](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)|用于管理安全性。|  
|[快照备份存储过程](./snapshot-backup-sp-delete-backup.md)|用于删除 FILE_SNAPSHOT 备份及其所有快照或删除单个备份文件快照。|  
|[空间索引存储过程](./spatial-index-stored-procedures-arguments-and-properties.md)|用于分析和提高空间索引的索引性能。|  
|[SQL Server 代理存储过程](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)|由 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 用于监视性能和活动。|  
|[SQL Server Profiler 存储过程](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)|由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理用于管理计划的活动和事件驱动的活动。|  
|[Stretch Database 存储过程](../../relational-databases/system-stored-procedures/stretch-database-extended-stored-procedures-transact-sql.md)|用于管理 stretch database。|  
|[时态表存储过程](./spatial-index-stored-procedures-arguments-and-properties.md)|用于临时表|  
|[XML 存储过程](../../relational-databases/system-stored-procedures/xml-stored-procedures-transact-sql.md)|用于 XML 文本管理。|  
  
> [!NOTE]  
>  除非另外特别说明，否则所有的系统存储过程将返回一个 0 值以表示成功。 若要表示失败，则返回一个非零值。  
  
## <a name="api-system-stored-procedures"></a>API 系统存储过程  
 针对 ADO、OLE DB 以及 ODBC 应用程序运行 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 的用户可能会注意到这些使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 引用未涵盖的系统存储过程的应用程序。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本机客户端 OLE DB 访问接口和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] native client ODBC 驱动程序使用这些存储过程来实现数据库 API 功能。 这些存储过程只不过是访问接口或驱动程序所使用的机制，用来传达用户对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的请求。 它们只供提供程序或驱动程序内部使用。 不支持从基于的应用程序中显式调用它们 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 Sp_createorphan 和 sp_droporphans 存储过程用于 ODBC **ntext**、 **text** 和 **image** 处理。  
  
 sp_reset_connection 存储过程由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用来支持事务中的远程存储过程调用。 从连接池中重用连接时，该存储过程还将导致激发 Audit Login 和 Audit Logout 事件。  
  
 下列表中的系统存储过程只在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中使用或通过客户端 API 使用，不适于一般客户使用。 随时可能对其进行更改，不保证兼容性。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书介绍了下列存储过程：  
  
:::row:::
    :::column:::
        sp_catalogs
    :::column-end:::
    :::column:::
        sp_column_privileges
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_column_privileges_ex
    :::column-end:::
    :::column:::
        sp_columns
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_ex
    :::column-end:::
    :::column:::
        sp_databases
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursor
    :::column-end:::
    :::column:::
        sp_cursorclose
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursorexecute
    :::column-end:::
    :::column:::
        sp_cursorfetch
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursoroption
    :::column-end:::
    :::column:::
        sp_cursoropen
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursorprepare
    :::column-end:::
    :::column:::
        sp_cursorprepexec
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursorunprepare
    :::column-end:::
    :::column:::
        sp_execute
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_datatype_info
    :::column-end:::
    :::column:::
        sp_fkeys
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreignkeys
    :::column-end:::
    :::column:::
        sp_indexes
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_pkeys
    :::column-end:::
    :::column:::
        sp_primarykeys
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_prepare
    :::column-end:::
    :::column:::
        sp_prepexec
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_prepexecrpc
    :::column-end:::
    :::column:::
        sp_unprepare
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_server_info
    :::column-end:::
    :::column:::
        sp_special_columns
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_sproc_columns
    :::column-end:::
    :::column:::
        sp_statistics
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_privileges
    :::column-end:::
    :::column:::
        sp_table_privileges_ex
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables
    :::column-end:::
    :::column:::
        sp_tables_ex
    :::column-end:::
:::row-end:::

&nbsp;
  
未介绍下列存储过程：  

:::row:::
    :::column:::
        sp_assemblies_rowset
    :::column-end:::
    :::column:::
        sp_assemblies_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_assemblies_rowset2
    :::column-end:::
    :::column:::
        sp_assembly_dependencies_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_assembly_dependencies_rowset_rmt
    :::column-end:::
    :::column:::
        sp_assembly_dependencies_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_bcp_dbcmptlevel
    :::column-end:::
    :::column:::
        sp_catalogs_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_catalogs_rowset;2
    :::column-end:::
    :::column:::
        sp_catalogs_rowset;5
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_catalogs_rowset_rmt
    :::column-end:::
    :::column:::
        sp_catalogs_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_check_constbytable_rowset
    :::column-end:::
    :::column:::
        sp_check_constbytable_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_check_constbytable_rowset2
    :::column-end:::
    :::column:::
        sp_check_constraints_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_check_constraints_rowset;2
    :::column-end:::
    :::column:::
        sp_check_constraints_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_column_privileges_rowset
    :::column-end:::
    :::column:::
        sp_column_privileges_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_column_privileges_rowset;5
    :::column-end:::
    :::column:::
        sp_column_privileges_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_column_privileges_rowset2
    :::column-end:::
    :::column:::
        sp_columns_90
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_90_rowset
    :::column-end:::
    :::column:::
        sp_columns_90_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_90_rowset2
    :::column-end:::
    :::column:::
        sp_columns_ex_90
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_rowset
    :::column-end:::
    :::column:::
        sp_columns_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_rowset;5
    :::column-end:::
    :::column:::
        sp_columns_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_rowset2
    :::column-end:::
    :::column:::
        sp_constr_col_usage_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_datatype_info_90
    :::column-end:::
    :::column:::
        sp_ddopen;1
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;10
    :::column-end:::
    :::column:::
        sp_ddopen;11
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;12
    :::column-end:::
    :::column:::
        sp_ddopen;13
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;2
    :::column-end:::
    :::column:::
        sp_ddopen;3
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;4
    :::column-end:::
    :::column:::
        sp_ddopen;5
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;6
    :::column-end:::
    :::column:::
        sp_ddopen;7
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;8
    :::column-end:::
    :::column:::
        sp_ddopen;9
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreign_keys_rowset
    :::column-end:::
    :::column:::
        sp_foreign_keys_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreign_keys_rowset;3
    :::column-end:::
    :::column:::
        sp_foreign_keys_rowset;5
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreign_keys_rowset_rmt
    :::column-end:::
    :::column:::
        sp_foreign_keys_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreign_keys_rowset3
    :::column-end:::
    :::column:::
        sp_indexes_90_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_indexes_90_rowset_rmt
    :::column-end:::
    :::column:::
        sp_indexes_90_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_indexes_rowset
    :::column-end:::
    :::column:::
        sp_indexes_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_indexes_rowset;5
    :::column-end:::
    :::column:::
        sp_indexes_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_indexes_rowset2
    :::column-end:::
    :::column:::
        sp_linkedservers_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_linkedservers_rowset;2
    :::column-end:::
    :::column:::
        sp_linkedservers_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_oledb_database
    :::column-end:::
    :::column:::
        sp_oledb_defdb
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_oledb_deflang
    :::column-end:::
    :::column:::
        sp_oledb_language
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_oledb_ro_usrname
    :::column-end:::
    :::column:::
        sp_primary_keys_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_primary_keys_rowset;2
    :::column-end:::
    :::column:::
        sp_primary_keys_rowset;3
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_primary_keys_rowset;5
    :::column-end:::
    :::column:::
        sp_primary_keys_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_primary_keys_rowset2
    :::column-end:::
    :::column:::
        sp_procedure_params_90_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_procedure_params_90_rowset2
    :::column-end:::
    :::column:::
        sp_procedure_params_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_procedure_params_rowset;2
    :::column-end:::
    :::column:::
        sp_procedure_params_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_procedures_rowset
    :::column-end:::
    :::column:::
        sp_procedures_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_procedures_rowset2
    :::column-end:::
    :::column:::
        sp_provider_types_90_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_provider_types_rowset
    :::column-end:::
    :::column:::
        sp_schemata_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_schemata_rowset;3
    :::column-end:::
    :::column:::
        sp_special_columns_90
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_sproc_columns_90
    :::column-end:::
    :::column:::
        sp_statistics_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_statistics_rowset;2
    :::column-end:::
    :::column:::
        sp_statistics_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_stored_procedures
    :::column-end:::
    :::column:::
        sp_table_constraints_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_constraints_rowset;2
    :::column-end:::
    :::column:::
        sp_table_constraints_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_privileges_rowset
    :::column-end:::
    :::column:::
        sp_table_privileges_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_privileges_rowset;5
    :::column-end:::
    :::column:::
        sp_table_privileges_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_privileges_rowset2
    :::column-end:::
    :::column:::
        sp_table_statistics_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_statistics_rowset;2
    :::column-end:::
    :::column:::
        sp_table_statistics2_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tablecollations
    :::column-end:::
    :::column:::
        sp_tablecollations_90
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_90_rowset
    :::column-end:::
    :::column:::
        sp_tables_info_90_rowset_64
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_90_rowset2
    :::column-end:::
    :::column:::
        sp_tables_info_90_rowset2_64
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_rowset
    :::column-end:::
    :::column:::
        sp_tables_info_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_rowset_64
    :::column-end:::
    :::column:::
        sp_tables_info_rowset_64;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_rowset2
    :::column-end:::
    :::column:::
        sp_tables_info_rowset2_64
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_rowset;2
    :::column-end:::
    :::column:::
        sp_tables_rowset;5
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_rowset_rmt
    :::column-end:::
    :::column:::
        sp_tables_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_usertypes_rowset
    :::column-end:::
    :::column:::
        sp_usertypes_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_usertypes_rowset2
    :::column-end:::
    :::column:::
        sp_views_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_views_rowset2
    :::column-end:::
    :::column:::
        sp_xml_schema_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_xml_schema_rowset2
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  

## <a name="see-also"></a>另请参阅  
 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)   
 [存储过程（数据库引擎）](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)   
 [&#40;OLE DB 运行存储过程&#41;](../../relational-databases/native-client/ole-db/stored-procedures-running.md)   
 [运行存储过程](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)   
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [运行存储过程](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)  
  
