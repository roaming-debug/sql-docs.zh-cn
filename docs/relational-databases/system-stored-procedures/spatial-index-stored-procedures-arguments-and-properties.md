---
description: 空间索引存储过程的参数和属性
title: 空间索引存储过程的参数和属性 |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- spatial indexes [SQL Server], stored procedures
ms.assetid: ee26082b-c0ed-40ff-b5ad-f5f6b00f0475
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f9faf0901643b07420f8627d6afda4b9713d034a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201762"
---
# <a name="spatial-index-stored-procedures---arguments-and-properties"></a>空间索引存储过程-参数和属性
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题介绍空间索引存储过程的参数和属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
 有关特定空间索引存储过程的语法，请参阅下列主题：  
  
-   [sp_help_spatial_geometry_index &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)  
  
-   [sp_help_spatial_geometry_index_xml &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-xml-transact-sql.md)  
  
-   [sp_help_spatial_geography_index &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-transact-sql.md)  
  
-   [sp_help_spatial_geography_index_xml &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-xml-transact-sql.md)  
  
## <a name="arguments"></a>参数  
`[ @tabname = ] 'tabname'` 已为其指定了空间索引的表的限定名称或非限定名称。  
  
 仅当指定了限定表时才需要引号。 如果提供的是完全限定名称（包括数据库名称），则数据库名称必须是当前数据库的名称。 *tabname* 是 **nvarchar** (776) ，无默认值。  
  
`[ @indexname = ] 'indexname'` 指定的空间索引的名称。 *indexname* 的值为 **sysname** ，无默认值。  
  
`[ @verboseoutput = ] 'verboseoutput'` 要返回的属性名称和值的范围。  
  
 0 = 核心属性  
  
 \>0 = 所有属性  
  
 *verboseoutput* 为 **tinyint** ，无默认值。  
  
`[ @query_sample = ] 'query_sample'` 是一个代表性的查询示例，可用于测试索引的有用性。 它可以是一个有代表性的对象或查询窗口。 *query_sample* 是不带默认值的 **几何图形** 。  
  
`[ @xml_output = ] 'xml_output'` 一个输出参数，它在 XML 片段中返回结果集。 *xml_output* 为 **xml** ，无默认值。  
  
