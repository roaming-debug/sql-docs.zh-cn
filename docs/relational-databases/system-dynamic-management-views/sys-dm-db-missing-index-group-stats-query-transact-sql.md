---
description: 'sys.dm_db_missing_index_group_stats_query (Transact-sql) '
title: 'sys.dm_db_missing_index_group_stats_query (Transact-sql) '
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_group_stats_query_TSQL
- sys.dm_db_missing_index_group_stats_query
- dm_db_missing_index_group_stats_query_TSQL
- dm_db_missing_index_group_stats_query
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_group_stats_query dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_group_stats_query dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current
ms.openlocfilehash: 01554b6abf8728ff317638f3e5e304b3823890ac
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551532"
---
# <a name="sysdm_db_missing_index_group_stats_query-transact-sql"></a>sys.dm_db_missing_index_group_stats_query (Transact-sql) 
[!INCLUDE [SQL Server 2019 SQL Database](../../includes/applies-to-version/sqlserver2019-asdb-asdbmi.md)]

  返回有关查询所需的信息，这些查询需要缺失索引组的缺失索引，不包括空间索引。 每个缺少的索引组可以返回多个查询。 缺少索引组可能有多个查询需要同一索引。
  
 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，动态管理视图不能公开将影响数据库包含的信息，也不能公开有关用户可以访问的其他数据库的信息。 为了避免公开此信息，每个包含不属于所连接的租户的数据的行都将被筛选掉。  
    
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**group_handle**|**int**|标识缺失索引组。 此标识符在服务器中是唯一的。<br /><br /> 其他列提供有关组中的索引被视为缺失的所有查询的信息。<br /><br /> 一个索引组仅包含一个索引。<BR><BR>`index_group_handle`在[sys.dm_db_missing_index_groups](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)中，可以联接到。|  
|**query_hash**|**二进制 (8)**|对查询计算的二进制哈希值，用于标识具有类似逻辑的查询。 可以使用查询哈希确定仅仅是文字值不同的查询的聚合资源使用情况。|  
|**query_plan_hash**|**二进制 (8)**|对查询执行计划计算的二进制哈希值，用于标识类似的查询执行计划。 可以使用查询计划哈希查找具有类似执行计划的查询的累积成本。<br /><br /> 当本机编译的存储过程查询内存优化的表时，此项将始终为 0x000。|  
|**last_sql_handle**|**varbinary(64)**|是唯一标识需要此索引的最后编译语句的批处理或存储过程的标记。<BR><BR>`last_sql_handle` 可以通过调用 [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) 动态管理函数，来检索查询的 SQL 文本。|
|**last_statement_start_offset**|**int**|指示行在其批处理或持久性对象的文本中所描述的查询的起始位置（以字节为单位），该语句在其 SQL 批处理中需要此索引的最后编译的语句的文本内。|
|**last_statement_end_offset**|**int**|指示行在其批处理或持久性对象的文本中所描述的查询的结束位置（以字节为单位），该语句在其 SQL 批处理中需要此索引的最后编译语句的文本。|
|**last_statement_sql_handle**|**varbinary(64)**|是唯一标识需要此索引的最后编译语句的批处理或存储过程的标记。<BR><BR>`last_statement_sql_handle`与和一起 `last_statement_start_offset` `last_statement_end_offset` 使用，可以通过调用 [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) 动态管理函数来检索查询的 SQL 文本。<BR><BR>如果编译查询时未启用查询存储，则返回0。|
|**user_seeks**|**bigint**|由可能使用了组中建议索引的用户查询所导致的查找次数。|  
|**user_scans**|**bigint**|由可能使用了组中建议索引的用户查询所导致的扫描次数。|  
|**last_user_seek**|**datetime**|由可能使用了组中建议索引的用户查询所导致的上次查找日期和时间。|  
|**last_user_scan**|**datetime**|由可能使用了组中建议索引的用户查询所导致的上次扫描日期和时间。|  
|**avg_total_user_cost**|**float**|可通过组中的索引减少的用户查询的平均成本。|  
|**avg_user_impact**|**float**|实现此缺失索引组后，用户查询可能获得的平均百分比收益。 该值表示如果实现此缺失索引组，则查询成本将按此百分比平均下降。|  
|**system_seeks**|**bigint**|由可能使用了组中建议索引的系统查询（如自动统计信息查询）所导致的查找次数。 有关详细信息，请参阅 [Auto Stats 事件类](../../relational-databases/event-classes/auto-stats-event-class.md)。|  
|**system_scans**|**bigint**|由可能使用了组中建议索引的系统查询所导致的扫描次数。|  
|**last_system_seek**|**datetime**|由可能使用了组中建议索引的系统查询所导致的上次系统查找日期和时间。|  
|**last_system_scan**|**datetime**|由可能使用了组中建议索引的系统查询所导致的上次系统扫描日期和时间。|  
|**avg_total_system_cost**|**float**|可通过组中的索引减少的系统查询的平均成本。|  
|**avg_system_impact**|**float**|实现此缺失索引组后，系统查询可能获得的平均百分比收益。 该值表示如果实现此缺失索引组，则查询成本将按此百分比平均下降。|  
  
## <a name="remarks"></a>备注  
 返回的信息 `sys.dm_db_missing_index_group_stats_query` 由每次查询执行更新，而不是每次查询编译或重新编译更新。 使用情况统计信息不会持久保存，而只会在重新启动数据库引擎之前保存。 如果数据库管理员要在服务器回收后保留使用情况统计信息，则应该定期制作缺失索引信息的备份副本。 使用 `sqlserver_start_time` [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) 中的列查找上次数据库引擎启动时间。   
 
  >[!NOTE]
  >此 DMV 的结果集限制为600行。 每一行都包含一个缺失索引。 如果缺少超过600个索引，则应该解决现有的缺失索引，以便可以查看更新的索引。

## <a name="permissions"></a>权限  
 若要查询此动态管理视图，必须授予用户 VIEW SERVER STATE 权限或隐含 VIEW SERVER STATE 权限的任何权限。  
  
## <a name="examples"></a>示例  
 下面的示例演示如何使用 `sys.dm_db_missing_index_group_stats_query` 动态管理视图。  
  
  
### <a name="a-find-the-latest-query-text-for-the-top-10-highest-anticipated-improvements-for-user-queries"></a>A. 查找用户查询的前10个最高预计改进的最新查询文本 
 下面的查询将返回10个缺失索引的最后记录的查询文本，该文本将以降序生成最高的预期累积改进。  

```sql
SELECT TOP 10 
    SUBSTRING
    (
            sql_text.text,
            misq.last_statement_start_offset / 2 + 1,
            (
            CASE misq.last_statement_Start_offset
                WHEN -1 THEN datalength(sql_text.text)
                ELSE misq.last_statement_end_offset
            END - misq.last_statement_start_offset
            ) / 2 + 1
    ),
    misq.*
FROM sys.dm_db_missing_index_group_stats_query AS misq
CROSS APPLY sys.dm_exec_sql_text(last_sql_handle) AS sql_text
ORDER BY avg_total_user_cost * avg_user_impact * (user_seeks + user_scans) DESC; 
```
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_db_missing_index_columns &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)  
 [sys.dm_os_sys_info &#40;Transact-sql&#41;](sys-dm-os-sys-info-transact-sql.md)