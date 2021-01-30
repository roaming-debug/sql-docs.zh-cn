---
description: 'sys.query_context_settings (Transact-sql) '
title: sys.query_context_settings (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/29/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- QUERY_CONTEXT_SETTINGS_TSQL
- SYS.QUERY_CONTEXT_SETTINGS_TSQL
- SYS.QUERY_CONTEXT_SETTINGS
- QUERY_CONTEXT_SETTINGS
dev_langs:
- TSQL
helpviewer_keywords:
- sys.query_context_settings catalog view
ms.assetid: 3c1887df-6bd8-491e-82fc-d25ad9589faf
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fba7c17453b0545763afb751aabd23b6c05c617c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185851"
---
# <a name="sysquery_context_settings-transact-sql"></a>sys.query_context_settings (Transact-sql) 
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  包含影响与查询关联的上下文设置的语义的相关信息。 中有许多可用的上下文设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，这些设置会影响查询语义 (定义查询) 的正确结果。 根据基础数据) ，在不同设置下编译的相同查询文本可能会产生不同的结果 (。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**context_settings_id**|**bigint**|主密钥。 此值在查询的显示计划 XML 中公开。|  
|**set_options**|**varbinary(8)**|用于反映多个 SET 选项的状态的位掩码。 有关详细信息，请参阅 [&#40;transact-sql&#41;sys.dm_exec_plan_attributes ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md)。|  
|**language_id**|**smallint**|语言的 id。 有关详细信息，请参阅 [ &#40;transact-sql&#41;sys.sys语言 ](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md)。|  
|**date_format**|**smallint**|日期格式。 有关详细信息，请参阅 [SET DATEFORMAT (Transact-SQL)](../../t-sql/statements/set-dateformat-transact-sql.md).|  
|**date_first**|**tinyint**|日期第一个值。 有关详细信息，请参阅 [SET DATEFIRST (Transact-SQL)](../../t-sql/statements/set-datefirst-transact-sql.md).|  
|**status**|**varbinary (2)**|位掩码字段，指示执行查询的查询或上下文的类型。 <br />列值可以组合多个标志 (以十六进制) 表示：<br /><br /> 0x0- (无特定标志的常规查询) <br /><br /> 0x1-通过一个游标 Api 存储过程执行的查询<br /><br /> 0x2-查询通知<br /><br /> 0x4-内部查询<br /><br /> 0x8-无通用参数化的自动参数化查询<br /><br /> 0x10-游标提取刷新查询<br /><br /> 0x20-在游标更新请求中使用的查询<br /><br /> 0x40-)  (游标自动提取时，将返回初始结果集<br /><br /> 0x80-加密查询<br /><br /> 0x100-查询行级别安全性谓词的上下文|  
|**required_cursor_options**|**int**|用户指定的游标选项，例如游标类型。|  
|**acceptable_cursor_options**|**int**|为了支持语句的执行，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以隐式转换采用的游标选项。|  
|**merge_action_type**|**smallint**|用作 **MERGE** 语句结果的触发器执行计划的类型。<br /><br /> 0指示非触发器计划、不作为 **merge** 语句的结果执行的触发器计划，或者作为仅指定了 **删除** 操作的 **MERGE** 语句结果执行的触发器计划。<br /><br /> 1表示作为 **MERGE** 语句结果运行的 **插入** 触发器计划。<br /><br /> 2表示作为 **MERGE** 语句结果运行的 **UPDATE** 触发器计划。<br /><br /> 3指示一个 **删除** 触发器计划，该计划作为包含相应 **插入** 或 **更新** 操作的 **MERGE** 语句的结果运行。<br /><br /> <br /><br /> 对于由级联操作运行的嵌套触发器，此值是导致级联的 **MERGE** 语句的操作。|  
|**default_schema_id**|**int**|默认架构的 ID，用于解析未完全限定的名称。|  
|**is_replication_specific**|**bit**|用于复制。|  
|**is_contained**|**varbinary (1)**|1指示包含的数据库。|  
  
## <a name="permissions"></a>权限  
 需要 **VIEW DATABASE STATE** 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sys.database_query_store_options &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)   
 [sys.query_store_runtime_stats_interval &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [相关视图、函数和过程](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [查询存储存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL)](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)  
  
  
