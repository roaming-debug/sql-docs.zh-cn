---
description: ERROR_SEVERITY (Transact-SQL)
title: ERROR_SEVERITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ERROR_SEVERITY_TSQL
- ERROR_SEVERITY
dev_langs:
- TSQL
helpviewer_keywords:
- errors [SQL Server], severity
- messages [SQL Server], severity
- TRY...CATCH [SQL Server]
- CATCH block
- ERROR_SEVERITY function
ms.assetid: 50228f2f-6949-4d2e-8e43-fad11bf973ab
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 937b3737d31aed7dbc34f210e5a64e043527fde3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158821"
---
# <a name="error_severity-transact-sql"></a>ERROR_SEVERITY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

如果该错误导致执行了 TRY…CATCH 构造的 CATCH 块，此函数将在发生错误的位置返回错误的严重性值。  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
ERROR_SEVERITY ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 **int**  
  
## <a name="return-value"></a>返回值  
在发生错误的 CATCH 块中调用时，`ERROR_SEVERITY` 返回导致 `CATCH` 块运行的错误的严重性值。  

如果在 CATCH 块作用域外调用，`ERROR_SEVERITY` 返回 NULL。  
  
## <a name="remarks"></a>备注  
`ERROR_SEVERITY` 支持在 CATCH 块作用域内的任意位置调用。  
  
无论 `ERROR_SEVERITY` 运行多少次或在 `CATCH` 块作用域内的任意位置运行，它都将返回错误的错误严重性值。 这与 @@ERROR 之类的函数不同，后者只在导致错误的语句的后一个语句中返回错误号。  
  
`ERROR_SEVERITY` 通常在嵌套的 `CATCH` 块中运行。 `ERROR_SEVERITY` 返回特定于引用该 `CATCH` 块的 `CATCH` 块的作用域的错误严重性值。 例如，外部 TRY...CATCH 构造的 `CATCH` 块可能具有内部 `TRY...CATCH` 构造。 在该内部 `CATCH` 块内，`ERROR_SEVERITY` 将返回调用内部 `CATCH` 块的错误的严重性值。 如果 `ERROR_SEVERITY` 在外部 `CATCH` 块中运行，它将返回调用该外部 `CATCH` 块的错误的错误严重性值。  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-using-error_severity-in-a-catch-block"></a>A. 在 CATCH 块中使用 ERROR_SEVERITY  
下面的示例显示生成被零除错误的存储过程。 `ERROR_SEVERITY` 返回该错误的严重性值。  

```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_SEVERITY() AS ErrorSeverity;  
END CATCH;  
GO  
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```
-----------

(0 row(s) affected)

ErrorSeverity
-------------
16

(1 row(s) affected)

```  
  
### <a name="b-using-error_severity-in-a-catch-block-with-other-error-handling-tools"></a>B. 使用其他错误处理工具在 CATCH 块中使用 ERROR_SEVERITY  
下面示例显示生成被零除错误的 `SELECT` 语句。 存储过程返回有关此错误的信息。  

```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_LINE() AS ErrorLine,  
        ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```
-----------

(0 row(s) affected)

ErrorNumber ErrorSeverity ErrorState  ErrorProcedure  ErrorLine   ErrorMessage
----------- ------------- ----------- --------------- ----------- ----------------------------------
8134        16            1           NULL            4           Divide by zero error encountered.

(1 row(s) affected)

```  
  
## <a name="see-also"></a>另请参阅  
 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [TRY...CATCH (Transact-SQL)](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE (Transact-SQL)](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE (Transact-SQL)](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER (Transact-SQL)](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE (Transact-SQL)](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_STATE (Transact-SQL&)](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR (Transact-SQL)](../../t-sql/functions/error-transact-sql.md)  
 [错误和事件参考（数据库引擎）](../../relational-databases/errors-events/errors-and-events-reference-database-engine.md)     
  
    

