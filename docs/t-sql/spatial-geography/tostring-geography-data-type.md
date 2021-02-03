---
description: ToString（geography 数据类型）
title: ToString（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ToString (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- ToString method
ms.assetid: 045c12fa-8fc6-441a-9500-7021cb4ff13e
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d7d2d4b6d4f786c7962155e11b7fe89bbb491a7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187743"
---
# <a name="tostring-geography-data-type"></a>ToString（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回 geography 实例的开放地理空间信息联盟 (OGC) 熟知文本 (WKT) 表示形式，增加了该实例传递的任何 Z（标高）和 M（度量）值。  
  
 这种 geography 数据类型方法支持大于半球的 FullGlobe 实例或空间实例。  
  
## <a name="syntax"></a>语法  
  
```  
  
.ToString ()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：nvarchar(max)  
  
 CLR 返回类型：SqlString  
  
## <a name="remarks"></a>注解  
 在针对 Null 实例调用时，此方法将返回字符串“Null”。 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中，服务器上可能的结果集已扩展到 FullGlobe 实例。 此方法将返回与 `AsTextZM()` 相同的值。  
  
 此方法不精确。  
  
## <a name="examples"></a>示例  
 下面的示例创建一个`LineString` 实例，并使用 `ToString()` 返回该实例的文本说明。  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>另请参阅  
 [Geography 实例上的扩展方法](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [AsTextZM（geography 数据类型）](../../t-sql/spatial-geography/astextzm-geography-data-type.md)  
  
  
