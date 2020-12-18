---
title: 估计聚集索引的大小 | Microsoft Docs
description: 使用此过程来估计在 SQL Server 中存储聚集索引中的数据所需的空间大小。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.prod_service: database-engine, sql-database
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- space allocation [SQL Server], index size
- size [SQL Server], tables
- disk space [SQL Server], indexes
- predicting table size [SQL Server]
- table size [SQL Server]
- estimating table size
- space [SQL Server], indexes
- clustered indexes, table size
- nonclustered indexes [SQL Server], table size
- designing databases [SQL Server], estimating size
- calculating table size
ms.assetid: 2b5137f8-98ad-46b5-9aae-4c980259bf8d
author: stevestein
ms.author: sstein
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: d963a8745d83be039fa2970a24d04a2a882bf7a9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478358"
---
# <a name="estimate-the-size-of-a-clustered-index"></a>估计聚集索引的大小

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  您可以使用下列步骤估计存储聚集索引中的数据所需的空间大小：  
  
1.  计算存储聚集索引叶级数据所用的空间。  
  
2.  计算存储聚集索引的索引信息所用的空间。  
  
3.  对计算出的值求和。  
  
## <a name="step-1-calculate-the-space-used-to-store-data-in-the-leaf-level"></a>步骤 1。 计算在叶级别存储数据所用的空间  
  
1.  指定表中显示的行数：  
  
     ***Num_Rows** _ = 表中的行数  
  
2.  指定固定长度和可变长度列的数量，并计算存储所需的空间：  
  
     计算每组列在数据行中所占据的空间。 列的大小取决于数据类型和长度规定。  
  
     _*_Num_Cols_*_ = 总列数（固定长度和可变长度）  
  
     _*_Fixed_Data_Size_*_ = 所有固定长度列的总字节大小  
  
     _*_Num_Variable_Cols_*_ = 可变长度列数  
  
     _*_Max_Var_Size_*_ = 所有可变长度列的最大字节大小  
  
3.  如果聚集索引不唯一，请考虑 _uniqueifier* 列：  
  
     唯一标识符是可为 Null 的可变长度列。 在具有非唯一键值的行中，它非 Null 而且大小为 4 个字节。 此值是索引键的一部分，用于确保每一行都具有唯一的键值。  
  
     ***Num_Cols** _  = _*_Num_Cols_*_ + 1  
  
     _*_Num_Variable_Cols_*_  = _*_Num_Variable_Cols_*_ + 1  
  
     _*_Max_Var_Size_*_  = _*_Max_Var_Size_*_ + 4  
  
     这些修改假定所有值都不是唯一的。  
  
4.  保留行中称为 Null 位图的部分以管理列的为空性。 计算其大小：  
  
     _*_Null_Bitmap_*_  = 2 + ((_*_Num_Cols_*_ + 7) / 8)  
  
     仅使用上述表达式中的整数部分，而放弃所有余数。  
  
5.  计算可变长度数据的大小：  
  
     如果表中有可变长度列，请确定在行中存储这些列需使用的空间：  
  
     _*_Variable_Data_Size_*_  = 2 + (_*_Num_Variable_Cols_*_ x 2) + _*_Max_Var_Size_*_  
  
     添加到 _*_Max_Var_Size_*_ 中的字节用于跟踪每个可变列。 此公式假设所有可变长度列均百分之百充满。 如果预计可变长度列占用的存储空间比例较低，则可以按照该比例调整 _*_Max_Var_Size_*_ 值，从而对整个表大小得出一个更准确的估计。  
  
    > [!NOTE]  
    >  你可以组合 _*varchar**、nvarchar、varbinary 或 sql_variant 列，使定义的表的总宽度超过 8,060 字节。   对于 **varchar**、 **varbinary** 或 **sql_variant** 中的每一列，其长度不能超过 8,000 字节，对于 **nvarchar** 列，不能超过 4,000 字节。 但是，表中这些列的组合宽度可超过 8,060 字节的限制。  
  
     如果没有可变长度列，请将 **_Variable_Data_Size_* _ 设置为 0。  
  
6.  计算总的行大小：  
  
     _*_Row_Size_*_  = _*_Fixed_Data_Size_*_ + _*_Variable_Data_Size_*_ + _*_Null_Bitmap_*_ + 4  
  
     值 4 是数据行的行标题的开销。  
  
