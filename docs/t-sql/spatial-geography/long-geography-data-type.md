---
description: Long（geography 数据类型）
title: Long（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Long_TSQL
- Long
dev_langs:
- TSQL
helpviewer_keywords:
- Long method
ms.assetid: bedbeced-70b8-4569-84f3-f86bfb04ce50
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 98fa19504777027a5d8e4bca5951b1d5ceec6fe6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207536"
---
# <a name="long-geography-data-type"></a>Long（geography 数据类型）

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  geography 实例的经度属性。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
.Long  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-value"></a>返回值  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 类型：float  
  
 CLR 类型：**SqlDouble**  
  
## <a name="remarks"></a>备注  
 在 OpenGIS 模型中，仅对由一个点组成的 geography 实例定义 Long。 如果 geography 实例包含多个点，则此属性返回 NULL。 此属性是精确属性，且是只读的。  
  
## <a name="examples"></a>示例  
 此示例创建 Point 实例并检索该点的经度。  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POINT(-122.34900 47.65100)', 4326);  
SELECT @g.Long;  
```  
  
## <a name="see-also"></a>另请参阅  
 [地理实例上的扩展方法](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
