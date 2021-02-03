---
description: STRelate（geometry 数据类型）
title: STRelate（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STRelate (geometry Data Type)
- STRelate_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STRelate (geometry Data Type)
ms.assetid: 9dcb5f58-35ab-4bb3-86ee-2d29eefba6d3
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 1e5e2a0ed17d34c178798770d386bf3a4dc9fe3d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184589"
---
# <a name="strelate-geometry-data-type"></a>STRelate（geometry 数据类型）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  如果一个 **geometry** 实例与另一个 **geometry** 实例相关（其关系由“维扩展 9 交集模型”(DE-9IM) 模式矩阵值定义），则返回 1；否则，返回 0。  
  
## <a name="syntax"></a>语法  
  
```  
  
.STRelate ( other_geometry, intersection_pattern_matrix )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *other_geometry*  
 将与调用 `STRelate()` 的实例进行比较的另一个 geometry 实例。  
  
 *intersection_pattern_matrix*  
 两个 **geometry** 实例间的 DE-9IM 模式矩阵设备的 **nchar(9)** 类型编码可接受值字符串。  
  
## <a name="remarks"></a>注解  
 如果 geometry 实例的空间引用 ID (SRID) 不匹配，则此方法始终返回 null。 如果矩阵格式不正确，此方法将引发 **ArgumentException**。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：bit  
  
 CLR 返回类型：SqlBoolean  
  
## <a name="examples"></a>示例  
 以下示例在显式 DE-9IM 模式下使用 `STRelate()` 来测试两个 **geometry** 实例是否在空间上不相联。  
  
```  
DECLARE @g geometry;  
DECLARE @h geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 2 0, 4 2)', 0);  
SET @h = geometry::STGeomFromText('POINT(5 5)', 0);  
SELECT @g.STRelate(@h, 'FF*FF****');  
```  
  
## <a name="see-also"></a>另请参阅  
 [几何图形实例上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  
