---
description: M（geometry 数据类型）
title: M（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- M (geometry Data Type)
- M_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- M (geometry Data Type)
ms.assetid: 443ae2ea-739b-41ef-96cc-ac5dfd65e10b
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 79a575b57bc3f41f48f9a00ed95784b09f84f5f6
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88497044"
---
# <a name="m-geometry-data-type"></a>M（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  geometry 实例的 M（度量）值。 度量值的语义是用户定义的。  

## <a name="syntax"></a>语法  
  
```  
  
.M  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 类型：float  
  
 CLR 类型：**SqlDouble**  
  
## <a name="remarks"></a>备注  
 如果 geometry 实例不是 Point，则此属性的值为 Null；对于未设置此属性的任何 Point 实例，此属性的值也为 Null。  
  
 此属性为只读。  
  
 M 值未在库进行的任何计算中使用，因此不通过任何库计算传递。  
  
## <a name="examples"></a>示例  
 下面的示例创建一个带有 Z（标高）和 M（度量）值的 `Point` 实例，并使用 `M` 获取该实例的 M 值。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POINT(1 2 3 4)', 0);  
SELECT @g.M;  
```  
  
## <a name="see-also"></a>另请参阅  
 [Geometry 实例上的扩展方法](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)   
 [Z（geometry 数据类型）](../../t-sql/spatial-geometry/z-geometry-data-type.md)   
 [AsTextZM（geometry 数据类型）](../../t-sql/spatial-geometry/astextzm-geometry-data-type.md)  
  
  

