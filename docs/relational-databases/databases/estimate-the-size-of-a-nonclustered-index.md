---
title: 估计非聚集索引的大小 | Microsoft Docs
description: 使用此过程来估计在 SQL Server 中存储非聚集索引所需的空间大小。
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.reviewer: ''
ms.prod_service: database-engine, sql-database
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- space allocation [SQL Server], index size
- size [SQL Server], tables
- predicting table size [SQL Server]
- table size [SQL Server]
- estimating table size
- clustered indexes, table size
- designing databases [SQL Server], estimating size
- calculating table size
ms.assetid: c183b0e4-ef4c-4bfc-8575-5ac219c25b0a
author: stevestein
ms.author: sstein
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: 9c234bb8371d99025bd97cfb86f5d85472d34ee4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474078"
---
# <a name="estimate-the-size-of-a-nonclustered-index"></a>估计非聚集索引的大小

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  按照下列步骤估计存储非聚集索引所需的空间大小：  
  
1.  计算步骤 2 和步骤 3 中所用变量的值。  
  
2.  计算用于存储非聚集索引的叶级中的索引信息的空间。  
  
3.  计算用于存储非聚集索引的非叶级中的索引信息的空间。  
  
4.  对计算出的值求和。  
  
## <a name="step-1-calculate-variables-for-use-in-steps-2-and-3"></a>步骤 1。 计算步骤 2 和步骤 3 中所用变量的值  
 可使用下列步骤计算所用变量的值以估计存储索引的较高级别所需的空间大小。  
  
1.  指定表中显示的行数：  
  
     ***Num_Rows** _ = 表中的行数  
  
2.  指定索引键中固定长度和可变长度列的数量，并计算存储所需的空间：  
  
     索引键列可以包括固定长度和可变长度列。 若要估计内部级别索引行的大小，请计算每组列在索引行中所占据的空间。 列的大小取决于数据类型和长度规定。  
  
     _*_Num_Key_Cols_*_ = 总键列数（固定长度和可变长度）  
  
     _*_Fixed_Key_Size_*_ = 所有固定长度键列的总字节大小  
  
     _*_Num_Variable_Key_Cols_*_ = 可变长度键列数  
  
     _*_Max_Var_Key_Size_*_ = 所有可变长度键列的最大字节大小  
  
3.  如果索引不是唯一的，对所需的数据行定位符说明如下：  
  
     如果非聚集索引不是唯一的，数据行定位符将与非聚集索引键组合使用，以便为每一行生成唯一的键值。  
  
     如果非聚集索引在堆上，则数据行定位符是堆 RID。 其大小是 8 个字节。  
  
     _*_Num_Key_Cols_*_  = _*_Num_Key_Cols_*_ + 1  
  
     _*_Num_Variable_Key_Cols_*_  = _*_Num_Variable_Key_Cols_*_ + 1  
  
     _*_Max_Var_Key_Size_*_  = _*_Max_Var_Key_Size_*_ + 8  
  
     如果非聚集索引在聚集索引之上，则数据行定位符是聚集键。 必须与非聚集索引键结合使用的列是聚集键中的以下列：不在非聚集索引键列集中的列。  
  
     _*_Num_Key_Cols_*_  = _*_Num_Key_Cols_*_ + 不在非群集索引键列集中的群集键列数（如果群集索引不唯一，则 + 1）  
  
     _*_Fixed_Key_Size_*_  = _*_Fixed_Key_Size_*_ + 不在非群集索引键列集中的固定长度群集键列的总字节大小  
  
     _*_Num_Variable_Key_Cols_*_  = _*_Num_Variable_Key_Cols_*_ + 不在非群集索引键列集中的可变长度群集键列数（如果群集索引不唯一，则 + 1）  
  
     _*_Max_Var_Key_Size_*_  = _*_Max_Var_Key_Size_*_ + 不在非群集索引键列集中的可变长度群集键列的最大字节大小（如果群集索引不唯一，则 + 4）  
  
4.  可以保留行的一部分（称为“空位图”），以管理列的为空性。 计算其大小：  
  
     如果索引键中有可为 Null 的列（包括步骤 1.3 中所述的所有必要的聚集键列），则保留索引行的一部分，以用于 Null 位图。  
  
     _*_Index_Null_Bitmap_*_ = 2 + ((索引行中的列数 + 7) / 8)  
  
     仅应使用上述表达式中的整数部分， 而放弃所有余数。  
  
     如果没有可为 Null 的键列，请将 _*_Index_Null_Bitmap_*_ 设置为 0。  
  
