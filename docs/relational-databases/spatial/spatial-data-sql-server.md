---
description: 空间数据 (SQL Server)
title: 空间数据 (SQL Server) | Microsoft Docs
ms.date: 10/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- geography data type [SQL Server], spatial storage design
- planar spatial data [SQL Server], designing
- spatial data types [SQL Server]
- geodetic spatial data [SQL Server]
- geometry data type [SQL Server], spatial storage design
- spatial storage [SQL Server]
- geodetic spatial data [SQL Server], designing
ms.assetid: 41a132a1-09e2-4426-b9df-225270cb8e15
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a08d8a7c2449b1d9cb1f850290040c18d27a06a5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473148"
---
# <a name="spatial-data-sql-server"></a>空间数据 (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  空间数据表示有关物理位置和几何对象形状的信息。 这些对象可能是点位置或更复杂的对象，例如国家/地区、道路或湖泊。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持两种空间数据类型： **geometry** 数据类型和 **geography** 数据类型。  
  
-   **Geometry** 类型表示欧几里得（平面）坐标系中的数据。  
  
-   **Geography** 类型表示圆形地球坐标系中的数据。  
  
 这两种数据类型在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中都是作为 .NET 公共语言运行时 (CLR) 数据类型实现的。  
  
##  <a name="related-tasks"></a><a name="reltasks"></a> 相关任务  
 [创建、构造和查询几何图形实例](../../relational-databases/spatial/create-construct-and-query-geometry-instances.md)  
 介绍可用于 geometry 数据类型实例的方法。  
  
 [创建、构造和查询地理实例](../../relational-databases/spatial/create-construct-and-query-geography-instances.md)  
 介绍可用于 geography 数据类型实例的方法。  
  
 [为 Nearest Neighbor 查询空间数据](../../relational-databases/spatial/query-spatial-data-for-nearest-neighbor.md)  
 介绍用于查找与特定的空间对象最接近的空间对象的常见查询模式。  
  
 [创建、修改和删除空间索引](../../relational-databases/spatial/create-modify-and-drop-spatial-indexes.md)  
 提供有关创建、更改和删除空间索引的信息。  
  
## <a name="related-content"></a>相关内容  
 [空间数据类型概述](../../relational-databases/spatial/spatial-data-types-overview.md)  
 介绍空间数据类型。  
  
-   [Point](../../relational-databases/spatial/point.md)  
  
-   [LineString](../../relational-databases/spatial/linestring.md)  
  
-   [CircularString](../../relational-databases/spatial/circularstring.md)  
  
-   [CompoundCurve](../../relational-databases/spatial/compoundcurve.md)  
  
-   [多边形](../../relational-databases/spatial/polygon.md)  
  
-   [CurvePolygon](../../relational-databases/spatial/curvepolygon.md)  
  
-   [MultiPoint](../../relational-databases/spatial/multipoint.md)  
  
-   [MultiLineString](../../relational-databases/spatial/multilinestring.md)  
  
-   [MultiPolygon](../../relational-databases/spatial/multipolygon.md)  
  
-   [GeometryCollection](../../relational-databases/spatial/geometrycollection.md)  
  
 [空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)  
 介绍空间索引并描述分割和分割方案。  
  
  
