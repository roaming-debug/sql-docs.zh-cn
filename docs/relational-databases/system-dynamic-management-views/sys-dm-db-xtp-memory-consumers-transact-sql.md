---
description: sys.dm_db_xtp_memory_consumers (Transact-SQL)
title: sys.dm_db_xtp_memory_consumers (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_xtp_memory_consumers
- dm_db_xtp_memory_consumers
- dm_db_xtp_memory_consumers_TSQL
- sys.dm_db_xtp_memory_consumers_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_memory_consumers dynamic management view
ms.assetid: f7ab2eaf-e627-464d-91fe-0e170b3f37bc
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a962925e0a359055286b6598914cd3e79cf8036c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474968"
---
# <a name="sysdm_db_xtp_memory_consumers-transact-sql"></a>sys.dm_db_xtp_memory_consumers (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  报告 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 数据库引擎中的数据库级内存消耗者。 视图为数据库引擎使用的每个内存消耗者返回一行。 使用此 DMV 查看如何在不同的内部对象之间分布内存。  
  
 有关详细信息，请参阅[内存中 OLTP&#40;内存中优化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|memory_consumer_id|**bigint**|内存消耗者的 ID（内部）。|  
|memory_consumer_type|**int**|内存消耗者的类型：<br /><br /> 0=聚合。 （聚合两个或多个消耗者的内存使用量。 不应显示。）<br /><br /> 2=VARHEAP（跟踪长度可变的堆的内存占用情况。）<br /><br /> 3=HASH（跟踪索引的内存占用情况。）<br /><br /> 5=DB 页池（跟踪用于运行时操作的数据库页池的内存占用情况。 例如，表变量和某些可序列化扫描。 每个数据库只有一个此类型的内存消耗者。）|  
|memory_consumer_type_desc|**nvarchar (64)**|内存消耗者的类型：VARHEAP、HASH 或 PGPOOL。<br /><br /> 0- (不应显示。 ) <br /><br /> 2 - VARHEAP<br /><br /> 3 - HASH<br /><br /> 5 - PGPOOL|  
|memory_consumer_desc|**nvarchar (64)**|对内存消耗者实例的说明：<br /><br /> VARHEAP <br />数据库堆。 用于为数据库分配用户数据（行）。<br />数据库系统堆。 用于分配将包含在内存转储中但不包含用户数据的数据库数据。<br />范围索引堆。 由范围索引用于分配 BW 页的专用堆。<br /><br /> HASH：无说明，因为 object_id 指示表和 index_id 哈希索引本身。<br /><br /> PGPOOL：对于数据库，只有一个页面池数据库64K 页池。|  
|object_id|**bigint**|所分配的内存所属的对象 ID。 负值表示系统对象。|  
|xtp_object_id|**bigint**|内存优化表的对象 ID。|  
|index_id|**int**|消耗者的索引 ID（如果有）。 NULL 表示基表。|  
|allocated_bytes|**bigint**|为此消耗者保留的字节数。|  
|used_bytes|**bigint**|此消耗者使用的字节数。 仅适用于 varheap。|  
|allocation_count|**int**|分配的数量。|  
|partition_count|**int**|仅限内部使用。|  
|sizeclass_count|**int**|仅限内部使用。|  
|min_sizeclass|**int**|仅限内部使用。|  
|max_sizeclass|**int**|仅限内部使用。|  
|memory_consumer_address|**varbinary**|消耗者的内部地址。 仅限内部使用。|  
|xtp_object_id|**bigint**|与内存优化表对应的内存中 OLTP 对象 ID。|  
  
## <a name="remarks"></a>备注  
 在输出中，数据库级分配器指用户表、索引和系统表。 object_id = NULL 的 VARHEAP 指分配给具有可变长度列的表的内存。  
  
## <a name="permissions"></a>权限  
 如果您对当前数据库拥有 VIEW DATABASE STATE 权限，将返回所有行。 否则，将返回一个空行集。  
  
 如果您没有 VIEW DATABASE 权限，将为表中您拥有 SELECT 权限的行返回所有列。  
  
 只会为拥有 VIEW DATABASE STATE 权限的用户返回系统表。  
  
## <a name="general-remarks"></a>一般备注  
 当内存优化表具有列存储索引时，系统将使用一些使用一些内存的内部表来跟踪列存储索引的数据。 有关这些内部表的详细信息以及显示其内存消耗情况的示例查询，请参阅 [ (transact-sql) sys.memory_optimized_tables_internal_attributes ](../../relational-databases/system-catalog-views/sys-memory-optimized-tables-internal-attributes-transact-sql.md)。
 
  
## <a name="examples"></a>示例  
  
```  
-- memory consumers (database level)  
SELECT OBJECT_NAME(object_id), *   
FROM sys.dm_db_xtp_memory_consumers;  
```  
  
## <a name="user-scenario"></a>使用方案  
  
```  
-- memory consumers (database level)  
  
select  convert(char(10), object_name(object_id)) as Name,   
convert(char(10),memory_consumer_type_desc ) as memory_consumer_type_desc, object_id,index_id, allocated_bytes,  used_bytes   
from sys.dm_db_xtp_memory_consumers  
```  
  
 下面是包含列子集的输出。 数据库级别的分配器引用用户表、索引和系统表。 object_id = NULL 的 VARHEAP（最后一行）引用分配给各表的数据行的内存（本例中为 t1）。 分配的字节数（转换为 MB）为 1340MB。  
  
```  
Name       memory_consumer_type_desc object_id   index_id    allocated_bytes      used_bytes  
---------- ------------------------- ----------- ----------- -------------------- --------------------  
t3         HASH                      629577281   2           8388608              8388608  
t2         HASH                      597577167   2           8388608              8388608  
t1         HASH                      565577053   2           1048576              1048576  
NULL       HASH                      -6          1           2048                 2048  
NULL       VARHEAP                   -6          NULL        0                    0  
NULL       HASH                      -5          3           8192                 8192  
NULL       HASH                      -5          2           8192                 8192  
NULL       HASH                      -5          1           8192                 8192  
NULL       HASH                      -4          1           2048                 2048  
NULL       VARHEAP                   -4          NULL        0                    0  
NULL       HASH                      -3          1           2048                 2048  
NULL       HASH                      -2          2           8192                 8192  
NULL       HASH                      -2          1           8192                 8192  
NULL       VARHEAP                   -2          NULL        196608               26496  
NULL       HASH                      0           1           2048                 2048  
NULL       PGPOOL                    0           NULL        0                    0  
NULL       VARHEAP                   NULL        NULL        1405943808           1231220560  
  
(17 row(s) affected)  
```  
  
 分配给此 DMV 并使用的总内存与 [sys.dm_db_xtp_table_memory_stats &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-table-memory-stats-transact-sql.md)中的对象级别相同。  
  
```  
select  sum(allocated_bytes)/(1024*1024) as total_allocated_MB,   
        sum(used_bytes)/(1024*1024) as total_used_MB  
from sys.dm_db_xtp_memory_consumers  
  
total_allocated_MB   total_used_MB  
-------------------- --------------------  
1358                 1191  
```  
  
## <a name="see-also"></a>另请参阅  
 [内存优化表动态管理视图 &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
