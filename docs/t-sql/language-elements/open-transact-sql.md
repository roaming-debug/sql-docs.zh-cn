---
description: OPEN (Transact-SQL)
title: OPEN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- OPEN_TSQL
- OPEN
dev_langs:
- TSQL
helpviewer_keywords:
- opening cursors
- cursors [SQL Server], opening
- populating cursors [SQL Server]
- OPEN statement
- Transact-SQL cursors, opening
ms.assetid: fd1c5e3b-6045-4a42-b646-3fca76e874c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: c0cab0ea4ed5eecc224c478018c16bf9d078d02f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102397"
---
# <a name="open-transact-sql"></a>OPEN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  打开 [!INCLUDE[tsql](../../includes/tsql-md.md)] 服务器游标，然后通过执行在 DECLARE CURSOR 或 SET *cursor_variable* 语句中指定的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句填充游标。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
OPEN { { [ GLOBAL ] cursor_name } | cursor_variable_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 GLOBAL  
 指定 cursor_name 是指全局游标。  
  
 cursor_name  
 已声明的游标的名称。 当同时存在以 cursor_name 作为名称的全局游标和局部游标时，如果指定 GLOBAL，则 cursor_name 是指全局游标；否则，cursor_name 是指局部游标。  
  
 cursor_variable_name  
 游标变量的名称，该变量引用一个游标。  
  
## <a name="remarks"></a>备注  
 如果使用 INSENSITIVE 或 STATIC 选项声明了游标，那么 OPEN 将创建一个临时表以保留结果集。 如果结果集中任意行的大小超过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表的最大行大小，OPEN 将失败。 如果使用 KEYSET 选项声明了游标，那么 OPEN 将创建一个临时表以保留键集。 临时表存储在 tempdb 中。  
  
 打开游标后，可以使用 @@CURSOR_ROWS 函数在上次打开的游标中接收合格行的数目。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不支持异步生成由键集驱动的或静态的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 游标。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 游标操作（如 OPEN 或 FETCH）均为批处理，所以无需异步生成 [!INCLUDE[tsql](../../includes/tsql-md.md)] 游标。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 由于每个游标操作都需要进行客户端往返，因此继续支持异步的由键集驱动的或静态的应用程序编程接口 (API) 服务器游标，对于这些游标，OPEN 实现低延迟时间很重要。  
  
## <a name="examples"></a>示例  
 以下示例打开一个游标并提取所有的行。  
  
```sql  
DECLARE Employee_Cursor CURSOR FOR  
SELECT LastName, FirstName  
FROM AdventureWorks2012.HumanResources.vEmployee  
WHERE LastName like 'B%';  
  
OPEN Employee_Cursor;  
  
FETCH NEXT FROM Employee_Cursor;  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    FETCH NEXT FROM Employee_Cursor  
END;  
  
CLOSE Employee_Cursor;  
DEALLOCATE Employee_Cursor;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CLOSE (Transact-SQL)](../../t-sql/language-elements/close-transact-sql.md)   
 [@@CURSOR_ROWS (Transact-SQL)](../../t-sql/functions/cursor-rows-transact-sql.md)   
 [DEALLOCATE (Transact-SQL)](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [DECLARE CURSOR (Transact-SQL)](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [FETCH (Transact-SQL)](../../t-sql/language-elements/fetch-transact-sql.md)  
  
  
