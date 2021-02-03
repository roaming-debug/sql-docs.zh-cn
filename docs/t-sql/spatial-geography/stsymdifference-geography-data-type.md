---
description: STSymDifference（geography 数据类型）
title: STSymDifference（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STSymDifference (geography Data Type)
- STSymDifference_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STSymDifference (geography Data Type)
ms.assetid: 82bbfa2c-a61b-4f41-9bf8-6f720f363bae
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: e15a71c734543853a170b69cd9c0d380534af820
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172521"
---
# <a name="stsymdifference-geography-data-type"></a>STSymDifference（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回一个对象，它表示位于一个 **geography** 实例或另一个 **geography** 实例中的所有点，但不包含同时位于这两个实例中的点。  
  
## <a name="syntax"></a>语法  
  
```  
  
.STSymDifference ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 other_geography  
 调用 STSymDistance() 的实例以外的另一个 geography 实例。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
 CLR 返回类型：SqlGeography  
  
## <a name="remarks"></a>备注  
 如果 geography 实例的空间引用标识符 (SRID) 不匹配，则此方法始终返回 null。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持大于半球的空间实例。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，服务器上可能的结果集已扩展到 FullGlobe 实例。  
  
 只有在输入实例包含圆弧线段时，结果才会包含圆弧线段。  
  
## <a name="examples"></a>示例  
  
### <a name="a-computing-the-symmetric-difference-of-two-polygons"></a>A. 计算两个多边形的余集  
 以下示例使用 `STSymDifference()` 计算两个 `Polygon` 实例的余集。  
  
```  
DECLARE @g geography;  
DECLARE @h geography;  
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);  
SET @h = geography::STGeomFromText('POLYGON((-122.351 47.656, -122.341 47.656, -122.341 47.661, -122.351 47.661, -122.351 47.656))', 4326);  
SELECT @g.STSymDifference(@h).ToString();  
```  
  
### <a name="b-computing-the-symmetric-difference-with-fullglobe"></a>B. 计算与 FullGlobe 的余集  
 以下示例计算 `Polygon` 与 `FullGlobe` 的余集。  
  
```
 DECLARE @g geography = 'POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.STSymDifference('FULLGLOBE').ToString();
 ```  
  
## <a name="see-also"></a>另请参阅  
 [Geography 实例上的 OGC 方法](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
