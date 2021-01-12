---
description: CREATE SPATIAL INDEX (Transact-SQL)
title: CREATE SPATIAL INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SPATIAL INDEX
- CREATE SPATIAL INDEX
- CREATE_SPATIAL_INDEX_TSQL
- SPATIAL_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- spatial indexes [SQL Server], creating
- index creation [SQL Server], spatial indexes
- CREATE SPATIAL INDEX statement
- CREATE INDEX statement
ms.assetid: ee6b9116-a7ff-463a-a9f0-b360804d8678
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ce2693109669da084a67849105e859066f818213
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102367"
---
# <a name="create-spatial-index-transact-sql"></a>CREATE SPATIAL INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中指定的表和列创建空间索引。 可在向表中填入数据前创建索引。 可通过指定限定的数据库名称，针对另一个数据库中的表或视图创建索引。 空间索引要求表具有聚集主键。 不能对索引视图指定空间索引。 有关空间索引的信息，请参阅[空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE SPATIAL INDEX index_name
  ON <object> ( spatial_column_name )  
    {  
       <geometry_tessellation> | <geography_tessellation>  
    }
  [ ON { filegroup_name | "default" } ]  
[;]
  
<object> ::=  
    { database_name.schema_name.table_name | schema_name.table_name | table_name }  
  
<geometry_tessellation> ::=  
{
  <geometry_automatic_grid_tessellation>
| <geometry_manual_grid_tessellation>
}  
  
<geometry_automatic_grid_tessellation> ::=  
{  
    [ USING GEOMETRY_AUTO_GRID ]  
          WITH  (  
        <bounding_box>  
            [ [,] <tessellation_cells_per_object> [ ,...n] ]  
            [ [,] <spatial_index_option> [ ,...n] ]  
                 )  
}  
  
<geometry_manual_grid_tessellation> ::=  
{  
       [ USING GEOMETRY_GRID ]  
         WITH (  
                    <bounding_box>  
                        [ [,]<tessellation_grid> [ ,...n] ]  
                        [ [,]<tessellation_cells_per_object> [ ,...n] ]  
                        [ [,]<spatial_index_option> [ ,...n] ]  
   )  
}
  
<geography_tessellation> ::=  
{  
      <geography_automatic_grid_tessellation> | <geography_manual_grid_tessellation>  
}  
  
<geography_automatic_grid_tessellation> ::=  
{  
    [ USING GEOGRAPHY_AUTO_GRID ]  
    [ WITH (  
        [ [,] <tessellation_cells_per_object> [ ,...n] ]  
        [ [,] <spatial_index_option> ]  
     ) ]  
}  
  
<geography_manual_grid_tessellation> ::=  
{  
    [ USING GEOGRAPHY_GRID ]  
    [ WITH (  
                [ <tessellation_grid> [ ,...n] ]  
                [ [,] <tessellation_cells_per_object> [ ,...n] ]  
                [ [,] <spatial_index_option> [ ,...n] ]  
                ) ]  
}  
  
<bounding_box> ::=  
{  
      BOUNDING_BOX = ( {  
       xmin, ymin, xmax, ymax
       | <named_bb_coordinate>, <named_bb_coordinate>, <named_bb_coordinate>, <named_bb_coordinate>
  } )  
}  
  
<named_bb_coordinate> ::= { XMIN = xmin | YMIN = ymin | XMAX = xmax | YMAX=ymax }  
  
<tessellation_grid> ::=  
{
    GRIDS = ( { <grid_level> [ ,...n ] | <grid_size>, <grid_size>, <grid_size>, <grid_size>  }
        )  
}  
<tessellation_cells_per_object> ::=  
{
   CELLS_PER_OBJECT = n
}  
  
<grid_level> ::=  
{  
     LEVEL_1 = <grid_size>
  |  LEVEL_2 = <grid_size>
  |  LEVEL_3 = <grid_size>
  |  LEVEL_4 = <grid_size>
}  
  
<grid_size> ::= { LOW | MEDIUM | HIGH }  
  
