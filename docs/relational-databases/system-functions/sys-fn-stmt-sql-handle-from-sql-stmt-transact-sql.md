---
description: sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL)
title: sys.fn_stmt_sql_handle_from_sql_stmt (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 6794e073-0895-4507-aba3-c3545acc843f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fbc14748b9799b78e8e92a983f8b0f5eb409f1ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201887"
---
# <a name="sysfn_stmt_sql_handle_from_sql_stmt-transact-sql"></a>sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  获取 [!INCLUDE[tsql](../../includes/tsql-md.md)] 给定参数化类型 (简单或强制) 下的语句的 stmt_sql_handle。 这使你可以通过在知道查询存储的文本时使用其 **stmt_sql_handle** 来引用其中存储的查询。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sys.fn_stmt_sql_handle_from_sql_stmt   
(  
    'query_sql_text',   
    [ query_param_type   
) [;]  
```  
  
## <a name="arguments"></a>参数  
 *query_sql_text*  
 要作为句柄的查询在查询存储中的文本。 *query_sql_text* 为 **nvarchar (max)**，无默认值。  
  
 *query_param_type*  
 查询的参数类型。 *query_param_type* 是 **tinyint**。 可能的值有：  
  
-   NULL-默认值为0  
  
-   0 - 无  
  
-   1-用户  
  
-   2-简单  
  
-   3-强制  
  
## <a name="columns-returned"></a>返回的列  
 下表列出了 sys.fn_stmt_sql_handle_from_sql_stmt 返回的列。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|**statement_sql_handle**|**varbinary(64)**|SQL 句柄。|  
|**query_sql_text**|**nvarchar(max)**|语句的文本 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。|  
|**query_parameterization_type**|**tinyint**|查询参数化类型。|  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
  
## <a name="permissions"></a>权限  
 要求对数据库具有 **EXECUTE** 权限，并对查询存储目录视图具有 **DELETE** 权限。  
  
## <a name="examples"></a>示例  
 下面的示例执行一个语句，然后使用 `sys.fn_stmt_sql_handle_from_sql_stmt` 返回该语句的 SQL 句柄。  
  
```  
SELECT * FROM sys.databases;   
SELECT * FROM sys.fn_stmt_sql_handle_from_sql_stmt('SELECT * FROM sys.databases', NULL);  
```  
  
 使用函数可以将查询存储数据与其他动态管理视图关联。 下面的示例：  
  
```  
SELECT qt.query_text_id, q.query_id, qt.query_sql_text, qt.statement_sql_handle,  
q.context_settings_id, qs.statement_context_id   
FROM sys.query_store_query_text AS qt  
JOIN sys.query_store_query AS q   
    ON qt.query_text_id = q.query_id  
CROSS APPLY sys.fn_stmt_sql_handle_from_sql_stmt (qt.query_sql_text, null) AS fn_handle_from_stmt  
JOIN sys.dm_exec_query_stats AS qs   
    ON fn_handle_from_stmt.statement_sql_handle = qs.statement_sql_handle;  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_query_store_force_plan &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)   
 [sp_query_store_remove_plan &#40;Transct-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md)   
 [sp_query_store_unforce_plan &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)   
 [sp_query_store_reset_exec_stats &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-query-store-reset-exec-stats-transact-sql.md)   
 [sp_query_store_flush_db &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-query-store-flush-db-transact-sql.md)   
 [sp_query_store_remove_query &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md)   
 [查询存储目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)   
 [使用查询存储来监视性能](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)  
  
  
