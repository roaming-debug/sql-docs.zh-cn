---
description: AsBinaryZM（geography 数据类型）
title: AsBinaryZM（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- AsBinaryZM
- AsBinaryZM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- AsBinaryZM, geography
- AsBinaryZM
ms.assetid: 37246adb-814d-4113-9983-4d336de8182c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: cb175163dccfd07c6c7941503401eddd77d4e4d6
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96115073"
---
# <a name="asbinaryzm-geography-data-type"></a>AsBinaryZM（geography 数据类型）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  返回 geometry 实例的开放地理空间信息联盟 (OGC) 已知二进制 (WKB) 表示形式，增加了该实例传递的任何 Z（标高）和 M（度量）值。  
  
## <a name="syntax"></a>语法  
  
```  
  
.AsBinaryZM()  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：varbinary(max)  
  
 CLR 返回类型：SqlBytes  
  
## <a name="remarks"></a>备注  
  
## <a name="examples"></a>示例  
  
```sql  
DECLARE @g1 GEOGRAPHY = 'Point(1 1 2 3)';  
  
SELECT @g1.STAsBinary();  
-- Returns: 0x0101000000000000000000F03F000000000000F03F  
  
SELECT @g1.AsBinaryZM();  
--Returns: 0x01B90B0000000000000000F03F000000000000F03F00000000000000400000000000000840  
```  
  
## <a name="see-also"></a>另请参阅  
 [Geography 实例上的扩展方法](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [M（geography 数据类型）](../../t-sql/spatial-geography/m-geography-data-type.md)   
 [Z（geography 数据类型）](../../t-sql/spatial-geography/z-geography-data-type.md)  
  
  
