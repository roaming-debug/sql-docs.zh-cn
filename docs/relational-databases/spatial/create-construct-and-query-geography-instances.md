---
description: 创建、构造和查询地理实例
title: 创建、构造和查询地理实例 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- geography data type [SQL Server]
- geodetic data type [SQL Server]
- geography data type [SQL Server], about geography data type
ms.assetid: b585851e-d15b-411f-adeb-aeabeb777c0b
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 96a5bcc2cb8811ac937d8c52858057232a4fe00c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473168"
---
# <a name="create-construct-and-query-geography-instances"></a>创建、构造和查询地理实例
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
   地理空间数据类型、地理表示圆形地球坐标系统中的数据。 此类型在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中作为 .NET 公共语言运行时 (CLR) 数据类型实现。  geography 数据类型存储椭球体（圆形地球）数据，如 GPS 纬度和经度坐标[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 **geography** 类型已进行预定义，可在每个数据库中使用。 你可以创建 **geography** 类型的表列并对 **geography** 数据进行操作，就像使用其他系统提供的数据类型一样。  
  
##  <a name="creating-or-constructing-a-new-geography-instance"></a><a name="creating"></a> 创建或构建新的地域实例  
  
###  <a name="creating-a-new-geography-instance-from-an-existing-instance"></a><a name="existing"></a> 从现有实例创建新的地域实例  
 **geography** 数据类型提供了许多内置方法，你可以使用这些方法基于现有实例创建新的 **geography** 实例。  
  
 **在地域周围创建缓冲区**  
 [STBuffer（geography 数据类型）](../../t-sql/spatial-geography/stbuffer-geography-data-type.md)  
  
 **在地域周围创建缓冲区，允许公差**  
 [BufferWithTolerance（geography 数据类型）](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md)  
  
 **通过两个地域实例的交集创建地域**  
 [STIntersection（geography 数据类型）](../../t-sql/spatial-geography/stintersection-geography-data-type.md)  
  
 **通过两个地域的实例并集创建地域**  
 [STUnion（geography 数据类型）](../../t-sql/spatial-geography/stunion-geography-data-type.md)  
  
 **通过一个地域中没有与另一个地域发生重叠的点创建地域**  
 [STDifference（geography 数据类型）](../../t-sql/spatial-geography/stdifference-geography-data-type.md)  
  
###  <a name="constructing-a-geography-instance-from-well-known-text-input"></a><a name="wkt"></a> 从熟知文本输入构造地域实例  
 **geography** 数据类型提供了若干种用开放地理空间联盟 (OGC) WKT 表示形式生成地域的内置方法。 WKT 标准是一种允许地域数据以文本形式交换的文本字符串。  
  
 **用 WKT 输入构造任意类型的地域实例**  
 [STGeomFromText（geography 数据类型）](../../t-sql/spatial-geography/stgeomfromtext-geography-data-type.md)  
  
 [Parse（geography 数据类型）](../../t-sql/spatial-geography/parse-geography-data-type.md)  
  
 **用 WKT 输入构造地域 Point 实例**  
 [STPointFromText（geography 数据类型）](../../t-sql/spatial-geography/stpointfromtext-geography-data-type.md)  
  
 **用 WKT 输入构造地域 MultiPoint 实例**  
 [STMPointFromText（geography 数据类型）](../../t-sql/spatial-geography/stmpointfromtext-geography-data-type.md)  
  
 **用 WKT 输入构造地域 LineString 实例**  
 STLineFromText（geography 数据类型）  
  
 **用 WKT 输入构造地域 MultiLineString 实例**  
 [STMLineFromText（geography 数据类型）](../../t-sql/spatial-geography/stmlinefromtext-geography-data-type.md)  
  
 **用 WKT 输入构造地域 Polygon 实例**  
 [STPolyFromText（geography 数据类型）](../../t-sql/spatial-geography/stpolyfromtext-geography-data-type.md)  
  
 **用 WKT 输入构造地域 MultiPolygon 实例**  
 [STMPolyFromText（geography 数据类型）](../../t-sql/spatial-geography/stmpolyfromtext-geography-data-type.md)  
  
 **用 WKT 输入构造地域 GeometryCollection 实例**  
 [STGeomCollFromText（geography 数据类型）](../../t-sql/spatial-geography/stgeomcollfromtext-geography-data-type.md)  
  
###  <a name="constructing-a-geography-instance-from-well-known-binary-input"></a><a name="wkb"></a> 从熟知二进制输入构造地域实例  
 WKB 是 OGC 规定的一种二进制格式，该格式允许 **Geography** 数据在客户端应用程序和 SQL 数据库之间进行交换。 以下函数接受使用 WKB 输入构造地域实例：  
  
 **用 WKB 输入构造任意类型的地域实例**  
 [STGeomFromWKB（geography 数据类型）](../../t-sql/spatial-geography/stgeomfromwkb-geography-data-type.md)  
  
 **用 WKB 输入构造地域 Point 实例**  
 [STPointFromWKB（geography 数据类型）](../../t-sql/spatial-geography/stpointfromwkb-geography-data-type.md)  
  
 **用 WKB 输入构造地域 MultiPoint 实例**  
 [STMPointFromWKB（geography 数据类型）](../../t-sql/spatial-geography/stmpointfromwkb-geography-data-type.md)  
  
 **用 WKB 输入构造地域 LineString 实例**  
 [STLineFromWKB（geography 数据类型）](../../t-sql/spatial-geography/stlinefromwkb-geography-data-type.md)  
  
 **用 WKB 输入构造地域 MultiLineString 实例**  
 [STMLineFromWKB（geography 数据类型）](../../t-sql/spatial-geography/stmlinefromwkb-geography-data-type.md)  
  
 **用 WKB 输入构造地域 Polygon 实例**  
 [STPolyFromWKB（geography 数据类型）](../../t-sql/spatial-geography/stpolyfromwkb-geography-data-type.md)  
  
 **用 WKB 输入构造地域 MultiPolygon 实例**  
 [STMPolyFromWKB（geography 数据类型）](../../t-sql/spatial-geography/stmpolyfromwkb-geography-data-type.md)  
  
 **用 WKB 输入构造地域 GeometryCollection 实例**  
 [STGeomCollFromWKB（geography 数据类型）](../../t-sql/spatial-geography/stgeomcollfromwkb-geography-data-type.md) STGeomCollFromWKB（geography 数据类型）  
  
###  <a name="constructing-a-geography-instance-from-gml-text-input"></a><a name="gml"></a> 用 GML 文本输入构造地域实例  
 **geography** 数据类型提供了一种用 GML（ **geography** 实例的 XML 表示形式）生成 **geography** 实例的方法。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持部分 GML。  
  
 有关地域标记语言的详细信息，请参阅 OGC 规范： [OGC Specifications, Geography Markup Language](https://go.microsoft.com/fwlink/?LinkId=93629)（OGC 规范，地域标记语言）。  
  
 **用 GML 输入构造任意类型的地域实例**  
 [GeomFromGML（geography 数据类型）](../../t-sql/spatial-geography/geomfromgml-geography-data-type.md)  
  
##  <a name="returning-well-known-text-and-well-known-binary-from-a-geography-instance"></a><a name="returning"></a> 从地域实例返回熟知文本和熟知二进制  
 可以使用以下方法返回 WKT 或 WKB 格式的 **geography** 实例：  
  
 **返回地域实例的 WKT 表示形式**  
 [STAsText（geography 数据类型）](../../t-sql/spatial-geography/stastext-geography-data-type.md)  
  
 [ToString（geography 数据类型）](../../t-sql/spatial-geography/tostring-geography-data-type.md)  
  
 **返回包括任何 Z 值和 M 值的地域实例的 WKT 表示形式**  
 [AsTextZM（geography 数据类型）](../../t-sql/spatial-geography/astextzm-geography-data-type.md)  
  
 **返回地域实例的 WKB 表示形式**  
 [STAsBinary（geography 数据类型）](../../t-sql/spatial-geography/stasbinary-geography-data-type.md)  
  
 **返回地域实例的 GML 表示形式**  
 [AsGml（geography 数据类型）](../../t-sql/spatial-geography/asgml-geography-data-type.md)  
  
##  <a name="querying-the-properties-and-behaviors-of-geography-instances"></a><a name="query"></a> 查询地域实例的属性和行为  
 所有 **geography** 实例都有很多可以通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供的方法进行检索的属性。 下列主题定义了地域类型的属性和行为，并为查询每种图形定义了方法。  
  
###  <a name="validity-instance-type-and-geometrycollection-information"></a><a name="valid"></a> 有效性、实例类型和 GeometryCollection 信息  
 构造 **geography** 实例后，就可以使用以下方法返回实例类型，或者，如果它是 **GeometryCollection** 实例，则返回特定的 **geography** 实例。  
  
 **返回地域的实例类型**  
 [STGeometryType（geography 数据类型）](../../t-sql/spatial-geography/stgeometrytype-geography-data-type.md)  
  
 **确定地域是否为给定的实例类型**  
 [InstanceOf（geography 数据类型）](../../t-sql/spatial-geography/instanceof-geography-data-type.md)  
  
 **确定地域实例对其实例类型而言格式是否正确**  
 [STNumGeometries（geography 数据类型）](../../t-sql/spatial-geography/stnumgeometries-geography-data-type.md)  
  
 **返回 GeometryCollection 实例中的特定地域**  
 [STGeometryN（geography 数据类型）](../../t-sql/spatial-geography/stgeometryn-geography-data-type.md)STGeometryN（geography 数据类型）  
  
###  <a name="number-of-points"></a><a name="number"></a> 点数  
 所有非空 **geography** 实例都由“点” 组成。 这些点表示球体的纬度和经度坐标，在其上可绘制 **geography** 实例。 **geography** 数据类型提供了许多用于查询实例点的内置方法。  
  
 **返回构成实例的点数。**  
 [STNumPoints（geography 数据类型）](../../t-sql/spatial-geography/stnumpoints-geography-data-type.md)  
  
 **返回实例中的特定点**  
 [STPointN（geometry 数据类型）](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md)  
  
 **返回实例的起始点**  
 [STStartPoint（geography 数据类型）](../../t-sql/spatial-geography/ststartpoint-geography-data-type.md)  
  
 **返回实例的终点**  
 [STEndpoint（geography 数据类型）](../../t-sql/spatial-geography/stendpoint-geography-data-type.md)  
  
###  <a name="dimension"></a><a name="dimension"></a> 维度  
 非空 **geography** 实例可以为零维、一维或二维。 零维 **geography** 实例（如 **Point** 和 **MultiPoint**）没有长度或面积。 一维对象（如 **LineString、CircularString**、 **CompoundCurve** 和 **MultiLineString**）有长度。 二维实例（如 **Polygon、CurvePolygon** 和 **MultiPolygon**）有面积和长度。 空实例会报告 -1 维，并且 **GeometryCollection** 会报告其内容的最大维度。  
  
 **返回实例的维度**  
 [STDimension（geography 数据类型）](../../t-sql/spatial-geography/stdimension-geography-data-type.md)  
  
 **返回实例的长度**  
 [STLength（geography 数据类型）](../../t-sql/spatial-geography/stlength-geography-data-type.md)  
  
 **返回实例的面积**  
 [STArea（geography 数据类型）](../../t-sql/spatial-geography/starea-geography-data-type.md)  
  
###  <a name="empty"></a><a name="empty"></a> Empty  
 空 **geography** 实例不包含任何点。 空的 **LineString, CircularString**、 **CompoundCurve** 和 **MultiLineString** 实例的长度为 0。 空的 **Polygon, CurvePolygon** 和 **MultiPolygon** 实例的面积为 0。  
  
 **确定实例是否为空**  
 [STIsEmpty（geography 数据类型）](../../t-sql/spatial-geography/stisempty-geography-data-type.md)  
  
###  <a name="closure"></a><a name="closure"></a> 闭合  
 闭合的 **geography** 实例是指起始点和终点相同的图形。 **Polygon** 实例被视为闭合的。 **Point** 实例不是闭合的。  
  
 环是一个简单、闭合的 **LineString** 实例。  
  
 **确定实例是否闭合**  
 [STIsClosed（geography 数据类型）](../../t-sql/spatial-geography/stisclosed-geography-data-type.md)  
  
 **返回多边形实例的环数**  
 [NumRings（geography 数据类型）](../../t-sql/spatial-geography/numrings-geography-data-type.md)  
  
 **返回地域实例的指定环**  
 [RingN（geography 数据类型）](../../t-sql/spatial-geography/ringn-geography-data-type.md)  
  
###  <a name="spatial-reference-id-srid"></a><a name="srid"></a> 空间引用标识符 (SRID)  
 空间引用标识符 (SRID) 是指定 **geography** 实例所在的椭球坐标系的标识符。 两个拥有不同 SRID 的 **geography** 实例是不可比的。  
  
 **设置或返回实例的 SRID**  
 [STSrid（geography 数据类型）](../../t-sql/spatial-geography/stsrid-geography-data-type.md)  
  
 此属性可以进行修改。  
  
##  <a name="determining-relationships-between-geography-instances"></a><a name="rel"></a> 确定地域实例之间的关系  
 **geography** 数据类型提供了许多内置方法，你可以使用这些方法确定两个 **geography** 实例之间的关系。  
  
 **确定两个实例是否包含相同的点集**  
 [STEquals（geometry 数据类型）](../../t-sql/spatial-geometry/stequals-geometry-data-type.md)  
  
 **确定两个实例是否不相接**  
 [STDisjoint（geometry 数据类型）](../../t-sql/spatial-geometry/stdisjoint-geometry-data-type.md)  
  
 **确定两个实例是否相交**  
 [STIntersects（geography 数据类型）](../../t-sql/spatial-geometry/stintersects-geometry-data-type.md)  
  
 **确定两个实例的交点**  
 [STIntersection（geography 数据类型）](../../t-sql/spatial-geography/stintersection-geography-data-type.md)  
  
 **确定两个地域实例中点之间的最短距离**  
 [STDistance（geography 数据类型）](../../t-sql/spatial-geometry/stdistance-geometry-data-type.md)  
  
 **确定两个地域实例之间点的不同**  
 [STDifference（geography 数据类型）](../../t-sql/spatial-geography/stdifference-geography-data-type.md)  
  
 **派生一个地域实例相比于另一个地域实例的余集或唯一点**  
 [STSymDifference（geography 数据类型）](../../t-sql/spatial-geography/stsymdifference-geography-data-type.md)  
  
##  <a name="geography-instances-must-use-supported-srid"></a><a name="supportedsrid"></a> 地域实例必须使用支持的 SRID  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持基于 EPSG 标准的 SRID。 必须使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]geography **实例的支持** 的 SRID 执行计算或将方法用于地域空间数据。 SRID 必须与 **sys.spatial_reference_systems** 目录视图中显示的 SRID 中的一个匹配。 如前所述，在使用 **geography** 数据类型对空间数据执行计算时，结果将取决于在创建数据时使用的是哪个椭圆体，因为为每个椭圆体都分配了一个特定空间引用标识符 (SRID)。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例使用方法时， **geography** 实例之间的关系。 如果要使用 WGS 84（或 SRID 4326）之外的某个空间引用系统中的数据，您需要确定地域空间数据的特定 SRID。  
  
##  <a name="examples"></a><a name="examples"></a> 示例  
以下示例说明如何添加和查询地理数据。  
  
### <a name="example-a"></a>示例 A。 
此示例创建了一个带有标识列和 `geography` 列 `GeogCol1` 的表。 第三列将 `geography` 列呈现为其开放地理空间信息联盟 (OGC) 熟知文本 (WKT) 表示形式，并使用 `STAsText()` 方法。 接下来将插入两行：一行包含 `LineString` 类型的 `geography`实例，一行包含 `Polygon` 实例。  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable   
  ( id int IDENTITY (1,1),  
    GeogCol1 geography,   
    GeogCol2 AS GeogCol1.STAsText()
   );  
GO  
  
INSERT INTO SpatialTable (GeogCol1)  
VALUES (geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326));  
  
INSERT INTO SpatialTable (GeogCol1)  
VALUES (geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326));  
GO  
```  
  
### <a name="example-b"></a>示例 B。
此示例使用 `STIntersection()` 方法返回此前插入的两个 `geography` 实例相交的点。  
  
```sql  
DECLARE @geog1 geography;  
DECLARE @geog2 geography;  
DECLARE @result geography;  
  
SELECT @geog1 = GeogCol1 FROM SpatialTable WHERE id = 1;  
SELECT @geog2 = GeogCol1 FROM SpatialTable WHERE id = 2;  
SELECT @result = @geog1.STIntersection(@geog2);  
SELECT @result.STAsText();  
```  
  
## <a name="see-also"></a>另请参阅  
 [空间数据 (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
