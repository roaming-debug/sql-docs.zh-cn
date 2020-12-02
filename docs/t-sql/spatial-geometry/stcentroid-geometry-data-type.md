---
description: STCentroid（geometry 数据类型）
title: STCentroid（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STCentroid_TSQL
- STCentroid (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STCentroid (geometry Data Type)
ms.assetid: 4dc5a004-7a53-4cce-81dd-9f5e1dd0db78
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 5bcdb5f682ebd279af1d5314917f61290a88b07e
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88479261"
---
# <a name="stcentroid-geometry-data-type"></a>STCentroid（geometry 数据类型）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

返回由一个或多个多边形组成的 **geometry** 实例的几何中心。
  
## <a name="syntax"></a>语法  
  
```  
  
.STCentroid ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geometry  
  
 CLR 返回类型：SqlGeometry  
  
 开放地理空间信息联盟 (OGC) 类型：Point  
  
## <a name="remarks"></a>备注  
 如果 **geometry** 实例的类型不是 **Polygon、CurvePolygon** 或 **MultiPolygon**，则 `STCentroid()` 返回 null。  
  
## <a name="examples"></a>示例  
  
### <a name="a-computing-the-centroid-of-a-polygon-instance"></a>A. 计算 Polygon 实例的形心  
 以下示例使用 `STCentroid()` 计算 `polygon``geometry` 实例的形心：  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0),(2 2, 2 1, 1 1, 1 2, 2 2))', 0);  
SELECT @g.STCentroid().ToString();  
```  
  
### <a name="b-computing-the-centroid-of-a-curvepolygon-instance"></a>B. 计算 CurvePolygon 实例的形心  
 以下示例计算 `CurvePolygon` 实例的形心：  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(CIRCULARSTRING(0 4, 4 0, 8 4, 4 8, 0 4), CIRCULARSTRING(2 4, 4 2, 6 4, 4 6, 2 4))';  
 SELECT @g.STCentroid().ToString() AS Centroid
 ```  
  
## <a name="see-also"></a>另请参阅  
 [几何图形实例上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

