---
description: 索引
title: 索引 | Microsoft Docs
ms.custom: ''
ms.date: 12/21/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- index types [SQL Server]
ms.assetid: 00863b10-e77c-44c5-8ac2-bb4ac454eec6
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 082670e6e7c728bc9d91b538760676680d8fa354
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481158"
---
# <a name="indexes"></a>索引
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

## <a name="available-index-types"></a>可用的索引类型
下表列出了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中可用的索引类型，并提供了指向其他信息的链接。  
  
|索引类型|描述|其他信息|  
|----------------|-----------------|----------------------------|  
|哈希|借助于哈希索引，可通过内存中的哈希表来访问数据。 哈希索引的内存用量固定不变，是存储桶数量的函数。|[在内存优化表上使用索引的准则](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)<br /><br /> [哈希索引设计指南](../../relational-databases/sql-server-index-design-guide.md#hash_index)|  
|内存优化非聚集索引|对于内存优化的非聚集索引，内存使用量依赖于行计数以及索引键列的大小|[在内存优化表上使用索引的准则](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)<br /><br /> [内存优化非聚集索引设计指南](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index)|  
|聚集|聚集索引基于聚集索引键按顺序排序和存储表或视图中的数据行。 聚集索引按 B 树索引结构实现，B 树索引结构支持基于聚集索引键值对行进行快速检索。|[描述的聚集索引和非聚集索引](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)<br /><br /> [创建聚集索引](../../relational-databases/indexes/create-clustered-indexes.md)<br /><br /> [聚集索引设计指南](../../relational-databases/sql-server-index-design-guide.md#Clustered)|  
|非聚集|既可以使用聚集索引来为表或视图定义非聚集索引，也可以根据堆来定义非聚集索引。 非聚集索引中的每个索引行都包含非聚集键值和行定位符。 此定位符指向聚集索引或堆中包含该键值的数据行。 索引中的行按索引键值的顺序存储，但是不保证数据行按任何特定顺序存储，除非对表创建聚集索引。|[描述的聚集索引和非聚集索引](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)<br /><br /> [创建非聚集索引](../../relational-databases/indexes/create-nonclustered-indexes.md)<br /><br /> [非聚集索引设计指南](../../relational-databases/sql-server-index-design-guide.md#Nonclustered)|  
|唯一|唯一索引确保索引键不包含重复的值，因此，表或视图中的每一行在某种程度上是唯一的。<br /><br /> 唯一性可以是聚集索引和非聚集索引的属性。|[创建唯一索引](../../relational-databases/indexes/create-unique-indexes.md)<br /><br /> [唯一索引设计指南](../../relational-databases/sql-server-index-design-guide.md#Unique)|  
|列存储|内存中列存储索引通过使用基于列的数据存储和基于列的查询处理来存储和管理数据。<br /><br /> 列存储索引适合于主要执行大容量加载和只读查询的数据仓库工作负荷。 与传统面向行的存储方式相比，使用列存储索引存档可最多提高 **10 倍查询性能** ，与使用非压缩数据大小相比，可提供多达 **7 倍数据压缩率** 。|[列存储索引指南](../../relational-databases/indexes/columnstore-indexes-overview.md)<br /><br /> [列存储索引设计指南](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)|  
|带有包含列的索引|一种非聚集索引，它扩展后不仅包含键列，还包含非键列。|[创建带有包含列的索引](../../relational-databases/indexes/create-indexes-with-included-columns.md)|  
|计算列上的索引|从一个或多个其他列的值或某些确定的输入值派生的列上的索引。|[计算列上的索引](../../relational-databases/indexes/indexes-on-computed-columns.md)|  
|Filtered|一种经过优化的非聚集索引，尤其适用于涵盖从定义完善的数据子集中选择数据的查询。 筛选索引使用筛选谓词对表中的部分行进行索引。 与全表索引相比，设计良好的筛选索引可以提高查询性能、减少索引维护开销并可降低索引存储开销。|[创建筛选索引](../../relational-databases/indexes/create-filtered-indexes.md)<br /><br /> [筛选索引设计指南](../../relational-databases/sql-server-index-design-guide.md#Filtered)|  
|空间|利用空间索引，可以更高效地对 *几何* 数据类型的列中的空间对象（ **空间数据** ）执行某些操作。 空间索引可减少需要应用开销相对较大的空间操作的对象数。|[空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)|  
|XML|**xml** 数据类型列中 XML 二进制大型对象 (BLOB) 的已拆分持久表示形式。|[XML 索引 (SQL Server)](../../relational-databases/xml/xml-indexes-sql-server.md)|  
|全文|一种特殊类型的基于标记的功能性索引，由 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]全文引擎生成和维护。 用于帮助在字符串数据中搜索复杂的词。|[填充全文索引](../../relational-databases/search/populate-full-text-indexes.md)|  
  
## <a name="related-content"></a>相关内容  
 [SQL Server 索引设计指南](../../relational-databases/sql-server-index-design-guide.md)      
 [用于索引的 SORT_IN_TEMPDB 选项](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md)     
 [禁用索引和约束](../../relational-databases/indexes/disable-indexes-and-constraints.md)     
 [启用索引和约束](../../relational-databases/indexes/enable-indexes-and-constraints.md)    
 [重命名索引](../../relational-databases/indexes/rename-indexes.md)     
 [设置索引选项](../../relational-databases/indexes/set-index-options.md)     
 [索引 DDL 操作的磁盘空间要求](../../relational-databases/indexes/disk-space-requirements-for-index-ddl-operations.md)     
 [重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)     
 [为索引指定填充因子](../../relational-databases/indexes/specify-fill-factor-for-an-index.md)     
 [页和区体系结构指南](../../relational-databases/pages-and-extents-architecture-guide.md)     
 [描述的聚集索引和非聚集索引](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)     
  
