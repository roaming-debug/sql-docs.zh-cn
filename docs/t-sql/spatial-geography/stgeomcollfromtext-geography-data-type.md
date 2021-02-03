---
description: STGeomCollFromText（geography 数据类型）
title: STGeomCollFromText（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STGeomCollFromText_TSQL
- STGeomCollFromText (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STGeomCollFromText method
ms.assetid: a5b3c344-1045-43a4-82fa-47f6206a288e
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 6c173292c93954de3a837878b300651bba532a88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210596"
---
# <a name="stgeomcollfromtext-geography-data-type"></a>STGeomCollFromText（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

从开放地理空间信息联盟 (OGC) 熟知文本 (WKT) 表示形式返回 geography 实例，增加了该实例传递的任何 Z（标高）和 M（度量）值。
  
## <a name="syntax"></a>语法  
  
```  
  
STGeomCollFromText ( 'geometrycollection_tagged_text' , SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 geometrycollection_tagged_text  
 希望返回的 geography 实例的 WKT 表示形式。 geometrycollection_tagged_text 是一个 nvarchar(max) 表达式。  
  
 SRID   
 一个 int 表达式，表示希望返回的 geography 实例的空间引用 ID (SRID)。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
 CLR 返回类型：SqlGeography  
  
## <a name="remarks"></a>备注  
 STGeomCollFromText() 返回的 geography 实例的 OGC 类型设置为相应的 WKT 输入。  
  
 如果输入无效，此方法将引发 ArgumentException。  
  
## <a name="examples"></a>示例  
 下面的示例使用 `STGeomCollFromText()` 创建 `geography` 实例。  
  
```  
DECLARE @g geography;  
DECLARE @g geography;  
SET @g = geography::STGeomCollFromText('GEOMETRYCOLLECTION ( POINT(-122.34900 47.65100), LINESTRING(-122.360 47.656, -122.343 47.656) )', 4326);  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>另请参阅  
 [OGC 静态地理方法](../../t-sql/spatial-geography/ogc-static-geography-methods.md)  
  
  
