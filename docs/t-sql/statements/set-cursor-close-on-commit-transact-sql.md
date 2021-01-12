---
description: SET CURSOR_CLOSE_ON_COMMIT (Transact-SQL)
title: SET CURSOR_CLOSE_ON_COMMIT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CURSOR_CLOSE_ON_COMMIT
- SET CURSOR_CLOSE_ON_COMMIT
- CURSOR_CLOSE_ON_COMMIT_TSQL
- SET_CURSOR_CLOSE_ON_COMMIT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CURSOR_CLOSE_ON_COMMIT option
- transactions [SQL Server], cursors
- closing cursors
- cursors [SQL Server], closing
- SET CURSOR_CLOSE_ON_COMMIT statement
ms.assetid: 7b976154-98ce-4a06-bbae-7e59c34211f7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f0a98ee853689cd05848526fbec4b685ca88b00e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091843"
---
# <a name="set-cursor_close_on_commit-transact-sql"></a>SET CURSOR_CLOSE_ON_COMMIT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  控制 [!INCLUDE[tsql](../../includes/tsql-md.md)] COMMIT TRANSACTION 语句的行为。 此设置的默认值为 OFF。 这表示提交事务时服务器将不会关闭游标。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
SET CURSOR_CLOSE_ON_COMMIT { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注
 如果 SET CURSOR_CLOSE_ON_COMMIT 为 ON，此设置将遵从 ISO 标准，在提交或回滚时关闭所有打开的游标。 如果 SET CURSOR_CLOSE_ON_COMMIT 为 OFF，则在提交事务时将不关闭游标。  
  
> [!NOTE]  
>  如果 SET CURSOR_CLOSE_ON_COMMIT 为 ON，则从 SAVE TRANSACTION 语句将回滚应用于 savepoint_name 时，将不关闭为回滚打开的游标。  
  
 如果 SET CURSOR_CLOSE_ON_COMMIT 为 OFF，则 ROLLBACK 语句只关闭未完全填充的打开的异步游标。 如果回滚了修改，则在修改之后打开的 STATIC 或 INSENSITIVE 游标将不再反映数据的状态。  
  
 SET CURSOR_CLOSE_ON_COMMIT 与 CURSOR_CLOSE_ON_COMMIT 数据库选项控制的行为相同。 如果 CURSOR_CLOSE_ON_COMMIT 设置为 ON 或 OFF，则该设置将用于连接。 如果尚未指定 SET CURSOR_CLOSE_ON_COMMIT，则应用 **sys.databases** 目录视图中的 **is_cursor_close_on_commit_on** 列中的值。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序在连接时都会将 CURSOR_CLOSE_ON_COMMIT 设置为 OFF。 DB-Library 不自动设置 CURSOR_CLOSE_ON_COMMIT 值。  
  
 如果 SET ANSI_DEFAULTS 为 ON，则会启用 SET CURSOR_CLOSE_ON_COMMIT。  
  
 SET CURSOR_CLOSE_ON_COMMIT 的设置是在执行或运行时设置的，而不是在分析时设置的。  
  
 要查看此设置的当前设置，请运行以下查询。  
  
```sql
DECLARE @CURSOR_CLOSE VARCHAR(3) = 'OFF';  
IF ( (4 & @@OPTIONS) = 4 ) SET @CURSOR_CLOSE = 'ON';  
SELECT @CURSOR_CLOSE AS CURSOR_CLOSE_ON_COMMIT;  
```  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
 以下示例将在事务中定义一个游标，并尝试在提交事务后使用该游标。  
  
```sql
-- SET CURSOR_CLOSE_ON_COMMIT  
-------------------------------------------------------------------------------  
SET NOCOUNT ON;  
  
CREATE TABLE t1 (a INT);  
GO   
  
INSERT INTO t1   
VALUES (1), (2);  
GO  
  
PRINT '-- SET CURSOR_CLOSE_ON_COMMIT ON';  
GO  
SET CURSOR_CLOSE_ON_COMMIT ON;  
GO  
PRINT '-- BEGIN TRAN';  
BEGIN TRAN;  
PRINT '-- Declare and open cursor';  
DECLARE testcursor CURSOR FOR  
    SELECT a FROM t1;  
OPEN testcursor;  
PRINT '-- Commit tran';  
COMMIT TRAN;  
PRINT '-- Try to use cursor';  
FETCH NEXT FROM testcursor;  
CLOSE testcursor;  
DEALLOCATE testcursor;  
GO  
PRINT '-- SET CURSOR_CLOSE_ON_COMMIT OFF';  
GO  
SET CURSOR_CLOSE_ON_COMMIT OFF;  
GO  
PRINT '-- BEGIN TRAN';  
BEGIN TRAN;  
PRINT '-- Declare and open cursor';  
DECLARE testcursor CURSOR FOR  
    SELECT a FROM t1;  
OPEN testcursor;  
PRINT '-- Commit tran';  
COMMIT TRAN;  
PRINT '-- Try to use cursor';  
FETCH NEXT FROM testcursor;  
CLOSE testcursor;  
DEALLOCATE testcursor;  
GO  
DROP TABLE t1;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [BEGIN TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [CLOSE (Transact-SQL)](../../t-sql/language-elements/close-transact-sql.md)   
 [COMMIT TRANSACTION (Transact-SQL)](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [ROLLBACK TRANSACTION (Transact-SQL)](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS (Transact-SQL)](../../t-sql/statements/set-ansi-defaults-transact-sql.md)  
  
  
