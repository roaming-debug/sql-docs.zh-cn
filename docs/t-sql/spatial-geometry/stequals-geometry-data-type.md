---
description: STEquals（geometry 数据类型）
title: STEquals（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STEquals (geometry Data Type)
- STEquals_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STEquals (geometry Data Type)
ms.assetid: 808f0e25-9e68-4ba7-9329-07ec950698f3
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 501921075c8da9dcbf1f2e9b0196f70439a7c8ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186306"
---
# <a name="stequals-geometry-data-type"></a>STEquals（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

如果一个 **geometry** 实例表示的点集与另一个 **geometry** 实例表示的点集相同，则返回 1。 否则，返回 0。
  
## <a name="syntax"></a>语法  
  
```  
  
.STEquals ( other_geometry )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *other_geometry*  
 将与调用 `STEquals()` 的实例进行比较的另一个 geometry 实例。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：bit  
  
 CLR 返回类型：SqlBoolean  
  
## <a name="remarks"></a>注解  
 如果 geometry 实例的空间引用 ID (SRID) 不匹配，则此方法始终返回 null。  
  
## <a name="examples"></a>示例  
 下面的示例使用 `STGeomFromText()` 创建两个等效但又不完全相等的 `geometry` 实例，并使用 `STEquals()` 测试它们的等效性。  
  
```  
DECLARE @g geometry  
DECLARE @h geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 2 0, 4 2)', 0);  
SET @h = geometry::STGeomFromText('MULTILINESTRING((4 2, 2 0), (0 2, 2 0))', 0);  
SELECT @g.STEquals(@h);  
```  
  
## <a name="see-also"></a>另请参阅  
 [空间索引概述](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [几何图形实例上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

