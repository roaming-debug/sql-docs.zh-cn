---
description: CurvePolygon
title: CurvePolygon | Microsoft Docs
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: e000a1d8-a049-4542-bfeb-943fd6ab3969
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 24be57e6e964fea4ddb0f050480dbe79353ddcd2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97459983"
---
# <a name="curvepolygon"></a>CurvePolygon
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  **CurvePolygon** 是由一个外部边界环以及零个或多个内环界定的在拓扑结构上闭合的图面。  
  
> [!IMPORTANT]  
> 有关 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]中引入的空间功能的详细说明和示例（包括 **CurvePolygon** 子类型），请下载白皮书 [SQL Server 2012 中的新空间功能](https://go.microsoft.com/fwlink/?LinkId=226407)。  
  
 下面的条件定义一个 **CurvePolygon** 实例的属性：  
  
-   该 **CurvePolygon** 实例的边界由外环和所有内环界定。  
  
-   该 **CurvePolygon** 实例的内部是外环和所有内环之间的空间。  
  
 **CurvePolygon** 实例不同于 **Polygon** 实例，因为 **CurvePolygon** 实例可以包含以下圆弧线段： **CircularString** 和 **CompoundCurve**。  
  
## <a name="compoundcurve-instances"></a>CompoundCurve 实例  
 下图显示有效的 **CurvePolygon** 图形：  
  
### <a name="accepted-instances"></a>接受的实例  
 为使 **CurvePolygon** 实例可接受，它需要或者为空，或者仅包含接受的圆弧环。 接受的圆弧环满足以下要求。  
  
1.  是接受的 **LineString**、 **CircularString** 或 **CompoundCurve** 实例。 有关接受的实例的详细信息，请参阅 [LineString](../../relational-databases/spatial/linestring.md)、 [CircularString](../../relational-databases/spatial/circularstring.md)和 [CompoundCurve](../../relational-databases/spatial/compoundcurve.md)。  
  
2.  具有至少四个点。  
  
3.  起点和终点具有相同的 X 和 Y 坐标。  

    > [!NOTE]  
    > Z 和 M 值被忽略。  
  
下面的示例显示接受的 **CurvePolygon** 实例。  
  
```sql  
DECLARE @g1 geometry = 'CURVEPOLYGON EMPTY';  
DECLARE @g2 geometry = 'CURVEPOLYGON((0 0, 0 0, 0 0, 0 0))';  
DECLARE @g3 geometry = 'CURVEPOLYGON((0 0 1, 0 0 2, 0 0 3, 0 0 3))'  
DECLARE @g4 geometry = 'CURVEPOLYGON(CIRCULARSTRING(1 3, 3 5, 4 7, 7 3, 1 3))';  
DECLARE @g5 geography = 'CURVEPOLYGON((-122.3 47, 122.3 -47, 125.7 -49, 121 -38, -122.3 47))';  
```  
  
`@g3` 已被接受，即使在起点和终点具有不同的 Z 值时也是如此，因为 Z 值被忽略。 `@g5` 已被接受，即使 **geography** 类型实例无效时也是如此。  
  
下面的示例引发 `System.FormatException`。  
  
```sql  
DECLARE @g1 geometry = 'CURVEPOLYGON((0 5, 0 0, 0 0, 0 0))';  
DECLARE @g2 geometry = 'CURVEPOLYGON((0 0, 0 0, 0 0))';  
```  
  
`@g1` 不被接受，因为起点和终点不具有相同的 Y 值。 `@g2` 不被接受，因为环没有足够的点。  
  
### <a name="valid-instances"></a>有效实例  
为使 **CurvePolygon** 实例有效，外环和内环都必须满足以下条件：  
  
1.  它们只能在单个切点处接触。  
2.  它们不能彼此交叉。  
3.  每个环都必须至少包含四个点。  
4.  每个环都必须是可接受的曲线类型。  
  
**CurvePolygon** 实例根据其是 **geometry** 还是 **geography** 数据类型，也需要满足特定的条件。  
  
#### <a name="geometry-data-type"></a>geometry 数据类型  
一个有效的 **geometryCurvePolygon** 实例必须具有以下属性：  
  
1.  所有内环都必须包含在外环内。  
2.  可具有多个内环，但一个外环不能包含另一个内环。  
3.  环不能与自身或其他环交叉。  
4.  环只能在单个切点处接触（环接触的点数必须有限）。  
5.  多边形的内部必须连接。  
  
下面的示例显示有效的 **geometryCurvePolygon** 实例。  
  
```sql  
DECLARE @g1 geometry = 'CURVEPOLYGON EMPTY';  
DECLARE @g2 geometry = 'CURVEPOLYGON(CIRCULARSTRING(1 3, 3 5, 4 7, 7 3, 1 3))';  
SELECT @g1.STIsValid(), @g2.STIsValid();  
```  
  
CurvePolygon 实例具有与 Polygon 实例大体上相同的有效性规则，只有一个例外，就是 CurvePolygon 实例可接受新的圆弧线段类型。 有关有效实例或无效实例的更多示例，请参阅 [Polygon](../../relational-databases/spatial/polygon.md)。  
  
#### <a name="geography-data-type"></a>geography 数据类型  
一个有效的 **geographyCurvePolygon** 实例必须具有以下属性：  
  
1.  多边形的内部使用左手定律进行连接。  
2.  环不能与自身或其他环交叉。  
3.  环只能在单个切点处接触（环接触的点数必须有限）。  
4.  多边形的内部必须连接。  
  
下面的示例显示一个有效的地理 CurvePolygon 实例。  
  
```sql  
DECLARE @g geography = 'CURVEPOLYGON((-122.3 47, 122.3 47, 125.7 49, 121 38, -122.3 47))';  
SELECT @g.STIsValid();  
```  
  
## <a name="examples"></a>示例  
  
### <a name="a-instantiating-a-geometry-instance-with-an-empty-curvepolygon"></a>A. 实例化具有空 CurvePolygon 的几何图形实例  
 该示例说明如何创建一个空的 **CurvePolygon** 实例：  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('CURVEPOLYGON EMPTY');  
```  
  
### <a name="b-declaring-and-instantiating-a-geometry-instance-with-a-curvepolygon-in-the-same-statement"></a>B. 在同一语句中声明和实例化一个具有 CurvePolygon 的几何图形实例  
 此代码段说明如何在同一语句中声明和实例化一个具有 **CurvePolygon** 的几何图形实例：  
  
```sql  
DECLARE @g geometry = 'CURVEPOLYGON(CIRCULARSTRING(2 4, 4 2, 6 4, 4 6, 2 4))'  
```  
  
### <a name="c-instantiating-a-geography-instance-with-a-curvepolygon"></a>C. 实例化一个具有 CurvePolygon 的几何图形实例  
 此代码段说明如何声明和实例化一个具有 **geography** 的 **CurvePolygon** 实例：  
  
```sql  
DECLARE @g geography = 'CURVEPOLYGON(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
```  
  
### <a name="d-storing-a-curvepolygon-with-only-an-exterior-bounding-ring"></a>D. 存储仅具有一个外部边界环的 CurvePolygon  
 此示例说明如何在一个 **CurvePolygon** 实例中存储一个简单的圆形（仅有一个外部边界环用于界定该圆形）：  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(2 4, 4 2, 6 4, 4 6, 2 4))');  
SELECT @g.STArea() AS Area;  
```  
  
### <a name="e-storing-a-curvepolygon-containing-interior-rings"></a>E. 存储包含内环的 CurvePolygon  
 此示例在 **CurvePolygon** 实例中创建一个圆环图（使用一个外部边界环和一个内环来界定该圆环图）：  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(0 4, 4 0, 8 4, 4 8, 0 4), CIRCULARSTRING(2 4, 4 2, 6 4, 4 6, 2 4))');  
