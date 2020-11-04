---
description: BufferWithCurves（geometry 数据类型）
title: BufferWithCurves（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- BufferWithCurves method (geometry)
ms.assetid: 8ffaba3f-d2dd-4e57-9f41-3ced9f14b600
author: MladjoA
ms.author: mlandzic
monikerRange: =azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b1249cafc3fd10cdded36ed7d73f190c38d02f5d
ms.sourcegitcommit: 442fbe1655d629ecef273b02fae1beb2455a762e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235517"
---
# <a name="bufferwithcurves-geometry-data-type"></a>BufferWithCurves（geometry 数据类型）
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  返回一个 geometry 实例，它表示与调用 geometry 实例的距离小于或等于 distance 参数的所有点的集合。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
.BufferWithCurves ( distance )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数  
 *distance*  
 一个 float，它表示组成缓冲区的点与 geometry 实例之间的最大距离。  
  
## <a name="return-types"></a>返回类型
SQL Server 返回类型：geometry  
  
 CLR 返回类型：SqlGeometry  
  
## <a name="exceptions"></a>例外  
 以下条件将引发 ArgumentException。  
  
-   没有为方法传递任何参数，例如，`@g.BufferWithCurves()`  
  
-   为方法传递了非数值参数，例如，`@g.BufferWithCurves('a')`  
  
-   向方法传递了 NULL，例如，`@g.BufferWithCurves(NULL)`  
  
## <a name="remarks"></a>备注  
 下图显示此方法返回的示例 geometry 实例。  
  
 ![显示此方法返回的示例 geometry 实例的示意图。](../../t-sql/spatial-geometry/media/bufferedcurve.gif)
  
 下表显示为不同距离值返回的结果。  
  
|距离值|类型维度|返回的空间类型|  
|--------------------|---------------------|---------------------------|  
|距离 < 0|0 或 1|空的 GeometryCollection 实例|  
|距离 < 0|2 或更大|具有负缓冲区的 CurvePolygon 或 GeometryCollection 实例。 **注意：** 负缓冲区可能会创建空的“GeometryCollection”|  
|距离 = 0|所有维度|调用 geometry 实例的副本|  
|distance > 0|所有维度|CurvePolygon 或 GeometryCollection 实例|  
  
> [!NOTE]  
>  由于 distance 的类型为 float，因此，很小的值可能在计算中等于零。 如果发生这种情况，则会返回调用 geometry 实例的副本。 请参阅 [float 和 real (Transact-SQL)](../../t-sql/data-types/float-and-real-transact-sql.md)。  
  
 负缓冲区删除距几何图形边界给定距离内的所有点。 下图以圆的明暗交错区域显示负缓冲区。 虚线是原始多边形的边界，实线是结果多边形的边界。  
  
 如果将 string 参数传递给方法，则会将其转换为 float；否则，将引发 `ArgumentException`。  
  
## <a name="examples"></a>示例  
  
### <a name="a-calling-bufferwithcurves-with-a-parameter-value--0-on-one-dimensional-geometry-instance"></a>A. 使用 < 0 的参数值对一维 geometry 实例调用 BufferWithCurves()  
 以下示例返回一个空 `GeometryCollection` 实例：  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(-1).ToString(); 
 ```
  
### <a name="b-calling-bufferwithcurves-with-a-parameter-value--0-on-a-two-dimensional-geometry-instance"></a>B. 使用 < 0 的参数值对二维 geometry 实例调用 BufferWithCurves()  
 以下示例返回一个具有负缓冲区的 `CurvePolygon` 实例：  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.BufferWithCurves(-1).ToString()
 ```  
  
### <a name="c-calling-bufferwithcurves-with-a-parameter-value--0-that-returns-an-empty-geometrycollection"></a>C. 使用 < 0 的参数值调用 BufferWithCurves()，它返回一个空 GeometryCollection  
 以下示例显示当 distance 参数等于 -2 时出现的情况：  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.BufferWithCurves(-2).ToString();
 ```  
  
 此 SELECT 语句返回 `GEOMETRYCOLLECTION EMPTY`  
  
### <a name="d-calling-bufferwithcurves-with-a-parameter-value--0"></a>D. 使用 = 0 的参数值调用 BufferWithCurves()  
 以下示例返回调用 geometry 实例的副本：  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(0).ToString();
 ```  
  
### <a name="e-calling-bufferwithcurves-with-a-non-zero-parameter-value-that-is-extremely-small"></a>E. 使用极小的非零参数值调用 BufferWithCurves()  
 以下示例还返回调用 geometry 实例的副本：  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 DECLARE @distance float = 1e-20; 
 SELECT @g.BufferWithCurves(@distance).ToString();
 ```  
  
### <a name="f-calling-bufferwithcurves-with-a-parameter-value--0"></a>F. 使用 > 0 的参数值调用 BufferWithCurves()  
 以下示例返回 `CurvePolygon` 实例：  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(2).ToString();
 ```  
  
### <a name="g-passing-a-valid-string-parameter"></a>G. 传递有效的字符串参数  
 以下示例返回与前面相同的 `CurvePolygon` 实例，但为方法传递一个字符串参数：  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves('2').ToString();
 ```  
  
### <a name="h-passing-an-invalid-string-parameter"></a>H. 传递无效的字符串参数  
 以下示例将引发错误：  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)' 
 SELECT @g.BufferWithCurves('a').ToString();
 ```  
  
 请注意，前面的两个示例为 `BufferWithCurves()` 方法传递字符串文字。 第一个示例有效，因为可以将字符串文字转换为数值。 但第二个示例引发 `ArgumentException`。  
  
### <a name="i-calling-bufferwithcurves-on-multipoint-instance"></a>I. 对 MultiPoint 实例调用 BufferWithCurves()  
 以下示例返回两个 `GeometryCollection` 实例和一个 `CurvePolygon` 实例。  
  
```
 DECLARE @g geometry = 'MULTIPOINT((1 1),(1 4))'; 
 SELECT @g.BufferWithCurves(1).ToString(); 
 SELECT @g.BufferWithCurves(1.5).ToString(); 
 SELECT @g.BufferWithCurves(1.6).ToString();
 ```  
  
 前两个 SELECT 语句返回一个 `GeometryCollection` 实例，因为 distance 参数小于或等于两个点 (1 1) 和 (1 4) 之间的距离的 1/2。 第三个 SELECT 语句返回一个 `CurvePolygon` 实例，因为两个点 (1 1) 和 (1 4) 的缓冲实例发生重叠。  
  
## <a name="see-also"></a>另请参阅  
 [几何图形实例上的扩展方法](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
 