## <a name="properties"></a>属性  
 设置 **\@ verboseoutput** = 0 以返回核心属性，如下表所示;**\@ verboseoutput** > 0 返回空间索引的所有属性。  
  
 **Base_Table_Rows**  
 基表中的行数 值为 **bigint**。  
  
 **Bounding_Box_xmin**  
 **几何图形** 类型的空间索引的 X 最小边界框属性。 对于 **geography** 类型，此属性值为 NULL。 值为 **float**。  
  
 **Bounding_Box_ymin**  
 **几何图形** 类型的空间索引的 Y 最小范围框属性。 对于 **geography** 类型，此属性值为 NULL。 值为 **float**。  
  
 **Bounding_Box_xmax**  
 **几何图形** 类型的空间索引的 X 最大边界框属性。 对于 **geography** 类型，此属性值为 NULL。 值为 **float**。  
  
 **Bounding_Box_ymax**  
 **几何图形** 类型的空间索引的 Y 最大边界框属性。 对于 **geography** 类型，此属性值为 NULL。 值为 **float**。  
  
 **Grid_Size_Level_1**  
 空间索引的第1级网格密度：  
  
 对于 LOW 为 16  
  
 对于 MEDIUM 为 64  
  
 对于 HIGH 为 256  
  
 值为 **int**。  
  
 **Grid_Size_Level_2**  
 空间索引的第2级网格密度：  
  
 对于 LOW 为 16  
  
 对于 MEDIUM 为 64  
  
 对于 HIGH 为 256  
  
 值为 **int**。  
  
 **Grid_Size_Level_3**  
 空间索引的第3级网格密度：  
  
 对于 LOW 为 16  
  
 对于 MEDIUM 为 64  
  
 对于 HIGH 为 256  
  
 值为 **int**。  
  
 **Grid_Size_Level_4**  
 空间索引的级别 4 网格密度：  
  
 对于 LOW 为 16  
  
 对于 MEDIUM 为 64  
  
 对于 HIGH 为 256  
  
 值为 **int**。  
  
 **Cells_Per_Object**  
 每个对象的单元格数（索引属性）。 值为 **int**。  
  
 **Total_Primary_Index_Rows**  
 索引中的行数。 值为 **bigint**。  
  
 **Total_Primary_Index_Pages**  
 索引中的页数。 值为 **bigint**。  
  
 **Average_Number_Of_Index_Rows_Per_Base_Row**  
 索引行/数字基表行的数量。 值为 **bigint**。  
  
 **Total_Number_Of_ObjectCells_In_Level0_For_QuerySample**  
 指示代表查询示例是否落在 **几何** 索引的边界框之外并 (level 0) 单元格的边界框。 这可以是 0（不在级别 0 单元格中）或 1。 如果在级别 0 单元格中，则所调查的索引不是该查询示例的适当索引。 这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_ObjectCells_In_Level0_In_Index**  
 在 **分割) 的** 边界框外，在级别 0 (根单元格的索引对象的单元格实例的数目。 这是一个核心属性。 值为 **bigint**。  
  
 对于 **几何** 索引，如果索引的边界框小于数据域，则会发生这种情况。 如果查询窗口位于边界框的外部，则级别0中的对象数可能需要辅助筛选器，并且将降低索引性能 (例如， **Total_Number_Of_ObjectCells_In_Level0_For_QuerySample** 为 1) 。 如果查询窗口落在边框之内，级别 0 中较多的对象数量实际上可能会提高索引的性能。  
  
 NULL 实例和空实例在级别 0 进行计数但是不会影响性能。 级别 0 的单元格数量将与基表处 NULL 实例和空实例的数量一样多。 对于 **地理** 索引，级别0的单元格数量将等于 NULL，空实例 + 1 个单元格，因为查询示例计数为1。  
  
 **Total_Number_Of_ObjectCells_In_Level1_In_Index**  
 分割的索引对象的单元格实例的数目。 这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_ObjectCells_In_Level2_In_Index**  
 与级别2精度分割的索引对象的单元格实例数。 这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_ObjectCells_In_Level3_In_Index**  
 分割3级精度的索引对象的单元格实例的数目。 这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_ObjectCells_In_Level4_In_Index**  
 使用级别 4 精度进行分割的索引对象的单元格实例数量。 这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_interior_ObjectCells_In_Level1_In_Index**  
 被分割级别1的对象完全覆盖的单元格数，因而是对象的内部。  (Cell_attributevalue 为2。 ) 这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_interior_ObjectCells_In_Level2_In_Index**  
 对象在分割级别2上完全覆盖的单元格的数目，因此是对象的内部。 （Cell_attribute 值为 2。）这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_interior_ObjectCells_In_Level3_In_Index**  
 在分割级别3由对象完全覆盖的单元格的数目，因此是对象的内部。 （Cell_attribute 值为 2。）这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_interior_ObjectCells_In_Level4_In_Index**  
 单元格的数目，这些单元格将被位于分割级别 4 的一个对象完全覆盖并因此位于该对象内部。 （Cell_attribute 值为 2。）这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level1_In_Index**  
 在分割级别1与对象相交的单元格的数目。  (Cell_attribute 值为1。 ) 这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level2_In_Index**  
 在镶嵌级别2上与对象相交的单元格的数目。  (Cell_attribute 值为1。 ) 这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level3_In_Index**  
 在分割级别3与对象相交的单元格的数目。  (Cell_attribute 值为1。 ) 这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_intersecting_ObjectCells_In_Level4_In_Index**  
 分割级别 4 中与某个对象相交的单元格的数目。  (Cell_attribute 值为1。 ) 这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_Border_ObjectCells_In_Level0_For_QuerySample**  
 指示查询示例是否位于边框之外的根单元格 0 中，但是与其相接触。 这是一个核心属性。 值为 **bigint**。  
  
> [!NOTE]  
>  此信息只有在确定是否存在边框可能漏掉的靠近边框的对象时才有用。  
  
 **Total_Number_Of_Border_ObjectCells_In_Level0_In_Index**  
 级别 0 中与边框接触的对象的数目。  (Cell_attribute 值为0。 ) 值为 **bigint**。  
  
 **Total_Number_Of_Border_ObjectCells_In_Level1_In_Index**  
 在分割级别1上触及网格单元边界的对象单元格的数目。  (Cell_attribute 值为0。 ) 这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_Border_ObjectCells_In_Level2_In_Index**  
 在分割级别2上触及网格单元边界的对象单元格的数目。  (Cell_attribute 值为0。 ) 这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_Border_ObjectCells_In_Level3_In_Index**  
 在分割级别3处触摸网格单元边界的对象单元格的数目。  (Cell_attribute 值为0。 ) 这是一个核心属性。 值为 **bigint**。  
  
 **Total_Number_Of_Border_ObjectCells_In_Level4_In_Index**  
 分割级别 4 中与某个网格单元格边界接触的对象单元格的数目。  (Cell_attribute 值为0。 ) 这是一个核心属性。 值为 **bigint**。  
  
 **Interior_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**  
 包含被某个对象覆盖的叶单元格的网格的总面积（全部叶单元格）百分比。  
  
 例如，某个对象被分割为位于 4 个不同网格级别的 10 个单元格，其覆盖的面积相当于总共 100 个叶单元格。 假设有 3 个被该对象完全覆盖的内部单元格。 被 3 个内部单元格覆盖的区域相当于 42 个叶单元格。 因此，被覆盖区域的百分比为 42%。 这个百分比很好地表明了索引中对象的破碎程度。  
  
 值为 **float**。  
  
 **Intersecting_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**  
 与 **Interior_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage** 相同，不同之处在于它们是部分覆盖的单元格。 值为 **float**。  
  
 **Border_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage**  
 与 **Interior_To_Total_Cells_Normalized_To_Leaf_Grid_Percentage** 相同，不同之处在于它们是边框单元。 值为 **float**。  
  
 **Average_Cells_Per_Object_Normalized_To_Leaf_Grid**  
 每个标准化为叶网格的对象的平均单元格数 通过它可以了解对象的空间大小，或者说对象究竟有多么大。 值为 **float**。  
  
 **Average_Objects_PerLeaf_GridCell**  
 索引的稀疏性。 每个叶单元格的对象的平均数。 值为 **float**。  
  
 **Number_Of_SRIDs_Found**  
 索引和列中唯一 SRID 的数量。 值为 **int**。  
  
 由于列可以包含多个 SRID，并且具有不同 SRID 的对象永远不相交，所以 SRID 的数量指出了索引的选择性。  
  
 **Width_Of_Cell_In_Level1**  
 索引网格中的单元格的宽度属性。 度量单位由索引提供，并依赖于索引数据的 SRID。 值为 **float**。  
  
 **Width_Of_Cell_In_Level2**  
 索引网格中的单元格的宽度属性。 度量单位由索引提供，并依赖于索引数据的 SRID。 值为 **float**。  
  
 **Width_Of_Cell_In_Level3**  
 索引网格中的单元格的宽度属性。 度量单位由索引提供，并依赖于索引数据的 SRID。 值为 **float**。  
  
 **Width_Of_Cell_In_Level4**  
 索引网格中的单元格的宽度属性。 度量单位由索引提供，并且取决于所索引数据的 SRID。 值为 **float**。  
  
 **Height_Of_Cell_In_Level1**  
 索引网格中的单元格的高度属性。 度量单位由索引提供，并依赖于索引数据的 SRID。 值为 **float**。  
  
 **Height_Of_Cell_In_Level2**  
 索引网格中的单元格的高度属性。 度量单位由索引提供，并依赖于索引数据的 SRID。 值为 **float**。  
  
 **Height_Of_Cell_In_Level3**  
 索引网格中的单元格的高度属性。 度量单位由索引提供，并依赖于索引数据的 SRID。 值为 **float**。  
  
 **Height_Of_Cell_In_Level4**  
 索引网格中的单元格的高度属性。 度量单位由索引提供，并依赖于索引数据的 SRID。 值为 **float**。  
  
 **Area_Of_Cell_In_Level1**  
 索引网格中的单元格的面积属性。 度量单位由索引提供，并依赖于索引数据的 SRID。 值为 **float**。  
  
 **Area_Of_Cell_In_Level2**  
 索引网格中的单元格的面积属性。 度量单位由索引提供，并依赖于索引数据的 SRID。 值为 **float**。  
  
 **Area_Of_Cell_In_Level3**  
 索引网格中的单元格的面积属性。 度量单位由索引提供，并依赖于索引数据的 SRID。 值为 **float**。  
  
 **Area_Of_Cell_In_Level4**  
 索引网格中的单元格的面积属性。 度量单位由索引提供，并依赖于索引数据的 SRID。 值为 **float**。  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level1**  
 范围1级单元的边界框覆盖率的百分比。 值为 **float**。  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level2**  
 由第2级单元格覆盖的边界框的百分比。 值为 **float**。  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level3**  
 由第3级单元格覆盖的边界框的百分比。 值为 **float**。  
  
 **CellArea_To_BoundingBoxArea_Percentage_In_Level4**  
 级别 4 单元格边框的覆盖面积百分比。 值为 **float**。  
  
 **Number_Of_Rows_Selected_By_Primary_Filter**  
 由主要筛选器选择的行数。 这是一个核心属性。 值为 **bigint。**  
  
 **Number_Of_Rows_Selected_By_Internal_Filter**  
 由内部筛选器选择的行数。 不会为这些行调用次要筛选器。 这是一个核心属性。 值为 **bigint。**  
  
 返回的数字仅适用于 **STintersects**。  
  
 **Number_Of_Times_Secondary_Filter_Is_Called**  
 次要筛选器的调用次数。 这是一个核心属性。 值为 **bigint。**  
  
 **Percentage_Of_Rows_NotSelected_By_Primary_Filter**  
 如果在基表中有 N 行，并且被主要筛选器选择了 P 行，则会返回百分比形式的 (N-P)/N。 这是一个核心属性。 值为 **float。**  
  
 **Percentage_Of_Primary_Filter_Rows_Selected_By_internal_Filter**  
 如果被主要筛选器选择了 P 行，并且被内部筛选器选择了 S 行，则会返回百分比形式的 S/P。 百分比越高，索引就能够越好地避免使用性能开销巨大的次要筛选器。 这是一个核心属性。 值为 **float。**  
  
 **Number_Of_Rows_Output**  
 查询输出的行数。 这是一个核心属性。 值为 **bigint。**  
  
 **Internal_Filter_Efficiency**  
 如果 O 是输出的行的数目，将返回百分比形式的 S/O。 这是一个核心属性。 值为 **float。**  
  
 **Primary_Filter_Efficiency**  
 如果 P 行由主筛选器选择，而 O 是输出的行数，则此 returnsO/P 为百分比。 主要筛选器的效率越高，次要筛选所必须处理的误报就越少。 这是一个核心属性。 值为 **float。**  
  
## <a name="permissions"></a>权限  
 用户必须是 **公共** 角色的成员。 需要服务器和对象的 READ ACCESS 权限。 这适用于所有空间索引存储过程。  
  
## <a name="remarks"></a>备注  
 包含 NULL 值的属性未包含在返回集中。  
  
## <a name="examples"></a>示例  
 有关示例，请参阅下列主题：  
  
-   [sp_help_spatial_geometry_index &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)  
  
-   [sp_help_spatial_geometry_index_xml &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-xml-transact-sql.md)  
  
-   [sp_help_spatial_geography_index &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-transact-sql.md)  
  
-   [sp_help_spatial_geography_index_xml &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geography-index-xml-transact-sql.md)  
  
## <a name="requirements"></a>要求  
  
## <a name="see-also"></a>另请参阅  
 [空间索引存储过程 &#40;Transact-sql&#41;]()   
 [sp_help_spatial_geometry_index &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-transact-sql.md)   
 [空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [XQuery 基础知识](../../xquery/xquery-basics.md)   
 [XQuery 语言参考 (SQL Server)](../../xquery/xquery-language-reference-sql-server.md)  
  
