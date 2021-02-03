---
description: 提示 (Transact-SQL) - 表
title: 表提示 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TABLE_HINT_TSQL
- Table Hint
dev_langs:
- TSQL
helpviewer_keywords:
- SERIALIZABLE table hint
- UPDLOCK table hint
- HOLDLOCK table hint
- TABLOCKX table hint
- READUNCOMMITTED table hint
- hints [SQL Server], tables
- READCOMMITTEDLOCK table hint
- FORCESCAN table hint
- ROWLOCK table hint
- XLOCK table hint
- NOLOCK table hint
- READPAST table hint
- NOWAIT table hint
- FORCESEEK table hint
- table hints [SQL Server]
- TABLOCK table hint
- REPEATABLEREAD table hint
- READ COMMITTED table hint
- NOEXPAND table hint
- PAGLOCK table hint
ms.assetid: 8bf1316f-c0ef-49d0-90a7-3946bc8e7a89
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 9f2014323d963e9affdab08e181b7454e6177d07
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207703"
---
# <a name="hints-transact-sql---table"></a>提示 (Transact-SQL) - 表
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  通过指定锁定方法、一个或多个索引、查询处理操作（如表扫描或索引查找）或其他选项，表提示在数据操作语言 (DML) 语句执行期间覆盖查询优化器的默认行为。 表提示在 DML 语句的 FROM 子句中指定，仅影响在该子句中引用的表或视图。  
  
> [!CAUTION]  
>  由于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器通常会为查询选择最佳执行计划，因此我们建议仅在最后迫不得已的情况下才可由资深的开发人员和数据库管理员使用提示。  
  
 **适用于：**  
  
 [DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
 [INSERT](../../t-sql/statements/insert-transact-sql.md)  
  
 [SELECT](../../t-sql/queries/select-transact-sql.md)  
  
 [UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
 [MERGE](../../t-sql/statements/merge-transact-sql.md)  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
WITH  ( <table_hint> [ [, ]...n ] )  
  
<table_hint> ::=   
{ NOEXPAND [ , INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> ) ]  
  | INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> )
  | FORCESEEK [ ( <index_value> ( <index_column_name> [,... ] ) ) ] 
  | FORCESCAN  
  | FORCESEEK  
  | HOLDLOCK   
  | NOLOCK   
  | NOWAIT  
  | PAGLOCK   
  | READCOMMITTED   
  | READCOMMITTEDLOCK   
  | READPAST   
  | READUNCOMMITTED   
  | REPEATABLEREAD   
  | ROWLOCK   
  | SERIALIZABLE   
  | SNAPSHOT   
  | SPATIAL_WINDOW_MAX_CELLS = <integer_value>  
  | TABLOCK   
  | TABLOCKX   
  | UPDLOCK   
  | XLOCK   
}   
  
<table_hint_limited> ::=  
{  
    KEEPIDENTITY   
  | KEEPDEFAULTS   
  | HOLDLOCK   
  | IGNORE_CONSTRAINTS   
  | IGNORE_TRIGGERS   
  | NOLOCK   
  | NOWAIT  
  | PAGLOCK   
  | READCOMMITTED   
  | READCOMMITTEDLOCK   
  | READPAST   
  | REPEATABLEREAD   
  | ROWLOCK   
  | SERIALIZABLE   
  | SNAPSHOT   
  | TABLOCK   
  | TABLOCKX   
  | UPDLOCK   
  | XLOCK   
}   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
WITH **(** \<table_hint> **)** [ [ **,** ]...*n* ]  
存在一些例外情况：只有在使用 WITH 关键字指定表提示时，才支持在 FROM 子句中使用这些提示。 指定表提示时必须使用括号。  
  