<spatial_index_option> ::=  
{  
    PAD_INDEX = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | SORT_IN_TEMPDB = { ON | OFF }  
  | IGNORE_DUP_KEY = OFF  
  | STATISTICS_NORECOMPUTE = { ON | OFF }  
  | DROP_EXISTING = { ON | OFF }  
  | ONLINE = OFF  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
    | DATA_COMPRESSION = { NONE | ROW | PAGE }  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

 index_name     
 索引的名称。 索引名称在表中必须唯一，但在数据库中不必唯一。 索引名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)的规则。  
  
 ON \<object> ( *spatial_column_name* )     
 指定要对其创建索引的对象（数据库、架构或表）以及空间列的名称。  
  
 *spatial_column_name* 指定索引所基于的空间列。 在一个空间索引定义中只能指定一个空间列；但是可以针对 **geometry** 或 **geography** 列创建多个空间索引。  
  
 USING      
 指示空间索引的分割方案。 此参数会使用下表所示的类型特定的值：  
  
|列的数据类型|分割方案|  
|-------------------------|-------------------------|  
|**geometry**|GEOMETRY_GRID|  
|**geometry**|GEOMETRY_AUTO_GRID|  
|**地理**|GEOGRAPHY_GRID|  
|**地理**|GEOGRAPHY_AUTO_GRID|  
  
 只能对类型为 geometry 或 geography 的列创建空间索引，否则会抛出错误。 如果为给定类型传递的参数无效，便会抛出错误。  
  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如何实现分割的信息，请参阅[空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)。  
  
 ON filegroup_name      
 **适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 为指定文件组创建指定索引。 如果未指定位置并且表未分区，则索引将与基础表使用相同的文件组。 该文件组必须已存在。  
  
 ON "default"     
 **适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 为默认文件组创建指定索引。  
  
 在此上下文中，“default”一词不是关键字。 它是默认文件组的标识符，并且必须进行分隔（类似于 ON "default" 或 ON [default]）。 如果指定了 "default"，则当前会话的 QUOTED_IDENTIFIER 选项必须为 ON。 这是默认设置。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。  
  
 **\<object>::=**     
 要为其建立索引的完全限定对象或非完全限定对象。  
  
 database_name      
 数据库的名称。  
  
 schema_name     
 表所属架构的名称。  
  
 *table_name*      
 要索引的表的名称。  
  
