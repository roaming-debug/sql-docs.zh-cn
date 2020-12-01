---
description: STAsBinary（geography 数据类型）
title: STAsBinary（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STAsBinary_TSQL
- STAsBinary (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STAsBinary method
ms.assetid: 99602a62-265d-4aa4-a8dc-92992ca55ba4
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 7ae9c800cffd7df80ba8447c2e16de745a0009bc
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88359443"
---
# <a name="stasbinary-geography-data-type"></a>STAsBinary（geography 数据类型）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  返回 geography 实例的开放地理空间信息联盟 (OGC) 熟知二进制 (WKB) 表示形式。  
  
 这种 geography 数据类型方法支持大于半球的 FullGlobe 实例或空间实例   。  
  
## <a name="syntax"></a>语法  
  
```  
  
.STAsBinary ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：varbinary(max)  
  
 CLR 返回类型：SqlBytes  
  
## <a name="remarks"></a>备注  
 geography 实例的 OGC 类型可通过调用 [STGeometryType()](../../t-sql/spatial-geography/stgeometrytype-geography-data-type.md) 来确定。  
  
## <a name="examples"></a>示例  
 下面的示例使用 `STAsBinary()` 根据文本创建一个从 (-122.360, 47.656) 到 (-122.343, 47.656) 的 `LineString``geography` 实例。 然后，它以 WKB 的形式返回结果。  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING( -122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STAsBinary();  
```  
  
## <a name="see-also"></a>另请参阅  
 [Geography 实例上的 OGC 方法](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
