---
description: SET QUOTED_IDENTIFIER (Transact-SQL)
title: SET QUOTED_IDENTIFIER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- QUOTED_IDENTIFIER_TSQL
- SET_QUOTED_IDENTIFIER_TSQL
- SET QUOTED_IDENTIFIER
- QUOTED_IDENTIFIER
dev_langs:
- TSQL
helpviewer_keywords:
- delimited identifiers [SQL Server]
- identifiers [SQL Server], delimited
- QUOTED_IDENTIFIER option
- quoted identifiers
- ISO delimited identifiers rules
- SET QUOTED_IDENTIFIER statement
ms.assetid: 10f66b71-9241-4a3a-9292-455ae7252565
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8bdb8bbd6d6c972b0b1fc5aa92ae466c6bc45c02
ms.sourcegitcommit: 0c0e4ab90655dde3e34ebc08487493e621f25dda
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96443090"
---
# <a name="set-quoted_identifier-transact-sql"></a>SET QUOTED_IDENTIFIER (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 遵从关于引号分隔标识符和文字字符串的 ISO 规则。 由双引号分隔的标识符可以是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 保留关键字，也可以包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符语法约定通常不允许的字符。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for SQL Server, Azure SQL Database and serverless SQL pool in Azure Synapse Analytics

SET QUOTED_IDENTIFIER { ON | OFF }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