5.  计算可变长度数据大小：  
  
     如果索引键中有可变长度的列（包括所有必要的聚集索引键列），请确定存储索引行中的这些列需使用的空间：  
  
     _*_Variable_Key_Size_*_  = 2 + (_*_Num_Variable_Key_Cols_*_ x 2) + _*_Max_Var_Key_Size_*_  
  
     添加到 _*_Max_Var_Key_Size_*_ 中的字节可用于跟踪每个可变列。此公式假定所有可变长度列均百分之百充满。 如果预计可变长度列占用的存储空间比例较低，则可以按照该比例调整 _*_Max_Var_Key_Size_*_ 值，从而对整个表大小得出一个更准确的估计。  
  
     如果没有可变长度列，请将 _*_Variable_Key_Size_*_ 设置为 0。  
  
6.  计算索引行大小：  
  
     _*_Index_Row_Size_*_  = _*_Fixed_Key_Size_*_ + _*_Variable_Key_Size_*_ + _*_Index_Null_Bitmap_*_ + 1（对应于索引行的行标题开销）+ 6（对应于子页 ID 指针）  
  
7.  下一步，计算每页的索引行数（每页有 8096 个可用字节）：  
  
     _*_Index_Rows_Per_Page_*_  = 8096 / (_*_Index_Row_Size_*_ + 2)  
  
     因为索引行不能跨页，所以每页的索引行数应向下舍入到最接近的整数。 公式中的 2 是计算行数时引入的行大小余量。  
  
## <a name="step-2-calculate-the-space-used-to-store-index-information-in-the-leaf-level"></a>步骤 2. 计算用于存储叶级中的索引信息的空间  
 可使用下列步骤估计存储叶级索引所需的空间大小。 需要使用从步骤 1 中保留的值来完成此步骤。  
  
1.  指定叶级的固定长度列和可变长度列的数量，并计算存储这些列所需的空间：  
  
    > [!NOTE]  
    >  您可以通过包含索引键列和非键列扩展非聚集索引。 这些额外的列只存储在叶级非聚集索引。 有关详细信息，请参阅 [Create Indexes with Included Columns](../../relational-databases/indexes/create-indexes-with-included-columns.md)。  
  
    > [!NOTE]  
    >  你可以组合 _*varchar**、nvarchar、varbinary 或 sql_variant 列，使定义的表的总宽度超过 8,060 字节。   对于 **varchar**、 **varbinary** 或 **sql_variant** 中的每一列，其长度不能超过 8,000 字节，对于 **nvarchar** 列，不能超过 4,000 字节。 但是，表中这些列的组合宽度可超过 8,060 字节的限制。 这也适用于具有包含列的非聚集索引叶行。  
  
     如果非聚集索引没有任何包含列，则使用步骤 1 中的值（包括在步骤 1.3 中进行的任何修改）：  
  
     **_Num_Leaf_Cols_* _  = _*_Num_Key_Cols_*_  
  
     _*_Fixed_Leaf_Size_*_  = _*_Fixed_Key_Size_*_  
  
     _*_Num_Variable_Leaf_Cols_*_  = _*_Num_Variable_Key_Cols_*_  
  
     _*_Max_Var_Leaf_Size_*_  = _*_Max_Var_Key_Size_*_  
  
     如果非聚集索引确实具有包含列，则对步骤 1 中的值加上适当的值（包括在步骤 1.3 中进行的任何修改）。 列的大小取决于数据类型和长度规定。 有关详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。  
  
     _*_Num_Leaf_Cols_*_  = _*_Num_Key_Cols_*_ + 包含的列数  
  
     _*_Fixed_Leaf_Size_*_  = _*_Fixed_Key_Size_*_ + 固定长度包含列的总字节大小  
  
     _*_Num_Variable_Leaf_Cols_*_  = _*_Num_Variable_Key_Cols_*_ + 可变长度包含列数  
  
     _*_Max_Var_Leaf_Size_*_  = _*_Max_Var_Key_Size_*_ + 可变长度包含列的最大字节大小  
  
