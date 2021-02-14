---
description: 'sys.internal_partitions (Transact-sql) '
title: sys.internal_partitions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0262df2b-5ba7-4715-b17b-3d9ce470a38e
author: ronortloff
ms.author: rortloff
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 46eb4bf2855cf6f5f8121d1480776c83703a5fcc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341355"
---
# <a name="sysinternal_partitions-transact-sql"></a>sys.internal_partitions (Transact-sql) 

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  为在基于磁盘的表上跟踪列存储索引的内部数据的每个行集返回一行。 这些行集是列存储索引的内部，跟踪已删除的行、行组映射和增量存储行组。 它们跟踪每个表分区的数据;每个表至少具有一个分区。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在每次重新生成列存储索引时重新创建行集。   
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|partition_id|**bigint**|此分区的分区 ID。 在数据库中是唯一的。|  
|object_id|**int**|包含分区的表的对象 ID。|  
|index_id|**int**|表中定义的列存储索引的索引 ID。<br /><br /> 1 = 聚集列存储索引<br /><br /> 2 = 非聚集列存储索引|  
|partition_number|**int**|分区号。<br /><br /> 1 = 已分区表的第一个分区，或未分区表的单个分区。<br /><br /> 2 = 第二个分区，依此类推。|  
|internal_object_type|**tinyint**|行集对象跟踪列存储索引的内部数据。<br /><br /> 2 = COLUMN_STORE_DELETE_BITMAP<br /><br /> 3 = COLUMN_STORE_DELTA_STORE<br /><br /> 4 = COLUMN_STORE_DELETE_BUFFER<br /><br /> 5 = COLUMN_STORE_MAPPING_INDEX|  
|internal_object_type_desc|**nvarchar(60)**|COLUMN_STORE_DELETE_BITMAP-此位图索引跟踪列存储中标记为已删除的行。 由于分区可以包含多个行组中的行，因此每个行组都使用位图。 行是仍在物理上存在并占用列存储中的空间的行。<br /><br /> COLUMN_STORE_DELTA_STORE 用于存储未压缩到列存储中的行组（称为行组）。 每个表分区可以有零个或多个增量存储行组。<br /><br /> COLUMN_STORE_DELETE_BUFFER-用于维护对可更新的非聚集列存储索引的删除。 当查询从基础行存储表中删除行时，删除缓冲区将跟踪列存储中的删除操作。 如果删除的行数超过1048576，则会将其合并回按后台元组移动器线程或显式重新组织命令的 delete 位图。  在任何给定的时间点，删除位图和 delete 缓冲区的并集表示所有删除的行。<br /><br /> COLUMN_STORE_MAPPING_INDEX-仅在聚集列存储索引具有辅助非聚集索引时使用。 这会将非聚集索引键映射到列存储中的正确行组和行 ID。 它只存储移动到不同行组的行的键;当增量行组压缩到列存储中，并且合并操作合并两个不同行组的行时，会发生这种情况。|  
|Row_group_id|**int**|增量存储行组的 ID。 每个表分区可以有零个或多个增量存储行组。|  
|hobt_id|**bigint**|内部行集对象的 ID (HoBT) 。 这是一个用于与其他 Dmv 进行联接的好密钥，用于获取有关内部行集的物理特性的详细信息。|  
|行|**bigint**|此分区中的大约行数。|  
|data_compression|**tinyint**|行集的压缩状态：<br /><br /> 0 = NONE<br /><br /> 1 = ROW<br /><br /> 2 = PAGE|  
|data_compression_desc|**nvarchar(60)**|每个分区的压缩状态。 行存储表的可能值为 NONE、ROW 和 PAGE。 列存储表的可能值为 COLUMNSTORE 和 COLUMNSTORE_ARCHIVE。|  
|optimize_for_sequential_key|**bit**|1 = 分区启用了上一页插入优化。<br><br>0 = 默认值。 分区已禁用上一页插入优化。|
  
## <a name="permissions"></a>权限  
 要求具有 `public` 角色的成员身份。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="general-remarks"></a>一般备注  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 每次创建或重新生成列存储索引时，都将重新创建新的列存储内部索引。  
  
## <a name="examples"></a>示例  
  
### <a name="a-view-all-of-the-internal-rowsets-for-a-table"></a>A. 查看表的所有内部行集  
 此示例返回表的所有内部列存储行集。 你还可以使用 hobt_id 来查找有关特定行集的详细信息。  
  
```sql  
SELECT i.object_id, i.index_id, i.name, p.hobt_id, p.internal_object_type_id, p.internal_object_type_desc  
FROM sys.internal_partitions AS p  
JOIN sys.indexes AS i  
on i.object_id = p.object_id  
WHERE p.object_id = OBJECT_ID ( '<table name' ) ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
