---
description: STPointFromText（geography 数据类型）
title: STPointFromText（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STPointFromText (geography Data Type)
- STPointFromText_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STPointFromText method
ms.assetid: e5fe54dc-0007-4631-8dde-7ae4d4c41f6e
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 9e395f3fe5c57be7e6220b797e4b25a25954622c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211257"
---
# <a name="stpointfromtext-geography-data-type"></a>STPointFromText（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

从开放地理空间信息联盟 (OGC) 熟知文本 (WKT) 表示形式返回 geography 实例，增加了该实例传递的任何 Z（标高）和 M（度量）值。
  
## <a name="syntax"></a>语法  
  
```  
  
STPointFromText ( 'point_tagged_text' , SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 point_tagged_text  
 希望返回的 geographyPoint 实例的 WKT 表示形式。 point_tagged_text 是一个 nvarchar(max) 表达式。  
  
 SRID   
 一个 int 表达式，表示希望返回的 geographyPoint 实例的空间引用 ID (SRID)。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
 CLR 返回类型：SqlGeography  
  
 OGC 类型：Point  
  
## <a name="remarks"></a>注解  
 如果输入的格式不正确，此方法将引发 FormatException。  
  
## <a name="examples"></a>示例  
 下面的示例使用 `STPointFromText()` 创建 `geography` 实例。  
  
```  
DECLARE @g geography;  
SET @g = geography::STPointFromText('POINT(-122.34900 47.65100)', 4326);  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>另请参阅  
 [OGC 静态地理方法](../../t-sql/spatial-geography/ogc-static-geography-methods.md)  
  
  