> [!IMPORTANT]  
> 不推荐省略 WITH 关键字：[!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
无论是否允许使用 `WITH` 关键字，都可以使用以下表提示：`NOLOCK`、`READUNCOMMITTED`、`UPDLOCK`、`REPEATABLEREAD`、`SERIALIZABLE`、`READCOMMITTED`、`TABLOCK`、`TABLOCKX`、`PAGLOCK`、`ROWLOCK`、`NOWAIT`、`READPAST`、`XLOCK`、`SNAPSHOT` 和 `NOEXPAND`。 如果指定的表提示不含 WITH 关键字，则必须单独指定该提示。 例如：  
  
```sql  
FROM t (TABLOCK)  
```  
  
如果指定的提示含其他选项，则指定的提示必须含 WITH 关键字：  
  
```sql  
FROM t WITH (TABLOCK, INDEX(myindex))  
```  
  
建议在表提示之间使用逗号。  
  
> [!IMPORTANT]  
> 用空格而不用逗号分隔提示是一项已弃用的功能：[!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
NOEXPAND  
指定查询优化器处理查询时，不扩展任何索引视图来访问基础表。 查询优化器将视图当成包含聚集索引的表处理。 NOEXPAND 仅适用于索引视图。 有关详细信息，请参阅[使用 NOEXPAND](#using-noexpand)。  
  
INDEX  (<index\_value> [,... n ] ) | INDEX =  ( <index\_value>)  
INDEX() 语法指定供查询优化器在处理该语句时使用的一个或多个索引的名称或 ID。 另一供选择的 `INDEX =` 语法指定单个索引值。 只能为每个表指定一个索引提示。  
  
如果存在聚集索引，则 `INDEX(0)` 强制执行聚集索引扫描，`INDEX(1)` 强制执行聚集索引扫描或查找。 如果不存在聚集索引，则 `INDEX(0)` 强制执行表扫描，`INDEX(1)` 被解释为错误。  
  
 如果在单个提示列表中使用了多个索引，则会忽略重复项，其余列出的索引将用于检索表中的行。 索引提示中的索引顺序很重要。 多索引提示还强制执行索引 AND 运算，查询优化器将对所访问的每个索引应用尽可能多的条件。 如果提示索引的集合并未包含查询引用的所有列，则会在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]检索所有索引列后执行提取操作以检索其余列。  
  
> [!NOTE]  
> 如果将引用多个索引的索引提示用于星型联接中的事实数据表，则优化器将忽略索引提示，并返回一个警告消息。 另外，不允许对包含指定索引提示的表执行索引 OR 操作。  
  
 表提示中的最大索引数为 250 个非聚集索引。  
  
KEEPIDENTITY  
只适用于 INSERT 语句（当 BULK 选项与 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) 一起使用时）。  
  
 指定导入数据文件中的标识值用于标识列。 如果不指定 KEEPIDENTITY，则将验证但不导入此列的标识值。查询优化器将根据创建表时指定的种子值和增量值自动分配唯一值。  
  
> [!IMPORTANT]  
> 如果数据文件不包含表或视图中的标识列的值，并且标识列不是表中的最后一列，则必须跳过标识列。 有关详细信息，请参阅[使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)。 如果成功跳过了一个标识列，则查询优化器自动将标识列的唯一值分配到导入的表行中。  
  
有关 `INSERT ... SELECT * FROM OPENROWSET(BULK...)` 语句中使用此提示的示例，请参阅[批量导入数据时保留标识值 (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)。  
  
有关检查表的标识值的信息，请参阅 [DBCC CHECKIDENT (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)。  
  
KEEPDEFAULTS  
只适用于 INSERT 语句（当 BULK 选项与 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) 一起使用时）。  
  
指定数据记录在某一表列缺少值时插入此列的默认值（如果有），而不是插入 NULL。  
  
有关在 INSERT ...SELECT * FROM OPENROWSET(BULK...) 语句中使用此提示的示例，请参阅[在批量导入期间保留 Null 或使用默认值 (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)。  
  
FORCESEEK [ (<index\_value>(<index\_column\_name> [ ,... n ] )) ]  
指定查询优化器仅使用索引查找操作作为表或视图中的数据的访问途径。 

> [!NOTE]
> 从 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] SP1 开始，还可以指定索引参数。 在这种情况下，查询优化器仅考虑通过指定的索引（至少使用指定的索引列）执行索引查找操作。  
  
 *index_value*  
 是索引名称或索引 ID 值。 不能指定索引 ID 0（堆）。 若要返回索引名称或 ID，请查询 **sys.indexes** 目录视图。  
  
 *index_column_name*  
 是要包含在查找操作中的索引列的名称。 指定带索引参数的 FORCESEEK 类似于将 FORCESEEK 与 INDEX 提示一起使用。 但是，您可以通过指定要查找的索引和查找操作中要考虑的索引列，更好地控制查询优化器使用的访问路径。 该优化器可以根据需要考虑其他列。 例如，如果指定非聚集索引，优化器除了使用指定的列之外，还可以选择使用聚集索引键列。  
  
可以通过以下方式指定 FORCESEEK 提示。  
  
|语法|示例|说明|  
|------------|-------------|-----------------|  
|没有索引或 INDEX 提示|`FROM dbo.MyTable WITH (FORCESEEK)`|查询优化器仅考虑执行索引查找操作以通过任意相关索引访问表或视图。|  
|与 INDEX 提示组合使用|`FROM dbo.MyTable WITH (FORCESEEK, INDEX (MyIndex))`|查询优化器仅考虑执行索引查找操作以通过指定的索引访问表或视图。|  
|通过指定索引和索引列进行参数化|`FROM dbo.MyTable WITH (FORCESEEK (MyIndex (col1, col2, col3)))`|查询优化器仅考虑执行索引查找操作，以通过指定的索引（至少使用指定的索引列）访问表或视图。|  
  
使用 FORCESEEK 提示（具有或不带索引参数）时，考虑以下准则：  
-   该提示可以指定为表提示或查询提示。 有关查询提示的详细信息，请参阅[查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)。  
-   若要将 FORCESEEK 应用到索引视图，还必须指定 NOEXPAND 提示。  
-   对每个表或视图最多应用该提示一次。  
-   不能为远程数据源指定该提示。 带索引提示指定 FORCESEEK 时，将返回错误 7377；不带索引提示使用 FORCESEEK 时，将返回错误 8180。  
-   如果 FORCESEEK 导致找不到计划，将返回错误 8622。  
  
使用索引参数指定 FORCESEEK 时，遵循以下准则和限制：  
-   不能为作为 INSERT、UPDATE 或 DELETE 语句的目标的表指定该提示。  
-   该提示不能与 INDEX 提示或另一个 FORCESEEK 提示一起指定。  
-   至少必须指定一个列且该列为第一个键列。  
-   可以指定其他索引列，但是不能跳过键列。 例如，如果指定的索引包含键列 `a`、`b` 和 `c`，则有效的语法应包含 `FORCESEEK (MyIndex (a))` 和 `FORCESEEK (MyIndex (a, b)`。 无效的语法应包含 `FORCESEEK (MyIndex (c))` 和 `FORCESEEK (MyIndex (a, c)`。  
-   在提示中指定的列名顺序必须与引用的索引中列的顺序匹配。  
-   不能指定不在索引键定义中的列。 例如，在非聚集索引中，只能指定定义的索引键列。 不能指定自动包含在索引中的聚集键列，但是优化器可以使用这些列。  
-   xVelocity 内存优化的列存储索引不能作为索引参数指定。 返回错误 366。  
-   修改索引定义（例如通过添加或删除列）可能需要修改引用该索引的查询。  
-   该提示阻止优化器考虑表的任何空间或 XML 索引。  
-   该提示不能与 FORCESCAN 提示一起指定。  
-   对于分区的索引，不能在 FORCESEEK 提示中指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 隐式添加的分区列。  
  
> [!CAUTION]  
> 指定带参数的 FORCESEEK 限制优化器可以考虑的计划数大于指定不带参数的 FORCESEEK 时的计划数。 这可能导致更多情况下发生 `Plan cannot be generated` 错误。 在未来的版本中，对查询优化器进行内部修改后可允许考虑更多计划。  
  
FORCESCAN 适用范围：[!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] SP1 及更高版本。
指定查询优化器仅使用索引扫描操作作为引用的表或视图的访问途经。 对于优化器低估受影响的行数并选择一个查找操作而非扫描操作的查询，FORCESCAN 提示很有用。 出现这样的情况时，授予该操作的内存量太小，查询性能将受影响。  
  
指定 FORCESCAN 时有无 INDEX 提示均可。 与索引提示组合使用 (`INDEX = index_name, FORCESCAN`) 时，查询优化器在访问引用的表时仅考虑通过指定的索引扫描访问路径。 可以带索引提示 INDEX(0) 指定 FORCESCAN，以强制对基表执行表扫描操作。  
  
对于分区的表和索引，在通过查询谓词评估消除分区后应用 FORCESCAN。 这意味着扫描仅适用于剩余分区而非整个表。  
  
FORCESCAN 提示存在以下限制：  
-   不能为作为 INSERT、UPDATE 或 DELETE 语句的目标的表指定该提示。  
-   该提示不能与一个以上的索引提示一起使用。  
-   该提示阻止查询优化器考虑表的任何空间或 XML 索引。  
-   不能为远程数据源指定该提示。  
-   该提示不能与 FORCESEEK 提示一起指定。  
  
HOLDLOCK  
等同于 SERIALIZABLE。 有关详细信息，请参阅本主题后面的 SERIALIZABLE。 HOLDLOCK 仅应用于那些为其指定了 HOLDLOCK 的表或视图，并且仅在使用了 HOLDLOCK 的语句定义的事务的持续时间内应用。 HOLDLOCK 不能被用于包含 FOR BROWSE 选项的 SELECT 语句。  
  
IGNORE_CONSTRAINTS  
只适用于 INSERT 语句（当 BULK 选项与 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) 一起使用时）。  
  
指定大容量导入操作将忽略对表的任何约束。 默认情况下，INSERT 会检查[唯一约束和 CHECK 约束](../../relational-databases/tables/unique-constraints-and-check-constraints.md)以及[主键和外键约束](../../relational-databases/tables/primary-and-foreign-key-constraints.md)。 当为大容量导入操作指定 IGNORE_CONSTRAINTS 时，INSERT 必须忽略对目标表的这些约束。 注意，您无法禁用 UNIQUE、PRIMARY KEY 或 NOT NULL 约束。  
  
如果输入数据包含违反约束的行，则您可能希望禁用 CHECK 和 FOREIGN KEY 约束。 通过禁用 CHECK 和 FOREIGN KEY 约束，可以导入数据，然后使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句清除该数据。  
  
不过，如果忽略了 CHECK 和 FOREIGN KEY 约束，在执行操作后，表的每个已忽略约束会在 [sys.check_constraints](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md) 或 [sys.foreign_keys](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md) 目录视图中标记为 **is_not_trusted**。 在某一时刻，应该检查全表约束。 如果在大容量导入操作之前表不为空，则重新验证约束的开销可能超过对增量数据应用 CHECK 和 FOREIGN KEY 约束的开销。  
  
IGNORE_TRIGGERS  
只适用于 INSERT 语句（当 BULK 选项与 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) 一起使用时）。  
  
