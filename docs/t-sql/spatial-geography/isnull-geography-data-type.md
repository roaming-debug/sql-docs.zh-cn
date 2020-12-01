---
description: IsNull（geography 数据类型）
title: IsNull（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IsNull (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- IsNull method
ms.assetid: c031074f-bfda-4584-a3bf-4e7c324f237f
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: cd161c4d14241a6596e2a54ae79cd919fb776c59
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88422371"
---
# <a name="isnull-geography-data-type"></a>IsNull（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  指定 geography 实例是否为 Null 的属性。 如果实例为 Null，则返回“TRUE”；如果实例不为 Null，则返回 0。  
  
## <a name="syntax"></a>语法  
  
```  
  
.IsNull  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 类型：bit  
  
 CLR 类型：SqlBoolean  
  
## <a name="remarks"></a>备注  
 `IsNull` 可用于测试 geography 实例是否为 Null。 这会产生有些令人混淆的结果：如果实例不为 Null，则返回 0，但是如果实例为 Null 则返回 Null。  
  
 此方法主要供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 基础结构使用；建议使用 T-SQL 谓词 IS NULL 来测试 geography 实例是否为 Null。 有关 T-SQL 谓词 IS NULL 的详细信息，请参阅 [IS NULL (Transact-SQL)](../../t-sql/queries/is-null-transact-sql.md)。  
  
## <a name="examples"></a>示例  
  
## <a name="see-also"></a>另请参阅  
 [地理实例上的扩展方法](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
