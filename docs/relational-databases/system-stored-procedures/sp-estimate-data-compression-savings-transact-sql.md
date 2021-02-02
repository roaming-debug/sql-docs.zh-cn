---
description: sp_estimate_data_compression_savings (Transact-SQL)
title: sp_estimate_data_compression_savings (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/26/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_estimate_data_compression_savings_TSQL
- sp_estimate_data_compression_savings
dev_langs:
- TSQL
helpviewer_keywords:
- compression [SQL Server], estimating
- sp_estimate_data_compression_savings
ms.assetid: 6f6c7150-e788-45e0-9d08-d6c2f4a33729
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 93d169d364369e87f1c363297c2a5c73c58e442b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237848"
---
# <a name="sp_estimate_data_compression_savings-transact-sql"></a>sp_estimate_data_compression_savings (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回所请求对象的当前大小并估算对象在所请求的压缩状态下的大小。 可对所有表或部分表评估压缩。 这包括堆、聚集索引、非聚集索引、列存储索引、索引视图以及表和索引分区。 可以使用 row、page、列存储或列存储存档压缩来压缩这些对象。 如果表、索引或分区已经过压缩，则可使用该过程来估计在重新压缩的情况下该表、索引或分区的大小。  
  
> [!NOTE]
> 并非在的每个版本中都提供压缩和 **sp_estimate_data_compression_savings** [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
 若要对使用请求的压缩设置的对象进行大小估算，该存储过程将对源对象进行采样并且将此数据加载到在 tempdb 中创建的等效表和索引中。 然后，将按照所请求的设置压缩在 tempdb 中创建的表和索引，并计算出估计的压缩节省量。  
  
 若要更改表、索引或分区的压缩状态，请使用 [ALTER table](../../t-sql/statements/alter-table-transact-sql.md) 或 [alter index](../../t-sql/statements/alter-index-transact-sql.md) 语句。 有关压缩的一般信息，请参阅 [数据压缩](../../relational-databases/data-compression/data-compression.md)。  
  
> [!NOTE]  
> 如果现有的数据含有碎片，则可以在不使用压缩的情况下通过重新生成索引来减小数据的大小。 对于索引，在索引重新生成的过程中将应用填充因子。 这可能会增加索引的大小。  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_estimate_data_compression_savings   
     [ @schema_name = ] 'schema_name'    
   , [ @object_name = ] 'object_name'   
   , [ @index_id = ] index_id   
   , [ @partition_number = ] partition_number   
   , [ @data_compression = ] 'data_compression'   
[;]  
```  
  
## <a name="arguments"></a>参数  
 [ @schema_name =] "*schema_name*"  
 包含表或索引视图的数据库架构的名称。 *schema_name* **sysname**。 如果 *schema_name* 为 NULL，则使用当前用户的默认架构。  
  
 [ @object_name =] "*object_name*"  
 索引所属的表或索引视图的名称。 object_name 为 sysname。  
  
 [ @index_id =] *index_id*  
 索引的 ID。 *index_id* 为 **int**，可以是下列值之一：索引的 id 号、NULL 或0（如果 *object_id* 为堆）。 若要返回基表或视图的所有索引的信息，请指定 NULL。 如果指定 NULL，则还必须为 *partition_number* 指定 null 值。  
  
 [ @partition_number =] *partition_number*  
 对象中的分区号。 *partition_number* 为 **int**，可以是下列值之一：索引或堆的分区号、NULL 或1（对于未分区的索引或堆）。  
  
 若要指定分区，还可以指定 [$partition](../../t-sql/functions/partition-transact-sql.md) 函数。 若要返回所属对象的所有分区的信息，请指定 NULL。  
  
 [ @data_compression =] "*data_compression*"  
 要评估的压缩的类型。 *data_compression* 可以是以下值之一： NONE、ROW、PAGE、列存储或 COLUMNSTORE_ARCHIVE。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 将返回以下结果集，以提供表、索引或分区的当前大小和估计大小。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_name|**sysname**|表或索引视图的名称。|  
|schema_name|**sysname**|表或索引视图的架构。|  
|index_id|**int**|索引的索引 ID：<br /><br /> 0 = 堆<br /><br /> 1 = 聚集索引<br /><br /> > 1 = 非聚集索引|  
|partition_number|**int**|分区号。 对于未分区的表或索引，返回 1。|  
|size_with_current_compression_setting (KB)|**bigint**|当前存在的所请求的表、索引或分区的大小。|  
|size_with_requested_compression_setting (KB)|**bigint**|使用请求的压缩设置及现有填充因子（如果适用）且假定不存在碎片时的表、索引或分区的估计大小。|  
|sample_size_with_current_compression_setting (KB)|**bigint**|使用当前压缩设置时的示例大小。 这包括任何碎片。|  
|sample_size_with_requested_compression_setting (KB)|**bigint**|使用请求的压缩设置及现有填充因子（如果适用）创建的且没有碎片的样本的大小。|  
  
## <a name="remarks"></a>备注  
 使用 `sp_estimate_data_compression_savings` 估算为行、页、列存储或列存储存档压缩启用表或分区时可能出现的节省量。 例如，如果行的平均大小可以减少 40%，则可能可以将对象大小减少 40%。 您可能无法节省空间，因为这取决于填充因子和行大小。 例如，如果行的长度为8000个字节，并将其大小减小40%，则在数据页上仍只能容纳一行。 因此不会节省空间。  
  
 如果运行 `sp_estimate_data_compression_savings` 的结果指示表的大小将增长，则表示表中的许多行使用的几乎是数据类型的完全精度，因而为满足压缩格式的需要而增加的少量开销大于该压缩所带来的节省量。 在这种极个别的情况下，请不要启用压缩。  
  
 如果对表启用压缩，请使用 `sp_estimate_data_compression_savings` 估算在未压缩该表的情况下该行的平均大小。  
  
 在此操作期间将获取该表的 (IS) 锁。 如果不能获取 (IS) 锁，则该过程将被阻止。 该表将在已提交读隔离级别下进行扫描。  
  
 如果请求的压缩设置与当前的压缩设置相同，则该存储过程将返回在没有数据碎片且使用现有填充因子时的估计大小。  
  
 如果索引或分区 ID 不存在，将不返回任何结果。  
  
## <a name="permissions"></a>权限  
 要求对表具有 `SELECT` 权限。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 在 SQL Server 2019 之前，此过程不适用于列存储索引，因此不接受数据压缩参数列存储和 COLUMNSTORE_ARCHIVE。  从 SQL Server 2019 开始，列存储索引可用作估算源对象，并可用作请求的压缩类型。

 > [!IMPORTANT]
 > 当在中启用了 [内存优化 TempDB 元数据](../databases/tempdb-database.md#memory-optimized-tempdb-metadata) 时 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ，不支持在临时表上创建列存储索引。 由于此限制，启用 Memory-Optimized TempDB 元数据时，列存储 COLUMNSTORE_ARCHIVE 和数据压缩参数 sp_estimate_data_compression_savings 不受支持。

## <a name="considerations-for-columnstore-indexes"></a>列存储索引的注意事项
 从开始 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ， `sp_estimate_compression_savings` 支持估计列存储和列存储存档压缩。 与页和行压缩不同，将列存储压缩应用于对象需要创建新的列存储索引。 出于此原因，使用此过程的列存储和 COLUMNSTORE_ARCHIVE 选项时，提供给过程的源对象的类型决定了用于压缩的大小估算的列存储索引的类型。 下表说明了在 @data_compression 将参数设置为列存储或 COLUMNSTORE_ARCHIVE 时，用于估计每个源对象类型的压缩节省量的引用对象。

 |源对象|Reference 对象|
 |-----------------|---------------|
 |堆|聚集列存储索引|
 |聚集索引|聚集列存储索引|
 |非聚集索引|非聚集列存储索引 (包含所提供的非聚集索引的键列和所有包含的列以及表的分区列（如果有）) |
 |非聚集列存储索引|非聚集列存储索引 (包括与所提供的非聚集列存储索引相同的列) |
 |聚集列存储索引|聚集列存储索引|

> [!NOTE]  
> 当通过行存储源对象估算列存储压缩时 (聚集索引、非聚集索引或堆) ，如果源对象中的任何列的数据类型在列存储索引中不受支持，则 sp_estimate_compression_savings 将失败并出现错误。

 同样，当 `@data_compression` 参数设置为 `NONE` 、或， `ROW` `PAGE` 并且源对象为列存储索引时，下表概述了所使用的引用对象。

 |源对象|Reference 对象|
 |-----------------|---------------|
 |聚集列存储索引|堆|
 |非聚集列存储索引|非聚集索引 (包括非聚集列存储索引中包含的列作为键列，表的分区列（如果有）作为包含列) |

> [!NOTE]  
> 在估算列存储源对象 (无、行或页) 时，请确保源索引不包含超过32列，因为这是行存储 (非聚集) 索引中支持的限制。
  
## <a name="examples"></a>示例  
 下面的示例估计 `Production.WorkOrderRouting` 表在使用 `ROW` 压缩进行压缩后的大小。  
  
```sql  
USE AdventureWorks2016;  
GO  
EXEC sp_estimate_data_compression_savings 'Production', 'WorkOrderRouting', NULL, NULL, 'ROW' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [sys.partitions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Unicode 压缩的实现](../../relational-databases/data-compression/unicode-compression-implementation.md)  
  
  
