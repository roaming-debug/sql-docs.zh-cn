---
description: DROP TABLE (Transact-SQL)
title: DROP TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_TABLE_TSQL
- DROP TABLE
dev_langs:
- TSQL
helpviewer_keywords:
- removing indexes
- table removal [SQL Server]
- deleting indexes
- dropping data
- deleting tables
- dropping indexes
- removing constraints
- removing permissions
- DROP TABLE statement
- removing tables
- deleting triggers
- removing data
- dropping tables
- deleting permissions
- dropping triggers
- deleting constraints
- removing triggers
- deleting data
- dropping constraints
- dropping permissions
ms.assetid: 0b6f2b6f-3aa3-4767-943f-43df3c3c5cfd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0b1d331ac2548e6facff2029d757827822517e86
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688966"
---
# <a name="drop-table-transact-sql"></a>DROP TABLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  删除一个或多个表定义以及这些表的所有数据、索引、触发器、约束和权限规范。 任何引用已删除表的视图或存储过程都必须使用 [DROP VIEW](../../t-sql/statements/drop-view-transact-sql.md) 或 [DROP PROCEDURE](../../t-sql/statements/drop-procedure-transact-sql.md) 显式删除。 若要报告表的依赖关系，请使用 [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DROP TABLE [ IF EXISTS ] { database_name.schema_name.table_name | schema_name.table_name | table_name } [ ,...n ]  
[ ; ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *database_name*  
 要在其中创建表的数据库的名称。  
  
 Azure SQL 数据库支持由三部分组成的名称格式 database_name.[schema_name].object_name，其中 database_name 为当前数据库，database_name 为 tempdb，object_name 以 # 开头。 Azure SQL 数据库不支持由四部分组成的名称。  
  
 IF EXISTS  
 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 到[当前版本](../../sql-server/what-s-new-in-sql-server-2016.md)）。  
  
 有条件地删除表（仅当其已存在时）。  
  
 *schema_name*  
 表所属架构的名称。  
  
 *table_name*  
 要删除的表的名称。  
  
## <a name="remarks"></a>备注  
 不能使用 DROP TABLE 删除被 FOREIGN KEY 约束引用的表。 必须先删除引用 FOREIGN KEY 约束或引用表。 如果要在同一个 DROP TABLE 语句中删除引用表以及包含主键的表，则必须先列出引用表。  
  
 可以在任何数据库中删除多个表。 如果一个要删除的表引用了另一个也要删除的表的主键，则必须先列出包含该外键的引用表，然后再列出包含要引用的主键的表。  
  
 删除表时，表的规则或默认值将被解除绑定，与该表关联的任何约束或触发器将被自动删除。 如果要重新创建表，则必须重新绑定相应的规则和默认值，重新创建某些触发器，并添加所有必需的约束。  
  
 如果使用 DELETE tablename 删除表中的所有行或使用 TRUNCATE TABLE 语句，则在表被删除之前，表将一直存在。  
  
 删除使用了超过 128 个区的大型表和索引时，需要分两个单独的阶段：逻辑和物理阶段。 在逻辑阶段中，对表使用的现有分配单元进行标记以便释放，并对其进行锁定，直到事务提交为止。 在物理阶段，标记为要释放的 IAM 页被成批地物理删除。  
  
 如果删除的表包含带有 FILESTREAM 属性的 VARBINARY (MAX) 列，则不会删除在文件系统中存储的任何数据。  
  
> [!IMPORTANT]  
>  不应在同一个批处理中对同一个表执行 DROP TABLE 和 CREATE TABLE。 否则，可能出现意外错误。  
  
## <a name="permissions"></a>权限  
 需要拥有该表所属架构的 ALTER 权限、该表的 CONTROL 权限或 **db_ddladmin** 固定数据库角色中的成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-dropping-a-table-in-the-current-database"></a>A. 删除当前数据库中的表  
 以下示例将从当前数据库中删除 `ProductVendor1` 表及其数据和索引。  
  
```sql  
DROP TABLE ProductVendor1 ;  
```  
  
### <a name="b-dropping-a-table-in-another-database"></a>B. 删除其他数据库中的表  
 以下示例将删除 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中的 `SalesPerson2` 表。 可以在服务器实例上的任何数据库中执行此示例。  
  
```sql  
DROP TABLE AdventureWorks2012.dbo.SalesPerson2 ;  
```  
  
### <a name="c-dropping-a-temporary-table"></a>C. 删除临时表  
 以下示例将创建一个临时表，测试该表是否存在，删除该表，然后再次测试该表是否存在。 此示例不使用 IF EXISTS 语法，该语法适用于 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及以上版本。  
  
```sql  
CREATE TABLE #temptable (col1 INT);  
GO  
INSERT INTO #temptable  
VALUES (10);  
GO  
SELECT * FROM #temptable;  
GO  
IF OBJECT_ID(N'tempdb..#temptable', N'U') IS NOT NULL   
DROP TABLE #temptable;  
GO  
--Test the drop.  
SELECT * FROM #temptable;  
```  
  
### <a name="d-dropping-a-table-using-if-exists"></a>D. 使用 IF EXISTS 删除表  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 到[当前版本](../../sql-server/what-s-new-in-sql-server-2016.md)）。  
  
 以下示例创建名为 T1 的表。 然后，第二条语句删除表。 第三条语句不执行任何操作，因为此表已删除，但这不会引起错误。  
  
```sql  
CREATE TABLE T1 (Col1 INT);  
GO  
DROP TABLE IF EXISTS T1;  
GO  
DROP TABLE IF EXISTS T1;  
```  
  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_spaceused (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [TRUNCATE TABLE (Transact-SQL)](../../t-sql/statements/truncate-table-transact-sql.md)   
 [DROP VIEW (Transact-SQL)](../../t-sql/statements/drop-view-transact-sql.md)   
 [DROP PROCEDURE (Transact-SQL)](../../t-sql/statements/drop-procedure-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)