2.  数据行定位符说明：  
  
     如果非聚集索引不是唯一的，则已在步骤 1.3 中考虑了数据行定位符的开销且不需要进行其他的修改。 转到下一步。  
  
     如果非聚集索引是唯一的，则必须在叶级的所有行中说明数据行定位符。  
  
     如果非聚集索引在堆上，则数据行定位符是堆 RID（大小为 8 字节）。  
  
     _*_Num_Leaf_Cols_*_  = _*_Num_Leaf_Cols_*_ + 1  
  
     _*_Num_Variable_Leaf_Cols_*_  = _*_Num_Variable_Leaf_Cols_*_ + 1  
  
     _*_Max_Var_Leaf_Size_*_  = _*_Max_Var_Leaf_Size_*_ + 8  
  
     如果非聚集索引在聚集索引之上，则数据行定位符是聚集键。 必须与非聚集索引键结合使用的列是聚集键中的以下列：不在非聚集索引键列集中的列。  
  
     _*_Num_Leaf_Cols_*_  = _*_Num_Leaf_Cols_*_ + 不在非群集索引键列集中的群集键列数（如果群集索引不唯一，则 + 1）  
  
     _*_Fixed_Leaf_Size_*_  = _*_Fixed_Leaf_Size_*_ + 不在非群集索引键列集中的固定长度群集键列数  
  
     _*_Num_Variable_Leaf_Cols_*_  = _*_Num_Variable_Leaf_Cols_*_ + 不在非群集索引键列集中的可变长度群集键列数（如果群集索引不唯一，则 + 1）  
  
     _*_Max_Var_Leaf_Size_*_  = _*_Max_Var_Leaf_Size_*_ + 不在非群集索引键列集中的可变长度群集键列的字节大小（如果群集索引不唯一，则 + 4）  
  
3.  计算 Null 位图大小：  
  
     _*_Leaf_Null_Bitmap_*_  = 2 + ((_*_Num_Leaf_Cols_*_ + 7) / 8)  
  
     仅应使用上述表达式中的整数部分， 而放弃所有余数。  
  
4.  计算可变长度数据大小：  
  
     如果存在可变长度的列（键列或包含列），包括在之前的步骤 2.2 中所述的所有必要的聚集索引键列，请确定存储索引行中的这些列所需的空间：  
  
     _*_Variable_Leaf_Size_*_  = 2 + (_*_Num_Variable_Leaf_Cols_*_ x 2) + _*_Max_Var_Leaf_Size_*_  
  
     添加到 _*_Max_Var_Key_Size_*_ 中的字节可用于跟踪每个可变列。此公式假定所有可变长度列均百分之百充满。 如果预计可变长度列占用的存储空间比例较低，则可以按照该比例调整 _*_Max_Var_Leaf_Size_*_ 值，从而对整个表大小得出一个更准确的估计。  
  
     如果没有可变长度的列（键列或包含列），则将 _*_Variable_Leaf_Size_*_ 设置为 0。  
  
5.  计算索引行大小：  
  
     _*_Leaf_Row_Size_*_  = _*_Fixed_Leaf_Size_*_ + _*_Variable_Leaf_Size_*_ + _*_Leaf_Null_Bitmap_*_ + 1（用于索引行的行标题开销）  
  
6.  下一步，计算每页的索引行数（每页有 8096 个可用字节）：  
  
     _*_Leaf_Rows_Per_Page_*_  = 8096 / (_*_Leaf_Row_Size_*_ + 2)  
  
     因为索引行不能跨页，所以每页的索引行数应向下舍入到最接近的整数。 公式中的 2 是计算行数时引入的行大小余量。  
  
7.  根据指定的 [填充因子](../../relational-databases/indexes/specify-fill-factor-for-an-index.md) 计算每页保留的空行数：  
  
     _*_Free_Rows_Per_Page_*_  = 8096 x ((100 - _*_Fill_Factor_*_) / 100) / (_*_Leaf_Row_Size_*_ + 2)  
  
     计算中使用的填充因子为整数值，而不是百分比。 因为行不跨页，所以每页的行数应向下舍入到最接近的整数。 填充因子增大时，每页将存储更多的数据，因此页数将减少。 公式中的 2 是计算行数时引入的行大小余量。  
  
8.  计算存储所有行所需的页数：  
  
     _*_Num_Leaf_Pages_*_  = _*_Num_Rows_*_ / (_*_Leaf_Rows_Per_Page_*_ - _*_Free_Rows_Per_Page_*_ )  
  
     估计的页数应向上舍入到最接近的整数。  
  
