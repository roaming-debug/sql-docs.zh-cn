---
description: 堆（没有聚集索引的表）
title: 堆（没有聚集索引的表）| Microsoft Docs
ms.custom: ''
ms.date: 11/01/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- heaps
- forward record
- forwarded record
- forwarding pointer
- RID
ms.assetid: df5c4dfb-d372-4d0f-859a-a2d2533ee0d7
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: be2a84e21cc55340d675041cdd353dab887531c2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465288"
---
# <a name="heaps-tables-without-clustered-indexes"></a>堆（没有聚集索引的表）
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  堆是不含聚集索引的表。 可在存储为堆的表上创建一个或多个非聚集索引。 数据存储于堆中并且无需指定顺序。 通常，数据最初以行插入表时的顺序存储，但 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 可能会在堆中四处移动数据，以便高效地存储行；因此，无法预测数据顺序。 若要确保从堆返回的行的顺序，您必须使用 `ORDER BY` 子句。 若要指定用于存储行的永久逻辑顺序，请对表创建聚集索引，以便表不是堆。  
  
> [!NOTE]  
> 有时候可能有必要将表保留为堆，而不是创建聚集索引，但高效率地使用堆需要较高的技能。 大多数表应该具有仔细选择的聚集索引，除非有足够的理由将表保留为堆。  
  
## <a name="when-to-use-a-heap"></a>何时使用堆  
在将某个表存储为堆时，通过引用由文件号、数据页码和页上的槽 (FileID:PageID:SlotID) 构成的 8 字节行标识符 (RID)，标识单独行。 行 ID 是一个小且高效的结构。 

可使用堆作为临时表来暂存大型无序插入操作。 由于插入数据时不应用严格的顺序，因此插入操作通常比插入到聚集索引的等效操作更快。 如果要读取堆的数据并将其处理成最终目标，创建一个包含读取查询所用搜索谓词的窄带非聚集索引可能会很有用。 

> [!NOTE]  
> 按数据页面的顺序从堆中检索数据，但这不一定是插入数据的顺序。 

始终通过非聚集索引访问数据且 RID 小于聚集索引键时，有时数据专业人员也会使用堆。 

如果某个表是堆并且不具有任何非聚集索引，则必须读取整个表（表扫描）以便找到任何行。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法直接在堆上查找 RID。 当表较小时，这可以接受。  
  
## <a name="when-not-to-use-a-heap"></a>何时不使用堆  
 在经常以排序后的顺序返回数据时，不要使用堆。 排序列上的聚集索引可以避免排序操作。  
  
 在数据经常组合在一起时，不要使用堆。 数据必须首先进行排序，然后才能分组，并且排序列上的聚集索引可以避免排序操作。  
  
 在经常从表查询数据范围时，不要使用堆。 范围列上的聚集索引将避免对整个堆进行排序。  
  
 当没有非聚集索引，并且表较大时，不要使用堆，除非你打算返回不包含任何指定顺序的整个表内容。 在某一堆中，若要找到任何行，必须读取该堆的所有行。  
 
 如果数据经常更新，请不要使用堆。 如果更新记录，并且更新在数据页中使用的空间比当前使用的空间多，则必须将记录移动到具有足够可用空间的数据页。 这会创建指向数据的新位置的前推记录，并且前推指针必须在以前保存该数据的页中写入，以指示新的物理位置。 这会在堆中引入碎片。 在扫描堆时，必须遵循这些指针，这会限制预读性能，并可能会产生额外的 I/O，从而降低扫描性能。 
  
## <a name="managing-heaps"></a>管理堆  
 若要创建堆，请创建没有聚集索引的表。 如果表已具有某一聚集索引，则删除该聚集索引以便将该表返回到某一堆。  
  
 若要删除堆，请在该堆上创建聚集索引。  
  
 重新生成堆以回收浪费的空间：
 -  在该堆上创建一个聚集索引，然后删除该聚集索引。  
 -  使用 `ALTER TABLE ... REBUILD` 命令重新生成堆。
  
> [!WARNING]  
> 创建或删除聚集索引要求重写整个表。 如果该表具有非聚集索引，则只要更改聚集索引，就必须全都重新创建所有非聚集索引。 因此，从堆更改为聚集索引结构或反之可能占用大量时间，并且要求足够的磁盘空间以便对 tempdb 中的数据重新进行排序。  

## <a name="heap-structures"></a>堆结构
堆是不含聚集索引的表。 堆的 [sys.partitions](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)中具有一行，对于堆使用的每个分区，都有 `index_id = 0` 。 默认情况下，一个堆有一个分区。 当堆有多个分区时，每个分区有一个堆结构，其中包含该特定分区的数据。 例如，如果一个堆有四个分区，则有四个堆结构；每个分区有一个堆结构。

根据堆中的数据类型，每个堆结构将有一个或多个分配单元来存储和管理特定分区的数据。 每个堆中每个分区至少有一个 `IN_ROW_DATA` 分配单元。 如果堆包含大型对象 (LOB) 列，则该堆的每个分区还将有一个 `LOB_DATA` 分配单元。 如果堆包含超过 8,060 字节的行大小限制的变量长度列，则它的每个分区中还会有一个 `ROW_OVERFLOW_DATA` 分配单元。

`first_iam_page` 系统视图中的列 `sys.system_internals_allocation_units` 指向 IAM 页链中的第一个 IAM 页，该 IAM 页链可管理分配给特定分区中的堆的空间。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 IAM 页在堆之间移动。 堆内的数据页和行没有任何特定的顺序，也不链接在一起。 数据页之间唯一的逻辑连接是记录在 IAM 页内的信息。

> [!IMPORTANT]  
> `sys.system_internals_allocation_units` 系统视图保留仅供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内部使用。 不保证以后的兼容性。
 
可以通过扫描 IAM 页对堆进行表扫描或串行读操作来找到容纳该堆的页的扩展盘区。 因为 IAM 按扩展盘区在数据文件内存在的顺序表示它们，所以这意味着串行堆扫描连续沿每个文件进行。 使用 IAM 页设置扫描顺序还意味着堆中的行一般不按照插入的顺序返回。

下图说明 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 如何使用 IAM 页检索具有单个分区的堆中的数据行。 

![iam_heap](../../relational-databases/indexes/media/iam-heap.gif)
  
## <a name="related-content"></a>相关内容  
[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)     
[DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)     
[描述的聚集索引和非聚集索引](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)     
  
  