7.  下一步，计算每页的行数（每页有 8096 个可用字节）：  
  
     _*_Rows_Per_Page_*_  = 8096 / (_*_Row_Size_*_ + 2)  
  
     因为行不跨页，所以每页的行数应向下舍入到最接近的整数。 公式中的数值 2 是计算行数时引入的行大小余量。  
  
8.  根据指定的 [填充因子](../../relational-databases/indexes/specify-fill-factor-for-an-index.md) 计算每页保留的空行数：  
  
     _*_Free_Rows_Per_Page_*_  = 8096 x ((100 - _*_Fill_Factor_*_) / 100) / (_*_Row_Size_*_ + 2)  
  
     计算中使用的填充因子为整数值，而不是百分比。 因为行不跨页，所以每页的行数应向下舍入到最接近的整数。 填充因子增大时，每页将存储更多的数据，因此页数将减少。 公式中的数值 2 是计算行数时引入的行大小余量。  
  
9. 计算存储所有行所需的页数：  
  
     _*_Num_Leaf_Pages_*_  = _*_Num_Rows_*_ / (_*_Rows_Per_Page_*_ - _*_Free_Rows_Per_Page_*_ )  
  
     估计的页数应向上舍入到最接近的整数。  
  
10. 计算在叶级别中存储数据所需的空间大小（每页共有 8192 个字节）：  
  
     _*_Leaf_space_used_*_  = 8192 x _*_Num_Leaf_Pages_*_  
  
## <a name="step-2-calculate-the-space-used-to-store-index-information"></a>步骤 2. 计算存储索引信息所用的空间  
 您可以使用下列步骤估计存储索引的较高级别所需的空间大小：  
  
1.  指定索引键中固定长度和可变长度列的数量，并计算存储所需的空间：  
  
     索引键列可以包括固定长度和可变长度列。 若要估计内部级别索引行的大小，请计算每组列在索引行中所占据的空间。 列的大小取决于数据类型和长度规定。  
  
     _*_Num_Key_Cols_*_ = 总键列数（固定长度和可变长度）  
  
     _*_Fixed_Key_Size_*_ = 所有固定长度键列的总字节大小  
  
     _*_Num_Variable_Key_Cols_*_ = 可变长度键列数  
  
     _*_Max_Var_Key_Size_*_ = 所有可变长度键列的最大字节大小  
  
2.  如果索引不唯一，则请说明所需的任意唯一标识符：  
  
     唯一标识符是可为 Null 的可变长度列。 它将是非 Null 的，在具有非唯一索引键值的行中的大小是 4 个字节。 此值是索引键的一部分，用于确保每一行都具有唯一的键值。  
  
     _*_Num_Key_Cols_*_  = _*_Num_Key_Cols_*_ + 1  
  
     _*_Num_Variable_Key_Cols_*_  = _*_Num_Variable_Key_Cols_*_ + 1  
  
     _*_Max_Var_Key_Size_*_  = _*_Max_Var_Key_Size_*_ + 4  
  
     这些修改假定所有值都不是唯一的。  
  
3.  计算 Null 位图大小：  
  
     如果索引键中有允许为 Null 的列，则索引行的一部分将为 Null 位图保留。 计算其大小：  
  
     _*_Index_Null_Bitmap_*_ = 2 + ((索引行中的列数 + 7) / 8)  
  
     仅应使用上述表达式中的整数部分， 而放弃所有余数。  
  
     如果没有可为 Null 的键列，请将 _*_Index_Null_Bitmap_*_ 设置为 0。  
  
4.  计算可变长度数据的大小：  
  
     如果索引中有可变长度列，请确定在索引行中存储这些列需使用的空间：  
  
     _*_Variable_Key_Size_*_  = 2 + (_*_Num_Variable_Key_Cols_*_ x 2) + _*_Max_Var_Key_Size_*_  
  
     添加到 _*_Max_Var_Key_Size_*_ 中的字节用于跟踪每个可变长度列。 此公式假设所有可变长度列均百分之百充满。 如果预计可变长度列占用的存储空间比例较低，则可以按照该比例调整 _*_Max_Var_Key_Size_*_ 值，从而对整个表大小得出一个更准确的估计。  
  
     如果没有可变长度列，请将 _*_Variable_Key_Size_*_ 设置为 0。  
  
