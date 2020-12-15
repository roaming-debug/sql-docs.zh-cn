---
title: T-SQL 语句
description: 用于分析平台系统的 t-sql 语句 (AP) SQL Server 并行数据仓库 (PDW) 。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: df390f724f6903a1cd87deee1f9d48b363ce4f57
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489625"
---
# <a name="t-sql-statements-for-parallel-data-warehouse"></a>并行数据仓库的 t-sql 语句
Transact-sql (用于分析平台系统的 T-sql) 语句 (AP) SQL Server 并行数据仓库 (PDW) 。

## <a name="data-definition-language-ddl-statements"></a>数据定义语言 (DDL) 语句
* [ALTER DATABASE](../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
* [ALTER INDEX](../t-sql/statements/alter-index-transact-sql.md)
* [ALTER PROCEDURE](../t-sql/statements/alter-procedure-transact-sql.md)
* [ALTER SCHEMA](../t-sql/statements/alter-schema-transact-sql.md)
* [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md)
* [CREATE COLUMNSTORE INDEX](../t-sql/statements/create-columnstore-index-transact-sql.md)
* [CREATE DATABASE](../t-sql/statements/create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
* [CREATE DATABASE SCOPED CREDENTIAL](../t-sql/statements/create-database-scoped-credential-transact-sql.md)
* [CREATE EXTERNAL DATA SOURCE](../t-sql/statements/create-external-data-source-transact-sql.md)
* [CREATE EXTERNAL FILE FORMAT](../t-sql/statements/create-external-file-format-transact-sql.md)
* [CREATE EXTERNAL TABLE](../t-sql/statements/create-external-table-transact-sql.md)
* [CREATE FUNCTION](../t-sql/statements/create-function-sql-data-warehouse.md)
* [CREATE INDEX](../t-sql/statements/create-index-transact-sql.md)
* [CREATE PROCEDURE](../t-sql/statements/create-procedure-transact-sql.md)
* [CREATE SCHEMA](../t-sql/statements/create-schema-transact-sql.md)
* [CREATE STATISTICS](../t-sql/statements/create-statistics-transact-sql.md)
* [CREATE TABLE](../t-sql/statements/create-table-azure-sql-data-warehouse.md)
* [CREATE TABLE AS SELECT](../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)
* [CREATE VIEW](../t-sql/statements/create-view-transact-sql.md)
* [DROP EXTERNAL DATA SOURCE](../t-sql/statements/drop-external-data-source-transact-sql.md)
* [DROP EXTERNAL FILE FORMAT](../t-sql/statements/drop-external-file-format-transact-sql.md)
* [DROP EXTERNAL TABLE](../t-sql/statements/drop-external-table-transact-sql.md)
* [DROP INDEX](../t-sql/statements/drop-index-transact-sql.md)
* [DROP PROCEDURE](../t-sql/statements/drop-procedure-transact-sql.md)
* [DROP STATISTICS](../t-sql/statements/drop-statistics-transact-sql.md)
* [DROP TABLE](../t-sql/statements/drop-table-transact-sql.md)
* [DROP SCHEMA](../t-sql/statements/drop-schema-transact-sql.md)
* [DROP VIEW](../t-sql/statements/drop-view-transact-sql.md)
* [RENAME](../t-sql/statements/rename-transact-sql.md)
* [TRUNCATE TABLE](../t-sql/statements/truncate-table-transact-sql.md)
* [UPDATE STATISTICS](../t-sql/statements/update-statistics-transact-sql.md)

## <a name="data-manipulation-language-dml-statements"></a>数据操作语言 (DML) 语句
* [DELETE](../t-sql/statements/delete-transact-sql.md)
* [INSERT](../t-sql/statements/insert-transact-sql.md)
* [UPDATE](../t-sql/queries/update-transact-sql.md)

## <a name="database-console-commands"></a>数据库控制台命令
* [DBCC DROPCLEANBUFFERS](../t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql.md)
* [DBCC FREEPROCCACHE](/previous-versions/sql/sql-server-2016/mt204018(v=sql.130))
* [DBCC SHRINKLOG](../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md?view=aps-pdw-2016-au7)
* [DBCC PDW_SHOWEXECUTIONPLAN](../t-sql/database-console-commands/dbcc-pdw-showexecutionplan-transact-sql.md?view=aps-pdw-2016-au7)
* [DBCC PDW_SHOWPARTITIONSTATS](../t-sql/database-console-commands/dbcc-pdw-showpartitionstats-transact-sql.md?view=aps-pdw-2016-au7)
* [DBCC PDW_SHOWSPACEUSED](../t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql.md)
* [DBCC SHOW_STATISTICS](../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md?view=sql-server-ver15)

## <a name="query-statements"></a>查询语句
* [SELECT](../t-sql/queries/select-transact-sql.md)
* [WITH common_table_expression](../t-sql/queries/with-common-table-expression-transact-sql.md)
* [EXCEPT 和 INTERSECT](../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)
* [EXPLAIN](../t-sql/queries/explain-transact-sql.md)
* [FROM](../t-sql/queries/from-transact-sql.md)
* [使用 PIVOT 和 UNPIVOT](../t-sql/queries/from-using-pivot-and-unpivot.md)
* [GROUP BY](../t-sql/queries/select-group-by-transact-sql.md)
* [HAVING](../t-sql/queries/select-having-transact-sql.md)
* [ORDER BY](../t-sql/queries/select-order-by-clause-transact-sql.md)
* [OPTION](../t-sql/queries/option-clause-transact-sql.md)
* [UNION](../t-sql/language-elements/set-operators-union-transact-sql.md)
* [WHERE](../t-sql/queries/where-transact-sql.md)
* [TOP](../t-sql/queries/top-transact-sql.md)
* [别名](../t-sql/queries/aliasing-azure-sql-data-warehouse-parallel-data-warehouse.md)
* [搜索条件](../t-sql/queries/search-condition-transact-sql.md)
* [子查询](../t-sql/queries/subqueries-azure-sql-data-warehouse-parallel-data-warehouse.md)

## <a name="security-statements"></a>安全语句
* 权限：[GRANT](../t-sql/statements/grant-transact-sql.md)、[DENY](../t-sql/statements/deny-transact-sql.md)、[REVOKE](../t-sql/statements/revoke-transact-sql.md)
* [ALTER AUTHORIZATION](../t-sql/statements/alter-authorization-transact-sql.md)
* [ALTER CERTIFICATE](../t-sql/statements/alter-certificate-transact-sql.md)
* [ALTER DATABASE ENCRYPTION KEY](../t-sql/statements/alter-database-encryption-key-transact-sql.md)
* [ALTER LOGIN](../t-sql/statements/alter-login-transact-sql.md)
* [ALTER MASTER KEY](../t-sql/statements/alter-master-key-transact-sql.md)
* [ALTER ROLE](../t-sql/statements/alter-role-transact-sql.md)
* [ALTER USER](../t-sql/statements/alter-user-transact-sql.md)
* [BACKUP CERTIFICATE](../t-sql/statements/backup-certificate-transact-sql.md)
* [CLOSE MASTER KEY](../t-sql/statements/close-master-key-transact-sql.md)
* [CREATE CERTIFICATE](../t-sql/statements/create-certificate-transact-sql.md)
* [CREATE DATABASE ENCRYPTION KEY](../t-sql/statements/create-database-encryption-key-transact-sql.md)
* [CREATE LOGIN](../t-sql/statements/create-login-transact-sql.md)
* [CREATE MASTER KEY](../t-sql/statements/create-master-key-transact-sql.md)
* [CREATE ROLE](../t-sql/statements/create-role-transact-sql.md)
* [CREATE USER](../t-sql/statements/create-user-transact-sql.md)
* [DROP CERTIFICATE](../t-sql/statements/drop-certificate-transact-sql.md)
* [DROP DATABASE ENCRYPTION KEY](../t-sql/statements/drop-database-encryption-key-transact-sql.md)
* [DROP LOGIN](../t-sql/statements/drop-login-transact-sql.md)
* [DROP MASTER KEY](../t-sql/statements/drop-master-key-transact-sql.md)
* [DROP ROLE](../t-sql/statements/drop-role-transact-sql.md)
* [DROP USER](../t-sql/statements/drop-user-transact-sql.md)
* [OPEN MASTER KEY](../t-sql/statements/open-master-key-transact-sql.md)

## <a name="next-steps"></a>后续步骤
有关更多参考信息，请参阅 [t-sql 语言元素](tsql-language-elements.md) 和 [t-sql 系统视图](tsql-system-views.md)。

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