指定大容量导入操作将忽略为表定义的所有触发器。 默认情况下，INSERT 将应用触发器。  
  
仅当应用程序不依赖任何触发器，并且必须最大程度地提高性能时，才使用 IGNORE_TRIGGERS。  
  
NOLOCK  
等同于 READUNCOMMITTED。 有关详细信息，请参阅本主题后面的 READUNCOMMITTED。  
  
> [!NOTE]  
> 对于 UPDATE 或 DELETE 语句：[!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
NOWAIT  
指示[!INCLUDE[ssDE](../../includes/ssde-md.md)]在遇到表的锁时，立即返回一条消息。 NOWAIT 等同于为特定表指定 `SET LOCK_TIMEOUT 0`。 当 TABLOCK 提示也包含在内时，NOWAIT 提示不起作用。 若要在使用 TABLOCK 提示时终止查询而不等待，请改为在查询前加上 `SETLOCK_TIMEOUT 0;`。  
  
PAGLOCK  
在通常行或键采用单个锁的地方，或者通常采用单个表锁的地方，请采用页锁。 默认情况下，请使用与操作相对应的锁模式。 在从 SNAPSHOT 隔离级别操作的事务中指定时，除非将 PAGLOCK 与需要锁的其他表提示（例如，UPDLOCK 和 HOLDLOCK）组合，否则不会取得页锁。  
  
READCOMMITTED  
指定读操作使用锁定或行版本控制来遵循有关 READ COMMITTED 隔离级别的规则。 如果 READ_COMMITTED_SNAPSHOT 数据库选项为 OFF，[!INCLUDE[ssDE](../../includes/ssde-md.md)]会在读取数据时获取共享锁，在读操作完成后释放这些锁。 如果数据库选项 READ_COMMITTED_SNAPSHOT 为 ON，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]不获取锁，并使用行版本控制。 有关隔离级别的详细信息，请参阅 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)。  
  
