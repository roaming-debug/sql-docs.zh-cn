---
description: SET IMPLICIT_TRANSACTIONS (Transact-SQL)
title: SET IMPLICIT_TRANSACTIONS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IMPLICIT_TRANSACTIONS
- SET IMPLICIT_TRANSACTIONS
- IMPLICIT_TRANSACTIONS_TSQL
- SET_IMPLICIT_TRANSACTIONS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- implicit transactions
- transactions [SQL Server], implicit
- connections [SQL Server], implicit transaction mode
- SET IMPLICIT_TRANSACTIONS statement
- IMPLICIT_TRANSACTIONS option
ms.assetid: a300ac43-e4c0-4329-8b79-a1a05e63370a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 38b45a33588220591326841a7b22f117022627e5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754297"
---
# <a name="set-implicit_transactions-transact-sql"></a>SET IMPLICIT_TRANSACTIONS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为了连接，将 BEGIN TRANSACTION 模式设置为“隐式”。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
SET IMPLICIT_TRANSACTIONS { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注
 为 ON 时，系统处于“隐式”事务模式。 这意味着如果 @@TRANCOUNT = 0，下列任一 Transact-SQL 语句都会开始新事务。 这等同于先执行一个不可见的 BEGIN TRANSACTION：  

:::row:::
    :::column:::
        ALTER TABLE
    :::column-end:::
    :::column:::
        FETCH
    :::column-end:::
    :::column:::
        REVOKE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        BEGIN TRANSACTION
    :::column-end:::
    :::column:::
        GRANT
    :::column-end:::
    :::column:::
        SELECT（参见下面的例外情况。）
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        CREATE
    :::column-end:::
    :::column:::
        INSERT
    :::column-end:::
    :::column:::
        TRUNCATE TABLE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        DELETE
    :::column-end:::
    :::column:::
        MERGE
    :::column-end:::
    :::column:::
        UPDATE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        DROP
    :::column-end:::
    :::column:::
        OPEN
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

&nbsp;

 为 OFF 时，上述每个 T-SQL 语句都受一个不可见的 BEGIN TRANSACTION 和一个不可见的 COMMIT TRANSACTION 语句限制。 为 OFF 时，事务模式为自动提交。 如果 T-SQL 代码发出了一个可见 BEGIN TRANSACTION，那么事务模式为显式。  
  
 有几点需要说明：  
  
-   事务模式为隐式时，如果 @@trancount > 0，则不会发出不可见的 BEGIN TRANSACTION。 但是，任何显式 BEGIN TRANSACTION 语句都会递增 @@TRANCOUNT。  
  
-   INSERT 语句和工作单元中的其他任务完成后，须发出 COMMIT TRANSACTION 语句，直到 @@TRANCOUNT 递减为 0。 也可以发出一个 ROLLBACK TRANSACTION。  
  
-   不会从表中选择的 SELECT 语句不会启动隐式事务。 例如，`SELECT GETDATE();` 或 `SELECT 1, 'ABC';` 不需要事务。  
  
-   由于 ANSI 默认值的原因，可能会意外打开隐式事务。 有关详细信息，请参阅 [SET ANSI_DEFAULTS (Transact-SQL)](../../t-sql/statements/set-ansi-defaults-transact-sql.md).  
  
     IMPLICIT_TRANSACTIONS ON 不常用。 大多数情况下，IMPLICIT_TRANSACTIONS 为 ON，是因为选择了 SET ANSI_DEFAULTS ON。  
  
-   进行连接时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB Provider for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序会自动将 IMPLICIT_TRANSACTIONS 设置为 OFF。 对于与 SQLClient 托管提供程序进行连接，及通过 HTTP 端点接收的 SOAP 请求，SET IMPLICIT_TRANSACTIONS 默认为 OFF。  
  
 要查看 IMPLICIT_TRANSACTIONS 的当前设置，请运行以下查询。  
  
```sql
DECLARE @IMPLICIT_TRANSACTIONS VARCHAR(3) = 'OFF';  
IF ( (2 & @@OPTIONS) = 2 ) SET @IMPLICIT_TRANSACTIONS = 'ON';  
SELECT @IMPLICIT_TRANSACTIONS AS IMPLICIT_TRANSACTIONS;  
```  
  
## <a name="examples"></a>示例  
 以下 Transact-SQL 脚本运行几个不同的测试用例。 还提供了文本输出，显示了每个测试用例的详细行为和结果。  
  
```sql  
-- Transact-SQL.  
-- Preparations.  
SET NOCOUNT ON;  
SET IMPLICIT_TRANSACTIONS OFF;  
GO  
WHILE (@@TranCount > 0) COMMIT TRANSACTION;  
GO  
IF (OBJECT_ID(N'dbo.t1',N'U') IS NOT NULL) DROP TABLE dbo.t1;  
GO  
CREATE table dbo.t1 (a INT);  
GO  
  
PRINT N'-------- [Test A] ---- OFF ----';  
PRINT N'[A.01] Now, SET IMPLICIT_TRANSACTIONS OFF.';  
PRINT N'[A.02] @@TranCount, at start, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
SET IMPLICIT_TRANSACTIONS OFF;  
GO 
INSERT INTO dbo.t1 VALUES (11);  
INSERT INTO dbo.t1 VALUES (12);  
PRINT N'[A.03] @@TranCount, after INSERTs, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO  
  
PRINT N' ';  
PRINT N'-------- [Test B] ---- ON ----';  
PRINT N'[B.01] Now, SET IMPLICIT_TRANSACTIONS ON.';  
PRINT N'[B.02] @@TranCount, at start, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
SET IMPLICIT_TRANSACTIONS ON;  
GO
INSERT INTO dbo.t1 VALUES (21);  
INSERT INTO dbo.t1 VALUES (22);  
PRINT N'[B.03] @@TranCount, after INSERTs, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO 
COMMIT TRANSACTION;  
PRINT N'[B.04] @@TranCount, after COMMIT, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO
  
PRINT N' ';  
PRINT N'-------- [Test C] ---- ON, then BEGIN TRAN ----';  
PRINT N'[C.01] Now, SET IMPLICIT_TRANSACTIONS ON.';  
PRINT N'[C.02] @@TranCount, at start, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
SET IMPLICIT_TRANSACTIONS ON;  
GO  
BEGIN TRANSACTION;  
INSERT INTO dbo.t1 VALUES (31);  
INSERT INTO dbo.t1 VALUES (32);  
PRINT N'[C.03] @@TranCount, after INSERTs, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO  
COMMIT TRANSACTION;  
PRINT N'[C.04] @@TranCount, after a COMMIT, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
COMMIT TRANSACTION;  
PRINT N'[C.05] @@TranCount, after another COMMIT, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO
  
PRINT N' ';  
PRINT N'-------- [Test D] ---- ON, INSERT, BEGIN TRAN, INSERT ----';  
PRINT N'[D.01] Now, SET IMPLICIT_TRANSACTIONS ON.';  
PRINT N'[D.02] @@TranCount, at start, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
SET IMPLICIT_TRANSACTIONS ON;  
GO 
INSERT INTO dbo.t1 VALUES (41);  
BEGIN TRANSACTION;  
INSERT INTO dbo.t1 VALUES (42);  
PRINT N'[D.03] @@TranCount, after INSERTs, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO 
COMMIT TRANSACTION;  
PRINT N'[D.04] @@TranCount, after a COMMIT, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
COMMIT TRANSACTION;  
PRINT N'[D.05] @@TranCount, after another COMMIT, == ' + CAST(@@TRANCOUNT AS NVARCHAR(10));  
GO
  
-- Clean up.  
SET IMPLICIT_TRANSACTIONS OFF;  
GO  
WHILE (@@TranCount > 0) COMMIT TRANSACTION;  
GO  
DROP TABLE dbo.t1;  
GO
```  
  
 下面是上述 Transact-SQL 脚本的文本输出。  
  
```
-- Text output from Transact-SQL:  
  
-------- [Test A] ---- OFF ----  
[A.01] Now, SET IMPLICIT_TRANSACTIONS OFF.  
[A.02] @@TranCount, at start, == 0  
[A.03] @@TranCount, after INSERTs, == 0  
  
-------- [Test B] ---- ON ----  
[B.01] Now, SET IMPLICIT_TRANSACTIONS ON.  
[B.02] @@TranCount, at start, == 0  
[B.03] @@TranCount, after INSERTs, == 1  
[B.04] @@TranCount, after COMMIT, == 0  
  
-------- [Test C] ---- ON, then BEGIN TRAN ----  
[C.01] Now, SET IMPLICIT_TRANSACTIONS ON.  
[C.02] @@TranCount, at start, == 0  
[C.03] @@TranCount, after INSERTs, == 2  
[C.04] @@TranCount, after a COMMIT, == 1  
[C.05] @@TranCount, after another COMMIT, == 0  
  
-------- [Test D] ---- ON, INSERT, BEGIN TRAN, INSERT ----  
[D.01] Now, SET IMPLICIT_TRANSACTIONS ON.  
[D.02] @@TranCount, at start, == 0  
[D.03] @@TranCount, after INSERTs, == 2  
[D.04] @@TranCount, after INSERTs, == 1  
[D.05] @@TranCount, after INSERTs, == 0  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [BEGIN TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [DROP TABLE (Transact-SQL)](../../t-sql/statements/drop-table-transact-sql.md)   
 [FETCH (Transact-SQL)](../../t-sql/language-elements/fetch-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [MERGE (Transact-SQL)](../../t-sql/statements/merge-transact-sql.md) [OPEN (Transact-SQL)](../../t-sql/language-elements/open-transact-sql.md)   
 [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS (Transact-SQL)](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [@@TRANCOUNT (Transact-SQL)](../../t-sql/functions/trancount-transact-sql.md)   
 [TRUNCATE TABLE (Transact-SQL)](../../t-sql/statements/truncate-table-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)  
  
  
