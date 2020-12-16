---
description: 创建、构造和查询几何图形实例
title: 创建、构造和查询几何图形实例 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- planar spatial data [SQL Server], getting started
- geometry data type [SQL Server], getting started
ms.assetid: c6b5c852-37d2-48d0-a8ad-e43bb80d6514
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7399b65d5816b3d0051da3c641c165ba87bacf45
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482958"
---
# <a name="create-construct-and-query-geometry-instances"></a>创建、构造和查询几何图形实例
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  平面空间数据类型 **geometry** 表示欧几里得（平面）坐标系中的数据。 此类型在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中作为公共语言运行时 (CLR) 数据类型实现。  
  
 **geometry** 类型已进行预定义，可在每个数据库中使用。 您可以创建 **geometry** 类型的表列并对 **geometry** 数据进行操作，就像使用其他 CLR 类型一样。  
  
 **支持的** geometry [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型（平面）符合开放地理空间联盟 (OGC) 的 SQL 简单特征规范 1.1.0 版。  
  
 有关 OGC 规范的详细信息，请参阅以下内容：  
  
-   [OGC Specifications, Simple Feature Access Part 1 - Common Architecture（OGC 规范：简单特征访问第 1 部分 - 公共体系结构）](https://go.microsoft.com/fwlink/?LinkId=93628)  
  
-   [OGC Specifications, Simple Feature Access Part 2 - SQL Options（OGC 规范：简单特征访问第 2 部分 - SQL 选项）](https://go.microsoft.com/fwlink/?LinkId=93629)  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持现有 GML 3.1 标准的子集，该标准在下面的架构中定义：[https://schemas.microsoft.com/sqlserver/profiles/gml/SpatialGML.xsd](https://go.microsoft.com/fwlink/?LinkId=230959)。  
  
##  <a name="creating-or-constructing-a-new-geometry-instance"></a><a name="creating"></a> 创建或构建新的几何图形实例  
  
###  <a name="creating-a-new-geometry-instance-from-an-existing-instance"></a><a name="existing"></a> 从现有实例创建新的几何图形实例  
 **geometry** 数据类型提供了许多内置方法，你可以使用这些方法基于现有实例创建新的 **geometry** 实例。  
  
 **在几何图形周围创建缓冲区**  
 [STBuffer（geometry 数据类型）](../../t-sql/spatial-geometry/stbuffer-geometry-data-type.md)  
  
 [BufferWithTolerance（geometry 数据类型）](../../t-sql/spatial-geometry/bufferwithtolerance-geometry-data-type.md)  
  
 **创建简化版的几何图形**  
 [Reduce（geometry 数据类型）](../../t-sql/spatial-geometry/reduce-geometry-data-type.md)  
  
 **创建几何图形的凸包**  
 [STConvexHull（geometry 数据类型）](../../t-sql/spatial-geometry/stconvexhull-geometry-data-type.md)  
  
 **从两个几何图形的交集创建几何图形**  
 [STIntersection（geometry 数据类型）](../../t-sql/spatial-geometry/stintersection-geometry-data-type.md)  
  
 **从两个几何图形的并集创建几何图形**  
 [STUnion（geometry 数据类型）](../../t-sql/spatial-geometry/stunion-geometry-data-type.md)  
  
 **从一个几何图形中与另一个几何图形不重叠的点创建几何图形**  
 [STDifference（geometry 数据类型）](../../t-sql/spatial-geometry/stdifference-geometry-data-type.md)  
  
 **从两个几何图形不重叠的点创建几何图形**  
 [STSymDifference（geometry 数据类型）](../../t-sql/spatial-geometry/stsymdifference-geometry-data-type.md)  
  
 **创建位于现有几何图形上的任意 Point 实例**  
 [STPointOnSurface（geometry 数据类型）](../../t-sql/spatial-geometry/stpointonsurface-geometry-data-type.md)  
  
  
###  <a name="constructing-a-geometry-instance-from-well-known-text-input"></a><a name="wkt"></a> 从熟知文本输入构造几何图形实例  
 **geometry** 数据类型提供了若干种用开放地理空间联盟 (OGC) WKT 表示形式生成几何图形的内置方法。 WKT 标准是一种允许几何图形数据以文本形式交换的文本字符串。  
  
 **用 WKT 输入构造任意类型的几何图形实例**  
 [STGeomFromText（geometry 数据类型）](../../t-sql/spatial-geometry/stgeomfromtext-geometry-data-type.md)  
  
 [Parse（geometry 数据类型）](../../t-sql/spatial-geometry/parse-geometry-data-type.md)  
  
 **用 WKT 输入构造几何图形 Point 实例**  
 [STPointFromText（geometry 数据类型）](../../t-sql/spatial-geometry/stpointfromtext-geometry-data-type.md)  
  
 **用 WKT 输入构造几何图形 MultiPoint 实例**  
 [STMPointFromText（geometry 数据类型）](../../t-sql/spatial-geometry/stmpointfromtext-geometry-data-type.md)  
  
 **用 WKT 输入构造几何图形 LineString 实例**  
 [STLineFromText（geometry 数据类型）](../../t-sql/spatial-geometry/stlinefromtext-geometry-data-type.md)  
  
 **用 WKT 输入构造几何图形 MultiLineString 实例**  
 [STMLineFromText（geometry 数据类型）](../../t-sql/spatial-geometry/stmlinefromtext-geometry-data-type.md)  
  
 **用 WKT 输入构造几何图形 Polygon 实例**  
 [STPolyFromText（geometry 数据类型）](../../t-sql/spatial-geometry/stpolyfromtext-geometry-data-type.md)  
  
 **用 WKT 输入构造几何图形 MultiPolygon 实例**  
 [STMPolyFromText（geometry 数据类型）](../../t-sql/spatial-geometry/stmpolyfromtext-geometry-data-type.md)  
  
 **用 WKT 输入构造几何图形 GeometryCollection 实例**  
 [STGeomCollFromText（geometry 数据类型）](../../t-sql/spatial-geometry/stgeomcollfromtext-geometry-data-type.md)  
  
  
###  <a name="constructing-a-geometry-instance-from-well-known-binary-input"></a><a name="wkb"></a> 从熟知二进制输入构造几何图形实例  
 WKB 是开放地理空间联盟 (OGC) 规定的一种二进制格式，该格式允许 **geometry** 数据在客户端应用程序和 SQL 数据库之间进行交换。 以下函数接受用于构造几何图形的 WKB 输入：  
  
 **用 WKB 输入构造任意类型的几何图形实例**  
 [STGeomFromWKB（geometry 数据类型）](../../t-sql/spatial-geometry/stgeomfromwkb-geometry-data-type.md)  
  
 **用 WKB 输入构造几何图形 Point 实例**  
 [STPointFromWKB（geometry 数据类型）](../../t-sql/spatial-geometry/stpointfromwkb-geometry-data-type.md)  
  
 **用 WKB 输入构造几何图形 MultiPoint 实例**  
 [STMPointFromWKB（geometry 数据类型）](../../t-sql/spatial-geometry/stmpointfromwkb-geometry-data-type.md)  
  
 **用 WKB 输入构造几何图形 LineString 实例**  
 [STLineFromWKB（geometry 数据类型）](../../t-sql/spatial-geometry/stlinefromwkb-geometry-data-type.md)  
  
 **用 WKB 输入构造几何图形 MultiLineString 实例**  
 [STMLineFromWKB（geometry 数据类型）](../../t-sql/spatial-geometry/stmlinefromwkb-geometry-data-type.md)  
  
 **用 WKB 输入构造几何图形 Polygon 实例**  
 [STPolyFromWKB（geometry 数据类型）](../../t-sql/spatial-geometry/stpolyfromwkb-geometry-data-type.md)  
  
 **用 WKB 输入构造几何图形 MultiPolygon 实例**  
 [STMPolyFromWKB（geometry 数据类型）](../../t-sql/spatial-geometry/stmpolyfromwkb-geometry-data-type.md)  
  
 **用 WKB 输入构造几何图形 GeometryCollection 实例**  
 [STGeomCollFromWKB（geometry 数据类型）](../../t-sql/spatial-geometry/stgeomcollfromwkb-geometry-data-type.md)  
  
  
###  <a name="constructing-a-geometry-instance-from-gml-text-input"></a><a name="gml"></a> 用 GML 文本输入构造几何图形实例  
 **geometry** 数据类型提供了一种用 GML（几何对象的 XML 表示形式）生成 **geometry** 实例的方法。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持部分 GML。  
  
 **用 GML 输入构造任意类型的几何图形实例**  
 [GeomFromGml（geometry 数据类型）](../../t-sql/spatial-geometry/geomfromgml-geometry-data-type.md)  
  
  
##  <a name="returning-well-known-text-and-well-known-binary-from-a-geometry-instance"></a><a name="returning"></a> 从几何图形实例返回熟知文本和熟知二进制  
 可以使用以下方法返回 WKT 或 WKB 格式的 **geometry** 实例：  
  
 **返回几何图形实例的 WKT 表示形式**  
 [STAsText（geometry 数据类型）](../../t-sql/spatial-geometry/stastext-geometry-data-type.md)  
  
 [ToString（geometry 数据类型）](../../t-sql/spatial-geometry/tostring-geometry-data-type.md)  
  
 **返回包括任何 Z 值和 M 值的几何图形的 WKT 表示形式**  
 [AsTextZM（geometry 数据类型）](../../t-sql/spatial-geometry/astextzm-geometry-data-type.md)  
  
 **返回几何图形实例的 WKB 表示形式**  
 [STAsBinary（geometry 数据类型）](../../t-sql/spatial-geometry/stasbinary-geometry-data-type.md)  
  
 **返回几何图形实例的 GML 表示形式**  
 [AsGml（geometry 数据类型）](../../t-sql/spatial-geometry/asgml-geometry-data-type.md)  
  
  
##  <a name="querying-the-properties-and-behaviors-of-geometry-instances"></a><a name="querying"></a> 查询几何图形实例的属性和行为  
 所有 **geometry** 实例都有很多可以通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供的方法进行检索的属性。 下列主题定义了几何图形类型的属性和行为，并为查询每种图形定义了方法。  
  
###  <a name="validity-instance-type-and-geometrycollection-information"></a><a name="valid"></a> 有效性、实例类型和 GeometryCollection 信息  
 构造 **geometry** 实例后，就可以使用以下方法确定其格式是否正确、返回实例类型，或者，如果它是集合实例，则返回特定的 **geometry** 实例。  
  
 **返回几何图形的实例类型**  
 [STGeometryType（geometry 数据类型）](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)  
  
 **确定几何图形是否为给定的实例类型**  
 [InstanceOf（geometry 数据类型）](../../t-sql/spatial-geometry/instanceof-geometry-data-type.md)  
  
 **确定几何图形实例对其实例类型而言格式是否正确**  
 [STIsValid（geometry 数据类型）](../../t-sql/spatial-geometry/stisvalid-geometry-data-type.md)  
  
 **将几何图形实例转换成具有实例类型的格式正确的几何图形实例**  
 [MakeValid（geometry 数据类型）](../../t-sql/spatial-geometry/makevalid-geometry-data-type.md)  
  
 **返回几何图形集合实例中的几何图形数目**  
 [STNumGeometries（geometry 数据类型）](../../t-sql/spatial-geometry/stnumgeometries-geometry-data-type.md)  
  
 返回几何图形集合实例中的特定几何图形  
 [STGeometryN（geometry 数据类型）](../../t-sql/spatial-geometry/stgeometryn-geometry-data-type.md)STGeometryN（geometry 数据类型）  
  
  
###  <a name="number-of-points"></a><a name="number"></a> 点数  
 所有非空 **geometry** 实例都由“点” 组成。 这些点表示在其上绘制几何图形的面的 X 和 Y 坐标。 **geometry** 提供许多用于查询实例的点的内置方法。  
  
 **返回构成实例的点数。**  
 [STNumPoints（geometry 数据类型）](../../t-sql/spatial-geometry/stnumpoints-geometry-data-type.md)  
  
 **返回实例中的特定点**  
 [STPointN](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md)  
  
 **返回位于实例上的某个任意点**  
 [STPointOnSurface](../../t-sql/spatial-geometry/stpointonsurface-geometry-data-type.md)  
  
 **返回实例的起始点**  
 [STStartPoint](../../t-sql/spatial-geometry/ststartpoint-geometry-data-type.md)  
  
 **返回实例的终点**  
 [STEndpoint](../../t-sql/spatial-geometry/stendpoint-geometry-data-type.md)  
  
 **返回点实例的 X 坐标**  
 [STX（geometry 数据类型）](../../t-sql/spatial-geometry/stx-geometry-data-type.md)  
  
 **返回点实例的 Y 坐标**  
 [STY](../../t-sql/spatial-geometry/sty-geometry-data-type.md)  
  
 **返回 Polygon、CurvePolygon 或 MultiPolygon 实例的几何中心点**  
 [STCentroid](../../t-sql/spatial-geometry/stcentroid-geometry-data-type.md)  
  
  
###  <a name="dimension"></a><a name="dimension"></a> 维度  
 非空 **geometry** 实例可以为零维、一维或二维。 零维 **geometry**（如 **Point** 和 **MultiPoint**）没有长度或面积。 一维对象（如 **LineString、CircularString、CompoundCurve** 和 **MultiLineString**）有长度。 二维实例（如 **Polygon**、 **CurvePolygon** 和 **MultiPolygon**）有面积和长度。 空实例将报告为 -1 维，并且 **GeometryCollection** 将根据其内容类型报告一个面积。  
  
 **返回实例的维度**  
 [STDimension](../../t-sql/spatial-geometry/stdimension-geometry-data-type.md)  
  
 **返回实例的长度**  
 [STLength](../../t-sql/spatial-geometry/stlength-geometry-data-type.md)  
  
 **返回实例的面积**  
 [STArea](../../t-sql/spatial-geometry/starea-geometry-data-type.md)  
  
  
###  <a name="empty"></a><a name="empty"></a> Empty  
 空 **geometry** 实例不包含任何点。 空的 **LineString, CircularString**、 **CompoundCurve** 和 **MultiLineString** 实例的长度为零。 空的 **Polygon**、 **CurvePolygon** 和 **MultiPolygon** 实例的面积为 0。  
  
 **确定实例是否为空**  
 [STIsEmpty](../../t-sql/spatial-geometry/stisempty-geometry-data-type.md)。  
  
  
###  <a name="simple"></a><a name="simple"></a> Simple  
 为了使实例的 **geometry** 变得“简单” ，必须符合以下全部两个要求：  
  
-   实例的每个图形不能与自身相交，但其终点除外。  
  
-   实例的任何两个图形可在某个点上相交，但两个边界上的点除外。  
  
> [!NOTE]  
>  空几何图形总是简单的。  
  
 **确定实例是否是简单的**  
 [STIsSimple](../../t-sql/spatial-geometry/stissimple-geometry-data-type.md)。  
  
  
###  <a name="boundary-interior-and-exterior"></a><a name="boundary"></a> 边界、内部和外部  
 **geometry** 实例的“内部”是指由实例占用的空间，而“外部”是指未占用的空间。  
  
 “边界” 由 OGC 定义，如下所示：  
  
-   **Point** 和 **MultiPoint** 实例没有边界。  
  
-   **LineString** 和 **MultiLineString** 边界由起始点和终点形成，并删除那些出现次数为偶数的点。  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('MULTILINESTRING((0 1, 0 0, 1 0, 0 1), (1 1, 1 0))');  
SELECT @g.STBoundary().ToString();  
```  
  
 **Polygon** 或 **MultiPolygon** 实例的边界是其环的集合。  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0), (1 1, 1 2, 2 2, 2 1, 1 1))');  
SELECT @g.STBoundary().ToString();  
```  
  
 **返回实例的边界**  
 [STBoundary](../../t-sql/spatial-geometry/stboundary-geometry-data-type.md)  
   
###  <a name="envelope"></a><a name="envelope"></a> 包络线  
 **geometry** 实例的“包络线”又称为“边界框”，它是一个由实例的最小和最大坐标 (X,Y) 形成的轴对齐矩形。  
  
 **返回实例的包络线**  
 [STEnvelope](../../t-sql/spatial-geometry/stenvelope-geometry-data-type.md)  
  
###  <a name="closure"></a><a name="closure"></a> 闭合  
 闭合的 **geometry** 实例是指起始点和终点相同的图形。 **Polygon** 实例被视为闭合的。 **Point** 实例不是闭合的。  
  
 环是一个简单、闭合的 **LineString** 实例。  
  
 **确定实例是否闭合**  
 [STIsClosed](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)  
  
 **确定实例是否为环**  
 [STIsRing](../../t-sql/spatial-geometry/stisring-geometry-data-type.md)  
  
 **返回多边形实例的外环**  
 [STExteriorRing](../../t-sql/spatial-geometry/stexteriorring-geometry-data-type.md)  
  
 **返回多边形的内环数**  
 [STNumInteriorRing](../../t-sql/spatial-geometry/stnuminteriorring-geometry-data-type.md)  
  
 **返回多边形的指定内环**  
 [STInteriorRingN](../../t-sql/spatial-geometry/stinteriorringn-geometry-data-type.md)  
  
  
###  <a name="spatial-reference-id-srid"></a><a name="srid"></a> 空间引用标识符 (SRID)  
 空间引用标识符 (SRID) 是指定 **geometry** 实例所在的坐标系的标识符。 两个拥有不同 SRID 的实例是不可比的。  
  
 **设置或返回实例的 SRID**  
 [STSrid](../../t-sql/spatial-geometry/stsrid-geometry-data-type.md)  
  
> [!NOTE]
> 此属性可以进行修改。  
  
##  <a name="determining-relationships-between-geometry-instances"></a><a name="rel"></a> 确定几何图形实例之间的关系  
 **geometry** 数据类型提供了许多内置方法，你可以使用这些方法确定两个 **geometry** 实例之间的关系。  
  
 **确定两个实例是否包含相同的点集**  
 [STEquals](../../t-sql/spatial-geometry/stequals-geometry-data-type.md)  
  
 **确定两个实例是否不相接**  
 [STDisjoint](../../t-sql/spatial-geometry/stdisjoint-geometry-data-type.md)  
  
 **确定两个实例是否相交**  
 [STIntersects](../../t-sql/spatial-geometry/stintersects-geometry-data-type.md)  
  
 **确定两个实例是否接触**  
 [STTouches](../../t-sql/spatial-geometry/sttouches-geometry-data-type.md)  
  
 **确定两个实例是否重叠**  
 [STOverlaps](../../t-sql/spatial-geometry/stoverlaps-geometry-data-type.md)  
  
 **确定两个实例是否交叉**  
 [STCrosses](../../t-sql/spatial-geometry/stcrosses-geometry-data-type.md)  
  
 **确定某个实例是否在另一个实例内部**  
 [STWithin](../../t-sql/spatial-geometry/stwithin-geometry-data-type.md)  
  
 **确定某个实例是否包含另一个实例**  
 [STContains](../../t-sql/spatial-geometry/stcontains-geometry-data-type.md)  
  
 **确定某个实例是否与另一个实例重叠**  
 [STOverlaps](../../t-sql/spatial-geometry/stoverlaps-geometry-data-type.md)  
  
 **确定两个实例是否存在空间关系**  
 [STRelate](../../t-sql/spatial-geometry/strelate-geometry-data-type.md)  
  
 **确定两个几何图形中的点之间的最短距离**  
 [STDistance](../../t-sql/spatial-geometry/stdistance-geometry-data-type.md)  
  
##  <a name="geometry-instances-default-to-zero-srid"></a><a name="defaultsrid"></a> 几何图形实例默认 SRID 为零  
 **中** geometry [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的默认 SRID 为 0。 利用 **geometry** 空间数据，执行计算是不需要空间实例的指定 SRID 的；因此，实例可驻留在未定义的平面空间。 若要在 **geometry** 数据类型方法的计算中指明未定义的平面空间， [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 使用 SRID 0。  
  
##  <a name="examples"></a><a name="examples"></a> 示例  
以下两个示例显示了如何添加和查询几何图形数据。  
  
### <a name="example-a"></a>示例 A。
此示例创建了一个带有标识列和 `geometry` 列 `GeomCol1` 的表。 第三列将 `geometry` 列呈现为其开放地理空间信息联盟 (OGC) 熟知文本 (WKT) 表示形式，并使用 `STAsText()` 方法。 接下来将插入两行：一行包含 `LineString` 类型的 `geometry`实例，一行包含 `Polygon` 实例。  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
DROP TABLE dbo.SpatialTable;  
GO  

CREATE TABLE SpatialTable   
  ( id int IDENTITY (1,1),  
    GeomCol1 geometry,   
    GeomCol2 AS GeomCol1.STAsText() 
  );  
GO  

INSERT INTO SpatialTable (GeomCol1)  
VALUES (geometry::STGeomFromText('LINESTRING (100 100, 20 180, 180 180)', 0));  

INSERT INTO SpatialTable (GeomCol1)  
VALUES (geometry::STGeomFromText('POLYGON ((0 0, 150 0, 150 150, 0 150, 0 0))', 0));  
GO  
```  
  
### <a name="example-b"></a>示例 B。
此示例使用 `STIntersection()` 方法返回此前插入的两个 `geometry` 实例相交的点。  
  
```sql  
DECLARE @geom1 geometry;  
DECLARE @geom2 geometry;  
DECLARE @result geometry;  

SELECT @geom1 = GeomCol1 FROM SpatialTable WHERE id = 1;  
SELECT @geom2 = GeomCol1 FROM SpatialTable WHERE id = 2;  
SELECT @result = @geom1.STIntersection(@geom2);  
SELECT @result.STAsText();  
```  
  
## <a name="see-also"></a>另请参阅  
 [空间数据 (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
