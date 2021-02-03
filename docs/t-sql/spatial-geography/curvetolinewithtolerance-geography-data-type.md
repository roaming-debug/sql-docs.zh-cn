---
description: CurveToLineWithTolerance（geography 数据类型）
title: CurveToLineWithTolerance（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CurveToLineWithTolerance_TSQL
- CurveToLineWithTolerance
dev_langs:
- TSQL
helpviewer_keywords:
- CurveToLineWithTolerance method (geography)
ms.assetid: 74369c76-2cf6-42ae-b9cc-e7a051db2767
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: f19dbc100f507d10a3c1c382735608b307a40ffd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210605"
---
# <a name="curvetolinewithtolerance-geography-data-type"></a>CurveToLineWithTolerance（geography 数据类型）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

返回包含圆弧线段的 geography 实例的多边形近似值。  
  
## <a name="syntax"></a>语法  
  
```  
  
.CurveToLineWithTolerance( tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
tolerance  
一个 double 表达式，它定义原始圆弧线段与其线性近似值之间的最大误差。  
  
_relative_  
一个 bool 表达式，它指示是否使用偏差的相对最大值。 如果 relative 设置为 false (0)，则为可能具有线性近似的偏差设置绝对最大值。 如果 relative 设置为 true (1)，则按 tolerance 参数与空间对象边界框直径的乘积来计算容差。  
  
## <a name="return-types"></a>返回类型  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
CLR 返回类型：SqlGeography  
  
## <a name="exceptions"></a>例外  
将 tolerance 设置为 <= 0 会引发 **ArgumentOutOfRange** 异常。  
  
## <a name="remarks"></a>备注  
此方法允许为生成的 **LineString** 指定公差大小。  
  
CurveToLineWithTolerance 方法将为 CircularString 或 CompoundCurve 实例返回 LineString 实例，为 CurvePolygon 实例返回 Polygon 实例。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-different-tolerance-values-on-a-circularstring-instance"></a>A. 在 CircularString 实例上使用不同的公差值  
以下示例说明设置容差如何影响从 `CircularString` 实例返回的 `LineString` 实例：  
  
```
DECLARE @g geography;  
SET @g = geography::Parse('CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)');  
SELECT @g.CurveToLineWithTolerance(0.1,0).STNumPoints(), @g.CurveToLineWithTolerance(0.01, 0).STNumPoints();
```  
  
### <a name="b-using-the-method-on-a-multilinestring-instance-containing-one-linestring"></a>B. 在包含一个 LineString 的 MultiLineString 实例上使用该方法  
以下示例显示从仅包含一个 `MultiLineString` 实例的 `LineString` 实例中返回的内容：  
  
```
DECLARE @g geography;  
SET @g = geography::Parse('MULTILINESTRING((-122.358 47.653, -122.348 47.649))');  
SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
```  
  
### <a name="c-using-the-method-on-a-multilinestring-instance-containing-multiple-linestrings"></a>C. 在包含多个 LineString 的 MultiLineString 实例上使用该方法  
以下示例显示从包含多个 `MultiLineString` 实例的 `LineString` 实例中返回的内容：  
  
```
DECLARE @g geography;  
SET @g = geography::Parse('MULTILINESTRING((-122.358 47.653, -122.348 47.649),(-123.358 47.653, -123.348 47.649))');  
SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
```  
  
### <a name="d-setting-relative-to-true-for-an-invoking-curvepolygon-instance"></a>D. 对于调用 CurvePolygon 实例，将 relative 设置为 true  
以下示例使用 `CurvePolygon` 实例调用 `CurveToLineWithTolerance()` 并将 relative 设置为 true：  
  
```
DECLARE @g geography = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658), (-122.348 47.658, -122.358 47.658, -122.358 47.653)))';  
SELECT @g.CurveToLineWithTolerance(.5,1).ToString();
```  
  
## <a name="see-also"></a>另请参阅  
[地理实例上的扩展方法](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
