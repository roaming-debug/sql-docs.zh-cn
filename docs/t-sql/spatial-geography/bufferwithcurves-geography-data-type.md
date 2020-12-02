---
description: BufferWithCurves（geography 数据类型）
title: BufferWithCurves（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BufferWithCurves
- BufferWithCurves_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- BufferWithCurves method (geography)
ms.assetid: abf0a11c-c99c-4faa-bf80-3ae8e04d7bfb
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 4f4c5943dd69d651a9038e5e8e27798115911a9c
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124352"
---
# <a name="bufferwithcurves-geography-data-type"></a>BufferWithCurves（geography 数据类型）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  返回一个 geography 实例，它表示与执行调用的 geography 实例的距离小于或等于 distance 参数的所有点的集合。  
  
## <a name="syntax"></a>语法  
  
```  
  
.BufferWithCurves ( distance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *distance*  
 一个 **float**，它表示组成缓冲区的点与 geography 实例之间的最大距离。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
 CLR 返回类型：SqlGeography  
  
## <a name="exceptions"></a>例外  
 以下条件将引发 ArgumentException。  
  
-   没有为方法传递任何参数，例如，`@g.BufferWithCurves()`  
  
-   为方法传递了非数值参数，例如，`@g.BufferWithCurves('a')`  
  
-   向方法传递了 NULL，例如，`@g.BufferWithCurves(NULL)`  
  
## <a name="remarks"></a>注解  
 下表显示为不同距离值返回的结果。  
  
|距离值|类型维度|返回的空间类型|  
|--------------------|---------------------|---------------------------|  
|距离 < 0|0 或 1|空的 GeometryCollection 实例|  
|distance \< 0|2 或更大|具有负缓冲区的 CurvePolygon 或 GeometryCollection 实例。<br /><br /> 注意：负缓冲区可能会创建空的 GeometryCollection|
|距离 = 0|所有维度|执行调用的 **geography** 实例的副本|  
|distance > 0|所有维度|CurvePolygon 或 GeometryCollection 实例|  
  
> [!NOTE]  
>  由于 distance 的类型为 float，因此，很小的值可能在计算中等于零。  如果发生这种情况，则会返回执行调用的 geography 实例的副本。  
  
 如果将 string 参数传递给方法，则会将其转换为 float；否则，将引发 `ArgumentException`。  
  
## <a name="examples"></a>示例  
  
### <a name="a-calling-bufferwithcurves-with-a-parameter-value--0-on-one-dimensional-geography-instance"></a>A. 使用 < 0 的参数值对一维 geography 实例调用 BufferWithCurves()  
 以下示例返回一个空 `GeometryCollection` 实例：  
  
 ```sql
 DECLARE @g geography= 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves(-1).ToString();
``` 
  
### <a name="b-calling-bufferwithcurves-with-a-parameter-value--0-on-a-two-dimensional-geography-instance"></a>B. 使用 < 0 的参数值对二维 geography 实例调用 BufferWithCurves()  
 以下示例返回一个具有负缓冲区的 `CurvePolygon` 实例：  
  
 ```sql
 DECLARE @g geography = 'CURVEPOLYGON(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.BufferWithCurves(-1).ToString()
 ```  
  
### <a name="c-calling-bufferwithcurves-with-a-parameter-value--0-that-returns-an-empty-geometrycollection"></a>C. 使用 < 0 的参数值调用 BufferWithCurves()，它返回一个空 GeometryCollection  
 以下示例显示当 distance 参数等于 -2 时出现的情况：  
  
 ```sql
 DECLARE @g geography = 'CURVEPOLYGON(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.BufferWithCurves(-2).ToString();
 ```  
  
 此 SELECT 语句返回 `GEOMETRYCOLLECTION EMPTY`  
  
### <a name="d-calling-bufferwithcurves-with-a-parameter-value--0"></a>D. 使用 = 0 的参数值调用 BufferWithCurves()  
 以下示例返回执行调用的 geography 实例的副本：  

 ```sql
 DECLARE @g geography = 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves(0).ToString();
 ```  
  
### <a name="e-calling-bufferwithcurves-with-a-non-zero-parameter-value-that-is-extremely-small"></a>E. 使用极小的非零参数值调用 BufferWithCurves()  
 以下示例还返回执行调用的 geography 实例的副本：  

 ```sql
 DECLARE @g geography = 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 DECLARE @distance float = 1e-20;  
 SELECT @g.BufferWithCurves(@distance).ToString();
 ```  
  
### <a name="f-calling-bufferwithcurves-with-a-parameter-value--0"></a>F. 使用 > 0 的参数值调用 BufferWithCurves()  
 以下示例返回 `CurvePolygon` 实例：  

 ```sql
 DECLARE @g geography= 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves(2).ToString();
 ```  
### <a name="g-passing-a-valid-string-parameter"></a>G. 传递有效的字符串参数  
 以下示例返回与前面相同的 `CurvePolygon` 实例，但为方法传递一个字符串参数：  

 ```sql
 DECLARE @g geography= 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.BufferWithCurves('2').ToString();
```  
  
### <a name="h-passing-an-invalid-string-parameter"></a>H. 传递无效的字符串参数  
 以下示例将引发错误：  

 ```sql
 DECLARE @g geography = 'LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)'  
 SELECT @g.BufferWithCurves('a').ToString();
 ```  
  
 请注意，前面的两个示例为 `BufferWithCurves()` 方法传递字符串文字。 第一个示例有效，因为可以将字符串文字转换为数值。 但第二个示例引发 `ArgumentException`。  
  
## <a name="see-also"></a>另请参阅  
 [Geography 实例上的扩展方法](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [BufferWithCurves（geometry 数据类型）](../../t-sql/spatial-geometry/bufferwithcurves-geometry-data-type.md)  
  
  
