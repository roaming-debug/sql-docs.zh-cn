---
description: sys.memory_optimized_tables_internal_attributes (Transact-SQL)
title: sys.memory_optimized_tables_internal_attributes (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.memory_optimized_tables_internal_attributes
- sys.memory_optimized_tables_internal_attributes_TSQL
- memory_optimized_tables_internal_attributes
- memory_optimized_tables_internal_attributes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.memory_optimized_tables_internal_attributes catalog view
ms.assetid: 78ef5807-0504-4de8-9a01-ede6c03c7ff1
author: kevin-farlee
ms.author: kfarlee
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a6a32b926eebf2e8a7d80823004824ec377ec199
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97404844"
---
# <a name="sysmemory_optimized_tables_internal_attributes-transact-sql"></a>sys.memory_optimized_tables_internal_attributes (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

对于用于存储用户内存优化表的每个内部内存优化表都包含一行。 每个用户表与一个或多个内部表对应。 单个表用于核心数据存储。 其他内部表用于支持功能，如用于内存优化表的临时、列存储索引和行外 (LOB) 存储。
 
| 列名称  | 数据类型  | 说明 |
| :------ |:----------| :-----|
|object_id  |**int**|       用户表的 ID。 为支持用户表（如针对 Hk/列存储组合情况的行外存储或已删除行）而存在的内部内存优化表将相同 object_id 作为其父级。 |
|xtp_object_id  |**bigint**|    与用于支持用户表的内部内存优化表对应的内存中 OLTP 对象 ID。 它在数据库中是唯一的，可以在对象的生存期内更改。 
|type|  **int** |   内部表的类型。<br/><br/> 0 => DELETED_ROWS_TABLE <br/> 1 => USER_TABLE <br/> 2 => DICTIONARIES_TABLE<br/>3 => SEGMENTS_TABLE<br/>4 => ROW_GROUPS_INFO_TABLE<br/>5 => INTERNAL OFF-ROW DATA TABLE<br/>252 => INTERNAL_TEMPORAL_HISTORY_TABLE | 
|type_desc| **nvarchar(60)**|   类型的说明<br/><br/>DELETED_ROWS_TABLE -> 跟踪列存储索引的已删除行的内部表<br/>USER_TABLE -> 包含行内用户数据的表<br/>DICTIONARIES_TABLE -> 列存储索引的字典<br/>SEGMENTS_TABLE -> 列存储索引的压缩段<br/>ROW_GROUPS_INFO_TABLE -> 有关列存储索引的压缩行组的元数据<br/>INTERNAL OFF-ROW DATA TABLE -> 用于行外列存储的内部表。 在这种情况下，minor_id 反映 column_id。<br/>INTERNAL_TEMPORAL_HISTORY_TABLE -> 基于磁盘的历史记录表的热结尾。 插入历史记录中的行会先插入此内部内存优化表中。 有一个后台任务以异步方式将行从此内部表移动到基于磁盘的历史记录表。 |
|minor_id|  **int**|    0 指示用户或内部表<br/><br/>非 0 指示行外存储的列的 ID。 在 sys.columns 中与 column_id 联接。<br/><br/>每个行外存储的列都在此系统视图中具有对应行。|

## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-all-columns-that-are-stored-off-row"></a>A. 返回行外存储的所有列

以下 T-SQL 脚本演示一个表，包含多个非 LOB 大型列和单个 LOB 列：

```Transact-SQL
CREATE TABLE dbo.LargeTableSample
(
      Id   int IDENTITY PRIMARY KEY NONCLUSTERED,
      C1   nvarchar(4000),
      C2   nvarchar(4000),
      C3   nvarchar(4000),
      C4   nvarchar(4000),
      Misc nvarchar(max)
) WITH (MEMORY_OPTIMIZED = ON);
GO
```

以下查询显示了存储在行外的所有列及其大小。 大小 -1 表示 LOB 列。 所有 LOB 列存储在行外。

```Transact-SQL
SELECT 
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table', 
  c.name AS 'column', 
  c.max_length
FROM sys.memory_optimized_tables_internal_attributes moa
     JOIN sys.columns c ON moa.object_id = c.object_id AND moa.minor_id=c.column_id
     JOIN sys.objects o on moa.object_id=o.object_id 
WHERE moa.type=5;
```

### <a name="b-returning-memory-consumption-of-all-columns-that-are-stored-off-row"></a>B. 返回行外存储的所有列的内存占用情况

要获取受管行外列的内存占用情况的详细信息，可以使用以下查询，它显示用于存储行外列的所有内部表及其索引的内存占用情况：

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  c.name AS 'column',
  c.max_length,
  mc.memory_consumer_desc,
  mc.index_id,
  mc.allocated_bytes,
  mc.used_bytes
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.columns c ON moa.object_id = c.object_id AND moa.minor_id=c.column_id
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id 
WHERE moa.type=5;
```

### <a name="c-returning-memory-consumption-of-columnstore-indexes-on-memory-optimized-tables"></a>C. 返回内存优化表上的列存储索引的内存占用情况

使用以下查询来显示内存优化表的列存储索引的内存占用情况：

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  i.name AS 'columnstore index',
  SUM(mc.allocated_bytes) / 1024 as [allocated_kb],
  SUM(mc.used_bytes) / 1024 as [used_kb]
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.indexes i ON moa.object_id = i.object_id AND i.type in (5,6)
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id
WHERE moa.type IN (0, 2, 3, 4)
GROUP BY o.schema_id, moa.object_id, i.name;
```

使用以下查询在内存优化表上分解用于列存储索引的内部结构的内存消耗：

```Transact-SQL
SELECT
  QUOTENAME(SCHEMA_NAME(o.schema_id)) + N'.' + QUOTENAME(OBJECT_NAME(moa.object_id)) AS 'table',
  i.name AS 'columnstore index',
  moa.type_desc AS 'internal table',
  mc.index_id AS 'index',
  mc.memory_consumer_desc,
  mc.allocated_bytes / 1024 as [allocated_kb],
  mc.used_bytes / 1024 as [used_kb]
FROM sys.memory_optimized_tables_internal_attributes moa
   JOIN sys.indexes i ON moa.object_id = i.object_id AND i.type in (5,6)
   JOIN sys.dm_db_xtp_memory_consumers mc ON moa.xtp_object_id=mc.xtp_object_id
   JOIN sys.objects o on moa.object_id=o.object_id
WHERE moa.type IN (0, 2, 3, 4)
```


