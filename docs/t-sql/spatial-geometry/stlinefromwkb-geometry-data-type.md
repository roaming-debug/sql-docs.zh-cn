---
description: STLineFromWKB（geometry 数据类型）
title: STLineFromWKB（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STLineFromWKB (geometry Data Type)
- STLineFromWKB_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STLineFromWKB (geometry Data Type)
ms.assetid: e674c8c4-c67f-4fc1-9873-d9c2ed46c659
author: MladjoA
ms.author: mlandzic
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1b94446130a62b1388b6e35c9261cd0bb832fe59
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179135"
---
# <a name="stlinefromwkb-geometry-data-type"></a>STLineFromWKB（geometry 数据类型）
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]

从开放地理空间信息联盟 (OGC) 已知二进制 (WKB) 表示形式返回 **geometryLineString** 实例。
  
## <a name="syntax"></a>语法  
  
```  
  
STLineFromWKB ( 'WKB_linestring' , SRID )  
```  
  
## <a name="arguments"></a>参数  
 *WKB_linestring*  
 希望返回的 **geometryLineString** 实例的 WKB 表示形式。 WKB_linestring 是一个 varbinary(max) 表达式。  
  
 SRID   
 一个 int 表达式，表示希望返回的 geometryLineString 实例的空间引用 ID (SRID)。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geometry  
  
 CLR 返回类型：SqlGeometry  
  
 OGC 类型：LineString  
  
## <a name="remarks"></a>注解  
 如果输入的格式不正确，此方法将引发 FormatException。  
  
## <a name="examples"></a>示例  
 下面的示例使用 `STLineFromWKB()` 创建 `geometry` 实例。  
  
```  
DECLARE @g geometry;   
SET @g = geometry::STLineFromWKB(0x0102000000020000000000000000005940000000000000594000000000000069400000000000006940, 0);  
SELECT @g.STAsText();  
```  
  
## <a name="see-also"></a>另请参阅  
 [OGC 静态几何图形方法](../../t-sql/spatial-geometry/ogc-static-geometry-methods.md)  
  
  