5.  计算索引行大小：  
  
     _*_Index_Row_Size_*_  = _*_Fixed_Key_Size_*_ + _*_Variable_Key_Size_*_ + _*_Index_Null_Bitmap_*_ + 1（对应于索引行的行标题开销）+ 6（对应于子页 ID 指针）  
  
6.  下一步，计算每页的索引行数（每页有 8096 个可用字节）：  
  
     _*_Index_Rows_Per_Page_*_  = 8096 / (_*_Index_Row_Size_*_ + 2)  
  
     因为索引行不能跨页，所以每页的索引行数应向下舍入到最接近的整数。 公式中的 2 是计算行数时引入的行大小余量。  
  
7.  计算索引中的级别数：  
  
     _*_Non-leaf_Levels_*_  = 1 + log (Index_Rows_Per_Page) (_*_Num_Leaf_Pages_*_ / _*_Index_Rows_Per_Page_*_)  
  
     将此值向上舍入到最接近的整数。 此值不包括聚集索引的叶级别。  
  
8.  计算索引中的非叶页数：  
  
     _*_Num_Index_Pages =_*_ ∑Level _*_ (Num_Leaf_Pages / (Index_Rows_Per_Page_ *_^Level_* _))_ *_  
  
     其中，1 <= Level <= _*_Non-leaf_Levels_*_  
  
     将每个被加数向上舍入到最接近的整数。 由于是个简单示例，请考虑使用 _*_Num_Leaf_Pages_*_ = 1000 和 _*_Index_Rows_Per_Page_*_ = 25 的索引。 页级别以上的第一个索引级别存储 1000 个索引行，即每个叶页一个索引行，每页可以包括 25 个索引行。 这意味着存储这 1000 个索引行需要 40 页。 下一级索引必须存储 40 行。 这意味着需要 2 页。 最后一级索引必须存储 2 行。 这意味着需要 1 页。 这就提供了 43 个非叶索引页。 如果将这些数用到前面的公式中，结果如下：  
  
     _*_Non-leaf_Levels_*_  = 1 + log(25) (1000 / 25) = 3  
  
     _*_Num_Index_Pages_*_ = 1000/(25^3)+ 1000/(25^2) + 1000/(25^1) = 1 + 2 + 40 = 43，这是上面的示例中所述的页数。  
  
9. 计算索引的大小（每页总共有 8192 个字节）：  
  
     _*_Index_Space_Used_*_  = 8192 x _*_Num_Index_Pages_*_  
  
## <a name="step-3-total-the-calculated-values"></a>步骤 3. 对计算出的值求和  
 对从前面两个步骤中得到的值求和：  
  
 群集索引大小（字节）= _*_Leaf_Space_Used_*_ + _*_Index_Space_used_*_  
  
 此计算不考虑以下因素：  
  
-   分区  
  
     分区的空间开销很小，但是计算复杂。 是否包括它并不重要。  
  
-   分配页  
  
     至少有一个 IAM 页用于跟踪为堆分配的页，但是空间开销很小，并且没有算法可以精确地计算出要使用的 IAM 页数。  
  
-   大型对象 (LOB) 值  
  
     精确确定存储 LOB 数据类型 _*varchar(max)**、varbinary(max)、nvarchar(max)、text、ntext、xml 和 image 值所用的空间量的算法非常复杂。      只需加上所期望的 LOB 值的平均大小，再乘以 **_Num_Rows_**，然后再加上群集索引的总大小就可以了。  
  
-   压缩  
  
     无法预先计算压缩索引的大小。  
  
-   稀疏列  
  
     有关稀疏列的空间要求的信息，请参阅 [Use Sparse Columns](../../relational-databases/tables/use-sparse-columns.md)。  
  
## <a name="see-also"></a>另请参阅  
 [描述的聚集索引和非聚集索引](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)   
 [估计表的大小](../../relational-databases/databases/estimate-the-size-of-a-table.md)   
 [创建聚集索引](../../relational-databases/indexes/create-clustered-indexes.md)   
 [创建非聚集索引](../../relational-databases/indexes/create-nonclustered-indexes.md)   
 [估计非聚集索引的大小](../../relational-databases/databases/estimate-the-size-of-a-nonclustered-index.md)   
 [估计堆的大小](../../relational-databases/databases/estimate-the-size-of-a-heap.md)   
 [估计数据库的大小](../../relational-databases/databases/estimate-the-size-of-a-database.md)  
  
  