> [!NOTE]  
> 对于 UPDATE 或 DELETE 语句：[!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
READCOMMITTEDLOCK  
指定读操作使用锁定来遵循有关 READ COMMITTED 隔离级别的规则。 无论 READ_COMMITTED_SNAPSHOT 数据库选项的设置如何，[!INCLUDE[ssDE](../../includes/ssde-md.md)]都将在读取数据时获取共享锁，在读操作完成后释放这些锁。 有关隔离级别的详细信息，请参阅 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)。 不能对 INSERT 语句的目标表指定此提示；将返回错误 4140。  
  
READPAST  
指定[!INCLUDE[ssDE](../../includes/ssde-md.md)]不读取由其他事务锁定的行。 如果指定 READPAST，则跳过行级锁，但不跳过页级锁。 也就是说，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将跳过这些行，而不是阻塞当前事务直到锁被释放。 例如，假设表 `T1` 包含一个单精度整数列，其值为 1、2、3、4 和 5。 如果事务 A 将值 3 更改为 8，但尚未提交，则 SELECT * FROM T1 (READPAST) 将生成值 1、2、4 和 5。 在实现使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表的工作队列时，READPAST 主要用于减少锁定争用。 使用 READPAST 的队列读取器会跳过被其他事务锁定的队列项，跳至下一个可用的队列项，而不是等待其他事务释放锁。  
  
