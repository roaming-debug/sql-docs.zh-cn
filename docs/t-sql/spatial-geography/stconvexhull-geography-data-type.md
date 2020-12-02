---
description: STConvexHull（geography 数据类型）
title: STConvexHull（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- STConvexHull method (geography)
ms.assetid: fb435db7-31bb-4243-9d8b-35379184cfb4
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 0db7ad59585b2ff0c57526ca40f7caa3576e99fc
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88479324"
---
# <a name="stconvexhull-geography-data-type"></a>STConvexHull（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回表示 geography 实例的凸包的对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
.STConvexHull ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
 CLR 返回类型：SqlGeography  
  
## <a name="remarks"></a>备注  
 针对具有大于 90 度的信封角的 geography 实例返回一个 `FullGlobe` 对象。  
  
 针对空的 geography 实例返回空的 geography 集合。  
  
 为未初始化的 geography 实例返回 null。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-stconvexhull-on-an-uninitialized-geography-instance"></a>A. 对未初始化的 geography 实例使用 STConvexHull()  
 以下示例对未初始化的 geography 示例使用 `STConvexHull()`。  
  
```
 DECLARE @g geography;  
 SELECT @g.STConvexHull();
 ```  
  
### <a name="b-using-stconvexhull-on-an-empty-geography-instance"></a>B. 对空的 geography 实例使用 STConvexHull  
 以下示例对空的 `STConvexHull()` 实例使用 `Polygon`。  
  
```
 DECLARE @g geography = 'POLYGON EMPTY';  
 SELECT @g.STConvexHull().ToString();
 ```  
  
### <a name="c-finding-the-convex-hull-of-a-non-convex-polygon-instance"></a>C. 查找非凸 Polygon 实例的凸包  
 下面的示例使用 `STConvexHull()` 查找非凸 `Polygon` 实例的凸包。  
  
```  
 DECLARE @g geography;  
 SET @g = geography::Parse('POLYGON((-120.533 46.566, -118.283 46.1, -122.3 47.45, -120.533 46.566))');  
 SELECT @g.STConvexHull().ToString();  
```  
  
### <a name="d-finding-the-convex-hull-on-a-geography-instance-with-an-envelope-angle-larger-than-90-degrees"></a>D. 在一个信封角大于 90 度的 geography 实例上查找凸包  
 以下示例在一个信封角大于 90 度的 geography 实例上使用 `STConvexHull()`。  
  
```
 DECLARE @g geography = 'POLYGON((20.533 46.566, -18.283 46.1, -22.3 47.45, 20.533 46.566))';  
 SELECT @g.STConvexHull().ToString();
 ```  
  
  
