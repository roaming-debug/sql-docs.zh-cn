---
description: BEGIN TRANSACTION (Transact-SQL)
title: BEGIN TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- BEGIN_TRANSACTION_TSQL
- TRANSACTION_TSQL
- TRANSACTION
- BEGIN TRANSACTION
- BEGIN TRAN
- BEGIN_TRAN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction logs [SQL Server], BEGIN TRANSACTION statement
- marked transactions [SQL Server], BEGIN TRANSACTION statement
- BEGIN TRANSACTION statement
- local transactions [SQL Server]
- marking transactions [SQL Server]
- transactions [SQL Server], starting
- transaction names [SQL Server]
- starting point marked for transactions
- starting transactions
ms.assetid: c6258df4-11f1-416a-816b-54f98c11145e
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e36a40d8db05de839f1fedc6844c1d6cd907e885
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102535"
---
# <a name="begin-transaction-transact-sql"></a>BEGIN TRANSACTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  标记一个显式本地事务的起始点。 显式事务以 BEGIN TRANSACTION 语句开始，并以 COMMIT 或 ROLLBACK 语句结束。  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
--Applies to SQL Server and Azure SQL Database
 
BEGIN { TRAN | TRANSACTION }   
    [ { transaction_name | @tran_name_variable }  
      [ WITH MARK [ 'description' ] ]  
    ]  
[ ; ]  
```  
 
```syntaxsql
--Applies to Azure Synapse Analytics and Parallel Data Warehouse
 