可为 UPDATE 或 DELETE 语句中以及 FROM 子句中引用的任何表指定 READPAST。 如果 READPAST 是在 UPDATE 语句中指定的，则仅当读取数据以标识要更新的记录时才应用 READPAST，而不考虑语句中指定 READPAST 的位置。 不能为 INSERT 语句的 INTO 子句中的表指定 READPAST。 读取外键或索引视图或者修改辅助索引时，使用 READPAST 的更新或删除操作可能发生阻塞。  
  
仅可在运行于 READ COMMITTED 或 REPEATABLE READ 隔离级别的事务中指定 READPAST。 在从 SNAPSHOT 隔离级别操作的事务中指定时，READPAST 必须与需要锁的其他表提示（例如，UPDLOCK 和 HOLDLOCK）组合。  
  
当 READ_COMMITTED_SNAPSHOT 数据库选项设置为 ON 并且满足以下条件之一时，无法指定 READPAST 表提示：  
-   会话的事务隔离级别为 READ COMMITTED。  
-   查询中也指定了 READCOMMITTED 表提示。  
  
若要在上述情况下指定 READPAST 提示，请删除 READCOMMITTED 表提示（如果存在），然后在查询中包括 READCOMMITTEDLOCK 表提示。  
  
READUNCOMMITTED  
指定允许脏读。 不发布共享锁来阻止其他事务修改当前事务读取的数据，其他事务设置的排他锁不会阻碍当前事务读取锁定数据。 允许脏读可能产生较多的并发操作，但其代价是读取以后会被其他事务回滚的数据修改。 这可能会使您的事务出错，向用户显示从未提交过的数据，或者导致用户两次看到记录（或根本看不到记录）。  
  
READUNCOMMITTED 和 NOLOCK 提示仅适用于数据锁。 所有查询（包括那些带有 READUNCOMMITTED 和 NOLOCK 提示的查询）都会在编译和执行过程中获取 Sch-S（架构稳定性）锁。 因此，当并发事务持有表的 Sch-M（架构修改）锁时，将阻塞查询。 例如，数据定义语言 (DDL) 操作在修改表的架构信息之前获取 Sch-M 锁。 所有并发查询（包括那些使用 READUNCOMMITTED 或 NOLOCK 提示运行的查询）都会在尝试获取 Sch-S 锁时被阻塞。 相反，持有 Sch-S 锁的查询将阻塞尝试获取 Sch-M 锁的并发事务。  
  
不能为通过插入、更新或删除操作修改过的表指定 READUNCOMMITTED 和 NOLOCK。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器忽略 FROM 子句中应用于 UPDATE 或 DELETE 语句的目标表的 READUNCOMMITTED 和 NOLOCK 提示。  
  
> [!NOTE]  
> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，将不再支持在 FROM 子句中使用应用于 UPDATE 或 DELETE 语句目标表的 READUNCOMMITTED 和 NOLOCK 提示。 请避免在新的开发工作上下文中使用这些提示，并计划修改当前使用它们的应用程序。  
  
可以通过使用以下任意一种方法，在保护事务避免对未提交的数据修改进行脏读的同时最大程度地减少锁争用：  
-   READ COMMITTED 隔离级别，其中 READ_COMMITTED_SNAPSHOT 数据库选项设置为 ON。  
-   SNAPSHOT 隔离级别。  
  
有关隔离级别的详细信息，请参阅 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)。  
  
> [!NOTE]  
> 如果在指定 READUNCOMMITTED 时收到[错误消息 601](../../relational-databases/errors-events/database-engine-events-and-errors.md#errors--2-to-999)，则按解决死锁错误（[错误消息 1205](../../relational-databases/errors-events/mssqlserver-1205-database-engine-error.md)）的方法解决该错误，然后重试语句。  
  
REPEATABLEREAD  
指定事务在 REPEATABLE READ 隔离级别运行时，使用相同的锁定语义执行一次扫描。 有关隔离级别的详细信息，请参阅 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)。  
  
