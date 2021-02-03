---
description: STIsRing（geometry 数据类型）
title: STIsRing（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STIsRing_TSQL
- STIsRing (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STIsRing (geometry Data Type)
ms.assetid: ea0063be-1c74-4cc4-ac6f-b65321ddfa54
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: b0f0cb68a9923491e64fa63f52b6d481abb73c14
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205003"
---
# <a name="stisring-geometry-data-type"></a>STIsRing（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

如果 **geometry** 实例符合下列要求，则返回 1：
-   它是 **LineString** 实例。  
-   该实例为闭合类型。  
-   该实例为简单类型。  
-   如果 **LineString** 实例不符合这些要求，则返回 0。  

 为了使 **geometry** 实例成为简单的闭合类型，当对该实例调用 [STIsClosed()](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md) 和 [STIsSimple()](../../t-sql/spatial-geometry/stissimple-geometry-data-type.md) 时，这两种方法都必须返回 1。 若要确定 **geometry** 的实例类型，请使用 [STGeometryType()](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
.STIsRing ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：bit  
  
 CLR 返回类型：SqlBoolean  
  
## <a name="remarks"></a>备注  
 如果该实例不是 **LineString**，则此方法返回 null。  
  
## <a name="examples"></a>示例  
 下面的示例创建一个 `LineString` 实例并使用 `STIsRing()` 来测试该实例是否为一个环。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0, 0 0)', 0);  
SELECT @g.STIsRing();  
```  
  
## <a name="see-also"></a>另请参阅  
 [STIsClosed（geometry 数据类型）](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)   
 [STGeometryType（geometry 数据类型）](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)   
 [STIsSimple（geometry 数据类型）](../../t-sql/spatial-geometry/stissimple-geometry-data-type.md)   
 [几何图形实例上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

