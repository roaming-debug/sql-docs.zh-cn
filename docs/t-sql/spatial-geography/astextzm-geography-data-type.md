---
description: AsTextZM（geography 数据类型）
title: AsTextZM（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- AsTextZM (geography Data Type)
- AsTextZM_TSQL
- AsTextZM
- AsTextZM_(geography_Data_Type)_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- AsTextZM method
ms.assetid: e9dc27f6-e945-4457-8498-7644db34008e
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 49857a94c95240386893854408d2b26cba533815
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96115161"
---
# <a name="astextzm-geography-data-type"></a>AsTextZM（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回 geography 实例的开放地理空间信息联盟 (OGC) 熟知文本 (WKT) 表示形式，增加了该实例传递的任何 Z（标高）和 M（度量）值  。  
  
## <a name="syntax"></a>语法  
  
```  
  
.AsTextZM ()  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：nvarchar(max)  
  
 CLR 返回类型：SqlChars  
  
## <a name="remarks"></a>备注  
  
## <a name="examples"></a>示例  
 下面的示例创建一个带有 Z（标高）和 M（度量）值的 `Point` 实例。 `STAsText()` 选择 WKT 值 (-122.34900 47.65100)；`AsTextZM()` 选择相同的 WKT 值，还返回 Z 和 M 的值，从而生成 (-122.34900 47.65100 10.3 12)。  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POINT(-122.34900 47.65100 10.3 12)', 4326);  
SELECT @g.STAsText();  
SELECT @g.AsTextZM();  
```  
  
## <a name="see-also"></a>另请参阅  
 [Geography 实例上的扩展方法](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [M（geography 数据类型）](../../t-sql/spatial-geography/m-geography-data-type.md)   
 [Z（geography 数据类型）](../../t-sql/spatial-geography/z-geography-data-type.md)  
  
  