ROWLOCK  
指定通常采用页锁或表锁时，采用行锁。 在从 SNAPSHOT 隔离级别操作的事务中指定时，除非将 ROWLOCK 与需要锁的其他表提示（例如，UPDLOCK 和 HOLDLOCK）组合，否则不会取得行锁。 ROWLOCK 不能用于具有聚集列存储索引的表。 以下示例将向应用程序返回[错误 651](../../relational-databases/errors-events/database-engine-events-and-errors.md#errors--2-to-999)。  

```sql 
UPDATE [dbo].[FactResellerSalesXL_CCI] WITH (ROWLOCK)
SET UnitPrice = 50
WHERE ProductKey = 150;
```  
  
SERIALIZABLE  
等同于 HOLDLOCK。 保持共享锁直到事务完成，使共享锁更具有限制性；而不是无论事务是否完成，都在不再需要所需表或数据页时立即释放共享锁。 执行扫描时所用的语义与在 SERIALIZABLE 隔离级别运行的事务的语义相同。 有关隔离级别的详细信息，请参阅 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)。  
  
SNAPSHOT  
**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。 
  
内存优化表在 SNAPSHOT 隔离下访问。 SNAPSHOT 只能用于内存优化表（不能用于基于磁盘的表），如以下示例所示。 有关详细信息，请参阅[内存优化表简介](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md)。  
  
```sql 
SELECT * 
FROM dbo.Customers AS c WITH (SNAPSHOT)   
LEFT JOIN dbo.[Order History] AS oh   
    ON c.customer_id=oh.customer_id;  
```  
  
SPATIAL_WINDOW_MAX_CELLS = <integer\_value>  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
指定在分割 geometry 或 geography 对象时使用的最大单元格数。 <integer\_value> 是介于 1 和 8192 之间的值。  
  
通过使用此选项，可以在主要和辅助筛选器执行时间之间权衡性能以微调查询执行时间。 较大的数字将减少辅助筛选器执行时间，但会增加主要筛选器执行时间，而较小的数字恰相反。 对于较密的空间数据，较大的数字通过为主要筛选器提供更好的近似值并减少辅助筛选器执行时间，从而缩短了执行时间。 对于较稀疏的数据，较小的数字将减少主要筛选器执行时间。  
  
 此选项适用于手动和自动网格分割。  
  
TABLOCK  
指定在表级别应用获取的锁。 获取的锁类型取决于正在执行的语句。 例如，SELECT 语句可能获取一个共享锁。 通过指定 TABLOCK，将该共享锁应用到整个表而非在行或页级别应用。 如果同时指定了 HOLDLOCK，则会一直持有表锁，直至事务结束。  
  
在使用 `INSERT INTO <target_table> SELECT <columns> FROM <source_table>` 语句将数据导入某个堆时，可通过为目标表指定 TABLOCK 提示，实现语句的最小日志记录和优化锁定。 此外，数据库的恢复模式必须设置为简单或大容量日志模式。 TABLOCK 提示还允许对堆或聚集列存储索引进行并行插入。 有关详细信息，请参阅 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)。  
  
在与 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) BULK 行集提供程序一起使用以将数据导入表时，TABLOCK 允许多个客户端使用优化日志记录和锁定，以并发方式将数据加载到目标表中。 有关详细信息，请参阅[在批量导入中按最小方式记录日志的前提条件](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md)。  
  
TABLOCKX  
指定对表采用排他锁。  
  
UPDLOCK  
指定采用更新锁并保持到事务完成。 UPDLOCK 仅对行级别或页级别的读操作采用更新锁。 如果将 UPDLOCK 与 TABLOCK 组合使用或出于一些其他原因采用表级锁，将采用排他 (X) 锁。  
  
指定 UPDLOCK 时，忽略 READCOMMITTED 和 READCOMMITTEDLOCK 隔离级别提示。 例如，如果将会话的隔离级别设置为 SERIALIZABLE 且查询指定 (UPDLOCK, READCOMMITTED)，则忽略 READCOMMITTED 提示且使用 SERIALIZABLE 隔离级别运行事务。  
  
XLOCK  
指定采用排他锁并保持到事务完成。 如果同时指定了 ROWLOCK, PAGLOCK 或 TABLOCK，则排他锁将应用于相应的粒度级别。  
  