SET QUOTED_IDENTIFIER ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注
`SET QUOTED_IDENTIFIER` 为 ON（默认）时，可以用双引号 (" ") 分隔标识符，而文字必须用单引号 (' ') 来分隔。 所有用双引号分隔的字符串都被解释为对象标识符。 因此，加引号的标识符不必符合 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符规则。 它们可以是保留关键字，并且可以包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符中通常不允许的字符。 不能使用双引号分隔文字字符串表达式，而必须用单引号括住文字字符串。 如果单引号 (') 是文本字符串的一部分，则可用两个单引号 ('') 表示。 当对数据库中的对象名使用保留关键字时，`SET QUOTED_IDENTIFIER` 必须为 ON。

当 `SET QUOTED_IDENTIFIER` 为 OFF 时，标识符不可加引号，且必须符合所有 [!INCLUDE[tsql](../../includes/tsql-md.md)] 标识符规则。 有关详细信息，请参阅 [Database Identifiers](../../relational-databases/databases/database-identifiers.md)。 文字可以由单引号或双引号分隔。 如果文字字符串由双引号分隔，则可以在字符串中包含嵌入式单引号，如省略号。

> [!NOTE]
> QUOTED_IDENTIFIER 不影响括在方括号 ([ ]) 中的分隔标识符。

在创建或更改计算列的索引或索引视图时，`SET QUOTED_IDENTIFIER` 必须为 ON。 如果 `SET QUOTED_IDENTIFIER` 为 OFF，则对计算列或索引视图的索引所在的表执行 CREATE、UPDATE、INSERT 和 DELETE 语句将失败。 有关计算列的索引视图和索引所需的 SET 选项设置的详细信息，请参阅[使用 SET 语句时的注意事项](../../t-sql/statements/set-statements-transact-sql.md#considerations-when-you-use-the-set-statements)。

创建筛选索引时，`SET QUOTED_IDENTIFIER` 必须为 ON。

调用 XML 数据类型方法时，`SET QUOTED_IDENTIFIER` 必须为 ON。

在进行连接时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB Provider for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动将 QUOTED_IDENTIFIER 设置为 ON。 这可以在 ODBC 数据源、ODBC 连接特性或 OLE DB 连接属性中进行配置。 对来自 DB-Library 应用程序的连接，SET QUOTED_IDENTIFIER 默认设置为 OFF。

创建表时，即使此时将 QUOTED IDENTIFIER 选项设置为 OFF，该选项在表的元数据中仍始终存储为 ON。

创建存储过程时，将捕获 SET QUOTED_IDENTIFIER 和 SET ANSI_NULLS 设置，并用于该存储过程的后续调用。

在存储过程内执行 SET QUOTED_IDENTIFIER 时，其设置不更改。

`SET ANSI_DEFAULTS` 为 ON 时，QUOTED_IDENTIFIER 也为 ON。

`SET QUOTED_IDENTIFIER` 还与 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) 的 QUOTED_IDENTIFIER 设置相对应。

`SET QUOTED_IDENTIFIER` 在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 分析时生效，只会影响分析，不影响查询优化或查询执行。

对于顶级即席批处理，请使用会话的当前 QUOTED_IDENTIFIER 设置开始分析。 分析批处理时，只要出现 `SET QUOTED_IDENTIFIER`，就会更改之后的分析行为，并保存会话的设置。 因此，分析和执行批处理后，会根据批处理中最后一次出现的 `SET QUOTED_IDENTIFIER`，设置会话的 QUOTED_IDENTIFER 设置。

存储过程中的静态 [!INCLUDE[tsql](../../includes/tsql-md.md)] 是使用对于创建或更改存储过程的批处理有效的 QUOTED_IDENTIFIER 设置分析的。 `SET QUOTED_IDENTIFIER` 作为静态 [!INCLUDE[tsql](../../includes/tsql-md.md)] 出现在存储过程的正文中时是无效的。

对于使用 `sp_executesql` 或 `exec()` 的嵌套批处理，使用会话的 QUOTED_IDENTIFIER 设置开始进行分析。 如果嵌套批处理在存储过程内，则使用存储过程的 QUOTED_IDENTIFIER 设置开始进行分析。 分析嵌套批处理时，只要出现 `SET QUOTED_IDENTIFIER`，就会改变之后的分析行为，但不会更新会话的 QUOTED_IDENTIFIER 设置。

要查看此设置的当前设置，请运行以下查询：

```sql
DECLARE @QUOTED_IDENTIFIER VARCHAR(3) = 'OFF';
IF ( (256 & @@OPTIONS) = 256 ) 
SET @QUOTED_IDENTIFIER = 'ON';

SELECT @QUOTED_IDENTIFIER AS QUOTED_IDENTIFIER;
```

## <a name="permissions"></a>权限
要求具有 `PUBLIC` 角色的成员身份。

## <a name="examples"></a>示例

### <a name="a-using-the-quoted-identifier-setting-and-reserved-word-object-names"></a>A. 使用加引号的标识符设置和保留字对象名

以下示例显示 `SET QUOTED_IDENTIFIER` 设置必须为 `ON`，而且表名内的关键字必须在双引号内，才能创建和使用具有保留关键字名称的对象。

```sql
SET QUOTED_IDENTIFIER OFF
GO

-- Create statement fails.
CREATE TABLE "select" ("identity" INT IDENTITY NOT NULL, "order" INT NOT NULL);
GO

SET QUOTED_IDENTIFIER ON;
GO

-- Create statement succeeds.
CREATE TABLE "select" ("identity" INT IDENTITY NOT NULL, "order" INT NOT NULL);
GO

SELECT "identity","order"
FROM "select"
ORDER BY "order";
GO

DROP TABLE "SELECT";
GO

SET QUOTED_IDENTIFIER OFF;
GO
```

### <a name="b-using-the-quoted-identifier-setting-with-single-and-double-quotation-marks"></a>B. 使用加单引号和双引号的标识符设置

 以下示例显示将 `SET QUOTED_IDENTIFIER` 设置为 `ON` 和 `OFF` 时，在字符串表达式中使用单引号和双引号的方式。

```sql
SET QUOTED_IDENTIFIER OFF;
GO

USE AdventureWorks2012;
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES
    WHERE TABLE_NAME = 'Test')
    DROP TABLE dbo.Test;
GO
USE AdventureWorks2012;
CREATE TABLE dbo.Test (ID INT, String VARCHAR(30)) ;
GO

-- Literal strings can be in single or double quotation marks.
INSERT INTO dbo.Test VALUES (1, "'Text in single quotes'");
INSERT INTO dbo.Test VALUES (2, '''Text in single quotes''');
INSERT INTO dbo.Test VALUES (3, 'Text with 2 '''' single quotes');
INSERT INTO dbo.Test VALUES (4, '"Text in double quotes"');
INSERT INTO dbo.Test VALUES (5, """Text in double quotes""");
INSERT INTO dbo.Test VALUES (6, "Text with 2 """" double quotes");
GO

SET QUOTED_IDENTIFIER ON;
GO

-- Strings inside double quotation marks are now treated
-- as object names, so they cannot be used for literals.
INSERT INTO dbo."Test" VALUES (7, 'Text with a single '' quote');
GO

-- Object identifiers do not have to be in double quotation marks
-- if they are not reserved keywords.
SELECT ID, String
FROM dbo.Test;
GO

DROP TABLE dbo.Test;
GO

SET QUOTED_IDENTIFIER OFF;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
 ID          String
 ----------- ------------------------------
 1           'Text in single quotes'
 2           'Text in single quotes'
 3           Text with 2 '' single quotes
 4           "Text in double quotes"
 5           "Text in double quotes"
 6           Text with 2 "" double quotes
 7           Text with a single ' quote
 ```

## <a name="see-also"></a>另请参阅
[CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)    
[CREATE DEFAULT](../../t-sql/statements/create-default-transact-sql.md)    
[CREATE PROCEDURE](../../t-sql/statements/create-procedure-transact-sql.md)    
[CREATE RULE](../../t-sql/statements/create-rule-transact-sql.md)    
[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)    
[CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md)    
[CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)    
[数据类型](../../t-sql/data-types/data-types-transact-sql.md)    
[EXECUTE](../../t-sql/language-elements/execute-transact-sql.md)    
[SELECT](../../t-sql/queries/select-transact-sql.md)    
[SET Statements](../../t-sql/statements/set-statements-transact-sql.md)    
[SET ANSI_DEFAULTS](../../t-sql/statements/set-ansi-defaults-transact-sql.md)    
[sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)    
[数据库标识符](../../relational-databases/databases/database-identifiers.md)
