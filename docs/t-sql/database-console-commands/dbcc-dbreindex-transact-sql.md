---
description: DBCC DBREINDEX (Transact-SQL)
title: DBCC DBREINDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DBCC DBREINDEX
- DBREINDEX_TSQL
- DBREINDEX
- DBCC_DBREINDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- index rebuilding [SQL Server]
- rebuilding indexes
- dynamic index rebuilding [SQL Server]
- DBCC DBREINDEX statement
ms.assetid: 6e929d09-ccb5-4855-a6af-b616022bc8f6
author: pmasl
ms.author: umajay
ms.openlocfilehash: ed6617453805361b5e8c1ddb888495c0cc5d1cb0
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596667"
---
# <a name="dbcc-dbreindex-transact-sql"></a>DBCC DBREINDEX (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
对指定数据库中的表重新生成一个或多个索引。
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]请改用 [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md)。  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到[当前版本](../../sql-server/what-s-new-in-sql-server-2016.md)）
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DBCC DBREINDEX   
(   
    table_name   
    [ , index_name [ , fillfactor ] ]  
)  
    [ WITH NO_INFOMSGS ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *table_name*  
 包含要重新生成的指定索引的表的名称。 表名必须遵循有关[标识符](../../relational-databases/databases/database-identifiers.md)的规则。  
  
 index_name  
 要重新生成的索引名。 索引名称必须符合标识符规则。 如果已指定 index_name，则必须指定 table_name。 如果未指定 index_name 或者该值为“ ”，则重新生成表的所有索引。  
  
 fillfactor  
 在创建或重新生成索引时，每个索引页上用于存储数据的空间的百分比。 创建索引后，fillfactor 将替换填充因子，从而成为该索引以及重新生成的任何其他非聚集索引（因为重新生成了聚集索引）的新默认值。  
 当 fillfactor 为 0 时，DBCC DBREINDEX 将使用上次为索引指定的填充因子值。 该值存储在 sys.indexes 目录视图中。   
 如果已指定 fillfactor，则必须指定 index_name。 如果未指定 fillfactor，则使用默认填充因子 100。 有关详细信息，请参阅 [为索引指定填充因子](../../relational-databases/indexes/specify-fill-factor-for-an-index.md)。  
  
 WITH NO_INFOMSGS  
 取消严重级别从 0 到 10 的所有信息性消息。  
  
## <a name="remarks"></a>备注  
DBCC DBREINDEX 重新生成表的一个索引或为表定义的所有索引。 通过允许动态重新生成索引，可以重新生成强制 PRIMARY KEY 或 UNIQUE 约束的索引，而不必删除并重新创建这些约束。 这意味着无需了解表的结构或其约束，即可重新生成索引。 这可能在将数据大容量复制到表中以后发生。

DBCC DBREINDEX 可以在一条语句中重新生成表的所有索引。 这要比对多条 DROP INDEX 和 CREATE INDEX 语句进行编码更容易。 由于这项工作是通过一条语句执行的，因此 DBCC DBREINDEX 自动成为原子性的，而单个 DROP INDEX 和 CREATE INDEX 语句则必须包含在事务中才能成为原子性的。 此外，DBCC DBREINDEX 提供了比单个 DROP INDEX 和 CREATE INDEX 语句更多的优化性能。

与 DBCC INDEXDEFRAG 或具有 REORGANIZE 选项的 ALTER INDEX 不同，DBCC DBREINDEX 是一个脱机操作。 如果重新生成了非聚集索引，则在该操作的持续时间内，相关表持有共享锁。 这可以禁止对表进行修改。 如果重新生成了聚集索引，则持有排他表锁。 这可以禁止任何表访问，因此可以有效地使表脱机。 为了执行联机索引重新生成，或控制索引重新生成操作期间的并行度，可使用具有 ONLINE 选项的 ALTER INDEX REBUILD 语句。

有关选择方法来重新生成或重新组织索引的详细信息，请参阅[重新组织和重新生成索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)。
  
## <a name="restrictions"></a>限制  
不支持对以下对象使用 DBCC DBREINDEX：
-   系统表  
-   空间索引  
-   xVelocity 内存优化的列存储索引  
  
## <a name="result-sets"></a>结果集  
除非指定了 NO_INFOMSGS（必须指定表名），否则 DBCC DBREINDEX 将始终返回：
  
```sql
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>权限  
调用方必须拥有此表，或是 sysadmin 固定服务器角色、db_owner 固定数据库角色或 db_ddladmin 固定数据库角色的成员    。
  
## <a name="examples"></a>示例  
### <a name="a-rebuilding-an-index"></a>A. 重新生成索引  
以下示例使用填充因子 `Employee_EmployeeID` 对 `80` 数据库中的 `Employee` 表重新生成 `AdventureWorks` 聚集索引。
  
```sql  
USE AdventureWorks2012;   
GO  
DBCC DBREINDEX ('HumanResources.Employee', PK_Employee_BusinessEntityID,80);  
GO  
```  
  
### <a name="b-rebuilding-all-indexes"></a>B. 重新生成所有索引  
以下示例使用填充因子值 `Employee` 对 `AdventureWorks` 中的 `70` 表重新生成所有索引。
  
```sql
USE AdventureWorks2012;   
GO  
DBCC DBREINDEX ('HumanResources.Employee', ' ', 70);  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
[sys.dm_db_index_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)  
[ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)  
  
