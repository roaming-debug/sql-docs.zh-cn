---
description: '&#x40;&#x40;ROWCOUNT (Transact-SQL)'
title: '@@ROWCOUNT (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 08/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@ROWCOUNT_TSQL'
- '@@ROWCOUNT'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@ROWCOUNT function'
- number of rows affected by statement
- row affected by statements [SQL Server]
- statements [SQL Server], last statement
- counting rows
ms.assetid: 97a47998-81d9-4331-a244-9eb8b6fe4a56
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 6eca1ac99c93c8faa062b46b09fe3715663a5c87
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159715"
---
# <a name="x40x40rowcount-transact-sql"></a>&#x40;&#x40;ROWCOUNT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回受上一语句影响的行数。 如果行数大于 20 亿，请使用 [ROWCOUNT_BIG](../../t-sql/functions/rowcount-big-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
@@ROWCOUNT  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 **int**  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句可以通过下列方式设置 @@ROWCOUNT 的值：  
  
-   将 @@ROWCOUNT 设置为受影响或被读取的行的数目。 可以将行发送到客户端，也可以不发送。  
  
-   保留前一个语句执行中的 @@ROWCOUNT。  
  
-   将 @@ROWCOUNT 重置为 0 但不将该值返回到客户端。  
  
 执行简单分配的语句始终将 @@ROWCOUNT 值设置为 1。 不将任何行发送到客户端。 这些语句的示例包括：SET @local_variable、RETURN、READTEXT 和在没有 SELECT GETDATE() 或 SELECT 'Generic Text' 等查询语句的情况下选择。  
  
 在查询中进行分配或使用 RETURN 的语句将 @@ROWCOUNT 值设置为受查询影响或由查询读取的行数，例如：SELECT @local_variable = c1 FROM t1。  
  
 数据操作语言 (DML) 语句将 @@ROWCOUNT 值设置为受查询影响的行数，并将该值返回到客户端。 DML 语句不会将任何行发送到客户端。  
  
 DECLARE CURSOR 和 FETCH 将 @@ROWCOUNT 值设置为 1。  
  
 EXECUTE 语句保留前一个 @@ROWCOUNT。  
  
 USE、SET \<option>、DEALLOCATE CURSOR、CLOSE CURSOR、PRINT、RAISERROR、BEGIN TRANSACTION 或 COMMIT TRANSACTION 等语句将 ROWCOUNT 值重置为 0。  
  
 本机编译存储过程保留以前的 @@ROWCOUNT。 本机编译存储过程中的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句不设置 @@ROWCOUNT。 有关详细信息，请参阅[本机编译的存储过程](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md)。  
  
## <a name="examples"></a>示例  
 以下示例执行 `UPDATE` 语句并使用 `@@ROWCOUNT` 来检测是否更改了任何行。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE HumanResources.Employee   
SET JobTitle = N'Executive'  
WHERE NationalIDNumber = 123456789  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were updated';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [System Functions (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [SET ROWCOUNT (Transact-SQL)](../../t-sql/statements/set-rowcount-transact-sql.md)  
  
