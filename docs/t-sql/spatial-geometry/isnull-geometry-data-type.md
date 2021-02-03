---
description: IsNull（geometry 数据类型）
title: IsNull（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IsNull (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- IsNull (geometry Data Type)
ms.assetid: f95813a5-26c0-48aa-bfb8-56d2a0980788
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 6ea6429f1776c18feaacf43508100d4c960cd7be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159427"
---
# <a name="isnull-geometry-data-type"></a>IsNull（geometry 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

geometry 实例的类型为 Null。 如果该实例不为 NULL，则返回 0。
  
## <a name="syntax"></a>语法  
  
```  
.IsNull  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 类型：bit  
  
 CLR 类型：SqlBoolean  
  
## <a name="remarks"></a>备注  
 `IsNull` 可用于测试 geometry 实例是否为 Null。 如果实例不为 NULL，则 `IsNull` 返回 0；如果实例为 NULL，则返回 NULL。  
  
 此方法主要供 SQL Server 基础结构使用；建议不要使用 `IsNull` 来测试实例是否为 Null。  
  

## <a name="see-also"></a>另请参阅  
 [几何图形实例上的扩展方法](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
  
  