## <a name="remarks"></a>备注  
如果查询计划不访问表，则将忽略表提示。 这可能是由于优化器选择了完全不访问该表，也可能是因为改成了访问索引视图。 在后一种情况中，使用 OPTION (EXPAND VIEWS) 查询提示可阻止访问索引视图。  
  
所有锁提示将传播到查询计划访问的所有表和视图，其中包括在视图中引用的表和视图。 另外，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还将执行对应的锁一致性检查。  
  
获取行级别锁的锁提示 ROWLOCK、UPDLOCK 和 XLOCK 可能对索引键而不是实际的数据行采用锁。 例如，如果表具有非聚集索引，而且由涵盖索引处理使用锁提示的 SELECT 语句，则获得的锁针对的是涵盖索引中的索引键，而不是基表中的数据行。  
  
如果表包含计算列，而该计算列是由访问其他表中的列的表达式或函数计算的，则不在这些表中使用表提示，并且不会传播这些提示。 例如，在查询的表中指定 NOLOCK 表提示。 此表包含的计算列是由访问另一表中的列的表达式和函数组合计算的。 表达式和函数引用的表在被访问时将不使用 NOLOCK 表提示。  
  
对于 FROM 子句中的每个表，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不允许存在多个来自以下各个组的表提示：  
-   粒度提示：PAGLOCK、NOLOCK、READCOMMITTEDLOCK、ROWLOCK、TABLOCK 或 TABLOCKX。  
-   隔离级别提示：HOLDLOCK、NOLOCK、READCOMMITTED、REPEATABLEREAD、SERIALIZABLE。  
  
## <a name="filtered-index-hints"></a>筛选索引提示  
 筛选索引可用作表提示，但如果未涵盖查询选择的所有行，则会导致查询优化器产生错误 8622。 下面是一个无效筛选索引提示的示例。 该示例创建了筛选索引 `FIBillOfMaterialsWithComponentID`，然后将其用作 SELECT 语句的索引提示。 筛选索引谓词包含 ComponentID 为 533、324 和 753 的数据行。 查询谓词也包含 ComponentID 为 533、324 和 753 的数据行，但它扩展了结果集，使之包含 ComponentID 为 855 和 924 的数据行，而筛选索引中则不包含这两行。 因此，查询优化器无法使用此筛选索引提示，并产生错误 8622。 有关详细信息，请参阅 [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md)。  
  
```sql  
IF EXISTS (SELECT name FROM sys.indexes  
    WHERE name = N'FIBillOfMaterialsWithComponentID'   
    AND object_id = OBJECT_ID(N'Production.BillOfMaterials'))  
DROP INDEX FIBillOfMaterialsWithComponentID  
    ON Production.BillOfMaterials;  
GO  
CREATE NONCLUSTERED INDEX "FIBillOfMaterialsWithComponentID"  
    ON Production.BillOfMaterials (ComponentID, StartDate, EndDate)  
    WHERE ComponentID IN (533, 324, 753);  
GO  
SELECT StartDate, ComponentID FROM Production.BillOfMaterials  
    WITH( INDEX (FIBillOfMaterialsWithComponentID) )  
    WHERE ComponentID in (533, 324, 753, 855, 924);  
GO  
```  
  
如果 SET 选项不包含筛选索引所需的值，查询优化器将不考虑使用索引提示。 有关详细信息，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  
  
## <a name="using-noexpand"></a>使用 NOEXPAND  
NOEXPAND 仅适用于 *索引视图*。 索引视图是包含为其创建的唯一聚集索引的视图。 如果查询包含对同时存在于索引视图和基表中的列的引用，而且查询优化器确定执行查询的最佳方法是使用索引视图，则查询优化器将对视图使用索引。 此功能称为 *索引视图匹配*。 在 [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] SP1 之前，仅在特定版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中支持查询优化器自动使用索引视图。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)、[SQL Server 2017 各个版本支持的功能](../../SQL-server/editions-and-components-of-SQL-server-2017.md)和 [SQL Server 2019 各个版本支持的功能](../../sql-server/editions-and-components-of-sql-server-version-15.md)。  
  
但是，为了使查询优化器考虑使用索引视图进行匹配，或者使用通过 NOEXPAND 提示引用的索引视图，则必须将以下 SET 选项设置为 ON。  

> [!NOTE]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 支持在不指定 NOEXPAND 提示的情况下自动使用索引视图。

- ANSI_NULLS
- ANSI_PADDING
- ANSI_WARNINGS
- ARITHABORT<sup>1</sup>
- CONCAT_NULL_YIELDS_NULL
- QUOTED_IDENTIFIER

