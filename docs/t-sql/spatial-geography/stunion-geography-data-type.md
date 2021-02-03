---
description: STUnion（geography 数据类型）
title: STUnion（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STUnion (geography Data Type)
- STUnion_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STUnion method
ms.assetid: 9bf87691-efd8-4c53-bd2f-eefe0acd19ca
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 1990e524b3816c20c374dca2f01e604411530303
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186441"
---
# <a name="stunion-geography-data-type"></a>STUnion（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回一个对象，它表示一个 **geography** 实例与另一个 **geography** 实例的并集。  
  
## <a name="syntax"></a>语法  
  
```  
  
.STUnion ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 other_geography  
 与调用 STUnion() 的实例形成并集的另一个 **geography** 实例。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
 CLR 返回类型：SqlGeography  
  
## <a name="exceptions"></a>例外  
 如果实例包含对拓边缘，此方法将引发 ArgumentException。  
  
## <a name="remarks"></a>备注  
 如果 geography 实例的空间引用标识符 (SRID) 不匹配，则此方法始终返回 null。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持大于半球的空间实例。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，服务器上可能返回的结果集已扩展到 FullGlobe 实例。  
  
 只有在输入实例包含圆弧线段时，结果才会包含圆弧线段。  
  
 此方法不精确。  
  
## <a name="examples"></a>示例  
  
### <a name="a-computing-the-union-of-two-polygons"></a>A. 计算两个多边形的并集  
 以下示例使用 `STUnion()` 计算两个 `Polygon` 实例的并集。  
  
```  
DECLARE @g geography;  
DECLARE @h geography;  
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);  
SET @h = geography::STGeomFromText('POLYGON((-122.351 47.656, -122.341 47.656, -122.341 47.661, -122.351 47.661, -122.351 47.656))', 4326);  
SELECT @g.STUnion(@h).ToString();  
```  
  
### <a name="b-producing-a-fullglobe-result"></a>B. 生成 FullGlobe 结果  
 以下示例在 `FullGlobe` 合并两个 `STUnion()` 实例时生成 `Polygon`。  
  
```
 DECLARE @g geography = 'POLYGON ((-122.358 47.653, -122.358 47.658,-122.348 47.658, -122.348 47.649, -122.358 47.653))';  
 DECLARE @h geography = 'POLYGON ((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.STUnion(@h).ToString();
 ```  
  
### <a name="c-producing-a-triagonal-hole-from-a-union-of-a-curvepolygon-and-a-triagonal-hole"></a>C. 基于 CurvePolygon 和一个三角形孔的并集生成一个三角形孔。  
 以下示例通过 `CurvePolygon` 和 `Polygon` 实例的并集生成一个三角形孔。  
  
```
 DECLARE @g geography = 'POLYGON ((-0.5 0, 0 1, 0.5 0.5, -0.5 0))';  
 DECLARE @h geography = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 0, 0.7 0.7, 0 1), (0 1, 0 0)))';  
 SELECT @g.STUnion(@h).ToString();
 ```  
  
## <a name="see-also"></a>另请参阅  
 [Geography 实例上的 OGC 方法](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
