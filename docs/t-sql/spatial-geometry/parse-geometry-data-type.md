---
description: Parse（geometry 数据类型）
title: Parse（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Parse (geometry Data Type)
ms.assetid: 6e080919-4b64-46cd-8dd2-254a9c232e53
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: cb74ea4c60eaf8a9b65633742152ee109ef692ae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207183"
---
# <a name="parse-geometry-data-type"></a>Parse（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

从开放地理空间信息联盟 (OGC) 已知文本 (WKT) 表示形式返回 **geometry** 实例。 `Parse()` 与 [STGeomFromText()](../../t-sql/spatial-geometry/parse-geometry-data-type.md) 等效，不同的是前者将值为 0 的空间引用 ID (SRID) 作为参数。 输入值可以根据需要包含 Z（标高）和 M（度量）值。
  
## <a name="syntax"></a>语法  
  
```  
  
Parse ( 'geometry_tagged_text' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *geometry_tagged_text*  
 希望返回的 **geometry** 实例的 WKT 表示形式。 *geometry_tagged_text* 是一个 **nvarchar** 表达式。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geometry  
  
 CLR 返回类型：SqlGeometry  
  
## <a name="remarks"></a>注解  
 `Parse()` 返回的 **geometry** 实例的 OGC 类型设置为相应的 WKT 输入。  
  
 字符串“Null”将被解释为 Null **geometry** 实例。  
  
 如果输入的格式不正确，此方法将引发 FormatException。  
  
## <a name="examples"></a>示例  
 下面的示例使用 `Parse()` 创建 `geometry` 实例。  
  
```  
DECLARE @g geometry;   
SET @g = geometry::Parse('LINESTRING (100 100, 20 180, 180 180)');  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>另请参阅  
 [STGeomFromText](../../t-sql/spatial-geometry/parse-geometry-data-type.md)   
 [扩展静态几何图形方法](../../t-sql/spatial-geometry/extended-static-geometry-methods.md)  
  
  

