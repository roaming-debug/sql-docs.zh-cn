---
description: END (BEGIN...END) (Transact-SQL)
title: END (BEGIN...END) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- END
- END_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- enclosing statements [SQL Server]
- END keyword
- BEGIN...END keyword
- END (BEGIN...END) keyword
ms.assetid: 354c4935-1375-4141-8195-61326662f4d2
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 15958d1b227f38a14048cb8290e8d492766800e4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102415"
---
# <a name="end-beginend-transact-sql"></a>END (BEGIN...END) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  括号中包含一系列 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，这些语句作为一个组执行。 BEGIN...END 语句块允许嵌套。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
BEGIN   
     { sql_statement | statement_block }   
END   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 { sql_statement| statement_block}   
 任何有效的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句或用语句块定义的语句分组。 若要定义语句块（批处理），请使用控制流语言关键字 BEGIN 和 END。 虽然所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句在 BEGIN...END 块内都有效，但有些 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句不能组合到同一个批（语句块）中。  
  
## <a name="result-types"></a>结果类型  
 **布尔值**  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 在下面的示例中，`BEGIN` 和 `END` 定义一系列一起运行的 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] 语句。 如果不包括 `BEGIN...END` 块，以下示例将处于连续循环中。  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @Iteration INTEGER = 0  
WHILE @Iteration <10  
BEGIN  
    SELECT FirstName, MiddleName   
    FROM dbo.DimCustomer WHERE LastName = 'Adams';  
SET @Iteration += 1  
END;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [BEGIN...END (Transact-SQL)](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [控制流语言 (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [ELSE (IF...ELSE) (Transact-SQL)](../../t-sql/language-elements/else-if-else-transact-sql.md)   
 [IF...ELSE (Transact-SQL)](../../t-sql/language-elements/if-else-transact-sql.md)   
 [WHILE (Transact-SQL)](../../t-sql/language-elements/while-transact-sql.md)  
  
  


