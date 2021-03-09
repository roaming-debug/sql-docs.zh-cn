---
description: 'sys.index_resumable_operations (Transact-sql) '
title: sys.index_resumable_operations (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.index_resumable_operations_TSQL
- sys.indexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.indexes
- sys.index_resumable_operations
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 62f3e422e65595de3b3d8a0b61c744c033ff4488
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464998"
---
# <a name="sysindex_resumable_operations-transact-sql"></a>sys.index_resumable_operations (Transact-sql) 

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]
**sys.index_resumable_operations** 是监视和检查当前执行状态以实现可恢复索引重新生成或创建的系统视图。  
**适用于**： SQL Server (2017 和更新版本的) 以及 Azure SQL 数据库
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|此索引所属的对象的 ID (不可为空) 。|  
|index_id|**int**|不能为 null) 的索引 (ID。 **index_id** 仅在对象中是唯一的。|
|name|**sysname**|索引的名称。 **名称** 仅在对象中是唯一的。|  
|**sql_text**|**nvarchar(max)**|DDL T-sql 语句文本|
|**last_max_dop**|**smallint**|上次 MAX_DOP 使用 (默认值 = 0) |
|**partition_number**|**int**|所属索引或堆中的分区号。 对于未分区的表和索引，或者如果正在重新生成所有分区，则此列的值为 NULL。|
|State |**tinyint**|可恢复索引的操作状态：<br /><br />0 = 正在运行<br /><br />1 = 暂停|
|**state_desc**|**nvarchar(60)**|可恢复索引 (运行或已暂停的操作状态的说明) |  
|**start_time**|**datetime**|索引操作开始时间 (不可为空) |
|**last_pause_time**|**datatime**| 索引操作上次暂停时间 (可以为 null) 。 如果操作正在运行且从未暂停，则为 NULL。|
|**total_execution_time**|**int**|开始时间的总执行时间（分钟） (不可为空) |
|**percent_complete**|**real**|索引操作进度完成百分比 ( 不可为 null) 。|
|**page_count**|**bigint**|索引生成操作为新的和映射索引分配的索引页总数 ( 不可为 null ) 。

## <a name="permissions"></a>权限

[!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  

## <a name="example"></a>示例：

 列出处于暂停状态的所有可恢复索引创建或重新生成操作。

```sql
SELECT * FROM  sys.index_resumable_operations WHERE STATE = 1;  
```

## <a name="see-also"></a>另请参阅

- [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md)
- [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)
- [目录视图](catalog-views-transact-sql.md)
- [对象目录视图](object-catalog-views-transact-sql.md)
- [sys.indexes](sys-xml-indexes-transact-sql.md)
- [sys.index_columns](sys-index-columns-transact-sql.md)
- [sys.xml_indexes](sys-xml-indexes-transact-sql.md)
- [sys.objects](sys-index-columns-transact-sql.md)
- [sys.key_constraints](sys-key-constraints-transact-sql.md)
- [sys.filegroups](sys-filegroups-transact-sql.md)
- [sys.partition_schemes](sys-partition-schemes-transact-sql.md)
- [查询 SQL Server 系统目录常见问题](querying-the-sql-server-system-catalog-faq.yml)
