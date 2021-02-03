---
description: SET TRANSACTION ISOLATION LEVEL (Transact-SQL)
title: SET TRANSACTION ISOLATION LEVEL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/22/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LEVEL
- LEVEL_TSQL
- SET TRANSACTION ISOLATION LEVEL
- ISOLATION
- ISOLATION_TSQL
- SET_TRANSACTION_ISOLATION_LEVEL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET TRANSACTION ISOLATION LEVEL statement
- row versioning [SQL Server], isolation levels
- TRANSACTION ISOLATION LEVEL option
- isolation levels [SQL Server], setting
- locking [SQL Server], isolation levels
- transactions [SQL Server], isolation levels
ms.assetid: 016fb05e-a702-484b-bd2a-a6eabd0d76fd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7fd0098e9b4e213619e65973c2367a03f0197852
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161097"
---
# <a name="set-transaction-isolation-level-transact-sql"></a>SET TRANSACTION ISOLATION LEVEL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


控制到 [!INCLUDE[tsql](../../includes/tsql-md.md)] 的连接发出的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句的锁定行为和行版本控制行为。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
SET TRANSACTION ISOLATION LEVEL
    { READ UNCOMMITTED
    | READ COMMITTED
    | REPEATABLE READ
    | SNAPSHOT
    | SERIALIZABLE
    }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse
  
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED
```

>[!NOTE]
> [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 实现 ACID 事务。 事务支持的隔离级别默认为 READ UNCOMMITTED。  你可以通过在连接到主数据库时为用户数据库打开 READ_COMMITTED_SNAPSHOT 数据库选项，将默认的隔离级别更改为 READ COMMITTED SNAPSHOT ISOLATION。  启用后，将在 READ COMMITTED SNAPSHOT ISOLATION 下执行此数据库中的所有事务，并且将不接受会话级别的设置 READ UNCOMMITTED。 有关详细信息，请查看 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)。  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 READ UNCOMMITTED  
 指定语句可以读取已由其他事务修改但尚未提交的行。  
  
 在 READ UNCOMMITTED 级别运行的事务，不会发出共享锁来防止其他事务修改当前事务读取的数据。 READ UNCOMMITTED 事务也不会被排他锁阻塞，排他锁会禁止当前事务读取其他事务已修改但尚未提交的行。 设置此选项之后，可以读取未提交的修改，这种读取称为脏读。 在事务结束之前，可以更改数据中的值，行也可以出现在数据集中或从数据集中消失。 该选项的作用与在事务内所有 SELECT 语句中的所有表上设置 NOLOCK 相同。 这是隔离级别中限制最少的级别。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，您还可以使用下列任意一种方法，在保护事务不脏读未提交的数据修改的同时尽量减少锁定争用：  
  
-   READ COMMITTED 隔离级别，并将 READ_COMMITTED_SNAPSHOT 数据库选项设置为 ON。  
  
-   SNAPSHOT 隔离级别。 有关快照隔离的详细信息，请参阅 [SQL Server 中的快照隔离](/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server)。 
  
 READ COMMITTED  
 指定语句不能读取已由其他事务修改但尚未提交的数据。 这样可以避免脏读。 其他事务可以在当前事务的各个语句之间更改数据，从而产生不可重复读取和虚拟数据。 该选项是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的默认设置。  
  
 READ COMMITTED 的行为取决于 READ_COMMITTED_SNAPSHOT 数据库选项的设置：  
  
-   如果将 READ_COMMITTED_SNAPSHOT 设置为 OFF（SQL Server 上的默认设置），则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 会使用共享锁防止其他事务在当前事务执行读取操作期间修改行。 共享锁还会阻止语句在其他事务完成之前读取由这些事务修改的行。 共享锁类型确定它将于何时释放。 行锁在处理下一行之前释放。 页锁在读取下一页时释放，表锁在语句完成时释放。  
  
-   如果将 READ_COMMITTED_SNAPSHOT 设置为 ON（Azure SQL 数据库上的默认设置），则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 会使用行版本控制为每个语句提供一个在事务上一致的数据快照，因为该数据在语句开始时就存在。 不使用锁来防止其他事务更新数据。

> [!IMPORTANT]  
> 选择事务隔离级别不影响为保护数据修改而获取的锁。 事务总是在其修改的任何数据上获取排他锁并在事务完成之前持有该锁，不管为该事务设置了什么样的隔离级别。 此外，在 READ_COMMITTED 隔离级别进行的更新使用所选数据行的更新锁，而在 SNAPSHOT 隔离级别进行的更新使用行版本来选择要更新的行。 对于读取操作，事务隔离级别主要定义保护级别，以防受到其他事务所做更改的影响。 有关详细信息，请参阅[事务锁定和行版本控制指南](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md)。

> [!NOTE]  
>  快照隔离支持 FILESTREAM 数据。 在快照隔离模式下，事务中任何语句读取的 FILESTREAM 数据都将是在事务开始时便存在的数据的事务性一致版本。  
  
 当 READ_COMMITTED_SNAPSHOT 数据库选项设置为 ON 时，您可以使用 READCOMMITTEDLOCK 表提示为 READ COMMITTED 隔离级别上运行的事务中的各语句请求共享锁，而不是行版本控制。  
  
> [!NOTE]  
>  设置 READ_COMMITTED_SNAPSHOT 选项时，数据库中仅允许存在执行 ALTER DATABASE 命令的连接。 在 ALTER DATABASE 完成之前，数据库中不允许有其他打开的连接。 数据库不必处于单用户模式。  
  
 REPEATABLE READ  
 指定语句不能读取已由其他事务修改但尚未提交的行，并且指定，其他任何事务都不能在当前事务完成之前修改由当前事务读取的数据。  
  
 对事务中的每个语句所读取的全部数据都设置了共享锁，并且该共享锁一直保持到事务完成为止。 这样可以防止其他事务修改当前事务读取的任何行。 其他事务可以插入与当前事务所发出语句的搜索条件相匹配的新行。 如果当前事务随后重试执行该语句，它会检索新行，从而产生虚拟读取。 由于共享锁一直保持到事务结束，而不是在每个语句结束时释放，因此并发级别低于默认的 READ COMMITTED 隔离级别。 此选项只在必要时使用。  
  
 SNAPSHOT  
 指定事务中任何语句读取的数据都将是在事务开始时便存在的数据的事务上一致的版本。 事务只能识别在其开始之前提交的数据修改。 在当前事务中执行的语句将看不到在当前事务开始以后由其他事务所做的数据修改。 其效果就好像事务中的语句获得了已提交数据的快照，因为该数据在事务开始时就存在。  
  
 除非正在恢复数据库，否则 SNAPSHOT 事务不会在读取数据时请求锁。 读取数据的 SNAPSHOT 事务不会阻止其他事务写入数据。 写入数据的事务也不会阻止 SNAPSHOT 事务读取数据。  
  
 在数据库恢复的回滚阶段，如果尝试读取由其他正在回滚的事务锁定的数据，则 SNAPSHOT 事务将请求一个锁。 在事务完成回滚之前，SNAPSHOT 事务会一直被阻塞。 当事务取得授权之后，便会立即释放锁。  
  
 必须将 ALLOW_SNAPSHOT_ISOLATION 数据库选项设置为 ON，才能开始一个使用 SNAPSHOT 隔离级别的事务。 如果使用 SNAPSHOT 隔离级别的事务访问多个数据库中的数据，则必须在每个数据库中将 ALLOW_SNAPSHOT_ISOLATION 都设置为 ON。  
  
 不能将通过其他隔离级别开始的事务设置为 SNAPSHOT 隔离级别，否则将导致事务中止。 如果一个事务在 SNAPSHOT 隔离级别开始，则可以将它更改为另一个隔离级别，然后再返回 SNAPSHOT。 事务在第一次访问数据时启动。  
  
 在 SNAPSHOT 隔离级别下运行的事务可以查看由该事务所做的更改。 例如，如果事务对表执行 UPDATE，然后对同一个表发出 SELECT 语句，则修改后的数据将包含在结果集中。  
  
> [!NOTE]  
>  在快照隔离模式下，事务中任何语句读取的 FILESTREAM 数据都将是在事务开始（而非语句开始）时便存在的数据的事务性一致版本。  
  
 SERIALIZABLE  
 请指定下列内容：  
  
-   语句不能读取已由其他事务修改但尚未提交的数据。  
  
-   任何其他事务都不能在当前事务完成之前修改由当前事务读取的数据。  
  
-   在当前事务完成之前，其他事务不能使用当前事务中任何语句读取的键值插入新行。  
  
 范围锁处于与事务中执行的每个语句的搜索条件相匹配的键值范围之内。 这样可以阻止其他事务更新或插入任何行，从而限定当前事务所执行的任何语句。 这意味着如果再次执行事务中的任何语句，则这些语句便会读取同一组行。 在事务完成之前将一直保持范围锁。 这是限制最多的隔离级别，因为它锁定了键的整个范围，并在事务完成之前一直保持范围锁。 因为并发级别较低，所以应只在必要时才使用该选项。 该选项的作用与在事务内所有 SELECT 语句中的所有表上设置 HOLDLOCK 相同。  
  
## <a name="remarks"></a>备注  
 一次只能设置一个隔离级别选项，而且设置的选项将一直对那个连接始终有效，直到显式更改该选项为止。 事务中执行的所有读取操作都会在指定的隔离级别的规则下运行，除非语句的 FROM 子句中的表提示为表指定了其他锁定行为或版本控制行为。  
  
 事务隔离级别定义了可为读取操作获取的锁类型。 针对 READ COMMITTED 或 REPEATABLE READ 获取的共享锁通常为行锁，尽管当读取引用了页或表中大量的行时，行锁可以升级为页锁或表锁。 如果某行在被读取之后由事务进行了修改，则该事务会获取一个用于保护该行的排他锁，并且该排他锁在事务完成之前将一直保持。 例如，如果 REPEATABLE READ 事务具有用于某行的共享锁，并且该事务随后修改了该行，则共享行锁便会转换为排他行锁。  
  
 在事务进行期间，可以随时将事务从一个隔离级别切换到另一个隔离级别，但有一种情况例外。 即在从任一隔离级别更改到 SNAPSHOT 隔离时，不能进行上述操作。 否则会导致事务失败并回滚。 但是，可以将在 SNAPSHOT 隔离中启动的事务更改为任何其他隔离级别。  
  
 将事务从一个隔离级别更改为另一个隔离级别之后，便会根据新级别的规则对更改后读取的资源执行保护。 在更改前读取的资源将继续按照以前级别的规则受到保护。 例如，如果某个事务从 READ COMMITTED 更改为 SERIALIZABLE，则在该事务结束前，更改后所获取的共享锁将一直处于保留状态。  
  
 如果在存储过程或触发器中发出 SET TRANSACTION ISOLATION LEVEL，则当对象返回控制时，隔离级别会重设为在调用对象时有效的级别。 例如，如果在批处理中设置 REPEATABLE READ，并且该批处理调用一个将隔离级别设置为 SERIALIZABLE 的存储过程，则当该存储过程将控制返回给该批处理时，隔离级别就会恢复为 REPEATABLE READ。  
  
> [!NOTE]  
>  用户定义的函数和公共语言运行时 (CLR) 用户定义的类型无法执行 SET TRANSACTION ISOLATION LEVEL。 但是，可使用表提示来重写隔离级别。 有关详细信息，请参阅[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。  
  
 当您使用 sp_bindsession 绑定两个会话时，每个会话都会保留它自身的隔离级别设置。 使用 SET TRANSACTION ISOLATION LEVEL 更改某个会话的隔离级别设置时，不会影响与该会话绑定的其他任何会话的设置。  
  
 SET TRANSACTION ISOLATION LEVEL 会在执行或运行时生效，而不是在分析时生效。  
  
 针对堆的优化大容量负载操作阻塞了运行在以下隔离级别下面的查询：  
  
-   SNAPSHOT  
  
-   READ UNCOMMITTED  
  
-   使用行版本控制的 READ COMMITTED  
  
 相反，运行在这些隔离级别下面的查询阻塞了针对堆的优化大容量负载操作。 有关大容量加载操作的详细信息，请参阅[批量导入和导出数据 (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)。  
  
 已启用 FILESTREAM 的数据库支持下列事务隔离级别。  
  
|隔离级别|Transact SQL 访问|文件系统访问|  
|---------------------|-------------------------|------------------------|  
|未提交读|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|不支持|  
|已提交读|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
|可重复读|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|不支持|  
|可序列化|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|不支持|  
|读提交的快照|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
|快照|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
  
## <a name="examples"></a>示例  
 以下示例为会话设置了 `TRANSACTION ISOLATION LEVEL`。 对于每个后续 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将所有共享锁一直保持到事务结束为止。  
  
```sql
USE AdventureWorks2012;  
GO  
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;  
GO  
BEGIN TRANSACTION;  
GO  
SELECT *   
    FROM HumanResources.EmployeePayHistory;  
GO  
SELECT *   
    FROM HumanResources.Department;  
GO  
COMMIT TRANSACTION;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [DBCC USEROPTIONS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-useroptions-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)  
  
