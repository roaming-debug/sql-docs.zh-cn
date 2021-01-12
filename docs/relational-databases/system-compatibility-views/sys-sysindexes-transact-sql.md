---
description: sys.sysindexes (Transact-SQL)
title: Transact-sql) sys.sys索引 (|Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysindexes
- sysindexes_TSQL
- sys.sysindexes
- sys.sysindexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysindexes system table
- sys.sysindexes compatibility view
ms.assetid: f483d89c-35c4-4a08-8f8b-737fd80d13f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cf8982115b1a4399c327aefc66a5e99a1d46c575
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094187"
---
# <a name="syssysindexes-transact-sql"></a>sys.sysindexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  当前数据库中的每个索引和表各对应一行。 此视图不支持 XML 索引。 此视图中不完全支持已分区的表和索引;请改用 [sys.databases](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) 目录视图。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**id**|**int**|索引所属表的 ID。|  
|**status**|**int**|系统状态信息。<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**first**|**binary(6)**|指向第一页或根页的指针。<br /><br /> 当 **indid** = 0 时未使用。<br /><br /> NULL = 当 **indid** > 1 时，对索引进行分区。<br /><br /> NULL = 当 **indid** 为0或1时，对表进行分区。|  
|**indid**|**smallint**|索引 ID：<br /><br /> 0 = 堆<br /><br /> 1 = 聚集索引<br /><br /> >1 = 非聚集索引|  
|**root**|**binary(6)**|对于 **indid** >= 1， **root** 是指向根页的指针。<br /><br /> 当 **indid** = 0 时未使用。<br /><br /> NULL = 当 **indid** > 1 时，对索引进行分区。<br /><br /> NULL = 当 **indid** 为0或1时，对表进行分区。|  
|**minlen**|**smallint**|行的最小大小。|  
|**keycnt**|**smallint**|键数。|  
|**groupid**|**smallint**|在其上创建对象的文件组 ID。<br /><br /> NULL = 当 **indid** > 1 时，对索引进行分区。<br /><br /> NULL = 当 **indid** 为0或1时，对表进行分区。|  
|**dpages**|**int**|对于 **indid** = 0 或 **indid** = 1， **dpages** 是所使用的数据页的计数。<br /><br /> 对于 **indid** > 1， **dpages** 是使用的索引页数。<br /><br /> 0 = 当 **indid** > 1 时，对索引进行分区。<br /><br /> 0 = 当 **indid** 为0或1时，对表进行分区。<br /><br /> 如果发生行溢出，则不会得出准确的结果。|  
|**保护**|**int**|对于 **indid** = 0 或 **indid** = 1， **reserved** 是为所有索引和表数据分配的页数。<br /><br /> 对于 **indid** > 1， **reserved** 是为索引分配的页数。<br /><br /> 0 = 当 **indid** > 1 时，对索引进行分区。<br /><br /> 0 = 当 **indid** 为0或1时，对表进行分区。<br /><br /> 如果发生行溢出，则不会得出准确的结果。|  
|**广泛**|**int**|对于 **indid** = 0 或 **indid** = 1， **使用** 的是用于所有索引和表数据的总页数。<br /><br /> 对于 **indid** > 1， **使用** 的是用于索引的页数。<br /><br /> 0 = 当 **indid** > 1 时，对索引进行分区。<br /><br /> 0 = 当 **indid** 为0或1时，对表进行分区。<br /><br /> 如果发生行溢出，则不会得出准确的结果。|  
|**rowcnt**|**bigint**|基于 **indid** = 0 和 **indid** = 1 的数据级行计数。<br /><br /> 0 = 当 **indid** > 1 时，对索引进行分区。<br /><br /> 0 = 当 **indid** 为0或1时，对表进行分区。|  
|**rowmodctr**|**int**|对自上次更新表的统计信息后插入、删除或更新行的总数进行计数。<br /><br /> 0 = 当 **indid** > 1 时，对索引进行分区。<br /><br /> 0 = 当 **indid** 为0或1时，对表进行分区。<br /><br /> 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本中， **rowmodctr** 与早期版本不完全兼容。 有关详细信息，请参阅“备注”。|  
|**reserved3**|**int**|返回 0。<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**reserved4**|**int**|返回 0。<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xmaxlen**|**smallint**|行的最大大小|  
|**maxirow**|**smallint**|最大非叶索引行大小。<br /><br /> 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本中， **maxirow** 与早期版本不完全兼容。|  
|**OrigFillFactor**|**tinyint**|创建索引时使用的初始填充因子值。 不保留该值；但如果需要重新创建索引但不记得当初使用的填充因子，则该值可能很有帮助。|  
|**StatVersion**|**tinyint**|返回 0。<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**reserved2**|**int**|返回 0。<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**FirstIAM**|**binary(6)**|NULL = 索引已分区。<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**impid**|**smallint**|索引实现标志。<br /><br /> 返回 0。<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**lockflags**|**smallint**|用于约束经过考虑的索引锁粒度。 例如，对于本质上是只读的查找表，可以将其设置为仅进行表级锁定以最大限度地降低锁定成本。|  
|**pgmodctr**|**int**|返回 0。<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**键**|**varbinary(816)**|组成索引键的列 ID 列表。<br /><br /> 返回 NULL。<br /><br /> 若要显示索引键列，请使用 [sys.sysindexkeys](../../relational-databases/system-compatibility-views/sys-sysindexkeys-transact-sql.md)。|  
|name|**sysname**|索引或统计信息的名称。 当 **indid** = 0 时返回 NULL。 修改应用程序以查找 NULL 堆名。|  
|**statblob**|**图像**|统计信息二进制大型对象 (BLOB)。<br /><br /> 返回 NULL。|  
|**maxlen**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**rows**|**int**|基于 **indid** = 0 和 **indid** = 1 的数据级行计数，并为 **indid** >1 重复值。|  
  
## <a name="remarks"></a>备注  
 不得使用定义为保留的列。  
  
 如果表或索引包含 ROW_OVERFLOW 分配单元中的数据，则 **dpages**、 **reserved** 和 **used** 列将不会返回准确的结果。 此外，将单独跟踪每个索引的页计数，并且不对基表的页计数进行聚合。 若要查看页计数，请使用 [sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md) 或 [sys.databases](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md) 目录视图或 [sys.dm_db_partition_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md) 动态管理视图。  
  
 在 SQL Server 2000 和更早期版本中，[!INCLUDE[ssDE](../../includes/ssde-md.md)]维护行级修改计数器。 现在，此类计数器在列级维护。 因此， **rowmodctr** 列的计算结果与早期版本中的结果相似，但并不完全相同。  
  
 如果使用 **rowmodctr** 中的值来确定更新统计信息的时间，请考虑以下解决方案：  
  
-   不执行任何操作。 新的 **rowmodctr** 值经常有助于确定何时更新统计信息，因为该行为与早期版本的结果相当接近。  
  
-   使用 AUTO_UPDATE_STATISTICS。 有关详细信息，请参阅 [统计](../../relational-databases/statistics/statistics.md)信息。  
  
-   使用时间限制确定更新统计信息的时间。 例如，每小时、每天或每周。  
  
-   使用应用程序级信息确定更新统计信息的时间。 例如，每次 **标识** 列的最大值更改超过10000时，或每次执行大容量插入操作时。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  
