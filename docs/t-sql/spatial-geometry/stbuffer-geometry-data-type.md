---
description: STBuffer（geometry 数据类型）
title: STBuffer （geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STBuffer (geometry Data Type)
- STBuffer_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STBuffer (geometry Data Type)
ms.assetid: ca6bf2dc-1d38-4503-b87e-f2ea033d36ba
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 787612ae8cdb2cc5751818c31f111831e4013cbf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187228"
---
# <a name="stbuffer-geometry-data-type"></a>STBuffer（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回一个几何图形对象，该对象表示所有与 geometry 实例的距离小于或等于指定值的点的并集。
  
## <a name="syntax"></a>语法  
  
```  
  
.STBuffer ( distance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *distance*  
 类型为 float（在 .NET Framework 中为 double）的值，用于指定与围绕其计算缓冲区的几何图形实例的距离。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geometry  
  
 CLR 返回类型：SqlGeometry  
  
## <a name="remarks"></a>注解  
 `STBuffer()` 计算缓冲区的方式与 [BufferWithTolerance](../../t-sql/spatial-geometry/bufferwithtolerance-geometry-data-type.md) 相似，指定 tolerance = distance\* .001 和 relative  = false。  
  
 如果 distance > 0，则返回 Polygon 或 MultiPolygon 实例。  
  
> [!NOTE]  
>  由于 distance 的类型为 float，因此，很小的值可能在计算中等于零。  如果发生这种情况，则会返回执行调用的 geometry 实例的副本。  请参阅 [float 和 real (Transact-SQL)](../../t-sql/data-types/float-and-real-transact-sql.md)  
  
 如果 distance = 0，则会返回调用 geometry 实例的副本。  
  
 如果 distance < 0，则  
  
-   当实例维度为 0 或 1 时，将返回空 GeometryCollection 实例。  
  
-   当实例维度为 2 或更大时，将返回负缓冲区。  
  
    > [!NOTE]  
    >  负缓冲区还可能会创建空的 GeometryCollection 实例。  
  
 负缓冲区删除距几何图形边界给定距离的所有点。  
  
 理论缓冲区与所计算的缓冲区之间的误差是 max(tolerance, extents * 1.E-7)，其中 tolerance = distance \* .001。 有关盘区的详细信息，请参阅 [geometry 数据类型方法引用](./spatial-types-geometry-transact-sql.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-calling-stbuffer-with-parameter_value--0-on-one-dimensional-geometry-instance"></a>A. 使用 < 0 的参数值在一维 geometry 实例上调用 STBuffer()  
 以下示例返回一个空 `GeometryCollection` 实例：  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer(-1).ToString();
 ```  
  
### <a name="b-calling-stbuffer-with-parameter_value--0-on-a-polygon-instance"></a>B. 使用 < 0 的参数值在 Polygon 实例上调用 STBuffer()  
 以下示例返回一个具有负缓冲区的 `Polygon` 实例：  
  
```
 DECLARE @g geometry = 'POLYGON((1 1, 1 5, 5 5, 5 1, 1 1))'; 
 SELECT @g.STBuffer(-1).ToString();
 ```  
  
### <a name="c-calling-stbuffer-with-parameter_value--0-on-a-curvepolygon-instance"></a>C. 使用 < 0 的参数值在 CurvePolygon 实例上调用 STBuffer()  
 以下示例从 `Polygon` 实例中返回一个具有负缓冲区的 `CurvePolygon` 实例：  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.STBuffer(-1).ToString();
 ```  
  
> [!NOTE]  
>  将返回 `Polygon` 实例，而非 `CurvePolygon` 实例。  若要返回 `CurvePolygon` 实例，请参阅 [BufferWithCurves（geometry 数据类型）](../../t-sql/spatial-geometry/bufferwithcurves-geometry-data-type.md)  
  
### <a name="d-calling-stbuffer-with-a-negative-parameter-value-that-returns-an-empty-instance"></a>D. 使用返回一个空实例的负参数值调用 STBuffer()  
 以下示例说明了针对前一实例，distance 参数等于 -2 时发生的情况。  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.STBuffer(-2).ToString();
 ```  
  
 此 SELECT 语句返回 `GEOMETRYCOLLECTION EMPTY.`  
  
### <a name="e-calling-stbuffer-with-parameter_value--0"></a>E. 当参数值 = 0 时调用 STBuffer()  
 以下示例返回调用 `geometry` 实例的副本：  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer(0).ToString();
 ```  
  
### <a name="f-calling-stbuffer-with-a-non-zero-parameter-value-that-is-extremely-small"></a>F. 使用极小的非零参数值调用 STBuffer()  
 以下示例还返回调用 `geometry` 实例的副本：  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)';  
 DECLARE @distance float = 1e-20;  
 SELECT @g.STBuffer(@distance).ToString();
 ```  
  
### <a name="g-calling-stbuffer-with-parameter_value--0"></a>G. 使用 > 0 的参数值调用 STBuffer()  
 以下示例返回 `Polygon` 实例：  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer(2).ToString();
 ```  
  
### <a name="h-calling-stbuffer-with-a-string-parameter-value"></a>H. 使用字符串参数值调用 STBuffer()  
 以下示例返回与前面相同的 `Polygon` 实例，但为方法传递一个字符串参数：  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer('2').ToString();
 ```  
  
 以下示例将引发错误：  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.STBuffer('a').ToString();
 ```  
  
> [!NOTE]  
>  前面的两个示例为 `STBuffer()` 方法传递字符串文字。  第一个示例有效，因为可以将字符串文字转换为数值。 但第二个示例引发 `ArgumentException`。  
  
### <a name="i-calling-stbuffer-on-a-multipoint-instance"></a>I. 对 MultiPoint 实例调用 STBuffer()  
 以下示例返回两个 `MultiPolygon` 实例和一个 `Polygon` 实例。  
  
```
 DECLARE @g geometry = 'MULTIPOINT((1 1),(1 4))'; 
 SELECT @g.STBuffer(1).ToString(); 
 SELECT @g.STBuffer(1.5).ToString(); 
 SELECT @g.STBuffer(1.6).ToString();
 ```  
  
 前两个 SELECT 语句返回一个 `MultiPolygon` 实例，因为 distance 参数小于或等于两个点 (1 1) 和 (1 4) 之间的距离的 1/2。 第三个 SELECT 语句返回一个 `Polygon` 实例，因为两个点 (1 1) 和 (1 4) 的缓冲实例发生重叠。  
  
## <a name="see-also"></a>另请参阅  
 [BufferWithTolerance（geometry 数据类型）](../../t-sql/spatial-geometry/bufferwithtolerance-geometry-data-type.md)   
 [几何图形实例上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
