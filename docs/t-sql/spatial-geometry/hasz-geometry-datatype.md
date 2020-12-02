---
description: HasZ（geometry 数据类型）
title: HasZ（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- HasZ geometry
ms.assetid: aa378943-252a-4079-848b-6c59344fcfce
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 58c43b02c466fa2f5f1bc2b06006855b894cd65c
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88497055"
---
# <a name="hasz-geometry-datatype"></a>HasZ（geometry 数据类型）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  如果空间对象包含至少一个 Z 值，则返回 1 (true)；否则返回 0 (false)。  
  
## <a name="syntax"></a>语法  
  
```  
  
.HasZ  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：bit  
  
 CLR 返回类型：Boolean  
  
## <a name="remarks"></a>备注  
  
## <a name="examples"></a>示例  
  
```sql  
DECLARE @p GEOMETRY = 'Point(1 1 1 1)'  
SELECT @p.HasZ   
--Returns: 1 (true)  
```  
  
## <a name="see-also"></a>另请参阅  
 [Geometry 实例上的扩展方法](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)   
 [Z（geometry 数据类型）](../../t-sql/spatial-geometry/z-geometry-data-type.md)  
  
  
