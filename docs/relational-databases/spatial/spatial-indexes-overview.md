---
description: 空间索引概述
title: 空间索引概述 | Microsoft Docs
ms.date: 09/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- spatial indexes [SQL Server]
ms.assetid: b1ae7b78-182a-459e-ab28-f743e43f8293
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bedf575888445fb0e14684d4e325b60eb8547a79
ms.sourcegitcommit: 7791bd2ba339edc5cd2078a6537c8f6bfe72a19b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2021
ms.locfileid: "98564458"
---
# <a name="spatial-indexes-overview"></a>空间索引概述
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持空间数据和空间索引。 “空间索引”  是一种扩展索引，允许您对空间列编制索引。 空间列是包含空间数据类型（如 **geometry** 或 **geography**）的数据的表列。  
  
> [!IMPORTANT]  
>   有关 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]中引入的空间功能的详细说明和示例（包括影响空间索引的功能），请下载白皮书 [SQL Server 2012 中的新空间功能](https://go.microsoft.com/fwlink/?LinkId=226407)。  
  
##  <a name="about-spatial-indexes"></a><a name="about"></a> 关于空间索引  
  
###  <a name="decomposing-indexed-space-into-a-grid-hierarchy"></a><a name="decompose"></a> 将索引空间分解成网格层次结构  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，空间索引使用 B 树构建而成，也就是说，这些索引必须按 B 树的线性顺序表示二维空间数据。 因此，将数据读入空间索引之前， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 先实现对空间的分层均匀分解。 索引创建过程会将空间分解成一个四级网格层次结构。 这些级别指的是第 1 级（顶级）、第 2 级、第 3 级和第 4 级。  
  
 每个后续级别都会进一步分解其上一级，因此上一级别的每个单元都包含下一级别的整个网格。 在给定级别上，所有网格沿两个轴都有相同数目的单元（例如 4x4 或 8x8），并且单元的大小都相同。  
  
 下图显示了网格层次结构每个级别的右上角单元被分解成 4x4 网格的情况。 事实上，所有单元都是以这种方式分解的。 因此，以此为例，将一个空间分解成四个级别的 4x4 网格实际上会总共产生 65,536 个第四级单元。  
  
 ![递归分割的四个级别](../../relational-databases/spatial/media/spndx-recursive-levels-telescoped.gif "递归分割的四个级别")  
  
> [!NOTE]  
>  针对空间索引进行的空间分解与应用程序数据使用的度量单位无关。  
  
 网格层次结构的单元是利用多种 Hilbert 空间填充曲线以线性方式编号的。 然而，出于演示目的，这里使用的是简单的按行编号，而不是由 Hilbert 曲线实际产生的编号。 在下图中，几个表示建筑物的多边形和表示街道的线已经放进了一个 4x4 的 1 级网格中。 第 1 级单元的编号为 1 到 16，编号从左上角的单元开始。  
  
 ![放置在 4x4 第 1 级网格上的多边形和直线](../../relational-databases/spatial/media/spndx-level-1-objects.gif "放置在 4x4 第 1 级网格上的多边形和直线")  
  
#### <a name="grid-density"></a>网格密度  
 沿网格轴的单元数目确定了网格的 *“密度”*：单元数目越大，网格的密度越大。 例如，8x8 网格（产生 64 个单元）的密度就大于 4x4 网格（产生 16 个单元）的密度。 网格密度是以每个级别为基础定义的。  
  
 空间索引的 [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句支持 GRIDS 子句，使用该子句可以在不同级别指定不同的网格密度。 可以使用下列关键字之一指定给定级别的网格密度。  
  
|关键字|网格配置|单元数目|  
|-------------|------------------------|---------------------|  
|LOW|4X4|16|  
|MEDIUM|8X8|64|  
|HIGH|16X16|256|  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，当数据库兼容级别设置为 100 或更低时，在所有级别上默认值为 MEDIUM。 当数据库兼容级别设置为 110 或更高时，默认值为自动网格方案。 （自动网格指示 8 级别配置 HLLLLLLL。）可以通过提示改变每个对象的单元格数和每个对象的查询窗口单元格数，而不是改变索引网格密度。 
  
 您可以通过指定非默认的网格密度控制分解过程。 例如，在不同级别指定不同网格密度对于基于索引空间的大小和空间列中的对象来优化索引可能非常有用。  
  
> [!NOTE]  
>  当数据库兼容级别设置为 100 或更低时，空间索引的网格密度显示在 [sys.spatial_index_tessellations](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md) 目录视图的 level_1_grid、level_2_grid、level_3_grid 和 level_4_grid 列中。 **GEOMETRY_AUTO_GRID**/**GEOGRAPHY_AUTO_GRID** 分割方案选项不会填充这些列。 使用自动网格选项时，sys.spatial_index_tessellations 目录视图对这些列使用 **NULL** 值。  
  
###  <a name="tessellation"></a><a name="tessellation"></a> 分割  
 将索引空间分解成网格层次结构后，空间索引将逐行读取空间列中的数据。 读取空间对象（或实例）的数据后，空间索引将为该对象执行 分割过程。 分割过程通过将对象与其接触的网格单元集（“接触单元”）相关联使该对象适合网格层次结构。 从网格层次结构的第 1 级开始，分割过程以“广度优先”  方式对整个级别进行处理。 在可能的情况下，此过程可以连续处理所有四个级别，一次处理一个级别。  
  
 分割过程的输出为对象的空间索引中所记录的接触单元集。 通过引用这些已记录单元，空间索引可以确定该对象在空间中相对于空间列中也存储在索引中的其他对象的位置。  
  
#### <a name="tessellation-rules"></a>分割规则  
 为了限制为对象记录的接触单元数，分割过程采用了几个分割规则。 这些规则确定分割过程的深度以及在索引中记录哪些接触单元。  
  
 这些规则如下：  
  
-   覆盖规则  
  
     如果一个对象完全盖住了某个单元，则称该单元由该对象所“覆盖”  。 被覆盖的单元会参与计数，但不进行分割。 此规则应用于网格层次结构的所有级别。 覆盖规则简化了分割过程，并减少了空间索引记录的数据量。  
  
-   每对象单元数规则  
  
     此规则强制执行每个对象的单元数限制，该限制确定每个对象可以具有的最大单元数（级别 1 例外）。 在较低级别上，每个对象的单元数规则控制可以记录有关该对象的信息量。  
  
-   最深单元规则  
  
     最深单元规则通过只记录已为对象分割的最底部单元来生成该对象的最近似对象。 父单元不计入每对象单元数，这些单元不记录在索引中。  
  
 这些分割规则依次逐步应用于每个网格级别。 此部分的其余内容更详细地介绍了这些分割规则。  
  
#### <a name="covering-rule"></a>覆盖规则  
 如果一个对象完全盖住了某个单元，则称该单元由该对象所“覆盖”  。 例如，在下图中，一个第 2 级单元 15.11 完全由八边形的中间部分所覆盖。  
  
 ![表面优化](../../relational-databases/spatial/media/spndx-opt-covering.gif "表面优化")  
  
 被覆盖的单元会参与计数并记录在索引中，但不再进行分割。  
  
#### <a name="cells-per-object-rule"></a>每对象单元数规则  
 每个对象的分割程度主要取决于空间索引的每对象单元数限制。 此限制确定了对于每个对象分割可以计数的最大单元数。 然而，请注意，每对象单元数规则不对第 1 级强制执行，因此可能超出此限制。 如果第 1 级计数达到（或超出）每对象单元数限制，则在较低级别不再进行分割。  
  
 只要计数低于每对象单元数限制，分割过程就将继续。 从编号最低的接触单元（例如上图中的单元 15.6）开始，此过程将测试每个单元以评估是对其进行计数还是进行分割。 如果分割某单元将超出每对象单元数限制，将对该单元进行计数而不进行分割。 否则，将对该单元进行分割，而对由对象接触的较低级别的单元进行计数。 分割过程将以这种方式在整个级别的广度范围内继续进行。 此过程对低级别网格的分割单元依次逐步进行重复，直至达到限制或不再有要计数的单元为止。  
  
 例如，上图显示了一个完全适合第 1 级网格的单元 15 的八边形。 在此图中，单元 15 已进行分割，将八边形分成了九个二级单元。 此图假定每对象单元数限制为 9 或更大。 然而，如果每对象单元数限制为 8 或更小，则单元 15 将不进行分割，而只为该对象对单元 15 进行计数。  
  
 默认情况下，每对象单元数限制为每个对象 16 个单元，这将在大多数空间索引的空间和精度之间提供一个令人满意的折中方案。 然而，[CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句支持 CELLS_PER_OBJECT = n 子句，使用该子句可以指定介于 1 和 8192（包含这两者）之间的每对象单元数限制。  
  
> [!NOTE]  
>  空间索引的 **cells_per_object** 设置显示在 [sys.spatial_index_tessellations](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md) 目录视图中。  
  
#### <a name="deepest-cell-rule"></a>最深单元规则  
 最深单元规则利用每个较低级别单元属于其上级单元这一事实：第 4 级单元属于第 3 级单元，第 3 级单元属于第 2 级单元，第 2 级单元属于第 1 级单元。 例如，属于单元 1.1.1.1 的对象也属于单元 1.1.1、1.1 和 1。 这种单元层次结构关系的知识内置到查询处理器。 因此，只有最深级别的单元需要记录在索引中，从而最大限度地减少了索引需要存储的信息。  
  
 在下图中，相对较小的菱形多边形被分割。 索引使用默认的每对象单元数限制 16，此对象较小，未达到该限制。 因此，分割一直下至第 4 级。 此多边形驻留在以下的第 1 级到第 3 级的单元中：4、4.4 以及 4.4.10 和 4.4.14。 然而，使用最深单元规则，分割将仅对十二个位于第 4 级的单元进行计数：4.4.10.13-15 以及 4.4.14.1-3、4.4.14.5-7 和 4.4.14.9-11。  
  
 ![最深单元优化](../../relational-databases/spatial/media/spndx-opt-deepest-cell.gif "最深单元优化")  
  
###  <a name="tessellation-schemes"></a><a name="schemes"></a> 分割方案  
 空间索引的行为部分取决于“分割方案” 。 分割方案特定于数据类型。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，空间索引支持两种分割方案：  
  
-   “几何图形网格分割”，这是适用于 **geometry** 数据类型的方案。  
  
-   “地理网格分割”，该方案适用于数据类型为 **geography** 的列。  
  
> [!NOTE]  
>  空间索引的 **tessellation_scheme** 设置显示在 [sys.spatial_index_tessellations](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md) 目录视图中。  
  
#### <a name="geometry-grid-tessellation-scheme"></a>几何图形网格分割方案  
 GEOMETRY_AUTO_GRID 分割是 **和更高版本的** geometry [!INCLUDE[ssNoVersion](../../includes/sssql11-md.md)] 数据类型的默认分割方案。  GEOMETRY_GRID 分割是 [!INCLUDE[ssNoVersion](../../includes/sskatmai-md.md)]中 geometry 数据类型的唯一可用分割方案。 本节讨论了与使用空间索引有关的几何图形网格分割的几个方面：支持的方法和边界框。  
  
> [!NOTE]  
>  可以使用 [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的 USING (GEOMETRY_AUTO_GRID/GEOMETRY_GRID) 子句显式指定此分割方案。  
  
##### <a name="the-bounding-box"></a>边界框  
 几何图形数据占有的平面可以是无限的。 然而，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，空间索引需要有限空间。 为了建立有限空间以用于分解，几何图形网格分割方案需要矩形“边界框” 。 该边界框由四个坐标 **(** _x-min_ **，** _y-min_ **)** 和 **(** _x-max_ **，** _y-max_ **)** 定义，这些坐标存储为空间索引的属性。 这些坐标所表示的意义如下：  
  
-   x-min 是边界框左下角的 x 坐标。  
  
-   y-min 是左下角的 y 坐标。  
  
-   x-max 是右上角的 x 坐标。  
  
-   y-max 是右上角的 y 坐标。  
  
> [!NOTE]  
>  这些坐标通过 [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的 BOUNDING_BOX 子句指定。  
  
 **(**_x-min_**，**_y-min_**)** 和 **(**_x-max_**，**_y-max_**)** 坐标确定边界框的位置和尺寸。 边界框的外部空间视作一个编号为 0 的单元。  
  
 空间索引将分解边界框的内部空间。 网格层次结构的第 1 级网格将填充边界框。 若要在网格层次结构中放置几何对象，空间索引会将该对象的坐标与边界框的坐标进行比较。  
  
 下图显示了由边界框的 **(**_x-min_**，**_y-min_**)** 和 **(**_x-max_**，**_y-max_**)** 坐标定义的点。 网格层次结构的顶级显示为 4x4 网格。 出于演示的目的，这里省略了较低级别。 边界框的外部空间用零 (0) 指示。 请注意，对象“A”部分超出了边界框，对象“B”完全位于边界框外部，即单元 0 中。  
  
 ![显示坐标和单元 0 的边界框。](../../relational-databases/spatial/media/spndx-bb-4x4-objects.gif "显示坐标和单元 0 的边界框。")  
  
 边界框与应用程序空间数据的某些部分相对应。 索引的边界框是完全包含存储在空间列中的数据还是只包含其中部分数据取决于应用程序。 只有针对完全位于边界框内部的对象的计算操作才会受益于空间索引。 因此，若要获得 **geometry** 列的空间索引所能提供的最大优势，需要指定一个包含所有或大多数对象的边界框。  
  
> [!NOTE]  
>  空间索引的网格密度显示在 [sys.spatial_index_tessellations](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md) 目录视图的 bounding_box_xmin、bounding_box_ymin、bounding_box_xmax 和 bounding_box_ymax 列中。  
  
#### <a name="the-geography-grid-tessellation-scheme"></a>地理网格分割方案  
 此分割方案仅适用于 **geography** 列。 此部分总结了地理网格分割支持的方法，并讨论了如何将测量空间投影到平面上，该平面随后将分解成网格层次结构。  
  
> [!NOTE]  
>  你可以使用 [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的 USING (GEOGRAPHY_AUTO_GRID/GEOGRAPHY_GRID) 子句显式指定此分割方案。  
  
##### <a name="projection-of-the-geodetic-space-onto-a-plane"></a>将测量空间投影到平面上  
 对 **geography** 实例（对象）的计算将包含对象的空间视为测量椭圆体。 若要分解此空间，地理网格分割方案将椭圆体表面分为上半球和下半球，然后执行下列步骤：  
  
1.  将每个半球投影在四边形棱锥图面上。  
  
2.  将两个棱锥图平展开。  
  
3.  联接平展的棱锥图以形成非欧几里得平面。  

 下图显示了此三步分解过程的示意图。 在棱锥图中，虚线表示每个棱锥图的四个面的边界。 步骤 1 和 2 显示测量椭圆体，使用一条绿色水平线表示赤道纬线，使用一系列绿色垂直线表示若干条经线。 步骤 1 显示要投影在两个半球上的棱锥图。 步骤 2 显示要平展的棱锥图。 步骤 3 显示平展的棱锥图，这些棱锥图已组合起来形成一个平面，显示出许多投影的经线。 请注意，这些投影线伸直后长度不一，具体取决于它们落在棱锥图上的位置。  
  
 ![椭圆体在平面上的投影](../../relational-databases/spatial/media/spndx-geodetic-projection.gif "椭圆体在平面上的投影")  
  
 空间投影到平面上之后，此平面将会分解成四级网格层次结构。 不同级别可以使用不同的网格密度。 下图显示了已分解成一个 4x4 的 1 级网格后的平面。 出于演示目的，这里省略了网格层次结构的较低级别。 事实上，此平面完全分解成了一个四级网格层次结构。 分解过程完成后，将逐行从 geography 列读取地理数据，并为每个对象依次执行分割过程。  
  
 ![第 1 级地理网格](../../relational-databases/spatial/media/spndx-geodetic-level1grid.gif "第 1 级地理网格")  
  
##  <a name="methods-supported-by-spatial-indexes"></a><a name="methods"></a> 空间索引支持的方法  
  
###  <a name="geometry-methods-supported-by-spatial-indexes"></a><a name="geometry"></a> 空间索引支持的几何图形方法  
 空间索引在某些情况下支持以下面向集合的 geometry 方法：STContains()、STDistance()、STEquals()、STIntersects()、STOverlaps()、STTouches() 和 STWithin()。 若要使空间索引支持这些方法，必须在查询的 WHERE 或 JOIN ON 子句中使用这些方法，并且必须在采用如下常规形式的谓词中执行这些方法：  
  
 *geometry1*.*method_name*(*geometry2*)*comparison_operator**valid_number*  
  
 若要返回非 NULL 结果， *geometry1* 和 *geometry2* 必须具有相同的 [空间引用标识符 (SRID)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md)。 否则，该方法将返回 NULL。  
  
 空间索引支持以下谓词形式：  
  
-   *geometry1*.[STContains](../../t-sql/spatial-geometry/stcontains-geometry-data-type.md)(*geometry2*) = 1  
  
-   *geometry1*.[STDistance](../../t-sql/spatial-geometry/stdistance-geometry-data-type.md)(*geometry2*) < *number*  
  
-   *geometry1*.[STDistance](../../t-sql/spatial-geometry/stdistance-geometry-data-type.md)(*geometry2*) <= *number*  
  
-   *geometry1*.[STEquals](../../t-sql/spatial-geometry/stequals-geometry-data-type.md)(*geometry2*)= 1  
  
-   *geometry1*）的数据的表列。[STIntersects](../../t-sql/spatial-geometry/stintersects-geometry-data-type.md)(*geometry2*)= 1  
  
-   *geometry1.* [STOverlaps (geometry2) = 1](../../t-sql/spatial-geometry/stoverlaps-geometry-data-type.md)   
  
-   *geometry1*[STTouches](../../t-sql/spatial-geometry/sttouches-geometry-data-type.md)(*geometry2*) = 1  
  
-   *geometry1*[STWithin](../../t-sql/spatial-geometry/stwithin-geometry-data-type.md)(*geometry2*) = 1  
  
###  <a name="geography-methods-supported-by-spatial-indexes"></a><a name="geography"></a> 空间索引支持的地域方法  
 在某些条件下，空间索引支持以下面向集合的地理方法：STIntersects()、STEquals() 和 STDistance()。 若要使空间索引支持这些方法，必须在查询的 WHERE 子句中使用这些方法，并且必须在采用如下常规形式的谓词中执行这些方法。  
  
 *geography1*.*method_name*(*geography2*)*comparison_operator**valid_number*  
  
 若要返回非 NULL 结果， *geography1* 和 *geography2* 必须具有相同的 [空间引用标识符 (SRID)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md)。 否则，该方法将返回 NULL。  
  
 空间索引支持以下谓词形式：  
  
-   *geography1*.[STIntersects](../../t-sql/spatial-geography/stintersects-geography-data-type.md)(*geography2*) = 1  
  
-   *geography1*.[STEquals](../../t-sql/spatial-geography/stequals-geography-data-type.md)(*geography2*) = 1  
  
-   *geography1*.[STDistance](../../t-sql/spatial-geography/stdistance-geography-data-type.md)(*geography2*) < *number*  
  
-   *geography1*.[STDistance](../../t-sql/spatial-geography/stdistance-geography-data-type.md)(*geography2*) <= *number*  
  
### <a name="queries-that-use-spatial-indexes"></a>使用空间索引的查询  
 仅 **WHERE** 子句中包含索引空间运算符的查询支持空间索引。 示例语法如下：  
  
```  
[spatial object].SpatialMethod([reference spatial object]) [ = | < ] [const literal or variable]  
```  
  
 查询优化器可理解空间操作的交换性（即 `@a.STIntersects(@b) = @b.STInterestcs(@a)` ）。 但是，如果比较的开头不包含空间运算符，将不会使用空间索引（例如， `WHERE 1 = spatial op` 将不会使用空间索引）。 要使用空间索引，请重写比较（例如 `WHERE spatial op = 1`）。  
  
 与任何其他索引一样，在支持使用空间索引时，系统将根据开销选择是否使用空间索引，因此，即使使用空间索引的所有要求都得到满足，查询优化器也可能不选用空间索引。 可使用显示计划查看是否使用了空间索引，在必要时，可提供查询提示以强制使用所需的查询计划。  
  
 邻近点查询类型也支持空间索引，但必须要编写特定的查询语法。 正确的语法为：  
  
```  
SELECT TOP(K) [WITH TIES] *   
FROM <Table> AS T [WITH(INDEX(<SpatialIndex>))]  
WHERE <SpatialColumn>.STDistance(@reference_object) IS NOT NULL  
ORDER BY <SpatialColumn>.STDistance(@reference_object) [;]  
```  
  
## <a name="see-also"></a>另请参阅  
 [空间数据 (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