BEGIN { TRAN | TRANSACTION }   
[ ; ]  
```  

  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 transaction_name  
 适用范围：SQL Server（从 2008 版开始）和 Azure SQL Database
 
 分配给事务的名称。 transaction_name 必须符合标识符规则，但标识符所包含的字符数不能大于 32。 仅在最外面的 BEGIN...COMMIT 或 BEGIN...ROLLBACK 嵌套语句对中使用事务名。 transaction_name 始终区分大小写，即使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例不区分大小写也是如此。  
  
 @*tran_name_variable*  
 适用范围：SQL Server（从 2008 版开始）和 Azure SQL Database
 
 用户定义的、含有有效事务名称的变量的名称。 必须使用 char、varchar、nchar 或 nvarchar 数据类型声明该变量   。 如果传递给该变量的字符多于 32 个，则仅使用前面的 32 个字符；其余的字符将被截断。  
  
 WITH MARK [ 'description' ]  
适用范围：SQL Server（从 2008 版开始）和 Azure SQL Database

指定在日志中标记事务。 description 是描述该标记的字符串。 在将长于 128 个字符的 description 存储到 msdb.dbo.logmarkhistory 表中之前，先将其截断为 128 个字符。  
  
 如果使用了 WITH MARK，则必须指定事务名。 WITH MARK 允许将事务日志还原到命名标记。  
  
## <a name="general-remarks"></a>一般备注
BEGIN TRANSACTION 使 @@TRANCOUNT 按 1 递增。
  
BEGIN TRANSACTION 代表一点，由连接引用的数据在该点逻辑和物理上都一致的。 如果遇上错误，在 BEGIN TRANSACTION 之后的所有数据改动都能进行回滚，以将数据返回到已知的一致状态。 每个事务继续执行直到它无误地完成并且用 COMMIT TRANSACTION 对数据库作永久的改动，或者遇上错误并且用 ROLLBACK TRANSACTION 语句擦除所有改动。  
  
BEGIN TRANSACTION 为发出本语句的连接启动一个本地事务。 根据当前事务隔离级别的设置，为支持该连接所发出的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句而获取的许多资源被该事务锁定，直到使用 COMMIT TRANSACTION 或 ROLLBACK TRANSACTION 语句完成该事务为止。 长时间处于未完成状态的事务会阻止其他用户访问这些锁定的资源，也会阻止日志截断。  
  
 虽然 BEGIN TRANSACTION 启动一个本地事务，但是在应用程序接下来执行一个必须记录的操作（如执行 INSERT、UPDATE 或 DELETE 语句）之前，它并不被记录在事务日志中。 应用程序能执行一些操作，例如为了保护 SELECT 语句的事务隔离级别而获取锁，但是直到应用程序执行一个修改操作后日志中才有记录。  
  
 在一系列嵌套的事务中用一个事务名给多个事务命名对该事务没有什么影响。 系统仅登记第一个（最外部的）事务名。 回滚到其他任何名称（有效的保存点名除外）都会产生错误。 事实上，回滚之前执行的任何语句都不会在错误发生时回滚。 这些语句仅当外层的事务回滚时才会进行回滚。  
  
 如果在语句提交或回滚之前执行了如下操作，由 BEGIN TRANSACTION 语句启动的本地事务将升级为分布式事务：  
  
-   执行一个引用链接服务器上的远程表的 INSERT、DELETE 或 UPDATE 语句。 如果用于访问链接服务器的 OLE DB 访问接口不支持 ITransactionJoin 接口，则 INSERT、UPDATE 或 DELETE 语句会失败。  
  
-   当启用了 REMOTE_PROC_TRANSACTIONS 选项时，将调用远程存储过程。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地副本成为事务控制器并且使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 分布式事务处理协调器 (MS DTC) 来管理分布式事务。  
  
 使用 BEGIN DISTRIBUTED TRANSACTION 可以将事务作为分布式事务显式执行。 有关详细信息，请参阅 [BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)。  
  
 SET IMPLICIT_TRANSACTIONS 设置为 ON 时，BEGIN TRANSACTION 语句创建两个嵌套的事务。 有关详细信息，请参阅 [SET IMPLICIT_TRANSACTIONS (Transact-SQL)](../../t-sql/statements/set-implicit-transactions-transact-sql.md)  
  
## <a name="marked-transactions"></a>标记的事务  
 WITH MARK 选项使事务名被置于事务日志中。 将数据库还原到早期状态时，可使用标记事务代替日期和时间。 有关详细信息，请参阅 [使用标记的事务一致地恢复相关的数据库的事务（完全恢复模式）](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md)和 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)。  
  
 另外，若要将一组相关数据库恢复到逻辑上一致的状态，必须使用事务日志标记。 标记可由分布式事务置于相关数据库的事务日志中。 将这组相关数据库恢复到这些标记将产生一组在事务上一致的数据库。 在相关数据库中放置标记需要特殊的过程。  
  
 只有当数据库由标记事务更新时，才在事务日志中放置标记。 不修改数据的事务不被标记。  
  
 在已存在的未标记事务中可以嵌套 BEGIN TRAN new_name WITH MARK。 嵌套后，new_name 便成为事务的标记名，不论是否已为事务提供了名称。 在以下示例中，`M2` 是标记名。  
  
```sql  
BEGIN TRAN T1;  
UPDATE table1 ...;  
BEGIN TRAN M2 WITH MARK;  
UPDATE table2 ...;  
SELECT * from table1;  
COMMIT TRAN M2;  
UPDATE table3 ...;  
COMMIT TRAN T1;  
```  
  
 嵌套事务时，尝试标记一个已标记的事务将产生警告（非错误）消息：  
  
 "BEGIN TRAN T1 WITH MARK ...;"  
  
 "UPDATE table1 ...;"  
  
 "BEGIN TRAN M2 WITH MARK ...;"  
  
 "Server: Msg 3920, Level 16, State 1, Line 3"  
  
 "WITH MARK option only applies to the first BEGIN TRAN WITH MARK."  
  
 "The option is ignored."  
  
## <a name="permissions"></a>权限  
 要求具有 public 角色的成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-an-explicit-transaction"></a>A. 使用显式事务
**适用对象：** SQL Server（从 2008 版开始）、Azure SQL 数据库、[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]、并行数据仓库

本示例使用 AdventureWorks。 

```sql
BEGIN TRANSACTION;  
DELETE FROM HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
COMMIT;  
```

### <a name="b-rolling-back-a-transaction"></a>B. 回滚事务
**适用对象：** SQL Server（从 2008 版开始）、Azure SQL 数据库、[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]、并行数据仓库

以下示例显示了回滚事务的效果。 在此示例中，ROLLBACK 语句将回滚 INSERT 语句，但已创建的表仍会存在。

```sql
CREATE TABLE ValueTable (id INT);  
BEGIN TRANSACTION;  
       INSERT INTO ValueTable VALUES(1);  
       INSERT INTO ValueTable VALUES(2);  
ROLLBACK;  

```

### <a name="c-naming-a-transaction"></a>C. 命名事务 
适用范围：SQL Server（从 2008 版开始）和 Azure SQL Database

下面的示例说明如何命名事务。  
  
```sql
DECLARE @TranName VARCHAR(20);  
SELECT @TranName = 'MyTransaction';  
  
BEGIN TRANSACTION @TranName;  
USE AdventureWorks2012;  
DELETE FROM AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
  
COMMIT TRANSACTION @TranName;  
GO  
```  
  
### <a name="d-marking-a-transaction"></a>D. 标记事务  
适用范围：SQL Server（从 2008 版开始）和 Azure SQL Database

以下示例显示如何标记事务。 将标记事务 `CandidateDelete`。  
  
```sql  
BEGIN TRANSACTION CandidateDelete  
    WITH MARK N'Deleting a Job Candidate';  
GO  
USE AdventureWorks2012;  
GO  
DELETE FROM AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
GO  
COMMIT TRANSACTION CandidateDelete;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [COMMIT TRANSACTION (Transact-SQL)](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK (Transact-SQL)](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION (Transact-SQL)](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK (Transact-SQL)](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION (Transact-SQL)](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
