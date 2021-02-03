---
description: Filter（geography 数据类型）
title: Filter（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Filter
- Filter_TSQL
- Filter (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- Filter method
ms.assetid: 82a8f54a-3a47-4e20-b13a-b148029c5448
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 4d3d5975a0e1d01a0b4dfbea123f483d8e004ab2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176322"
---
# <a name="filter-geography-data-type"></a>Filter（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  一种方法，可提供一种快速、仅索引相交的方法，用于确定一个 geography 实例是否与另一个 geography 实例相交（假定有可用索引）。  
  
 如果 geography 实例与另一个 geography 实例存在相交的可能，则返回 1。 该方法可能产生负正返回，并且确切结果可能是依赖于计划的。 如果 geography 实例之间不存在相交，则返回精确的 0 值（真负返回）。  
  
 在无可用索引或未使用索引的情况下，该方法返回的值将与使用相同参数调用 STIntersects() 返回的值相同。  
  
## <a name="syntax"></a>语法  
  
```  
  
.Filter ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 other_geography  
 与对其调用 Filter() 的实例进行比较的其他 geography 实例。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：bit  
  
 CLR 返回类型：SqlBoolean  
  
## <a name="remarks"></a>注解  
 此方法是不具有确定性的方法，而且不精确。  
  
## <a name="examples"></a>示例  
 下面的示例使用 `Filter()` 确定两个 `geography` 实例是否彼此相交。  
  
```  
CREATE TABLE sample (id int primary key, g geography);  
INSERT INTO sample VALUES  
   (0, geography::Point(45, -120, 4326)),  
   (1, geography::Point(45, -120.1, 4326)),  
   (2, geography::Point(45, -120.2, 4326)),  
   (3, geography::Point(45, -120.3, 4326)),  
   (4, geography::Point(45, -120.4, 4326));  
  
CREATE SPATIAL INDEX sample_idx on sample(g);  
SELECT id  
FROM sample   
WHERE g.Filter(geography::Parse(  
   'POLYGON((-120.1 44.9, -119.9 44.9, -119.9 45.1, -120.1 45.1, -120.1 44.9))')) = 1;  
```  
  
## <a name="see-also"></a>另请参阅  
 [Geography 实例上的扩展方法](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [STIntersects（geography 数据类型）](../../t-sql/spatial-geography/stintersects-geography-data-type.md)  
  
  
