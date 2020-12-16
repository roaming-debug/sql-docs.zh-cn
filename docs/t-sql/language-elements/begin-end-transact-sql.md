---
description: BEGIN...END (Transact-SQL)
title: BEGIN...END (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BEGIN
- BEGIN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- enclosing statements [SQL Server]
- BEGIN statement
- control-of-flow language [SQL Server], BEGIN...END statement
- BEGIN...END keyword
- grouping statements, BEGIN...END statement
- executing Transact-SQL statements together [SQL Server]
- statements [SQL Server], grouping
ms.assetid: fc2c7f76-f1f9-4f91-beef-bc8ef0da2feb
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 56c4c30c93c82709c1a6340358a19693164cc038
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461128"
---
# <a name="beginend-transact-sql"></a>BEGIN...END (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  包括一系列的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，从而可以执行一组 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 BEGIN 和 END 是控制流语言的关键字。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
BEGIN  
    { sql_statement | statement_block }   
END  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 { sql_statement | statement_block }    
 使用语句块定义的任何有效的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句或语句组。  
  
## <a name="remarks"></a>注解  
 BEGIN...END 语句块允许嵌套。  
  
 虽然所有的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句在 BEGIN...END 块内都有效，但有些 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句不应分组在同一批处理或语句块中。  
  
## <a name="examples"></a>示例  
 在下面的示例中，`BEGIN` 和 `END` 定义一系列一起执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 如果不包括 `BEGIN...END` 块，则将执行两个 `ROLLBACK TRANSACTION` 语句，并返回两条 `PRINT` 消息。  
  
```sql
USE AdventureWorks2012
GO  
BEGIN TRANSACTION
GO  
IF @@TRANCOUNT = 0  
BEGIN  
    SELECT FirstName, MiddleName   
    FROM Person.Person WHERE LastName = 'Adams'
    ROLLBACK TRANSACTION
    PRINT N'Rolling back the transaction two times would cause an error.'
END
ROLLBACK TRANSACTION
PRINT N'Rolled back the transaction.'
GO  
/*  
Rolled back the transaction.  
*/  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 在下面的示例中，`BEGIN` 和 `END` 定义一系列一起运行的 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] 语句。 如果不包括 `BEGIN...END` 块，以下示例将处于连续循环中。  
  
```sql
-- Uses AdventureWorks  

DECLARE @Iteration Integer = 0  
WHILE @Iteration <10  
BEGIN  
    SELECT FirstName, MiddleName   
    FROM dbo.DimCustomer WHERE LastName = 'Adams'
    SET @Iteration += 1  
END
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [控制流语言 (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [END (BEGIN...END) (Transact-SQL)](../../t-sql/language-elements/end-begin-end-transact-sql.md)
