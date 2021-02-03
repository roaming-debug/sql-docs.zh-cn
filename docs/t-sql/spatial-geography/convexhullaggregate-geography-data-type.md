---
description: ConvexHullAggregate（geography 数据类型）
title: ConvexHullAggregate（geography 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ConvexHullAggregate_TSQL
- ConvexHullAggregate
dev_langs:
- TSQL
helpviewer_keywords:
- ConvexHullAggregate method (geography)
ms.assetid: 21784c66-2725-471b-9e2d-a8c2e3695197
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: fba09550ac86e630ccf1e29083cbb7565ef2265e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159660"
---
# <a name="convexhullaggregate-geography-data-type"></a>ConvexHullAggregate（geography 数据类型）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

对于给定的一组 geography 对象返回一个凸包。
  
## <a name="syntax"></a>语法  
  
```  
  
ConvexHullAggregate ( geography_operand )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 geography_operand  
 表示一组 geography 对象的 geography 类型表列。  
  
## <a name="return-types"></a>返回类型  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：geography  
  
## <a name="exception"></a>异常  
 在输入值无效时引发 `FormatException`。 请参阅 [STIsValid（geography 数据类型）](../../t-sql/spatial-geography/stisvalid-geography-data-type.md)  
  
## <a name="remarks"></a>注解  
 在输入为空或具有不同的 SRID 时，方法返回 null。 请参阅[空间引用标识符 (SRID)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md)  
  
 方法忽略 null 输入。  
  
> [!NOTE]  
>  如果所有输入值均为 null，则方法返回 null。  
  
## <a name="examples"></a>示例  
 下面的示例返回一组 geography 对象的凸包。  
  
 ```
 USE AdventureWorks2012  
 GO  
 SELECT geography::ConvexHullAggregate(SpatialLocation).ToString() AS SpatialLocation  
 FROM Person.Address  
 WHERE City LIKE ('Bothell')
 ```  
  
## <a name="see-also"></a>另请参阅  
 [扩展静态地理方法](../../t-sql/spatial-geography/extended-static-geography-methods.md)  
  
  