[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 支持由三部分组成的名称格式 database_name.[schema_name].object_name，其中 database_name 为当前数据库，或 database_name 为 tempdb，object_name 以 # 开头。  
  
### <a name="using-options"></a>USING 选项

 GEOMETRY_GRID      
 指定要使用的 **geometry** 网格分割方案。 只能对 **geometry** 数据类型的列指定 GEOMETRY_GRID。  GEOMETRY_GRID 允许手动调整分割方案。  
  
 GEOMETRY_AUTO_GRID      
 **适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 只能对 geometry 数据类型的列指定。 这是此数据类型的默认设置，无需指定。  
  
 GEOGRAPHY_GRID      
 指定地理网格分割方案。 只能对 **geography** 数据类型的列指定 GEOGRAPHY_GRID。  
  
 GEOGRAPHY_AUTO_GRID      
 **适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 只能对 geography 数据类型的列指定。  这是此数据类型的默认设置，无需指定。  
  
### <a name="with-options"></a>WITH 选项

BOUNDING_BOX      
指定定义边界框四个坐标的一个数值四元组：左下角的最小 x 坐标和最小 y 坐标，以及右上角的最大 x 坐标和最大 y 坐标。  
  
 xmin      
 指定边界框左下角的 x 坐标。  
  
 ymin      
 指定边界框左下角的 y 坐标。  
  
 xmax     
 指定边界框右上角的 x 坐标。  
  
 ymax     
 指定边界框右上角的 y 坐标。  
  
 XMIN = xmin     
 指定边界框左下角 x 坐标的属性名称和值。  
  
 YMIN =ymin      
 指定边界框左下角 y 坐标的属性名称和值。  
  
 XMAX =xmax      
 指定边界框右上角 x 坐标的属性名称和值。  
  
 YMAX =ymax     
 指定边界框右上角 y 坐标的属性名称和值。  
  
 > [!NOTE]
 > 边界框坐标只在 USING GEOMETRY_GRID 子句内应用。  
 >
 > *xmax* 必须大于 *xmin*，*ymax* 必须大于 *ymin*。 你可以指定任何有效的 [浮点](../../t-sql/data-types/float-and-real-transact-sql.md)值表示形式，前提是：*xmax* > *xmin* 且 *ymax* > *ymin*。 否则，将引发相应错误。  
 >
 > 这里没有默认值。  
 >
 > 边界框属性名称不区分大小写，而不管数据库排序规则如何。  
  
 若要指定属性名称，必须且只能指定每个属性名称一次。 可以按任意顺序指定它们。 例如，以下子句是等效的：  
  
- BOUNDING_BOX =( XMIN =*xmin*, YMIN =*ymin*, XMAX =*xmax*, YMAX =*ymax* )  
  
- BOUNDING_BOX =( XMIN =*xmin*, XMAX =*xmax*, YMIN =*ymin*, YMAX =*ymax*)  
  
GRIDS      
定义分割方案中每一级别的网格密度。 当选择 GEOMETRY_AUTO_GRID 和 GEOGRAPHY_AUTO_GRID 时，此选项被禁用。  
  
 有关分割的信息，请参阅[空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)。  
  
 GRIDS 参数如下：  
  
 LEVEL_1     
 指定第一级（顶级）网格。  
  
 LEVEL_2    
 指定第二级网格。  
  
 LEVEL_3    
 指定第三级网格。  
  
 LEVEL_4    
 指定第四级网格。  
  
 LOW    
 为给定级别的网格指定可能的最低网格密度。 LOW 等于 16 个单元（4x4 网格）。  
  
 MEDIUM     
 为给定级别的网格指定中等网格密度。 MEDIUM 等于 64 个单元（8x8 网格）。  
  
 HIGH     
 为给定级别的网格指定可能的最高网格密度。 HIGH 等于 256 个单元（16x16 网格）。  
  
> [!NOTE]
> 使用级别名称可以按任意顺序指定级别，并且可以省略级别。 如果为级别使用名称，则必须为所指定的任何其他级别也使用名称。 如果省略某个级别，则其密度将默认为 MEDIUM。  

> [!WARNING]
> 如果指定的密度无效，则会引发错误。  
  
CELLS_PER_OBJECT =n     
指定可由分割进程用于在索引中单个空间对象的每个对象的分割单元格数。 n 可以是介于 1 和 8192 之间（含 1 和 8192）的任何整数。 如果传递的数字无效或者该数字大于指定分割的最大单元格数，则会引发错误。  
  
 CELLS_PER_OBJECT 的默认值如下：  
  
|USING 选项|每个对象的默认单元格数|  
|------------------|------------------------------|  
|GEOMETRY_GRID|**16**|  
|GEOMETRY_AUTO_GRID|**8**|  
|GEOGRAPHY_GRID|**16**|  
|GEOGRAPHY_AUTO_GRID|**12**|  
  
在顶级，如果对象包含的单元格多于 *n* 指定的单元格，则索引操作将根据需要使用尽可能多的单元格来提供完整的顶级分割。 在这种情况下，对象收到的单元格数可能会大于指定的单元格数。 此时，最大数即为顶级网格生成的单元格数，具体取决于密度。  
  
“每个对象的单元格数”分割规则使用 CELLS_PER_OBJECT 值。 有关分割规则的信息，请参阅[空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)。  
  
PAD_INDEX = { ON | OFF }     
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
指定索引填充。 默认为 OFF。  
  
ON     
指示由 *fillfactor* 指定的可用空间百分比应用于索引的中间级页。  
  
OFF 或未指定 fillfactor     
指示考虑到中间级页上的键集，将中间级页填充到接近其容量的程度，以留出足够的空间，使之至少能够容纳索引的最大的一行。  
  
PAD_INDEX 选项只有在指定了 FILLFACTOR 时才有用，因为 PAD_INDEX 使用由 FILLFACTOR 指定的百分比。 如果为 FILLFACTOR 指定的百分比不够大，无法容纳一行，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将在内部覆盖该百分比以允许最小值。 无论 fillfactor 的值有多小，中间级索引页上的行数永远都不会小于两行。  
  
FILLFACTOR =fillfactor     
 **适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 指定一个百分比，指示在[!INCLUDE[ssDE](../../includes/ssde-md.md)]创建或重新生成索引的过程中，应将每个索引页面的叶级填充到什么程度。 fillfactor 必须是 1 到 100 之间的整数。 默认值为 0。 如果 fillfactor 为 100 或 0，[!INCLUDE[ssDE](../../includes/ssde-md.md)]会创建完全填充叶级页的索引。  
  
> [!NOTE]  
> 填充因子的值 0 和 100 在所有方面都是相同的。
  
 FILLFACTOR 设置仅在创建或重新生成索引时应用。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]并不会在页中动态保持指定的可用空间百分比。 若要查看填充因子设置，请使用 [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) 目录视图。  
  
