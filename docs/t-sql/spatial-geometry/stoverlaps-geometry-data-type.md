---
description: STOverlaps（geometry 数据类型）
title: STOverlaps（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STOverlaps (geometry Data Type)
- STOverlaps_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STOverlaps (geometry Data Type)
ms.assetid: 1813cba1-5780-456a-9489-6b40a79569b3
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d92e984fb803cc8b15c58f4b137df2dd51e83f9b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88454232"
---
# <a name="stoverlaps-geometry-data-type"></a>STOverlaps（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

如果一个 **geometry** 实例与另一个 **geometry** 实例重叠，则返回 1。 否则，返回 0。
  
## <a name="syntax"></a>语法  
  
```  
  
.STOverlaps ( other_geometry )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *other_geometry*  
 将与调用 `STOverlaps()` 的实例进行比较的另一个 geometry 实例。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：bit  
  
 CLR 返回类型：SqlBoolean  
  
## <a name="remarks"></a>注解  
 如果表示两个 **geometry** 实例交集的区域与这两个实例具有相同的维度，而且该区域不等于这两个实例当中的任意一个，则说明这两个实例重叠。  
  
 如果两个 **geometry** 实例的交点与这两个实例具有不同的维度，则 `STOverlaps()` 始终返回 0。  
  
 如果 geometry 实例的空间引用 ID (SRID) 不匹配，则此方法始终返回 null。  
  
## <a name="examples"></a>示例  
 以下示例使用 `STOverlaps()` 测试两个 **geometry** 实例是否重叠。  
  
```  
DECLARE @g geometry;  
DECLARE @h geometry;  
SET @g = geometry::STGeomFromText('POLYGON((0 0, 2 0, 2 2, 0 2, 0 0))', 0);  
SET @h = geometry::STGeomFromText('POLYGON((1 1, 3 1, 3 3, 1 3, 1 1))', 0);  
SELECT @g.STOverlaps(@h);  
```  
  
## <a name="see-also"></a>另请参阅  
 [几何图形实例上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

