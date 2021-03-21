---
description: SET ANSI_NULL_DFLT_ON (Transact-SQL)
title: SET ANSI_NULL_DFLT_ON (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ANSI_NULL_DFLT_ON
- ANSI_NULL_DFLT_ON_TSQL
- SET ANSI_NULL_DFLT_ON
- SET_ANSI_NULL_DFLT_ON_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET ANSI_NULL_DFLT_ON statement
- ANSI_NULL_DFLT_ON option
- default nullability
- null values [SQL Server], overriding
- overriding default nullability
ms.assetid: 8c925924-a466-4c8b-aeb2-7e0d341f32db
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 484ce155e200c51f3607157579078c5c7ec1b94a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755347"
---
# <a name="set-ansi_null_dflt_on-transact-sql"></a>SET ANSI_NULL_DFLT_ON (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  当数据库的 **ANSI null default** 选项为 **false** 时，修改会话的行为以覆盖新列的默认为 Null 性。 有关设置 **ANSI null default** 的值的详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database

SET ANSI_NULL_DFLT_ON {ON | OFF}
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

SET ANSI_NULL_DFLT_ON ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注
 仅当在 CREATE TABLE 和 ALTER TABLE 语句中没有指定列的为空性时，该设置才能影响新列的为空性。 SET ANSI_NULL_DFLT_ON 为 ON 时，如果没有显式指定列的为空性状态，则使用 ALTER TABLE 和 CREATE TABLE 语句创建的新列可以使用空值。 SET ANSI_NULL_DFLT_ON 对使用显式 NULL 或 NOT NULL 创建的列无效。  
  
 不能同时将 SET ANSI_NULL_DFLT_OFF 和 SET ANSI_NULL_DFLT_ON 设置为 ON。 如果将一个选项设置为 ON，则将另一个选项设置为 OFF。 因此，可以将 ANSI_NULL_DFLT_OFF 或 ANSI_NULL_DFLT_ON 设置为 ON，或者将二者都设置为 OFF。 如果有一个选项为 ON，则该设置（SET ANSI_NULL_DFLT_OFF 或 SET ANSI_NULL_DFLT_ON）生效。 如果将这两个选项都设置为 OFF，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将使用 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中 **is_ansi_null_default_on** 列的值。  
  
 为使 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本在含有不同为空性设置的数据库中获得更可靠的操作，最好始终在 CREATE TABLE 和 ALTER TABLE 语句中指定 NULL 或 NOT NULL。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序在连接时会自动将 ANSI_NULL_DFLT_ON 设置为 ON。 对于来自 DB-Library 应用程序的连接，SET ANSI_NULL_DFLT_ON 的默认设置为 OFF。  
  
 SET ANSI_DEFAULTS 为 ON 时，将启用 SET ANSI_NULL_DFLT_ON。  
  
 SET ANSI_NULL_DFLT_ON 的设置是在执行或运行时设置的，而不是在分析时设置的。  
  
 在使用 SELECT INTO 语句创建表时，SET ANSI_NULL_DFLT_ON 的设置不适用。  
  
 要查看此设置的当前设置，请运行以下查询。  
  
```sql  
DECLARE @ANSI_NULL_DFLT_ON VARCHAR(3) = 'OFF';  
IF ( (1024 & @@OPTIONS) = 1024 ) SET @ANSI_NULL_DFLT_ON = 'ON';  
SELECT @ANSI_NULL_DFLT_ON AS ANSI_NULL_DFLT_ON;  
  
```  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
 以下示例显示当 **ANSI null default** 数据库选项在两种设置下时，对 `SET ANSI_NULL_DFLT_ON` 的影响。  
  
```sql  
USE AdventureWorks2012;  
GO  
  
-- The code from this point on demonstrates that SET ANSI_NULL_DFLT_ON  
-- has an effect when the 'ANSI null default' for the database is false.  
-- Set the 'ANSI null default' database option to false by executing  
-- ALTER DATABASE.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT OFF;  
GO  
-- Create table t1.  
CREATE TABLE t1 (a TINYINT) ;  
GO   
-- NULL INSERT should fail.  
INSERT INTO t1 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to ON and create table t2.  
SET ANSI_NULL_DFLT_ON ON;  
GO  
CREATE TABLE t2 (a TINYINT);  
GO   
-- NULL insert should succeed.  
INSERT INTO t2 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to OFF and create table t3.  
SET ANSI_NULL_DFLT_ON OFF;  
GO  
CREATE TABLE t3 (a TINYINT);  
GO  
-- NULL insert should fail.  
INSERT INTO t3 (a) VALUES (NULL);  
GO  
  
-- The code from this point on demonstrates that SET ANSI_NULL_DFLT_ON   
-- has no effect when the 'ANSI null default' for the database is true.  
-- Set the 'ANSI null default' database option to true.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT ON  
GO  
  
-- Create table t4.  
CREATE TABLE t4 (a TINYINT);  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t4 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to ON and create table t5.  
SET ANSI_NULL_DFLT_ON ON;  
GO  
CREATE TABLE t5 (a TINYINT);  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t5 (a) VALUES (NULL);  
GO  
  
-- SET ANSI_NULL_DFLT_ON to OFF and create table t6.  
SET ANSI_NULL_DFLT_ON OFF;  
GO  
CREATE TABLE t6 (a TINYINT);  
GO   
-- NULL INSERT should succeed.  
INSERT INTO t6 (a) VALUES (NULL);  
GO  
  
-- Set the 'ANSI null default' database option to false.  
ALTER DATABASE AdventureWorks2012 SET ANSI_NULL_DEFAULT ON;  
GO  
  
-- Drop tables t1 through t6.  
DROP TABLE t1,t2,t3,t4,t5,t6;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS (Transact-SQL)](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [SET ANSI_NULL_DFLT_OFF (Transact-SQL)](../../t-sql/statements/set-ansi-null-dflt-off-transact-sql.md)  
  
  
