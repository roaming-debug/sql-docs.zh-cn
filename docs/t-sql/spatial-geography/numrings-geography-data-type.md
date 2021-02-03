---
description: NumRings（geography 数据类型）
title: NumRings（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- NumRings_TSQL
- NumRings
dev_langs:
- TSQL
helpviewer_keywords:
- NumRings method
ms.assetid: 0e4e4fa2-b608-4cc4-98ba-0845ddb4214c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 1a4827b3b3972864b560f3951a81889321580a10
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186416"
---
# <a name="numrings-geography-data-type"></a>NumRings（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回 Polygon 实例中的总环数。 在  geography 类型中，不区分外环和内环，因为任何环都可以作为外环[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="syntax"></a>语法  
  
```  
  
.NumRings ()  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：int  
  
 CLR 返回类型：SqlInt32  
  
## <a name="remarks"></a>备注  
 如果该实例不是 Polygon 实例，则此方法返回 NULL；如果该实例为空，则将返回 0。 此方法是精确方法。  
  
## <a name="examples"></a>示例  
 下面的示例创建一个具有两个环的 `Polygon` 实例并确认该实例具有两个环。  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653), (-122.357 47.654, -122.357 47.657, -122.349 47.657, -122.349 47.650, -122.357 47.654))', 4326);  
SELECT @g.NumRings();  
```  
  
## <a name="see-also"></a>另请参阅  
 [地理实例上的扩展方法](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