> [!IMPORTANT]  
> 使用低于 100 的 FILLFACTOR 值创建聚集索引会影响数据占用的存储空间量，因为[!INCLUDE[ssDE](../../includes/ssde-md.md)]在创建聚集索引时会重新分布数据。  
  
 有关详细信息，请参阅 [为索引指定填充因子](../../relational-databases/indexes/specify-fill-factor-for-an-index.md)。  
  
SORT_IN_TEMPDB = { ON | OFF }       
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 指定是否在 tempdb 中存储临时排序结果。 默认为 OFF。  
  
 ON     
 在 tempdb 中存储用于生成索引的中间排序结果。 如果 tempdb 与用户数据库不在同一组磁盘上，就可缩短创建索引所需的时间。 但是，这会增加索引生成期间所使用的磁盘空间量。  
  
 OFF     
 中间排序结果与索引存储在同一数据库中。  
  
 除在用户数据库中创建索引所需的空间外，tempdb 还必须有大约相同的额外空间来存储中间排序结果。 有关详细信息，请参阅[用于索引的 SORT_IN_TEMPDB 选项](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md)。  
  
IGNORE_DUP_KEY =OFF     
对空间索引不起作用，这是因为此索引类型永远不唯一。 请不要将此选项设置为 ON，否则会引发错误。  
  
STATISTICS_NORECOMPUTE = { ON | OFF}     
指定是否重新计算分布统计信息。 默认为 OFF。  
  
 ON    
 不会自动重新计算过时的统计信息。  
  
 OFF    
 启用统计信息自动更新功能。  
  
 若要恢复统计信息自动更新，请将 STATISTICS_NORECOMPUTE 设置为 OFF，或执行 UPDATE STATISTICS 但不包含 NORECOMPUTE 子句。  
  
> [!IMPORTANT]  
> 如果禁用分布统计的自动重新计算，可能会妨碍查询优化器为涉及该表的查询选取最佳执行计划。  
  
DROP_EXISTING = { ON | OFF }     
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 指定应删除并重新生成已命名的先前存在的空间索引。 默认为 OFF。  
  
 ON     
 删除并重新生成现有索引。 指定的索引名称必须与当前的现有索引相同；但可以修改索引定义。 例如，可以指定不同的列、排序顺序、分区方案或索引选项。  
  
 OFF     
 如果指定的索引名称已存在，则会显示一条错误。  
  
 使用 DROP_EXISTING 不能更改索引类型。  
  
ONLINE =OFF      
指定在索引操作期间基础表和关联的索引不可用于查询和数据修改操作。 在此版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，空间索引不支持联机索引生成操作。 如果针对某个空间索引将此选项设置为 ON，则会引发错误。 请省略 ONLINE 选项或将 ONLINE 设为 OFF。  
  
 创建、重新生成或删除空间索引的脱机索引操作将获取表的架构修改 (Sch-M) 锁。 这样可以防止所有用户在操作期间访问基础表。  
  
> [!NOTE]  
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的各版本中均不提供联机索引操作。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
ALLOW_ROW_LOCKS = { ON | OFF }     
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 指定是否允许行锁。 默认值为 ON。  
  
 ON     
 在访问索引时允许使用行锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]确定何时使用行锁。  
  
 OFF     
 不使用行锁。  
  
ALLOW_PAGE_LOCKS = { ON | OFF }     
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 指定是否允许使用页锁。 默认值为 ON。  
  
 ON    
 在访问索引时允许使用页锁。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]确定何时使用页锁。  
  
 OFF     
 不使用页锁。  
  
MAXDOP =max_degree_of_parallelism      
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 只在索引操作期间覆盖 `max degree of parallelism` 配置选项。 使用 MAXDOP 可以限制在执行并行计划的过程中使用的处理器数量。 最大数量为 64 个处理器。  
  
