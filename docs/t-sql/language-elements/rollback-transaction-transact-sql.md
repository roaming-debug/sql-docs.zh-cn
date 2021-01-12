---
description: ROLLBACK TRANSACTION (Transact-SQL)
title: ROLLBACK TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ROLLBACK TRANSACTION
- ROLLBACK
- ROLLBACK_TSQL
- ROLLBACK_TRANSACTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transaction rollbacks [SQL Server]
- ROLLBACK TRANSACTION statement
- erasing data modifications [SQL Server]
- rolling back transactions, ROLLBACK TRANSACTION
- roll back transactions [SQL Server]
- savepoints [SQL Server]
ms.assetid: 6882c5bc-ff74-476a-984b-164aeb036c66
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c785699c469ca1337eaa1a6b27fbab9aed149a5e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097037"
---
# <a name="rollback-transaction-transact-sql"></a>ROLLBACK TRANSACTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  将显式事务或隐性事务回滚到事务的起点或事务内的某个保存点。 可以使用 ROLLBACK TRANSACTION 清除自事务的起点或到某个保存点所做的所有数据修改。 它还释放由事务控制的资源。  
  
  这不包括对局部变量或表变量所做的更改。 该语句不会清除这些更改。
  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ROLLBACK { TRAN | TRANSACTION }   
     [ transaction_name | @tran_name_variable  
     | savepoint_name | @savepoint_variable ]   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 transaction_name  
 是为 BEGIN TRANSACTION 上的事务分配的名称。 transaction_name 必须符合标识符规则，但只使用事务名称的前 32 个字符。 嵌套事务时，transaction_name 必须是最外面的 BEGIN TRANSACTION 语句中的名称。 transaction_name 始终区分大小写，即使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例不区分大小写也是如此。  
  
 **@** *tran_name_variable*  
 用户定义的、含有有效事务名称的变量的名称。 必须使用 char、varchar、nchar 或 nvarchar 数据类型声明该变量   。  
  
 savepoint_name  
 SAVE TRANSACTION 语句中的 savepoint_name。 savepoint_name 必须遵守标识符规则。 当条件回滚应只影响事务的一部分时，可使用 savepoint_name。  
  
 **@** *savepoint_variable*  
 是用户定义的、包含有效保存点名称的变量的名称。 必须使用 char、varchar、nchar 或 nvarchar 数据类型声明该变量   。  
  
## <a name="error-handling"></a>错误处理  
 ROLLBACK TRANSACTION 语句不生成显示给用户的消息。 如果在存储过程或触发器中需要警告，请使用 RAISERROR 或 PRINT 语句。 RAISERROR 是用于指出错误的首选语句。  
  
## <a name="general-remarks"></a>一般备注  
 不带 savepoint_name 和 transaction_name 的 ROLLBACK TRANSACTION 将回滚到事务的起点。 嵌套事务时，该语句将所有内层事务回滚到最外面的 BEGIN TRANSACTION 语句。 在这两种情况下，ROLLBACK TRANSACTION 都将 @@TRANCOUNT 系统函数减小为 0。 ROLLBACK TRANSACTION savepoint_name 不会减小 @@TRANCOUNT。  
  
 在由 BEGIN DISTRIBUTED TRANSACTION 显式启动或从本地事务升级而来的分布式事务中，ROLLBACK TRANSACTION 不能引用 savepoint_name。  
  
 在执行 COMMIT TRANSACTION 语句后不能回滚事务，但是 COMMIT TRANSACTION 与包含在要回滚的事务中的嵌套事务关联时除外。 在这种情况下，嵌套事务会回滚，即使已对它发出 COMMIT TRANSACTION，也不例外。  
  
 在事务内允许有重复的保存点名称，但如果 ROLLBACK TRANSACTION 使用重复的保存点名称，则只回滚到最近的使用该保存点名称的 SAVE TRANSACTION。  
  
