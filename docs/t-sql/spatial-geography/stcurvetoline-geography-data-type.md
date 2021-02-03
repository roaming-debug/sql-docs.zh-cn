---
description: STCurveToLine（geography 数据类型）
title: STCurveToLine（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STCurveToLine_TSQL
- STCurveToLine
dev_langs:
- TSQL
helpviewer_keywords:
- STCurveToLine method (geography)
ms.assetid: 2f863a85-6168-465a-b32f-bb5e3de58dee
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d73859f9036fc22935e5cb59632d5ba189e417f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204667"
---
# <a name="stcurvetoline-geography-data-type"></a>STCurveToLine（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回包含圆弧线段的 geography 实例的多边形近似值。  
  
## <a name="syntax"></a>语法  
  
```  
  
.STCurveToLine()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
 CLR 返回类型：SqlGeography  
  
## <a name="remarks"></a>备注  
 为 CircularString 或 CompoundCurve 实例返回 LineString 实例。  
  
 为 CurvePolygon 实例返回 Polygon 实例。  
  
 返回不包含 CircularString、CompoundCurve、CurvePolygon 实例的 geography 实例的副本。  
  
 与 SQL MM 规范不同，此方法不使用 Z 坐标值计算多边形近似值。 忽略调用 geography 实例中存在的任何 Z 坐标值。  
  
## <a name="examples"></a>示例  
 以下示例返回作为 `LineString` 实例的多边形近似值的 `CircularString` 实例。  
  
```
 DECLARE @g1 geography = 'CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 DECLARE @g2 geography;  
 SET @g2 = @g1.STCurveToLine();  
 SELECT @g1.STNumPoints() AS G1, @g2.STNumPoints() AS G2;
 ```  
  
## <a name="see-also"></a>另请参阅  
 [STLength（geography 数据类型）](../../t-sql/spatial-geography/stlength-geography-data-type.md)   
 [STNumPoints（geography 数据类型）](../../t-sql/spatial-geography/stnumpoints-geography-data-type.md)   
 [空间数据类型概述](../../relational-databases/spatial/spatial-data-types-overview.md)  
  
  