> [!IMPORTANT]  
> 虽然从语法上讲支持 MAXDOP 选项，但当前 CREATE SPATIAL INDEX 始终只使用一个处理器。  
  
 max_degree_of_parallelism 可以是：  
  
 1     
 取消生成并行计划。  
  
 \>1    
 基于当前系统工作负荷，将并行索引操作中使用的最大处理器数限制为指定数量或更少。  
  
 0（默认值）    
 根据当前系统工作负荷使用实际的处理器数量或更少数量的处理器。  
  
 有关详细信息，请参阅 [配置并行索引操作](../../relational-databases/indexes/configure-parallel-index-operations.md)。  
  
> [!NOTE]
> 并非在 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的每个版本中均支持并行索引操作。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
DATA_COMPRESSION = {NONE | ROW | PAGE}     
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 确定索引所使用的数据压缩级别。  
  
 无     
 对索引所使用的数据不使用压缩  
  
 ROW    
 对索引所使用的数据使用行压缩  
  
 PAGE    
 对索引所使用的数据使用页压缩  
  
## <a name="remarks"></a>备注
每个选项在每个 CREATE SPATIAL INDEX 语句中只能指定一次。 重复指定任何选项都将引发错误。  
  
对于表中的每个空间列，最多可以创建 249 个空间索引。 对特定空间列创建多个空间索引可能很有用，例如，在要对单个列中的不同分割参数进行索引时。  
  
> [!IMPORTANT]  
> 创建空间索引还有许多其他限制。 有关详细信息，请参阅[空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)。  
  
索引的生成过程无法利用可用的进程并行机制。  
  
## <a name="methods-supported-on-spatial-indexes"></a>空间索引支持的方法
在某些条件下，空间索引支持多种面向集合的几何图形方法。 有关详细信息，请参阅[空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)。  
  
## <a name="spatial-indexes-and-partitioning"></a>空间索引和分区
默认情况下，如果对已分区表创建了空间索引，则索引将根据该表的分区方案进行分区。 这样可以确保索引数据和相关行存储在相同的分区中。  
  
在这种情况下，若要更改基表的分区方案，必须先删除空间索引，然后才能对基表进行重新分区。 若要避免这种限制，可以在创建空间索引时指定“ON filegroup”选项。 有关详细信息，请参阅本主题后面的“空间索引和文件组”。  
  
## <a name="spatial-indexes-and-filegroups"></a>空间索引和文件组
默认情况下，空间索引与为其指定了该索引的表分区到同一文件组。 可以通过指定文件组覆盖此默认设置：  
  
[ ON { *filegroup_name* | "default" } ]     
如果为某个空间索引指定了文件组，则该索引将置于此文件组上，而不管表的分区方案如何。  
  
## <a name="catalog-views-for-spatial-indexes"></a>空间索引的目录视图

 空间索引具有以下特定目录视图：  
  
 [sys.spatial_indexes](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)  
 表示空间索引的主索引信息。  
  
 [sys.spatial_index_tessellations](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md)  
 表示每个空间索引的分割方案和参数的相关信息。  
  
## <a name="additional-remarks-about-creating-indexes"></a>关于创建索引的其他备注

 有关创建索引的详细信息，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md) 中的“备注”部分。  
  
## <a name="permissions"></a>权限
用户必须具有对表或视图的 `ALTER` 权限，或是 sysadmin 固定服务器角色或 `db_ddladmin` 和 `db_owner` 固定数据库角色的成员。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-spatial-index-on-a-geometry-column"></a>A. 对 geometry 列创建空间索引
以下示例创建一个名为 `SpatialTable` 的表，表中包含一个 **geometry** 类型的列 `geometry_col`。 然后，该示例将对 `SIndx_SpatialTable_geometry_col1` 创建一个空间索引 `geometry_col`。 该示例使用默认分割方案并指定边界框。  
  
```sql  
CREATE TABLE SpatialTable(id int primary key, geometry_col geometry);  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col1
   ON SpatialTable(geometry_col)  
   WITH ( BOUNDING_BOX = ( 0, 0, 500, 200 ) );  
```  
  
