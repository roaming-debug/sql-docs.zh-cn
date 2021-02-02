---
description: ERROR_NUMBER (Transact-SQL)
title: ERROR_NUMBER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ERROR_NUMBER_TSQL
- ERROR_NUMBER
dev_langs:
- TSQL
helpviewer_keywords:
- errors [SQL Server], line number
- messages [SQL Server], numbers
- TRY...CATCH [SQL Server]
- ERROR_NUMBER function
- CATCH block
ms.assetid: 1de85fff-1ca2-4b31-841b-926e571cb150
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0f431c978c0470a7d865c3d1954f08d3e980aa6a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99101878"
---
# <a name="error_number-transact-sql"></a>ERROR_NUMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数返回错误的错误号，该错误导致执行了 TRY…CATCH 构造的 CATCH 块。  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
ERROR_NUMBER ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 **int**  
  
## <a name="return-value"></a>返回值  
在 CATCH 块中调用时，`ERROR_NUMBER` 返回导致 CATCH 块运行的错误的错误号。  

在 CATCH 块作用域外调用时，`ERROR_NUMBER` 返回 NULL。  
  
## <a name="remarks"></a>备注  
`ERROR_NUMBER` 支持在 CATCH 块作用域内的任意位置调用。  
  
无论 `ERROR_NUMBER` 运行多少次或在 `CATCH` 块作用域内的任意位置运行，它都将返回相关的错误号。 这与 @@ERROR 之类的函数不同，后者只在导致错误的语句的后一个语句中返回错误号。  

在嵌套 `CATCH` 块中，`ERROR_NUMBER` 返回特定于引用该 `CATCH` 块的 `CATCH` 块的作用域的错误号。 例如，外部 TRY...CATCH 构造的 `CATCH` 块可能具有内部 `TRY...CATCH` 构造。 在该内部 `CATCH` 块内，`ERROR_NUMBER` 将返回调用内部 `CATCH` 块的错误号。 如果 `ERROR_NUMBER` 在外部 `CATCH` 块中运行，它将返回调用该外部 `CATCH` 块的错误号。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-error_number-in-a-catch-block"></a>A. 在 CATCH 块中使用 ERROR_NUMBER  
此示例显示生成被零除错误的 `SELECT` 语句。 `CATCH` 块返回错误号。  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_NUMBER() AS ErrorNumber;  
END CATCH;  
GO  
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```
-----------

(0 row(s) affected)

ErrorNumber
-----------
8134

(1 row(s) affected)

```  
  
### <a name="b-using-error_number-in-a-catch-block-with-other-error-handling-tools"></a>B. 在包含其他错误处理工具的 CATCH 块中使用 ERROR_NUMBER  
此示例显示生成被零除错误的 `SELECT` 语句。 `CATCH` 块将返回错误号和有关此错误的信息。  

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

ErrorNumber ErrorSeverity ErrorState  ErrorProcedure   ErrorLine  ErrorMessage
----------- ------------- ----------- ---------------  ---------- ----------------------------------
8134        16            1           NULL             4          Divide by zero error encountered.

(1 row(s) affected)

```  
  
## <a name="see-also"></a>另请参阅  
 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [TRY...CATCH (Transact-SQL)](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE (Transact-SQL)](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE (Transact-SQL)](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_PROCEDURE (Transact-SQL)](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY (Transact-SQL)](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE (Transact-SQL&)](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR (Transact-SQL)](../../t-sql/functions/error-transact-sql.md)     
 [错误和事件参考（数据库引擎）](../../relational-databases/errors-events/errors-and-events-reference-database-engine.md)     
  
  

