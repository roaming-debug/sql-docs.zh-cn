---
title: 估计堆的大小 | Microsoft Docs
description: 使用此过程估算在 SQL Server 的堆中存储数据所需的空间量。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- disk space [SQL Server], indexes
- estimating heap size
- size [SQL Server], heap
- space [SQL Server], indexes
- heaps
ms.assetid: 81fd5ec9-ce0f-4c2c-8ba0-6c483cea6c75
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: aa3910c7ff3b66474835f2d3335e0cdac44430bf
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748047"
---
# <a name="estimate-the-size-of-a-heap"></a>估计堆的大小
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  可以使用以下步骤估计在堆中存储数据所需的空间量：  
  
1.  指定表中显示的行数：  
  
     **Num_Rows**  = 表中的行数  
  
2.  指定固定长度和可变长度列的数量，并计算存储所需的空间：  
  
     计算每组列在数据行中所占据的空间。 列的大小取决于数据类型和长度规定。  
  
     **Num_Cols**  = 总列数（固定长度和可变长度）  
  
     **Fixed_Data_Size**  = 所有固定长度列的总字节大小  
  
     **Num_Variable_Cols**  = 可变长度列数  
  
     **Max_Var_Size**  = 所有可变长度列的最大总字节大小  
  
3.  保留行中称为 Null 位图的部分以管理列的为空性。 计算其大小：  
  
     **Null_Bitmap**  = 2 + ((**Num_Cols** + 7) / 8)   
  
     只应使用该表达式的整数部分。 而放弃所有余数。  
  
4.  计算可变长度数据的大小：  
  
     如果表中有可变长度列，请确定在行中存储这些列需使用的空间：  
  
     **Variable_Data_Size**  = 2 + (**Num_Variable_Cols** x 2) + **Max_Var_Size**    
  
     添加到 Max_Var_Size 中的字节用于跟踪每个可变长度列。 此公式假设所有可变长度列均百分之百充满。 如果预计可变长度列占用的存储空间比例较低，则可以按照该比例调整 Max_Var_Size 值，从而对整个表大小得出一个更准确的估计。  
  
    > [!NOTE]  
    >  你可以组合 **varchar**、 **nvarchar**、 **varbinary** 或 **sql_variant** 列，使定义的表的总宽度超过 8,060 字节。 对于 varchar、nvarchar、varbinary 或 sql_variant 列，每列的长度仍不得超过 8,000 字节  。 但是，表中这些列的组合宽度可超过 8,060 字节的限制。  
  
     如果没有可变长度列，请将 Variable_Data_Size 设置为 0。  
  
5.  计算总的行大小：  
  
     **Row_Size**  = **Fixed_Data_Size** + **Variable_Data_Size** + **Null_Bitmap** + 4     
  
     公式中的值 4 是数据行的行标题开销。  
  
6.  下一步，计算每页的行数（每页有 8096 个可用字节）：  
  
     **Rows_Per_Page**  = 8096 / (**Row_Size** + 2)   
  
     因为行不跨页，所以每页的行数应向下舍入到最接近的整数。 公式中的数值 2 是计算行数时引入的行大小余量。  
  
7.  计算存储所有行所需的页数：  
  
     **Num_Pages**  = **Num_Rows** / **Rows_Per_Page**    
  
     估计的页数应向上舍入到最接近的整数。  
  
8.  计算在堆中存储数据所需的空间量（每页的总字节为 8192）：  

     堆大小（字节）= 8192 x Num_Pages  
  
 此计算不考虑以下因素：  
  
-   分区  
  
     分区的空间开销很小，但是计算复杂。 是否包括它并不重要。  
  
-   分配页  
  
     至少有一个 IAM 页用于跟踪为堆分配的页，但是空间开销很小，并且没有算法可以精确地计算出要使用的 IAM 页数。  
  
-   大型对象 (LOB) 值  
  
     精确确定存储 LOB 数据类型 **varchar(max)** 、 **varbinary(max)** 、 **nvarchar(max)** 、 **text**、 **ntextxml** 和 **image** 值所用的空间量的算法非常复杂。 只添加所期望的 LOB 值的平均大小就足够了，然后将其添加至总的堆大小中。  
  
-   压缩  
  
     无法预先计算压缩堆的大小。  
  
-   稀疏列  
  
     有关稀疏列的空间要求的信息，请参阅 [Use Sparse Columns](../../relational-databases/tables/use-sparse-columns.md)。  
  
## <a name="see-also"></a>另请参阅  
 [堆（没有聚集索引的表）](../../relational-databases/indexes/heaps-tables-without-clustered-indexes.md)   
 [描述的聚集索引和非聚集索引](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)   
 [创建聚集索引](../../relational-databases/indexes/create-clustered-indexes.md)   
 [创建非聚集索引](../../relational-databases/indexes/create-nonclustered-indexes.md)   
 [估计表的大小](../../relational-databases/databases/estimate-the-size-of-a-table.md)   
 [估计聚集索引的大小](../../relational-databases/databases/estimate-the-size-of-a-clustered-index.md)   
 [估计非聚集索引的大小](../../relational-databases/databases/estimate-the-size-of-a-nonclustered-index.md)   
 [估计数据库的大小](../../relational-databases/databases/estimate-the-size-of-a-database.md)  
  
  
