---
description: STCurveN（geometry 数据类型）
title: STCurveN（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- STCurveN method (geometry)
ms.assetid: 64adf1a1-3a41-41fb-b7d1-44390c3e4ea9
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 56ecf81209f125958a72e57b7dc70409d492e5ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187679"
---
# <a name="stcurven-geometry-data-type"></a>STCurveN（geometry 数据类型）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

返回从 geometry 实例中指定的曲线，该实例的数据类型为 LineString、CircularString、CompoundCurve 或 MultiLineString。
  
## <a name="syntax"></a>语法  
  
```  
  
.STCurveN ( curve_index )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *curve_index*  
 一个 **int** 表达式，其值介于 1 和 **geometry** 实例中的曲线数之间。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geometry  
  
 CLR 返回类型：SqlGeometry  
  
## <a name="exceptions"></a>例外  
 如果 *curve_index* < 1，则引发 `ArgumentOutOfRangeException`。  
  
## <a name="remarks"></a>备注  
 如果出现以下任何情况，则会返回 **NULL**：  
  
-   声明 **geometry** 实例，但未将其实例化  
  
-   **geometry** 实例为空  
  
-   *curve_index* 超过 **geometry** 实例中的曲线数  
  
-   **geometry** 实例为 **Point**、**MultiPoint**、**Polygon**、**CurvePolygon** 或 **MultiPolygon**  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-stcurven-on-a-circularstring-instance"></a>A. 在 CircularString 实例上使用 STCurveN()  
 以下示例返回 `CircularString` 实例中的第二条曲线：  
  
```
 DECLARE @g geometry = 'CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 本主题前面的示例返回：  
  
 `CIRCULARSTRING (3 6.3246, 0 7, -3 6.3246)`  
  
### <a name="b-using-stcurven-on-a-compoundcurve-instance-with-one-circularstring-instance"></a>B. 在具有一个 CircularString 实例的 CompoundCurve 实例上使用 STCurveN()  
 以下示例返回 `CompoundCurve` 实例中的第二条曲线：  
  
```
 DECLARE @g geometry = 'COMPOUNDCURVE(CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0))';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 本主题前面的示例返回：  
  
 `CIRCULARSTRING (3 6.3246, 0 7, -3 6.3246)`  
  
### <a name="c-using-stcurven-on-a-compoundcurve-instance-with-three-circularstring-instances"></a>C. 在具有三个 CircularString 实例的 CompoundCurve 实例上使用 STCurveN()  
 以下示例使用 `CompoundCurve` 实例，该实例将三个单独的 `CircularString` 实例合并为与上一示例相同的曲线序列：  
  
```
 DECLARE @g geometry = 'COMPOUNDCURVE (CIRCULARSTRING (0 0, 1 2.1082, 3 6.3246), CIRCULARSTRING(3 6.3246, 0 7, -3 6.3246), CIRCULARSTRING(-3 6.3246, -1 2.1082, 0 0))';  
 SELECT @g.STCurveN(2).ToString();
 ```  
  
 本主题前面的示例返回：  
  
 `CIRCULARSTRING (3 6.3246, 0 7, -3 6.3246)`  
  
 请注意，前面的三个示例的结果是相同的。 无论使用哪种 WKT（熟知文本）格式输入相同的曲线序列，在使用 `STCurveN()` 实例时，`CompoundCurve` 返回的结果都是相同的。  
  
### <a name="d-validating-the-parameter-before-calling-stcurven"></a>D. 在调用 STCurveN() 之前验证参数  
 以下示例说明如何在调用 `STCurveN()` 方法之前确保 `@n` 有效：  
  
```
 DECLARE @g geometry;  
 DECLARE @n int;  
 SET @n = 3;  
 SET @g = geometry::Parse('CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)');  
 IF @n >= 1 AND @n <= @g.STNumCurves()  
 BEGIN  
 SELECT @g.STCurveN(@n).ToString();  
 END
 ```  
  
## <a name="see-also"></a>另请参阅  
 [STNumCurves（geometry 数据类型）](../../t-sql/spatial-geometry/stnumcurves-geometry-data-type.md)   
 [几何图形实例上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

