---
description: MinDbCompatibilityLevel（geometry 数据类型）
title: MinDbCompatibilityLevel（geometry 数据类型）| Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- MinDbCompatibilityLevel method (geometry)
ms.assetid: c848b974-8ccb-4c5c-a7eb-b019a9538d99
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 752d9516dbe58448e857f83fbcf025139d4884b5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159392"
---
# <a name="mindbcompatibilitylevel-geometry-data-type"></a>MinDbCompatibilityLevel（geometry 数据类型）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

返回识别 geometry 数据类型实例的最基本的数据库兼容级别。
  
## <a name="syntax"></a>语法  
  
```  
  
.MinDbCompatibilityLevel ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 返回类型：int  
  
 CLR 返回类型：int  
  
## <a name="remarks"></a>备注  
 在更改数据库的兼容级别之前应使用 `MinDbCompatibilityLevel()` 测试空间对象的兼容性。  
  
## <a name="examples"></a>示例  
  
### <a name="a-testing-circularstring-type-for-compatibility-with-compatibility-level-110"></a>A. 在兼容级别为 110 的情况下测试 CircularString 类型是否兼容  
 下面的示例测试 `CircularString` 实例是否与较早版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 兼容：  
  
```
 DECLARE @g geometry = 'CIRCULARSTRING(3 4, 8 9, 5 6)'; 
 IF @g.MinDbCompatibilityLevel() <= 110 
 BEGIN 
 SELECT @g.ToString(); 
 END
 ```  
  
### <a name="b-testing-linestring-type-for-compatibility-with-compatibility-level-100"></a>B. 在兼容级别为 100 的情况下测试 LineString 类型是否兼容  
 下面的示例测试 `LineString` 实例是否与 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 兼容：  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 9, 5 6)'; 
 IF @g.MinDbCompatibilityLevel() <= 100 
 BEGIN 
 SELECT @g.ToString(); 
 END
``` 
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)  
  
  

