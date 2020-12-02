---
title: 事务 (Azure Synapse Analytics)
description: 事务由一组全部提交或全部回滚的一个或多个数据库语句组成。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 87e5e593-a121-4428-9d3c-3af876224e35
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 5a8b1aa27a301d67df200967b6cba36f042a7f75
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92038891"
---
# <a name="transactions-azure-synapse-analytics"></a>事务 (Azure Synapse Analytics)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  事务由一组全部提交或全部回滚的一个或多个数据库语句组成。 每个事务都是原子级的、一致的、孤立的和持久的 (ACID)。 如果事务成功，其中的所有语句都将提交。 如果事务失败，则组中至少有一个语句失败，然后整个组都将回滚。  
  
 事务的起点和终点取决于 AUTOCOMMIT 设置以及 BEGIN TRANSACTION、COMMIT 和 ROLLBACK 语句。 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 支持下列事务类型：  
  
-   显式事务从 BEGIN TRANSACTION 语句开始，到 COMMIT 或 ROLLBACK 语句为止。  
  
-   自动提交事务在会话中自动启动，但不会从 BEGIN TRANSACTION 语句开始。 AUTOCOMMIT 设置为 ON 时，每个语句都在事务中运行，并且无需显式 COMMIT 或 ROLLBACK。 AUTOCOMMIT 设置为 OFF 时，需要 COMMIT 或 ROLLBACK 语句来确定事务的结果。 在 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 中，自动提交事务会在 COMMIT 或 ROLLBACK 语句或 SET AUTOCOMMIT OFF 语句后立刻开始。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
BEGIN TRANSACTION [;]  
COMMIT [ TRAN | TRANSACTION | WORK ] [;]  
ROLLBACK [ TRAN | TRANSACTION | WORK ] [;]  
SET AUTOCOMMIT { ON | OFF } [;]  
SET IMPLICIT_TRANSACTIONS { ON | OFF } [;]  
```  
  
## <a name="arguments"></a>参数  
 BEGIN TRANSACTION  
 标记显式事务的起始点。  
  
 COMMIT [ WORK ]  
 标记显式或自动提交事务的末尾。 此语句会导致事务中的更改始终提交到数据库。 COMMIT 语句等同于 COMMIT WORK、COMMIT TRAN 和 COMMIT TRANSACTION。  
  
 ROLLBACK [ WORK ]  
 将事务回滚到事务的起点。 事务的任何更改都不会提交到数据库。 ROLLBACK 语句等同于 ROLLBACK WORK、ROLLBACK TRAN 和 ROLLBACK TRANSACTION。  
  
 SET AUTOCOMMIT { ON | OFF }  
 确定事务的启动和结束方式。  
  
 ON  
 每个语句在其自己的事务下运行，不需要显式 COMMIT 或 ROLLBACK 语句。 AUTOCOMMIT 为 ON 时，允许显式事务。  
  
 OFF  
 如果没有事务正在运行，[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将自动启动事务。 任何后续语句都在事务运行期间运行，并且需要 COMMIT 或 ROLLBACK 来确定事务的结果。 在此操作模式下，当出现事务提交或回滚时，此模式将保持 OFF 状态，[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将启动新事务。 AUTOCOMMIT 为 OFF 时，不允许显式事务。  
  
 如果在事务活动期间更改 AUTOCOMMIT 设置，该设置不会影响当前事务，并且直到事务完成才会生效。  
  
 AUTOCOMMIT 为 ON 时，运行另一个 SET AUTOCOMMIT ON 语句将不起作用。 同样，AUTOCOMMIT 为 OFF 时，运行另一个 SET AUTOCOMMIT OFF 将不起作用。  
  
 SET IMPLICIT_TRANSACTIONS { ON | OFF }  
 这会切换为与 SET AUTOCOMMIT 相同的模式。 如果设置为 ON，SET IMPLICIT_TRANSACTIONS 将连接设置为隐式事务模式。 如果设置为 OFF，则使连接恢复为自动提交模式。  有关详细信息，请参阅 [SET IMPLICIT_TRANSACTIONS (Transact-SQL)](../../t-sql/statements/set-implicit-transactions-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 运行与事务相关的语句不需要特定权限。 运行事务内的语句需要相关权限。  
  
## <a name="error-handling"></a>错误处理  
 如果运行 COMMIT 或 ROLLBACK 并且没有活动事务，则将引发错误。  
  
 如果在运行事务的同时运行 BEGIN TRANSACTION，则将引发错误。 如果在成功的 BEGIN TRANSACTION 语句之后发生 BEGIN TRANSACTION，或会话处于 SET AUTOCOMMIT OFF 的条件下，则会出现上述情况。  
  
 如果运行时语句错误以外的错误使显式事务无法成功完成，[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将自动回滚事务并释放该事物占用的所有资源。 例如，如果客户端与 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 实例的网络连接中断或客户端已注销应用程序，那么当网络向实例通知该中断后，该连接的所有未提交事务均会被回滚。  
  
 如果批处理中出现运行时语句错误，[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 的行为将与设置为 ON 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] XACT_ABORT 的行为一致，并且整个事务都将回滚。 有关 XACT_ABORT 设置的详细信息，请参阅 [SET XACT_ABORT (Transact-SQL)](../statements/set-xact-abort-transact-sql.md)。  
  
## <a name="general-remarks"></a>一般备注  
 给定时间内，一个会话只能运行一个事务；不支持保存点和嵌套事务。  
  
 仅当事务引用的所有数据在逻辑上都正确时，[!INCLUDE[DWsql](../../includes/dwsql-md.md)] 程序员才应发出 COMMIT 命令。  
  
 如果会话在事务完成之前终止，事务将会回滚。  
  
 事务模式按会话级别进行管理。 例如，如果一个会话以显式事务开始，或将 AUTOCOMMIT 设置为 OFF，或将 IMPLICIT_TRANSACTIONS 设置为 ON，则它不会对任何其他会话的事务模式产生任何影响。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 不能在发出一个 COMMIT 语句之后回滚事务，因为数据修改已经成为数据库的一个永久部分。  
  
 不能在显式事务内使用 [CREATE DATABASE &#40;Azure Synapse Analytics&#41;](../statements/create-database-transact-sql.md) 和 [DROP DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-transact-sql.md) 命令。  
  
 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 没有事务共享机制。 这意味着，在任何给定时间点，只能有一个会话处理系统中的任何事务。  
  
## <a name="locking-behavior"></a>锁定行为  
 当多个用户同时访问数据时，[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 使用以下锁定确保事务的完整性和保持数据库的一致性。 隐式和显式事务均可使用锁定。 每个事务对所依赖的资源（如表或数据库）请求不同类型的锁。 所有 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 锁都是表级别或更高级别。 锁可以阻止其他事务以某种可能会导致事务请求锁出错的方式修改资源。 每个事务将在不再依赖锁定资源时释放锁；显式事务会保留锁，直到事务完成（提交或回滚）。  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-using-an-explicit-transaction"></a>A. 使用显式事务  
  
```sql  
BEGIN TRANSACTION;  
DELETE FROM HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
COMMIT;  
```  
  
### <a name="b-rolling-back-a-transaction"></a>B. 回滚事务  
 以下示例显示了回滚事务的效果。  在此示例中，ROLLBACK 语句将回滚 INSERT 语句，但已创建的表仍会存在。  
  
```sql  
CREATE TABLE ValueTable (id INT);  
BEGIN TRANSACTION;  
       INSERT INTO ValueTable VALUES(1);  
       INSERT INTO ValueTable VALUES(2);  
ROLLBACK;  
```  
  
### <a name="c-setting-autocommit"></a>C. 设置 AUTOCOMMIT  
 下面的示例将 AUTOCOMMIT 设置设为 `ON`。  
  
```sql  
SET AUTOCOMMIT ON;  
```  
  
 下面的示例将 AUTOCOMMIT 设置设为 `OFF`。  
  
```sql  
SET AUTOCOMMIT OFF;  
```  
  
### <a name="d-using-an-implicit-multi-statement-transaction"></a>D. 使用隐式多语句事务  
  
```sql  
SET AUTOCOMMIT OFF;  
CREATE TABLE ValueTable (id INT);  
INSERT INTO ValueTable VALUES(1);  
INSERT INTO ValueTable VALUES(2);  
COMMIT;  
```  
  
## <a name="see-also"></a>另请参阅  
 [SET IMPLICIT_TRANSACTIONS (Transact-SQL)](../../t-sql/statements/set-implicit-transactions-transact-sql.md)   
 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)   
 [@@TRANCOUNT (Transact-SQL)](../../t-sql/functions/trancount-transact-sql.md)  
  
