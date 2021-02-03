---
description: '&#x40;&#x40;FETCH_STATUS (Transact-SQL)'
title: FETCH_STATUS (Transact-SQL)
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@FETCH_STATUS'
- '@@FETCH_STATUS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- FETCH statement
- status information [SQL Server], FETCH
- '@@FETCH_STATUS function'
ms.assetid: 93659193-e4ff-4dfb-9043-0c4114921b91
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2a3bed4ab14d6efc43ae208523118bb063c39967
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187792"
---
# <a name="x40x40fetch_status-transact-sql"></a>&#x40;&#x40;FETCH_STATUS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此函数将返回最后一条游标 FETCH 语句的状态，该语句可以是针对连接当前打开的任何游标发出的。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
@@FETCH_STATUS  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>返回类型  
 **integer**  
  
## <a name="return-value"></a>返回值  
  
|返回值|描述|  
|------------------|-----------------|  
|&nbsp;0|FETCH 语句成功。|  
|-1|FETCH 语句失败或行不在结果集中。|  
|-2|提取的行不存在。|
|-9|游标未执行提取操作。|  
  
## <a name="remarks"></a>注解  
由于 `@@FETCH_STATUS` 对于在一个连接上的所有游标都是全局性的，所以要谨慎使用。 在执行一条 FETCH 语句后，必须在对另一游标执行另一 FETCH 语句前测试 `@@FETCH_STATUS`。 在此连接上出现任何提取操作之前，`@@FETCH_STATUS` 没有定义。  
  
例如，用户从一个游标执行一条 FETCH 语句，然后调用一个存储过程，此存储过程打开并处理另一个游标的结果。 从被调用的存储过程返回控制时，`@@FETCH_STATUS` 反映的是在存储过程中执行的最后的 FETCH 语句的结果，而不是在调用存储过程之前执行的 FETCH 语句的结果。  
  
若要检索特定游标的最后提取状态，请查询 sys.dm_exec_cursors 动态管理函数的 fetch_status 列。  
  
## <a name="examples"></a>示例  
此示例使用 `@@FETCH_STATUS` 来控制 `WHILE` 循环中的游标活动。  
  
```sql  
DECLARE Employee_Cursor CURSOR FOR  
SELECT BusinessEntityID, JobTitle  
FROM AdventureWorks2012.HumanResources.Employee;  
OPEN Employee_Cursor;  
FETCH NEXT FROM Employee_Cursor;  
WHILE @@FETCH_STATUS = 0  
   BEGIN  
      FETCH NEXT FROM Employee_Cursor;  
   END;  
CLOSE Employee_Cursor;  
DEALLOCATE Employee_Cursor;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [游标函数 (Transact-SQL)](../../t-sql/functions/cursor-functions-transact-sql.md)   
 [FETCH (Transact-SQL)](../../t-sql/language-elements/fetch-transact-sql.md)  
  
  
