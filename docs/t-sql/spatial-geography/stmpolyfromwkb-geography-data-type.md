---
description: STMPolyFromWKB（geography 数据类型）
title: STMPolyFromWKB（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STMPolyFromWKB (geography Data Type)
- STMPolyFromWKB_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STMPolyFromWKB method
ms.assetid: c4d0e649-0abb-4343-a3f0-3a702c8bbbdb
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 0131b12b730cac94bc507a2fc388d696feee72b2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180163"
---
# <a name="stmpolyfromwkb-geography-data-type"></a>STMPolyFromWKB（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

从开放地理空间信息联盟 (OGC) 已知二进制 (WKB) 表示形式返回 geographyMultiPolygon 实例。
  
## <a name="syntax"></a>语法  
  
```  
  
STMPolyFromWKB ( 'WKB_multipolygon' , SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 WKB_multipolygon  
 希望返回的 geographyMultiPolygon 实例的 WKB 表示形式。 WKB_multipolygon 是一个 varbinary(max) 表达式。  
  
 SRID   
 一个 int 表达式，表示希望返回的 geographyMultiPolygon 实例的空间引用 ID (SRID)。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
 CLR 返回类型：SqlGeography  
  
 OGC 类型：MultiPolygon  
  
## <a name="examples"></a>示例  
 下面的示例使用 `STMPolyFromWKB()` 创建 `geography` 实例。  
  
```  
DECLARE @g geography;  
SET @g = geography::STMPolyFromWKB(0x01060000000200000001030000000100000004000000F4FDD478E9965EC0DD24068195D3474083C0CAA145965EC0508D976E12D34740F4FDD478E9965EC04E62105839D44740F4FDD478E9965EC0DD24068195D3474001030000000100000004000000E7FBA9F1D2955EC08716D9CEF7D34740E7FBA9F1D2955EC0F853E3A59BD447405839B4C876965EC0F853E3A59BD44740E7FBA9F1D2955EC08716D9CEF7D34740, 4326);  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>另请参阅  
 [OGC 静态地理方法](../../t-sql/spatial-geography/ogc-static-geography-methods.md)  
  
  
