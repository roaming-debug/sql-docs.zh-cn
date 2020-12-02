---
description: STCurveN（geography 数据类型）
title: STCurveN（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STCurveN
- STCurveN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STCurveN method (geography)
ms.assetid: 99ef7100-2c4b-4f07-8d66-b343da94b023
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 18d3a992b3f3d5eeecb09a16ce3fb6d582ee2fb2
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88479333"
---
# <a name="stcurven-geography-data-type"></a>STCurveN（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回从 geography 实例中指定的曲线，该实例的数据类型为 LineString、CircularString 或 CompoundCurve。  
  
## <a name="syntax"></a>语法  
  
```  
  
.STCurveN( n )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *n*  
 一个 int 表达式，其值介于 1 与 geography 实例中的曲线数之间。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
 CLR 返回类型：SqlGeography  
  
## <a name="exceptions"></a>例外  
 如果 n < 1，则会引发 ArgumentOutOfRangeException。  
  
## <a name="remarks"></a>备注  
 如果满足以下条件，则会返回 NULL。  
  
-   已声明 geography 实例，但未将其实例化  
  
-   geography 实例为空  
  
-   n 超过 geography 实例中的曲线数目（请参阅 [STNumCurves（geography 数据类型）](../../t-sql/spatial-geography/stnumcurves-geography-data-type.md)）  
  
-   geography 实例的维度不相等（请参阅 [STDimension（geography 数据类型）](../../t-sql/spatial-geography/stdimension-geography-data-type.md)）  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-stcurven-on-a-circularstring"></a>A. 在 CircularString 上使用 STCurveN()  
 以下示例返回 CircularString 实例中的第二条曲线：  
  
```
 DECLARE @g geography = 'CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 该示例返回：  
  
 `CIRCULARSTRING (-122.348 47.658, -122.358 47.658, -122.358 47.653)`  
  
### <a name="b-using-stcurven-on-a-compoundcurve"></a>B. 在 CompoundCurve 上使用 STCurveN()  
 以下示例返回 CompoundCurve 实例中的第二条曲线：  
  
```
 DECLARE @g geography = 'COMPOUNDCURVE(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 该示例返回：  
  
 `CIRCULARSTRING (-122.348 47.658, -122.358 47.658, -122.358 47.653)`  
  
### <a name="c-using-stcurven-on-a-compoundcurve-containing-three-circularstrings"></a>C. 在包含三个 CircularString 的 CompoundCurve 实例上使用 STCurveN()  
 以下示例使用 CompoundCurve 实例，该实例将三个单独的 CircularString 实例合并为与上一示例相同的曲线序列：  
  
```
 DECLARE @g geography = 'COMPOUNDCURVE (CIRCULARSTRING (-122.358 47.653, -122.348 47.649, -122.348 47.658), CIRCULARSTRING(-122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 该示例返回：  
  
 `CIRCULARSTRING (-122.348 47.658, -122.358 47.658, -122.358 47.653)`  
  
 `STCurveN()` 返回相同的结果，而无论使用的是哪种熟知文本 （WKT） 格式。  
  
### <a name="d-testing-for-validity-before-calling-stcurve"></a>D. 在调用 STCurve() 之前测试有效性  
 以下示例说明如何在调用 STCurveN() 方法之前确保 n 有效：  
  
```
 DECLARE @g geography;  
 DECLARE @n int;  
 SET @n = 2;  
 SET @g = geography::Parse('LINESTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)');  
 IF @n >= 1 AND @n <= @g.STNumCurves()  
 BEGIN  
 SELECT @g.STCurveN(@n).ToString();  
 END
  ```  
  
## <a name="see-also"></a>另请参阅  
 [Geography 实例上的 OGC 方法](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
