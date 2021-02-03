---
description: Reduce（geometry 数据类型）
title: Reduce（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Reduce_TSQL
- Reduce
dev_langs:
- TSQL
helpviewer_keywords:
- Reduce method
ms.assetid: 132184bf-c4d2-4a27-900d-8373445dce2a
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: dc6fbf1f3952e7ab59688c928eff3770776c308a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187716"
---
# <a name="reduce-geometry-data-type"></a>Reduce（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回给定“geometry”实例的近似值。 通过在具有给定容差的实例上运行 Douglas-Peucker 算法的扩展来生成近似值。
  
## <a name="syntax"></a>语法  
  
```  
  
.Reduce ( tolerance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 tolerance  
 类型为 float 的值  。 *tolerance* 是要为近似算法输入的公差。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geometry  
  
 CLR 返回类型：SqlGeometry  
  
## <a name="remarks"></a>备注  
 对于集合类型，此算法单独作用于包含在该实例中的每个 **geometry**。  
  
 此算法不会修改“Point”实例。  
  
 在“LineString”、“CircularString”和“CompoundCurve”实例上，近似算法保留实例的原始起点和终点。 接下来，算法以迭代方式从与结果偏离最大的原始实例中将此点添加回来。 此过程持续运行，直到没有点偏离超过给定的公差。  
  
 `Reduce()` 为 CircularString 实例返回 LineString、CircularString 或 CompoundCurve 实例。  `Reduce()` 为 **CompoundCurve** 实例返回 **CompoundCurve** 或 **LineString** 实例。  
  
 在 **Polygon** 实例中，近似算法独立应用于每个环。 如果返回的“Polygon”实例无效，该方法将生成 `FormatException`；例如，如果应用 `Reduce()` 的目的是简化实例中的每个环，并且所生成的环发生重叠，则会创建无效的“MultiPolygon”实例。  在有外环但无内环的“CurvePolygon”实例中，`Reduce()` 会返回“CurvePolygon”、“LineString”或“Point”实例。  如果 **CurvePolygon** 具有内环，则会返回 **CurvePolygon** 或 **MultiPoint** 实例。  
  
 找到圆弧段时，近似算法会检查是否可以通过在给定公差一半之内的弦得出弧的近似值。 对于满足此条件的弦，计算中会用该弦来替换圆弧。 如果弦不满足此条件，则保留圆弧，并将近似算法应用于其余的段。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-reduce-to-simplify-a-linestring"></a>A. 使用 Reduce() 简化 LineString  
 以下示例创建一个 `LineString` 实例，并使用 `Reduce()` 简化该实例。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 0 1, 1 0, 2 1, 3 0, 4 1)', 0);  
SELECT @g.Reduce(.75).ToString();  
```  
  
### <a name="b-using-reduce-with-varying-tolerance-levels-on-a-circularstring"></a>B. 对 CircularString 使用具有不同公差级别的 Reduce()  
 以下示例对 **CircularString** 实例使用具有三个公差级别的 `Reduce()`：  
  
```
 DECLARE @g geometry = 'CIRCULARSTRING(0 0, 8 8, 16 0, 20 -4, 24 0)'; 
 SELECT @g.Reduce(5).ToString(); 
 SELECT @g.Reduce(15).ToString(); 
 SELECT @g.Reduce(16).ToString();
 ```  
  
 该示例产生下面的输出：  
  
 ```
 CIRCULARSTRING (0 0, 8 8, 16 0, 20 -4, 24 0) 
 COMPOUNDCURVE (CIRCULARSTRING (0 0, 8 8, 16 0), (16 0, 24 0)) 
 LINESTRING (0 0, 24 0)
 ```  
  
 返回的每个实例都包含端点 (0 0) 和 (24 0)。  
  
### <a name="c-using-reduce-with-varying-tolerance-levels-on-a-compoundcurve"></a>C. 对 CompoundCurve 使用具有不同公差级别的 Reduce()  
 以下示例对 **CompoundCurve** 实例使用具有两个公差级别的 `Reduce()`：  
  
```
 DECLARE @g geometry = 'COMPOUNDCURVE(CIRCULARSTRING(0 0, 8 8, 16 0, 20 -4, 24 0),(24 0, 20 4, 16 0))';  
 SELECT @g.Reduce(15).ToString();  
 SELECT @g.Reduce(16).ToString();
 ```  
  
 在此示例中，请注意第二个 **SELECT** 语句返回 **LineString** 实例：`LineString(0 0, 16 0)`。  
  
### <a name="showing-an-example-where-the-original-start-and-end-points-are-lost"></a>显示原始起点和终点丢失的示例  
 下面的示例演示生成的实例为何无法保留原始的起点和终点。 出现该行为的原因是保留原始的起点和终点会产生无效的“LineString”实例。  
  
```  
DECLARE @g geometry = 'LINESTRING(0 0, 4 0, 2 .01, 1 0)';  
DECLARE @h geometry = @g.Reduce(1);  
SELECT @g.STIsValid() AS Valid  
SELECT @g.ToString() AS Original, @h.ToString() AS Reduced;  
```  
  
## <a name="see-also"></a>另请参阅  
 [扩展静态几何图形方法](../../t-sql/spatial-geometry/extended-static-geometry-methods.md)  
  
