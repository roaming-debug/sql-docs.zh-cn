---
description: STGeomFromWKB（geometry 数据类型）
title: STGeomFromWKB（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STGeomFromWKB (geometry Data Type)
- STGeomFromWKB_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STGeomFromWKB (geometry Data Type)
ms.assetid: 6546ddb0-4a5f-46e5-ba04-8007486c95ec
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: dbd382664925059128b21a5a1dce048d7703fc2a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88479327"
---
# <a name="stgeomfromwkb-geometry-data-type"></a>STGeomFromWKB（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

从开放地理空间信息联盟 (OGC) 已知二进制 (WKB) 表示形式中返回 geometry 实例。
  
## <a name="syntax"></a>语法  
  
```  
  
STGeomFromWKB ( 'WKB_geometry' , SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 WKB_geometry  
 希望返回的 geometry 实例的 WKB 表示形式。 WKB_geometry 是一个 varbinary(max) 表达式。  
  
 SRID   
 一个 int 表达式，表示希望返回的 geometry 实例的空间引用 ID (SRID)。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geometry  
  
 CLR 返回类型：SqlGeometry  
  
## <a name="remarks"></a>备注  
 `STGeomFromText()` 返回的 geometry 实例的 OGC 类型设置为相应的 WKB 输入。  
  
 如果输入的格式不正确，此方法将引发 FormatException。  
  
## <a name="examples"></a>示例  
 下面的示例使用 `STGeomFromWKB()` 创建 geometry 实例。  
  
```  
DECLARE @g geometry;   
SET @g = geometry::STGeomFromWKB(0x010200000003000000000000000000594000000000000059400000000000003440000000000080664000000000008066400000000000806640, 0);  
SELECT @g.STAsText();  
```  
  
## <a name="see-also"></a>另请参阅  
 [OGC 静态几何图形方法](../../t-sql/spatial-geometry/ogc-static-geometry-methods.md)  
  
  