9. 计算索引的大小（每页总共有 8192 个字节）：  
  
     _*_Leaf_Space_Used_*_  = 8192 x _*_Num_Leaf_Pages_*_  
  
## <a name="step-3-calculate-the-space-used-to-store-index-information-in-the-non-leaf-levels"></a>步骤 3. 计算用于存储非叶级中的索引信息的空间  
 按照下列步骤估计存储索引的中间级和根级所需的空间大小。 需要使用从步骤 2 和步骤 3 中保留的值来完成此步骤。  
  
1.  计算索引中的非叶级数：  
  
     _*_Non-leaf Levels_*_  = 1 + log( Index_Rows_Per_Page) (_*_Num_Leaf_Pages_*_ / _*_Index_Rows_Per_Page_*_)  
  
     将此值向上舍入到最接近的整数。 此值不包括非聚集索引的叶级别。  
  
2.  计算索引中的非叶页数：  
  
     _*_Num_Index_Pages_*_  = ∑Level (_*_Num_Leaf_Pages/Index_Rows_Per_Page_*_^Level) 其中，1 <= Level <= _*_Levels_*_  
  
     将每个被加数向上舍入到最接近的整数。 由于是个简单示例，请考虑使用 _*_Num_Leaf_Pages_*_ = 1000 和 _*_Index_Rows_Per_Page_*_ = 25 的索引。 页级别以上的第一个索引级别存储 1000 个索引行，即每个叶页一个索引行，每页可以包括 25 个索引行。 这意味着存储这 1000 个索引行需要 40 页。 下一级索引必须存储 40 行。 这意味着需要 2 页。 最后一级索引必须存储 2 行。 这意味着需要 1 页。 这就产生了 43 个非叶索引页。 如果将这些数用到前面的公式中，结果如下：  
  
     _*_Non-leaf_Levels_*_  = 1 + log(25) (1000 / 25) = 3  
  
     _*_Num_Index_Pages_*_ = 1000/(25^3)+ 1000/(25^2) + 1000/(25^1) = 1 + 2 + 40 = 43，这是上面的示例中所述的页数。  
  
3.  计算索引的大小（每页总共有 8192 个字节）：  
  
     _*_Index_Space_Used_*_  = 8192 x _*_Num_Index_Pages_*_  
  
## <a name="step-4-total-the-calculated-values"></a>步骤 4. 对计算出的值求和  
 对从前面两个步骤中得到的值求和：  
  
 Nonclustered index size (bytes) = _*_Leaf_Space_Used_*_ + _*_Index_Space_used_*_  
  
 此计算不考虑以下因素：  
  
-   分区  
  
     分区的空间开销很小，但是计算复杂。 是否包括它并不重要。  
  
-   分配页  
  
     至少有一个 IAM 页用于跟踪为堆分配的页，但是空间开销很小，并且没有算法可以精确地计算出要使用的 IAM 页数。  
  
-   大型对象 (LOB) 值  
  
     精确确定存储 LOB 数据类型 _*varchar(max)**、varbinary(max)、nvarchar(max)、text、ntext、xml 和 image 值所用的空间量的算法非常复杂。      只需加上期望的 LOB 值的平均大小，再乘以 **_Num_Rows_**，然后将所得结果加到非群集索引的总大小。  
  
-   压缩  
  
     无法预先计算压缩索引的大小。  
  
-   稀疏列  
  
     有关稀疏列的空间要求的信息，请参阅 [Use Sparse Columns](../../relational-databases/tables/use-sparse-columns.md)。  
  
## <a name="see-also"></a>另请参阅  
 [描述的聚集索引和非聚集索引](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)   
 [创建非聚集索引](../../relational-databases/indexes/create-nonclustered-indexes.md)   
 [创建聚集索引](../../relational-databases/indexes/create-clustered-indexes.md)   
 [估计表的大小](../../relational-databases/databases/estimate-the-size-of-a-table.md)   
 [估计聚集索引的大小](../../relational-databases/databases/estimate-the-size-of-a-clustered-index.md)   
 [估计堆的大小](../../relational-databases/databases/estimate-the-size-of-a-heap.md)   
 [估计数据库的大小](../../relational-databases/databases/estimate-the-size-of-a-database.md)  
  
  
