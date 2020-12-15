---
description: CompoundCurve
title: CompoundCurve | Microsoft Docs
ms.date: 07/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: ae357f9b-e3e2-4cdf-af02-012acda2e466
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 919e71894dbf01ce015bed8eb3cc801bbad29c4a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97415954"
---
# <a name="compoundcurve"></a>CompoundCurve
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  **CompoundCurve** 是几何图形或地域类型的零个或多个连续 **CircularString** 或 **LineString** 实例的集合。  
  
可以实例化空的 **CompoundCurve** 实例，但要使 **CompoundCurve** 有效，它必须满足以下条件：  
  
1.  它必须至少包含一个 **CircularString** 或 **LineString** 实例。  
  
2.  **CircularString** 或 **LineString** 实例的序列必须是连续的。  

如果 **CompoundCurve** 包含由多个 **CircularString** 和 **LineString** 实例构成的序列，在该序列中，每个实例（最后一个实例除外）的结束端点必须是下一个实例的起始端点。 这意味着，如果在序列中前一个实例的结束点是 (4 3 7 2)，则该序列中的下一个实例的起始点必须是 (4 3 7 2)。 请注意，该点的 Z（标高）和 M（度量）值也必须相同。 如果这两个点之间存在差异，则将引发 `System.FormatException` 。 **CircularString** 中的点不一定必须有 Z 值或 M 值。 如果未向前一个实例的结束点提供 Z 值或 M 值，下一个实例的起始点就不能包含 Z 值或 M 值。 如果前一个序列的结束点是 (4 3)，则后一个序列的起始点必须是 (4 3)；它不能是 (4 3 7 2）。 **CompoundCurve** 实例中的所有点要么不得有 Z 值，要么其 Z 值必须相同。  
  
## <a name="compoundcurve-instances"></a>CompoundCurve 实例  
下图显示了有效的 **CompoundCurve** 类型。  
  
![CompoundCurve 示例](../../relational-databases/spatial/media/f278742e-b861-4555-8b51-3d972b7602bf.gif)  
 
### <a name="accepted-instances"></a>接受的实例  
 如果 **CompoundCurve** 实例是空实例或者它满足以下条件，则接受该实例。  
  
1.  **CompoundCurve** 实例包含的所有实例都是接受的圆弧线段实例。 有关接受的圆弧线段实例的详细信息，请参阅 [LineString](../../relational-databases/spatial/linestring.md) 和 [CircularString](../../relational-databases/spatial/circularstring.md)。  
  
2.  **CompoundCurve** 实例中的所有圆弧线段都连接在一起。 每个后续圆弧线段上的第一个点与前一个圆弧线段上的最后一个点相同。  
  
    > [!NOTE]  
    > Z 和 M 坐标也要相同。 因此，所有四个坐标 X、Y、Z 和 M 必须相同。  
  
3.  所有包含的实例都不是空实例。  
  
下面的示例显示接受的 **CompoundCurve** 实例。  
  
```sql  
DECLARE @g1 geometry = 'COMPOUNDCURVE EMPTY';  
DECLARE @g2 geometry = 'COMPOUNDCURVE(CIRCULARSTRING(1 0, 0 1, -1 0), (-1 0, 2 0))';  
```  
  
下面的示例显示未被接受的 **CompoundCurve** 实例。 这些实例引发 `System.FormatException`。  
  
```sql  
DECLARE @g1 geometry = 'COMPOUNDCURVE(CIRCULARSTRING EMPTY)';  
DECLARE @g2 geometry = 'COMPOUNDCURVE(CIRCULARSTRING(1 0, 0 1, -1 0), (1 0, 2 0))';  
```  
  
### <a name="valid-instances"></a>有效实例  
 如果满足以下条件，则 **CompoundCurve** 实例有效。  
  
1.  **CompoundCurve** 实例是接受的实例。  
  
2.  **CompoundCurve** 实例包含的所有圆弧线段实例都是有效实例。  
  
下面的示例显示有效的 **CompoundCurve** 实例。  
  
```sql  
DECLARE @g1 geometry = 'COMPOUNDCURVE EMPTY';  
DECLARE @g2 geometry = 'COMPOUNDCURVE(CIRCULARSTRING(1 0, 0 1, -1 0), (-1 0, 2 0))';  
DECLARE @g3 geometry = 'COMPOUNDCURVE(CIRCULARSTRING(1 1, 1 1, 1 1), (1 1, 3 5, 5 4))';  
SELECT @g1.STIsValid(), @g2.STIsValid(), @g3.STIsValid();  
```  
  
`@g3` 有效，因为 **CircularString** 实例有效。 有关 **CircularString** 实例的有效性的详细信息，请参阅 [CircularString](../../relational-databases/spatial/circularstring.md)。  
  
下面的示例显示无效的 **CompoundCurve** 实例。  
  
```sql  
DECLARE @g1 geometry = 'COMPOUNDCURVE(CIRCULARSTRING(1 1, 1 1, 1 1), (1 1, 3 5, 5 4, 3 5))';  
DECLARE @g2 geometry = 'COMPOUNDCURVE((1 1, 1 1))';  
DECLARE @g3 geometry = 'COMPOUNDCURVE(CIRCULARSTRING(1 1, 2 3, 1 1))';  
SELECT @g1.STIsValid(), @g2.STIsValid(), @g3.STIsValid();  
```  
  
`@g1` 无效，因为第二个实例是无效的 LineString 实例。 `@g2` 无效，因为 **LineString** 实例无效。 `@g3` 无效，因为 **CircularString** 实例无效。 有关有效的 **CircularString** 和 **LineString** 实例的详细信息，请参阅 [CircularString](../../relational-databases/spatial/circularstring.md) 和 [LineString](../../relational-databases/spatial/linestring.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-instantiating-a-geometry-instance-with-an-empty-compooundcurve"></a>A. 使用空的 CompoundCurve 实例化一个几何图形实例  
 下面的示例演示如何创建空的 `CompoundCurve` 实例：  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('COMPOUNDCURVE EMPTY');  
```  
  
### <a name="b-declaring-and-instantiating-a-geometry-instance-using-a-compoundcurve-in-the-same-statement"></a>B. 在同一语句中使用 CompoundCurve 声明和实例化一个几何图形实例  
 下面的示例演示如何在同一语句中使用 `geometry` 声明和初始化 `CompoundCurve`实例：  
  
```sql  
DECLARE @g geometry = 'COMPOUNDCURVE ((2 2, 0 0),CIRCULARSTRING (0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0))';  
```  
  
### <a name="c-instantiating-a-geography-instance-with-a-compoundcurve"></a>C. 使用 CompoundCurve 实例化一个地域实例  
 下面的示例演示如何使用 **声明和初始化** geography `CompoundCurve`实例：  
  
```sql  
DECLARE @g geography = 'COMPOUNDCURVE(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
```  
  
### <a name="d-storing-a-square-in-a-compoundcurve-instance"></a>D. 将一个正方形存储在 CompoundCurve 实例中  
 下面的示例通过两种不同方式使用 `CompoundCurve` 实例存储一个正方形。  
  
```sql  
DECLARE @g1 geometry, @g2 geometry;  
SET @g1 = geometry::Parse('COMPOUNDCURVE((1 1, 1 3), (1 3, 3 3),(3 3, 3 1), (3 1, 1 1))');  
SET @g2 = geometry::Parse('COMPOUNDCURVE((1 1, 1 3, 3 3, 3 1, 1 1))');  
SELECT @g1.STLength(), @g2.STLength();  
```  
  
 `@g1` 和 `@g2` 的长度相同。 请注意，在该示例中 **CompoundCurve** 实例可以存储一个或多个 `LineString`的实例。  
  
### <a name="e-instantiating-a-geometry-instance-using-a-compoundcurve-with-multiple-circularstrings"></a>E. 使用包含多个 CircularString 的 CompoundCurve 实例化一个几何图形实例  
 下面的示例演示如何使用两个不同的 `CircularString` 实例初始化 `CompoundCurve`。  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(0 2, 2 0, 4 2), CIRCULARSTRING(4 2, 2 4, 0 2))');  
SELECT @g.STLength();  
```  
  
此示例产生输出 `12.5663706143592`，相当于 4∏。 该示例中的 `CompoundCurve` 实例存储一个半径为 2 的圆。 前面的两个代码示例不一定非要使用 `CompoundCurve`。 对于第一个示例， `LineString` 实例将更为简单，对于第二个示例， `CircularString` 实例将更为简单。 然而，下一个示例显示在何种情况下 `CompoundCurve` 提供更好的替代方法。  
  
### <a name="f-using-a-compoundcurve-to-store-a-semicircle"></a>F. 使用 CompoundCurve 存储一个半圆  
 下面的示例使用 `CompoundCurve` 实例存储一个半圆。  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(0 2, 2 0, 4 2), (4 2, 0 2))');  
SELECT @g.STLength();  
```  
  
### <a name="g-storing-multiple-circularstring-and-linestring-instances-in-a-compoundcurve"></a>G. 将多个 CircularString 和 LineString 实例存储在一个 CompoundCurve 中  
 下面的示例演示如何使用 `CircularString` 存储多个 `LineString` 和 `CompoundCurve`实例。  
  
```sql  
DECLARE @g geometry  
SET @g = geometry::Parse('COMPOUNDCURVE((3 5, 3 3), CIRCULARSTRING(3 3, 5 1, 7 3), (7 3, 7 5), CIRCULARSTRING(7 5, 5 7, 3 5))');  
SELECT @g.STLength();  
```  
  
### <a name="h-storing-instances-with-z-and-m-values"></a>H. 存储具有 Z 值和 M 值的实例  
 下面的示例演示如何使用 `CompoundCurve` 实例存储具有 Z 值和 M 值的 `CircularString` 和 `LineString` 实例的序列。  
  
```sql  
SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(7 5 4 2, 5 7 4 2, 3 5 4 2), (3 5 4 2, 8 7 4 2))');  
```  
  
### <a name="i-illustrating-why-circularstring-instances-must-be-explicitly-declared"></a>I. 说明为什么必须显式声明 CircularString 实例  
 下面的示例说明了为什么必须显式声明 `CircularString` 实例。 程序员试图将一个圆形存储在 `CompoundCurve` 实例中。  
  
```sql  
DECLARE @g1 geometry;    
DECLARE @g2 geometry;  
SET @g1 = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(0 2, 2 0, 4 2), (4 2, 2 4, 0 2))');  
SELECT 'Circle One', @g1.STLength() AS Perimeter;  -- gives an inaccurate amount  
SET @g2 = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(0 2, 2 0, 4 2), CIRCULARSTRING(4 2, 2 4, 0 2))');  
SELECT 'Circle Two', @g2.STLength() AS Perimeter;  -- now we get an accurate amount  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Circle One11.940039...  
Circle Two12.566370...  
```  
  
圆形 2 的周长大约为 4∏，这是周长的实际值。 但是，圆形 1 的周长很不准确。 圆形 1 的 `CompoundCurve` 实例存储一个圆弧线段 (ABC) 和两条直线段（CD、DA)。 `CompoundCurve` 实例必须存储两个圆弧线段（ABC、CDA）才可以定义一个圆形。 `LineString` 实例定义圆形 1 的 `CompoundCurve` 实例中的第二组点（4 2、2 4、0 2）。 必须在 `CircularString` 内部显式声明 `CompoundCurve`实例。  
  
## <a name="see-also"></a>另请参阅  
 [STIsValid（geometry 数据类型）](../../t-sql/spatial-geometry/stisvalid-geometry-data-type.md)   
 [STLength（geometry 数据类型）](../../t-sql/spatial-geometry/stlength-geometry-data-type.md)   
 [STStartPoint（geometry 数据类型）](../../t-sql/spatial-geometry/ststartpoint-geometry-data-type.md)   
 [STEndpoint（geometry 数据类型）](../../t-sql/spatial-geometry/stendpoint-geometry-data-type.md)   
 [LineString](../../relational-databases/spatial/linestring.md)   
 [CircularString](../../relational-databases/spatial/circularstring.md)   
 [空间数据类型概述](../../relational-databases/spatial/spatial-data-types-overview.md)   
 [Point](../../relational-databases/spatial/point.md)  
  