<sup>1</sup> 如果 ANSI_WARNINGS 设置为 ON，则 ARITHABORT 将隐式设置为 ON。 因此，不必手动调整此设置。  

另外，必须将 NUMERIC_ROUNDABORT 选项设置为 OFF。  
  
 若要强制查询优化器对索引视图使用索引，请指定 NOEXPAND 选项。 仅当查询中也命名了此视图时才能使用此提示。 如果某个查询没有在 FROM 子句中直接命名特定索引视图，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不提供用于在此查询中强制使用此视图的提示；但是，即使查询中未直接引用索引视图，查询优化器仍会考虑使用索引视图。 使用 NOEXPAND 表提示时，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 仅自动创建索引视图的统计信息。 忽略此提示可能会导致出现执行计划警告：缺少无法通过手动创建统计信息解决的统计信息。 查询优化期间，[!INCLUDE[ssde_md](../../includes/ssde_md.md)] 将使用查询直接引用视图时自动或手动创建的视图统计信息，并使用 NOEXPAND 提示。    
  
## <a name="using-a-table-hint-as-a-query-hint"></a>将表提示用作查询提示  
 也可以使用 OPTION (TABLE HINT) 子句将 *表提示* 指定为查询提示。 我们建议仅在[计划指南](../../relational-databases/performance/plan-guides.md)的上下文中将表提示用作查询提示。 对于即席查询，请将这些提示仅指定为表提示。 有关详细信息，请参阅[查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)。  
  
## <a name="permissions"></a>权限  
 KEEPIDENTITY、IGNORE_CONSTRAINTS 和 IGNORE_TRIGGERS 提示需要具有对表的 `ALTER` 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-the-tablock-hint-to-specify-a-locking-method"></a>A. 使用 TABLOCK 提示指定锁定方法  
 下面的示例指定对 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的 `Production.Product` 表采用共享锁，并保持到 UPDATE 语句结束。  
  
```sql  
UPDATE Production.Product  
WITH (TABLOCK)  
SET ListPrice = ListPrice * 1.10  
WHERE ProductNumber LIKE 'BK-%';  
GO  
```  
  
### <a name="b-using-the-forceseek-hint-to-specify-an-index-seek-operation"></a>B. 使用 FORCESEEK 提示指定索引查找操作  
 以下示例使用未指定索引的 FORCESEEK 提示强制查询优化器对 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的 `Sales.SalesOrderDetail` 表执行索引查找操作。  
  
```sql
SELECT *  
FROM Sales.SalesOrderHeader AS h  
INNER JOIN Sales.SalesOrderDetail AS d WITH (FORCESEEK)  
    ON h.SalesOrderID = d.SalesOrderID   
WHERE h.TotalDue > 100  
AND (d.OrderQty > 5 OR d.LineTotal < 1000.00);  
GO  
  
```  
  
 以下示例使用指定索引的 FORCESEEK 提示强制查询优化器对指定的索引和索引列执行索引查找操作。  
  
```sql  
SELECT h.SalesOrderID, h.TotalDue, d.OrderQty  
FROM Sales.SalesOrderHeader AS h  
    INNER JOIN Sales.SalesOrderDetail AS d   
    WITH (FORCESEEK (PK_SalesOrderDetail_SalesOrderID_SalesOrderDetailID (SalesOrderID)))   
    ON h.SalesOrderID = d.SalesOrderID   
WHERE h.TotalDue > 100  
AND (d.OrderQty > 5 OR d.LineTotal < 1000.00);   
GO  
  
```  
  
### <a name="c-using-the-forcescan-hint-to-specify-an-index-scan-operation"></a>C. 使用 FORCESCAN 提示指定索引扫描操作  
 以下示例使用 FORCESCAN 提示强制查询优化器对 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的 `Sales.SalesOrderDetail` 表执行扫描操作。  
  
```sql  
SELECT h.SalesOrderID, h.TotalDue, d.OrderQty  
FROM Sales.SalesOrderHeader AS h  
    INNER JOIN Sales.SalesOrderDetail AS d   
    WITH (FORCESCAN)   
    ON h.SalesOrderID = d.SalesOrderID   
WHERE h.TotalDue > 100  
AND (d.OrderQty > 5 OR d.LineTotal < 1000.00);  
```  
  
## <a name="see-also"></a>另请参阅  
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql.md)   
 [查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)  
  
