---
description: CURSOR_STATUS (Transact-SQL)
title: CURSOR_STATUS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CURSOR_STATUS
- CURSOR_STATUS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], cursors
- CURSOR_STATUS function
- cursors [SQL Server], status information
ms.assetid: 3a4a840e-04f8-43bd-aada-35d78c3cb6b0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 853cae9252c43a176d208b7208f2ca595366a151
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093684"
---
# <a name="cursor_status-transact-sql"></a>CURSOR_STATUS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

对于给定参数，`CURSOR_STATUS` 显示游标声明是否已返回游标或结果集。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CURSOR_STATUS   
     (  
          { 'local' , 'cursor_name' }   
          | { 'global' , 'cursor_name' }   
          | { 'variable' , 'cursor_variable' }   
     )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
'local'  
指定一个常量，该常量用于指示游标源是一个本地游标名。
  
'cursor_name'  
游标的名称。 游标名必须符合[数据库标识符规则](../../relational-databases/databases/database-identifiers.md)。
  
'global'  
指定一个常量，该常量用于指示游标的源是一个全局游标名。
  
'variable'  
指定一个常量，该常量用于指示游标的源是一个本地变量。
  
'cursor_variable'  
游标变量的名称。 必须使用 cursor 数据类型定义游标变量。
  
## <a name="return-types"></a>返回类型
**smallint**
  
|返回值|游标名|游标变量|  
|---|---|---|
|1|游标结果集至少有一行。<br /><br /> 对于不区分的游标和键集游标，结果集至少有一行。<br /><br /> 对于动态游标，结果集可以有零行、一行或多行。|分配给该变量的游标已打开。<br /><br /> 对于不区分的游标和键集游标，结果集至少有一行。<br /><br /> 对于动态游标，结果集可以有零行、一行或多行。|  
|0|游标结果集为空。*|分配给该变量的游标已经打开，然而结果集肯定为空。*|  
|-1|游标被关闭。|分配给该变量的游标被关闭。|  
|-2|不适用。|可能性如下：<br /><br /> 先前调用的过程未向此 OUTPUT 变量分配任何游标。<br /><br /> 先前分配的过程向此 OUTPUT 变量分配了一个游标，但在过程结束时，游标处于关闭状态。 因此，游标被释放，并且没有返回调用过程。<br /><br /> 未向已声明的游标变量分配任何游标。|  
|-3|具有指定名称的游标不存在。|具有指定名称的游标变量不存在，或者即使存在这样一个游标变量，则该游标变量未分配到任何游标。|  
  
* 动态游标从不返回此结果。
  
## <a name="examples"></a>示例  
此示例使用 `CURSOR_STATUS` 函数显示游标在声明后、打开后和关闭后的状态。
  
```sql
CREATE TABLE #TMP  
(  
   ii INT  
)  
GO  
  
INSERT INTO #TMP(ii) VALUES(1)  
INSERT INTO #TMP(ii) VALUES(2)  
INSERT INTO #TMP(ii) VALUES(3)  
  
GO  
  
--Create a cursor.  
DECLARE cur CURSOR  
FOR SELECT * FROM #TMP  
  
--Display the status of the cursor before and after opening  
--closing the cursor.  
  
SELECT CURSOR_STATUS('global','cur') AS 'After declare'  
OPEN cur  
SELECT CURSOR_STATUS('global','cur') AS 'After Open'  
CLOSE cur  
SELECT CURSOR_STATUS('global','cur') AS 'After Close'  
  
--Remove the cursor.  
DEALLOCATE cur  
  
--Drop the table.  
DROP TABLE #TMP  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
After declare
---------------
-1  
  
After Open
----------
1  
  
After Close
-----------
-1
```  
  
## <a name="see-also"></a>另请参阅
[游标函数 (Transact-SQL)](../../t-sql/functions/cursor-functions-transact-sql.md)  
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)
  
  