SELECT @g.STArea() AS Area;  
```  
  
 此示例说明在使用内环时的一个有效的 **CurvePolygon** 实例和一个无效的实例：  
  
```sql  
DECLARE @g1 geometry, @g2 geometry;  
SET @g1 = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(0 5, 5 0, 0 -5, -5 0, 0 5), (-2 2, 2 2, 2 -2, -2 -2, -2 2))');  
IF @g1.STIsValid() = 1  
  BEGIN  
     SELECT @g1.STArea();  
  END  
SET @g2 = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(0 5, 5 0, 0 -5, -5 0, 0 5), (0 5, 5 0, 0 -5, -5 0, 0 5))');  
IF @g2.STIsValid() = 1  
  BEGIN  
     SELECT @g2.STArea();  
  END  
SELECT @g1.STIsValid() AS G1, @g2.STIsValid() AS G2;  
```  
  
 `@g1` 和 `@g2` 都使用相同的外部边界环：一个半径为 5 的圆形，并且它们都使用一个正方形作为内环。  不过，实例 `@g1` 有效，而实例 `@g2` 却无效。 @g2 无效的原因在于内环将外环界定的内部空间拆分为四个单独的区域。 下图显示所发生的情况：  
  
## <a name="see-also"></a>另请参阅  
 [Polygon](../../relational-databases/spatial/polygon.md)   
 [CircularString](../../relational-databases/spatial/circularstring.md)   
 [CompoundCurve](../../relational-databases/spatial/compoundcurve.md)   
 [geometry 数据类型方法引用](../../t-sql/spatial-geometry/spatial-types-geometry-transact-sql.md)   
 [geography 数据类型方法引用](../../t-sql/spatial-geography/stequals-geography-data-type.md)   
 [空间数据类型概述](../../relational-databases/spatial/spatial-data-types-overview.md)  
  
