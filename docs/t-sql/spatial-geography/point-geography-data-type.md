---
description: Point（geography 数据类型）
title: Point（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 10/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Point
- Point_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Point method
- Point (geography Data Type)
ms.assetid: 0dc6f422-7aae-4016-b7f4-3289fa8f989c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 207b4c8fb9c9d171e7caad003e9b398c97dc4152
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210150"
---
# <a name="point-geography-data-type"></a>Point（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

根据 Point 实例的纬度值和经度值以及空间引用 ID (SRID) 构造一个表示该实例的 geography 实例   。
  
## <a name="syntax"></a>语法  
  
```  
  
Point ( Lat, Long, SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *Lat*  
 一个 float 表达式，表示正在生成的 Point 的 Y 坐标   。  
  
 *Long*  
 一个 float 表达式，表示正在生成的 Point 的 X 坐标   。 有关有效的纬度值和经度值的详细信息，请参阅 [Point](../../relational-databases/spatial/point.md)。  
  
 SRID   
 一个 int 表达式，表示希望返回的 geography 实例的[空间引用标识符](../../relational-databases/spatial/spatial-reference-identifiers-srids.md)   。  
  
> [!NOTE]  
>  Point（地理数据类型）方法的参数具有与 WKT 相反的坐标。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
 CLR 返回类型：**SqlGeography**  
  
## <a name="examples"></a>示例  
 下面的示例使用 `Point()` 创建 `geography` 实例。  
  
```  
DECLARE @g geography;   
SET @g = geography::Point(47.65100, -122.34900, 4326)  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>另请参阅  
 [扩展静态地理方法](../../t-sql/spatial-geography/extended-static-geography-methods.md)