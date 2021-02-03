---
description: STEndpoint（geometry 数据类型）
title: STEndpoint（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STEndpoint (geometry Data Type)
- STEndpoint_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STEndpoint (geometry Data Type)
ms.assetid: 61773c45-b568-4e0c-94da-1310c42de7f5
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 3dccbee5616cdcea74c6d24c932e8108cf0bf308
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204645"
---
# <a name="stendpoint-geometry-data-type"></a>STEndpoint（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回 geometry 实例的终点。
  
## <a name="syntax"></a>语法  
  
```  
  
.STEndPoint ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geometry  
  
 CLR 返回类型：SqlGeometry  
  
 开放地理空间信息联盟 (OGC) 类型：Point  
  
## <a name="remarks"></a>注解  
 `STEndPoint()` 等效于 [STPointN](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md) (x.NumPoints())。  
  
 如果针对空 geometry 实例调用此方法，则此方法返回 Null。  
  
## <a name="examples"></a>示例  
 下面的示例使用 `LineString` 创建 `STGeomFromText()` 实例，并使用 `STEndpoint()` 检索 `LineString` 的终点。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);  
SELECT @g.STEndPoint().ToString();  
```  
  
## <a name="see-also"></a>另请参阅  
 [几何图形实例上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

