---
description: STIntersects（geography 数据类型）
title: STIntersects（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: synapse-analytics, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STIntersects (geography Data Type)
- STIntersects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STIntersects method
ms.assetid: c9db8b42-83c7-48c6-8963-fce54eb34c05
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 479d5fb2d33100ad19773b8b412bb678b9111313
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753877"
---
# <a name="stintersects-geography-data-type"></a>STIntersects（geography 数据类型）
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  如果 geography 实例与另一个 geography 实例相交，则返回 1。 否则，返回 0。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
.STIntersects ( other_geography )  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]
  
## <a name="arguments"></a>参数

other_geography  
与对其调用 `STIntersects()` 的实例进行比较的另一个 geography 实例。  
  
## <a name="return-types"></a>返回类型

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：bit  
  
 CLR 返回类型：SqlBoolean  
  
## <a name="remarks"></a>注解  
 如果 geography 实例的空间引用 ID (SRID) 不匹配，则此方法始终返回 Null 。  
  
## <a name="examples"></a>示例  
 以下示例使用 `STIntersects()` 确定两个 `geography` 实例是否彼此相交。  
  
```  
 DECLARE @g geography;  
 DECLARE @h geography;  
 SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);  
 SET @h = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
```  
  
 ```
 SELECT CASE @g.STIntersects(@h) 
 WHEN 1 THEN '@g intersects @h'  
 ELSE '@g does not intersect @h'  
 END;
 ```  
  
## <a name="see-also"></a>另请参阅  
 [Geography 实例上的 OGC 方法](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
