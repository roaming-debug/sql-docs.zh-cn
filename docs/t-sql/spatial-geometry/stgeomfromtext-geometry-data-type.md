---
description: STGeomFromText（geometry 数据类型）
title: STGeomFromText（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STGeomFromText (geometry Data Type)
- STGeomFromText_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STGeomFromText (geometry Data Type)
ms.assetid: 20cace39-02e5-46c1-a9a5-841d04d0da16
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 0917ef0900d317eeaeb916604cd8e1dc53387b03
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88479332"
---
# <a name="stgeomfromtext-geometry-data-type"></a>STGeomFromText（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

从开放地理空间信息联盟 (OGC) 熟知文本 (WKT) 表示形式返回 geometry 实例，增加了该实例传递的任何 Z（标高）和 M（度量）值。
  
## <a name="syntax"></a>语法  
  
```  
  
STGeomFromText ( 'geometry_tagged_text' , SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *geometry_tagged_text*  
 希望返回的 **geometry** 实例的 WKT 表示形式。 geometry_tagged_text 是一个 nvarchar(max) 表达式。  
  
 SRID   
 一个 int 表达式，表示希望返回的 geometry 实例的空间引用 ID (SRID)。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geometry  
  
 CLR 返回类型：SqlGeometry  
  
## <a name="remarks"></a>注解  
 `STGeomFromText()` 返回的 **geometry** 实例的 OGC 类型设置为相应的 WKT 输入。  
  
 如果输入的格式不正确，此方法将引发 FormatException。  
  
## <a name="examples"></a>示例  
 下面的示例使用 `STGeomFromText()` 创建 `geometry` 实例。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING (100 100, 20 180, 180 180)', 0);  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>另请参阅  
 [OGC 静态几何图形方法](../../t-sql/spatial-geometry/ogc-static-geometry-methods.md)  
  
  

