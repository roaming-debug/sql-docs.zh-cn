---
description: sp_spaceused (Transact-SQL)
title: sp_spaceused (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_spaceused_TSQL
- sp_spaceused
dev_langs:
- TSQL
helpviewer_keywords:
- sp_spaceused
ms.assetid: c6253b48-29f5-4371-bfcd-3ef404060621
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 480b61493dc38ea91679e590f3abe63bde3a48c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189308"
---
# <a name="sp_spaceused-transact-sql"></a>sp_spaceused (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  显示行数、保留的磁盘空间以及当前数据库中的表、索引视图或 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 队列所使用的磁盘空间，或显示由整个数据库保留和使用的磁盘空间。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
sp_spaceused [[ @objname = ] 'objname' ]   
[, [ @updateusage = ] 'updateusage' ]  
[, [ @mode = ] 'mode' ]  
[, [ @oneresultset = ] oneresultset ]  
[, [ @include_total_xtp_storage = ] include_total_xtp_storage ]
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>参数  

对于 [!INCLUDE[sssdw-md](../../includes/sssdw-md.md)] 和 [!INCLUDE[sspdw-md](../../includes/sspdw-md.md)] ， `sp_spaceused` 必须指定命名参数 (例如， `sp_spaceused (@objname= N'Table1');` 而不是依赖于参数的序号位置。 

`[ @objname = ] 'objname'`
   
 请求其空间使用信息的表、索引视图或队列的限定或非限定名称。 仅当指定限定对象名称时，才需要使用引号。 如果提供完全限定对象名称（包括数据库名称），则数据库名称必须是当前数据库的名称。  
如果未指定 *objname* ，则返回整个数据库的结果。  
*objname* 是 **nvarchar (776)**，默认值为 NULL。  
> [!NOTE]  
> [!INCLUDE[sssdw-md](../../includes/sssdw-md.md)] 和 [!INCLUDE[sspdw-md](../../includes/sspdw-md.md)] 仅支持数据库对象和表对象。
  
`[ @updateusage = ] 'updateusage'` 指示应运行 DBCC UPDATEUSAGE 以更新空间使用情况信息。 如果未指定 *objname* ，则对整个数据库运行语句;否则，语句将在 *objname* 上运行。 值可以为 **true** 或 **false**。 *updateusage* 是 **varchar (5)**，默认值为 **false**。  
  
`[ @mode = ] 'mode'` 指示结果的范围。 对于延伸的表或数据库，可以使用 *mode* 参数包括或排除对象的远程部分。 有关详细信息，请参阅 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)。  
  
 *Mode* 参数可具有以下值：  
  
|值|说明|  
|-----------|-----------------|  
|ALL|返回对象或数据库的存储统计信息，其中包括本地部分和远程部分。|  
|LOCAL_ONLY|返回仅对象或数据库的本地部分的存储统计信息。 如果对象或数据库未启用 Stretch，则返回与 = ALL 相同的统计信息 @mode 。|  
|REMOTE_ONLY|返回仅对象或数据库的远程部分的存储统计信息。 如果满足以下条件之一，则此选项将引发错误：<br /><br /> 该表未启用延伸。<br /><br /> 表已启用延伸，但你从未启用数据迁移。 在这种情况下，远程表还没有架构。<br /><br /> 用户已手动删除远程表。<br /><br /> 设置远程数据存档返回了成功状态，但实际上它失败了。|  
  
 *模式* 为 **varchar (11)**，默认值为 **N'ALL '**。  
  
`[ @oneresultset = ] oneresultset` 指示是否返回单个结果集。 *Oneresultset* 参数可具有以下值：  
  
|值|说明|  
|-----------|-----------------|  
|0|当 *\@ objname* 为 null 或未指定时，将返回两个结果集。 默认行为是两个结果集。|  
|1|如果 *\@ objname* = null 或未指定，则返回单个结果集。|  
  
 *oneresultset* 的值为 **bit**，默认值为 **0**。  

`[ @include_total_xtp_storage] 'include_total_xtp_storage'`
**适用于：** [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] 、 [!INCLUDE[sssds-md](../../includes/sssds-md.md)] 。  
  
 当 @oneresultset = 1 时，参数 @include_total_xtp_storage 确定单个结果集是否包含 MEMORY_OPTIMIZED_DATA 存储的列。 默认值为0，即默认情况下 (如果省略参数，则) XTP 列不包含在结果集中。  

## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 如果省略 *objname* 并且 *oneresultset* 的值为0，则将返回以下结果集以提供当前数据库大小信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|当前数据库的名称。|  
|**database_size**|**varchar (18)**|当前数据库的大小 (MB)。 **database_size** 包括数据文件和日志文件。|  
|**未分配空间**|**varchar (18)**|未保留供数据库对象使用的数据库空间。|  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**保护**|**varchar (18)**|由数据库中对象分配的空间总量。|  
|**data**|**varchar (18)**|数据使用的空间总量。|  
|**index_size**|**varchar (18)**|索引使用的空间总量。|  
|**用**|**varchar (18)**|为数据库中的对象保留但尚未使用的空间总量。|  
  
 如果省略 *objname* 并且 *oneresultset* 的值为1，则返回以下单个结果集以提供当前数据库大小信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|当前数据库的名称。|  
|**database_size**|**varchar (18)**|当前数据库的大小 (MB)。 **database_size** 包括数据文件和日志文件。|  
|**未分配空间**|**varchar (18)**|未保留供数据库对象使用的数据库空间。|  
|**保护**|**varchar (18)**|由数据库中对象分配的空间总量。|  
|**data**|**varchar (18)**|数据使用的空间总量。|  
|**index_size**|**varchar (18)**|索引使用的空间总量。|  
|**用**|**varchar (18)**|为数据库中的对象保留但尚未使用的空间总量。|  
  
 如果指定了 *objname* ，则将为指定的对象返回下面的结果集。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name |**nvarchar(128)**|请求其空间使用信息的对象的名称。<br /><br /> 不返回对象的架构名称。 如果架构名称是必需的，请使用 [sys.dm_db_partition_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md) 或 [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) 动态管理视图来获取等效大小的信息。|  
|**rows**|**char (20)**|表中现有的行数。 如果指定的对象是 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 队列，该列将指示队列中的消息数。|  
|**保护**|**varchar (18)**|*Objname* 的保留空间总量。|  
|**data**|**varchar (18)**|*Objname* 中的数据所用的空间总量。|  
|**index_size**|**varchar (18)**|*Objname* 中的索引使用的空间总量。|  
|**用**|**varchar (18)**|为 *objname* 保留但尚未使用的空间总量。|  
 
如果未指定任何参数，则这是默认模式。 将返回以下结果集，其中详细说明了磁盘上的数据库大小信息。 

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|当前数据库的名称。|  
|**database_size**|**varchar (18)**|当前数据库的大小 (MB)。 **database_size** 包括数据文件和日志文件。 如果数据库具有 MEMORY_OPTIMIZED_DATA 文件组，则这包括文件组中所有检查点文件的磁盘总大小。|  
|**未分配空间**|**varchar (18)**|未保留供数据库对象使用的数据库空间。 如果数据库具有 MEMORY_OPTIMIZED_DATA 文件组，则这包括文件组中状态为预创建的检查点文件的磁盘总大小。|  

数据库中的表使用的空间： (此结果集不会反映内存优化表，因为没有每表的磁盘使用情况统计信息)  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**保护**|**varchar (18)**|由数据库中对象分配的空间总量。|  
|**data**|**varchar (18)**|数据使用的空间总量。|  
|**index_size**|**varchar (18)**|索引使用的空间总量。|  
|**用**|**varchar (18)**|为数据库中的对象保留但尚未使用的空间总量。|

**仅当** 数据库包含至少具有一个容器的 MEMORY_OPTIMIZED_DATA 文件组时，才返回下面的结果集： 

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**xtp_precreated**|**varchar (18)**|状态为预创建的检查点文件的总大小（KB）。 计算整个数据库中的未分配空间。 [例如，如果存在 600000 KB 的预创建检查点文件，则此列包含 "600000 KB"]|  
|**xtp_used**|**varchar (18)**|状态为 "构造"、"活动" 和 "合并目标" 的检查点文件的总大小（KB）。 这是内存优化表中的数据所用的磁盘空间。|  
|**xtp_pending_truncation**|**varchar (18)**|状态 WAITING_FOR_LOG_TRUNCATION 的检查点文件的总大小（KB）。 这是在发生日志截断后用于等待清理的检查点文件的磁盘空间。|

如果省略 *objname* ，则 oneresultset 的值为1， *include_total_xtp_storage* 为1，则返回以下单个结果集以提供当前数据库大小信息。 如果 `include_total_xtp_storage` 为 0 (默认) ，则省略最后三列。 

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|当前数据库的名称。|  
|**database_size**|**varchar (18)**|当前数据库的大小 (MB)。 **database_size** 包括数据文件和日志文件。 如果数据库具有 MEMORY_OPTIMIZED_DATA 文件组，则这包括文件组中所有检查点文件的磁盘总大小。|
|**未分配空间**|**varchar (18)**|未保留供数据库对象使用的数据库空间。 如果数据库具有 MEMORY_OPTIMIZED_DATA 文件组，则这包括文件组中状态为预创建的检查点文件的磁盘总大小。|  
|**保护**|**varchar (18)**|由数据库中对象分配的空间总量。|  
|**data**|**varchar (18)**|数据使用的空间总量。|  
|**index_size**|**varchar (18)**|索引使用的空间总量。|  
|**用**|**varchar (18)**|为数据库中的对象保留但尚未使用的空间总量。|
|**xtp_precreated**|**varchar (18)**|状态为预创建的检查点文件的总大小（KB）。 这将计入整个数据库中的未分配空间。 如果数据库没有至少具有一个容器的 memory_optimized_data 文件组，则返回 NULL。 *只有在 @include_total_xtp_storage = 1 时才包括此列*。| 
|**xtp_used**|**varchar (18)**|状态为 "构造"、"活动" 和 "合并目标" 的检查点文件的总大小（KB）。 这是内存优化表中的数据所用的磁盘空间。 如果数据库没有至少具有一个容器的 memory_optimized_data 文件组，则返回 NULL。 *只有在 @include_total_xtp_storage = 1 时才包括此列*。| 
|**xtp_pending_truncation**|**varchar (18)**|状态 WAITING_FOR_LOG_TRUNCATION 的检查点文件的总大小（KB）。 这是在发生日志截断后用于等待清理的检查点文件的磁盘空间。 如果数据库没有至少具有一个容器的 memory_optimized_data 文件组，则返回 NULL。 只有此列包含在内 `@include_total_xtp_storage=1` 。|

## <a name="remarks"></a>备注  
 **database_size** 通常大于 **保留**  +  **未分配空间** 的总和，因为它包括日志文件的大小，但 **保留** 和 **unallocated_space** 只考虑数据页。 在某些情况下，Azure Synapse Analytics 不能为 true。 
  
 XML 索引和全文索引使用的页包括在两个结果集的 **index_size** 中。 当指定 *objname* 时，对象的 XML 索引和全文索引的页也将计入总 **保留** 和 **index_size** 结果中。  
  
 如果为数据库或具有空间索引的对象计算空间使用量，则空间大小列（如 **database_size**、 **保留** 和 **index_size**）包括空间索引的大小。  
  
 指定 *updateusage* 时，会 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 扫描数据库中的数据页，并对每个表使用的存储空间的 **sys.allocation_units** 和 **sys.databases** 目录视图进行任何所需的更正。 在某些情况下（例如删除索引后、表的空间信息不是当前信息时），需要执行该操作。 *updateusage* 可能需要一些时间才能在大型表或数据库上运行。 仅当您怀疑返回了不正确的值，并且该进程不会对数据库中的其他用户或进程产生不利影响时，才使用 *updateusage* 。 如果首选该进程，则可以单独运行 DBCC UPDATEUSAGE。  
  
> [!NOTE]  
>  在删除或重新生成大型索引时，或者在删除或截断大型表时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将延迟实际页释放及其关联锁，直至事务提交完毕为止。 延迟的删除操作不会立即释放已分配的空间。 因此，在删除或截断大型对象之后 **sp_spaceused** 立即返回的值可能不会反映可用的实际磁盘空间。  
  
## <a name="permissions"></a>权限  
 执行 **sp_spaceused** 的权限授予 **public** 角色。 只有 db_owner 固定数据库角色的成员可以指定 **updateusage 参数\@** 。  
  
## <a name="examples"></a>示例  
  
### <a name="a-displaying-disk-space-information-about-a-table"></a>A. 显示表的磁盘空间信息  
 以下示例报告 `Vendor` 表及其索引的磁盘空间信息。  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_spaceused N'Purchasing.Vendor';  
GO  
```  
  
### <a name="b-displaying-updated-space-information-about-a-database"></a>B. 显示数据库的已更新空间信息  
 下例对当前数据库中使用的空间进行了汇总，并使用可选参数 `@updateusage` 确保返回当前值。  
  
```sql  
USE AdventureWorks008R2;  
GO  
EXEC sp_spaceused @updateusage = N'TRUE';  
GO  
```  
  
### <a name="c-displaying-space-usage-information-about-the-remote-table-associated-with-a-stretch-enabled-table"></a>C. 显示与已启用延伸的表相关联的远程表的空间使用情况信息  
 下面的示例通过使用 **\@ mode** 参数指定远程目标来汇总与启用 Stretch 的表关联的远程表所使用的空间。 有关详细信息，请参阅 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)。  
  
```sql  
USE StretchedAdventureWorks2016  
GO  
EXEC sp_spaceused N'Purchasing.Vendor', @mode = 'REMOTE_ONLY'  
```  
  
### <a name="d-displaying-space-usage-information-for-a-database-in-a-single-result-set"></a>D. 显示单个结果集中数据库的空间使用情况信息  
 下面的示例在一个结果集中汇总了当前数据库的空间使用率。  
  
```sql  
USE AdventureWorks2016  
GO  
EXEC sp_spaceused @oneresultset = 1  
```  

### <a name="e-displaying-space-usage-information-for-a-database-with-at-least-one-memory_optimized-file-group-in-a-single-result-set"></a>E. 在单个结果集中显示至少具有一个 MEMORY_OPTIMIZED 文件组的数据库的空间使用信息 
 下面的示例使用一个结果集中至少一个 MEMORY_OPTIMIZED 文件组来汇总当前数据库的空间使用率。
 
```sql
USE WideWorldImporters
GO
EXEC sp_spaceused @updateusage = 'FALSE', @mode = 'ALL', @oneresultset = '1', @include_total_xtp_storage = '1';
GO
``` 

### <a name="f-displaying-space-usage-information-for-a-memory_optimized-table-object-in-a-database"></a>F. 显示数据库中 MEMORY_OPTIMIZED 表对象的空间使用情况信息。
 下面的示例汇总了当前数据库中包含至少一个 MEMORY_OPTIMIZED 文件组的 MEMORY_OPTIMIZED 表对象的空间使用情况。
 
```sql
USE WideWorldImporters
GO
EXEC sp_spaceused
@objname = N'VehicleTemparatures',
@updateusage = 'FALSE',
@mode = 'ALL',
@oneresultset = '0',
@include_total_xtp_storage = '1';
GO
```  

## <a name="see-also"></a>另请参阅  
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [DBCC UPDATEUSAGE &#40;Transact-sql&#41;](../../t-sql/database-console-commands/dbcc-updateusage-transact-sql.md)   
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.partitions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