## <a name="interoperability"></a>互操作性  
 在存储过程中，不带 savepoint_name 或 transaction_name 的 ROLLBACK TRANSACTION 语句会将所有语句回滚到最外面的 BEGIN TRANSACTION。 在存储过程中，ROLLBACK TRANSACTION 语句使 @@TRANCOUNT 在存储过程完成时的值不同于调用此存储过程时的 @@TRANCOUNT 值，并且生成信息性消息。 该信息不影响后面的处理。  
  
 如果在触发器中发出 ROLLBACK TRANSACTION：  
  
-   将回滚对当前事务中的那一点所做的所有数据修改，包括触发器所做的修改。  
  
-   触发器继续执行 ROLLBACK 语句之后的所有其余语句。 如果这些语句中的任意语句修改数据，则不回滚这些修改。 执行其余的语句不会激发嵌套触发器。  
  
-   在批处理中，不执行所有位于激发触发器的语句之后的语句。  
  
当输入触发器时，@@TRANCOUNT 将以 1 递增，即使在自动提交模式下也是如此。 （系统将触发器视为隐含的嵌套事务处理。）  
  
在存储过程中，ROLLBACK TRANSACTION 语句不影响调用该过程的批处理中的后续语句；将执行批处理中的后续语句。 在触发器中，ROLLBACK TRANSACTION 语句终止包含激发触发器的语句的批处理；不执行批处理中的后续语句。  
  
ROLLBACK 对游标的影响由下面三个规则定义：  
  
1.  当 CURSOR_CLOSE_ON_COMMIT 设置为 ON 时，ROLLBACK 关闭但不释放所有打开的游标。  
  
2.  当 CURSOR_CLOSE_ON_COMMIT 设置为 OFF 时，ROLLBACK 不影响任何打开的同步 STATIC 或 INSENSITIVE 游标，也不影响已完全填充的异步 STATIC 游标。 将关闭但不释放任何其他类型的打开的游标。  
  
3.  终止批处理并生成内部回滚的错误将释放在包含错误声明的批处理中声明的所有游标。 将释放所有游标，而不考虑它们的类型或 CURSOR_CLOSE_ON_COMMIT 的设置情况。 这包括在错误批处理调用的存储过程中声明的游标。 在错误批处理之前的批处理中声明的游标遵循规则 1 和 2。 死锁错误便是此类错误的一个示例。 在触发器中发出的 ROLLBACK 语句也将自动生成此类错误。  
  
## <a name="locking-behavior"></a>锁定行为  
 指定了 savepoint_name 的 ROLLBACK TRANSACTION 语句释放在保存点之后获得的任何锁，但升级和转换除外。 这些锁不会被释放，而且不会转换回先前的锁模式。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
 以下示例显示了回滚已命名事务的效果。 创建表后，下列语句将启动已命名事务，插入两行，然后回滚到在 @TransactionName 变量中命名的事务。 已命名的事务外的另一个语句将插入两行。 查询将返回前面的语句的结果。   
  
```sql    
USE tempdb;  
GO  
CREATE TABLE ValueTable ([value] INT);  
GO  
  
DECLARE @TransactionName VARCHAR(20) = 'Transaction1';  
  
BEGIN TRAN @TransactionName  
       INSERT INTO ValueTable VALUES(1), (2);  
ROLLBACK TRAN @TransactionName;  
  
INSERT INTO ValueTable VALUES(3),(4);  
  
SELECT [value] FROM ValueTable;  
  
DROP TABLE ValueTable;  
```  
[!INCLUDE[ssresult-md](../../includes/ssresult-md.md)]  
```  
value  
-----   
3    
4  
```  
  
  
## <a name="see-also"></a>另请参阅  
 [BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [BEGIN TRANSACTION (Transact-SQL)](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION (Transact-SQL)](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK (Transact-SQL)](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK WORK (Transact-SQL)](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION (Transact-SQL)](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