### <a name="b-creating-a-spatial-index-on-a-geometry-column"></a>B. 对 geometry 列创建空间索引
下例对 `SIndx_SpatialTable_geometry_col2` 表中的 `geometry_col` 列创建第二个空间索引 `SpatialTable`。 该示例指定 `GEOMETRY_GRID` 作为分割方案。 它还指定了边界框和不同网格级别上的不同密度，并指定每个对象有 64 个单元格。 该示例还将索引填充设为 `ON`。  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col2  
   ON SpatialTable(geometry_col)  
   USING GEOMETRY_GRID  
   WITH (  
    BOUNDING_BOX = ( xmin=0, ymin=0, xmax=500, ymax=200 ),  
    GRIDS = (LOW, LOW, MEDIUM, HIGH),  
    CELLS_PER_OBJECT = 64,  
    PAD_INDEX  = ON );  
```  
  
### <a name="c-creating-a-spatial-index-on-a-geometry-column"></a>C. 对 geometry 列创建空间索引
以下示例对 `SpatialTable` 表中的 `geometry_col` 列创建第三个空间索引 `SIndx_SpatialTable_geometry_col3`。 该示例使用默认分割方案。 该示例指定了边界框，并为第三和第四级网格使用不同的单元格密度，同时指定每个对象采用默认单元格数。  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col3  
   ON SpatialTable(geometry_col)  
   WITH (  
    BOUNDING_BOX = ( 0, 0, 500, 200 ),  
    GRIDS = ( LEVEL_4 = HIGH, LEVEL_3 = MEDIUM ) );  
```  
  
### <a name="d-changing-an-option-that-is-specific-to-spatial-indexes"></a>D. 更改特定于空间索引的选项
下面的示例通过指定新的 `SIndx_SpatialTable_geography_col3` 密度并指定 DROP_EXISTING = ON，来重新生成在上例中创建的空间索引 `LEVEL_3`。  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col3  
   ON SpatialTable(geography_col)  
   WITH ( BOUNDING_BOX = ( 0, 0, 500, 200 ),  
        GRIDS = ( LEVEL_3 = LOW ),  
        DROP_EXISTING = ON );  
```  
  
### <a name="e-creating-a-spatial-index-on-a-geography-column"></a>E. 对 geography 列创建空间索引
以下示例创建一个名为 `SpatialTable2` 的表，表中包含一个 **geography** 类型的列 `geography_col`。 然后，该示例将对 `SIndx_SpatialTable_geography_col1` 创建一个空间索引 `geography_col`。 该示例使用 GEOGRAPHY_AUTO_GRID 分割方案的默认参数值。  
  
```sql  
CREATE TABLE SpatialTable2(id int primary key, object GEOGRAPHY);  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col1
   ON SpatialTable2(object);  
```  
  
> [!NOTE]  
> 对于地理网格索引，不能指定边界框。
  
### <a name="f-creating-a-spatial-index-on-a-geography-column"></a>F. 对 geography 列创建空间索引
下例对 `SIndx_SpatialTable_geography_col2` 表中的 `geography_col` 列创建第二个空间索引 `SpatialTable2`。 该示例指定 `GEOGRAPHY_GRID` 作为分割方案。 它还为不同网格级别指定了不同的网格密度，并指定每个对象有 64 个单元格。 该示例还将索引填充设为 `ON`。  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col2  
   ON SpatialTable2(object)  
   USING GEOGRAPHY_GRID  
   WITH (  
    GRIDS = (MEDIUM, LOW, MEDIUM, HIGH ),  
    CELLS_PER_OBJECT = 64,  
    PAD_INDEX  = ON );  
```  
  
### <a name="g-creating-a-spatial-index-on-a-geography-column"></a>G. 对 geography 列创建空间索引
下例对 `SIndx_SpatialTable_geography_col3` 表中的 `geography_col` 列创建第三个空间索引 `SpatialTable2`。 该示例使用默认分割方案 GEOGRAPHY_GRID 和默认 CELLS_PER_OBJECT 值 (16)。  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col3  
   ON SpatialTable2(object)  
   WITH ( GRIDS = ( LEVEL_3 = HIGH, LEVEL_2 = HIGH ) );  
```  
  
## <a name="see-also"></a>另请参阅
[ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)       
[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)       
[CREATE PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/create-partition-function-transact-sql.md)       
[CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md)       
[CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)       
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)       
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)       
[DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)       
[DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)       
[EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)       
[sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)       
[sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)       
[sys.spatial_index_tessellations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md)       
[sys.spatial_indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)       
[空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)       
