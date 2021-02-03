---
description: STX（geometry 数据类型）
title: STX（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 06/23/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STX (geometry Data Type)
- STX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STX (geometry Data Type)
ms.assetid: 2aef77e8-0460-43f9-bad6-2aae6d8c36f9
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: c66bc8f83328eaaaad62d7dceb291d5aaaaa5bd7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191850"
---
# <a name="stx-geometry-data-type"></a>STX（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Point 实例的 X 坐标属性。
  
## <a name="syntax"></a>语法  
  
```  
  
.STX  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 类型：float  
  
 CLR 类型：**SqlDouble**  
  
## <a name="remarks"></a>备注  
 如果 **geometry** 实例不是一个点，此属性的值将为 null。  
  
 此属性为只读。  
  
## <a name="examples"></a>示例  
 下面的示例创建一个 `Point` 实例，并使用 `STX` 检索该实例的 X 坐标。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POINT(3 8)', 0);  
SELECT @g.STX;  
```  
  
## <a name="see-also"></a>另请参阅  
 [STY（geometry 数据类型）](../../t-sql/spatial-geometry/sty-geometry-data-type.md)   
 [STSrid（geometry 数据类型）](../../t-sql/spatial-geometry/stsrid-geometry-data-type.md)   
 [几何图形实例上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

