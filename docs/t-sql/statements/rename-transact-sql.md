---
description: RENAME (Transact-SQL)
title: RENAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2019
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
ms.assetid: 0907cfd9-33a6-4fa6-91da-7d6679fee878
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 70c72d0d9b884431a314290de4c4514c720e85ee
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186474"
---
# <a name="rename-transact-sql"></a>RENAME (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

在 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 中重命名用户创建的表。 在 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中重命名用户创建的表、用户创建的表或数据库中的列。

> [!NOTE]
> 若要重命名 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 中的数据库，请使用 [ALTER DATABASE ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)。 若要重命名 Azure SQL 数据库中的数据库，可使用 [ALTER DATABASE (Azure SQL Database)](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true) 语句。 若要在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中重命名数据库，请使用存储过程 [sp_renamedb](../../relational-databases/system-stored-procedures/sp-renamedb-transact-sql.md)。

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for Azure Synapse Analytics

-- Rename a table.
RENAME OBJECT [::] [ [ database_name . [schema_name ] ] . ] | [schema_name . ] ] table_name TO new_table_name
[;]

```

```syntaxsql
-- Syntax for Analytics Platform System

-- Rename a table
RENAME OBJECT [::] [ [ database_name . [ schema_name ] . ] | [ schema_name . ] ] table_name TO new_table_name
[;]

-- Rename a database
RENAME DATABASE [::] database_name TO new_database_name
[;]

-- Rename a column 
RENAME OBJECT [::] [ [ database_name . [schema_name ] ] . ] | [schema_name . ] ] table_name COLUMN column_name TO new_column_name [;]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>参数

RENAME OBJECT [::] [ [database_name  . [ schema_name ] .  ] | [ schema_name .  ] ]table_name  TO new_table_name  
适用范围：  [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]、[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

更改用户定义的表的名称。 使用一部分、两部分或三部分名称指定要重命名的表。 以一部分名称的形式指定新表 new_table_name  。

RENAME DATABASE [::] [ database_name TO new_database_name
适用于：[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]   

将用户定义的数据库的名称从 database_name 更改为 new_database_name   。 无法将数据库重命名为以下任何 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 保留数据库名称：

- 主
- 模型
- msdb
- tempdb
- pdwtempdb1
- pdwtempdb2
- DWConfiguration
- DWDiagnostics
- DWQueue


RENAME OBJECT [::] [ [database_name  . [ schema_name ] .  ] | [ schema_name .  ] ]table_name COLUMN column_name TO new_column_name
适用对象：[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]    

更改表中列的名称。 

## <a name="permissions"></a>权限

运行此命令需要以下权限：

- 对表的 **ALTER** 权限

## <a name="limitations-and-restrictions"></a>限制和局限

### <a name="cannot-rename-an-external-table-indexes-or-views"></a>无法重命名外部表、索引或视图

无法重命名外部表、索引或视图。 可以删除外部表、索引或视图，然后使用新名称重新创建它，而不是进行重命名。

### <a name="cannot-rename-a-table-in-use"></a>无法重命名正在使用的表

无法重命名正在使用的表或数据库。 重命名表需要在表上使用排他锁。 如果表正在使用中，则可能需要终止使用表的会话。 若要终止会话，可以使用 KILL 命令。 应谨慎使用 KILL，因为终止会话时会回滚任何未提交的工作。 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中的会话以“SID”作为前缀。 调用 KILL 命令时包括“SID”和会话编号。 此示例查看活动或空闲会话的列表，然后终止会话“SID1234”。

### <a name="rename-column-restrictions"></a>重命名列限制

不能重命名用于表的分布的列。 也不能重命名外部表或临时表中的任何列。 

### <a name="views-are-not-updated"></a>视图不会进行更新

重命名数据库时，使用以前数据库名称的所有视图都会变为无效状态。 此行为适用于数据库内部和外部的视图。 例如，如果对 Sales 数据库进行重命名，则包含 `SELECT * FROM Sales.dbo.table1` 的视图会变为无效状态。 若要解决此问题，可以避免在视图中使用三部分名称，或更新视图以引用新数据库名称。

重命名表时，视图不会进行更新以引用新表名。 数据库内部或外部引用以前表名的每个视图都会变为无效状态。 若要解决此问题，可以更新每个视图以引用新表名。

重命名列时，视图不会进行更新，因此不会引用新列名。 在执行 alter view 之前，视图将一直显示旧的列名称。 在某些情况下，视图可能会变为无效，需要删除并重新创建。

## <a name="locking"></a>锁定

重命名表会在 DATABASE 对象上采用共享锁、在 SCHEMA 对象上采用共享锁以及在表上采用排他锁。

## <a name="examples"></a>示例

### <a name="a-rename-a-database"></a>A. 重命名数据库

适用范围  ：仅限 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

此示例将用户定义的数据库 AdWorks 重命名为 AdWorks2。

```sql
-- Rename the user defined database AdWorks
RENAME DATABASE AdWorks to AdWorks2;

```

 重命名表时，与表关联的所有对象和属性都会进行更新以引用新表名。 例如，表定义、索引、约束和权限会进行更新。 视图不会进行更新。

### <a name="b-rename-a-table"></a>B. 重命名表

适用于：[!INCLUDE[ssSDW](../../includes/sssdw-md.md)][!INCLUDE[ssPDW](../../includes/sspdw-md.md)]、 

此示例将 Customer 表重命名为 Customer1。

```sql
-- Rename the customer table
RENAME OBJECT Customer TO Customer1;

RENAME OBJECT mydb.dbo.Customer TO Customer1;
```

重命名表时，与表关联的所有对象和属性都会进行更新以引用新表名。 例如，表定义、索引、约束和权限会进行更新。 视图不会进行更新。

### <a name="c-move-a-table-to-a-different-schema"></a>C. 将表移动到另一个架构

适用于：[!INCLUDE[ssSDW](../../includes/sssdw-md.md)][!INCLUDE[ssPDW](../../includes/sspdw-md.md)]、 

如果要将对象移动到另一个架构，请使用 [ALTER SCHEMA](../../t-sql/statements/alter-schema-transact-sql.md)。 例如，以下语句会将表项从 product 架构移动到 dbo 架构。

```sql
ALTER SCHEMA dbo TRANSFER OBJECT::product.item;
```

### <a name="d-terminate-sessions-before-renaming-a-table"></a>D. 在重命名表之前终止会话

适用于：[!INCLUDE[ssSDW](../../includes/sssdw-md.md)][!INCLUDE[ssPDW](../../includes/sspdw-md.md)]、 

无法重命名正在使用的表。 重命名表需要在表上使用排他锁。 如果表正在使用中，则可能需要终止使用表的会话。 若要终止会话，可以使用 KILL 命令。 应谨慎使用 KILL，因为终止会话时会回滚任何未提交的工作。 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中的会话以“SID”作为前缀。 调用 KILL 命令时需要包括“SID”和会话编号。 此示例查看活动或空闲会话的列表，然后终止会话“SID1234”。

```sql
-- View a list of the current sessions
SELECT session_id, login_name, status
FROM sys.dm_pdw_exec_sessions
WHERE status='Active' OR status='Idle';

-- Terminate a session using the session_id.
KILL 'SID1234';
```

### <a name="e-rename-a-column"></a>E. 重命名列 

适用对象：[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 

此示例将 Customer 表的 FName 列重命名为 FirstName。

```sql
-- Rename the Fname column of the customer table
RENAME OBJECT::Customer COLUMN FName TO FirstName;

RENAME OBJECT mydb.dbo.Customer COLUMN FName TO FirstName;
```
