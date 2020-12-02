---
description: AsTextZM（geometry 数据类型）
title: AsTextZM（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- AsTextZM_TSQL
- AsTextZM
- AsTextZM (geometry Data Type)
- AsTextZM_(geometry_Data_Type)_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- AsTextZM (geometry Data Type)
ms.assetid: 08ac8aa0-aff7-4b22-87e0-1a1d55dcbc04
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 46f2f296289263a5ad8e4d315f5aa778792b1639
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128193"
---
# <a name="astextzm-geometry-data-type"></a>AsTextZM（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

返回几何图形实例的开放地理空间信息联盟 (OGC) 熟知文本 (WKT) 表示形式，增加了该实例传递的任何 Z（标高）和 M（度量）值 。
  
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
 下面的示例创建一个带有 Z（标高）和 M（度量）值的 `Point` 实例。 `STAsText()` 选择 WKT 值 (1 2)；`AsTextZM()` 选择相同的 WKT 值，还返回 Z 和 M 的值，从而生成 (1 2 3 4)。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POINT(1 2 3 4)', 0);  
SELECT @g.STAsText();  
SELECT @g.AsTextZM();  
```  
  
## <a name="see-also"></a>另请参阅  
 [Geometry 实例上的扩展方法](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)   
 [M（geometry 数据类型）](../../t-sql/spatial-geometry/m-geometry-data-type.md)   
 [Z（geometry 数据类型）](../../t-sql/spatial-geometry/z-geometry-data-type.md)  
  
  

