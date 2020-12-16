---
description: SET ANSI_NULLS (Transact-SQL)
title: SET ANSI_NULLS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/24/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET_ANSI_NULLS_TSQL
- ANSI_NULLS
- SET ANSI_NULLS
- ANSI_NULLS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET ANSI_NULLS statement
- not equal to operator (<>)
- ANSI_NULLS option
- equals operator (=)
- null values [SQL Server], comparison operators
- comparison operators [SQL Server], null values
ms.assetid: aae263ef-a3c7-4dae-80c2-cc901e48c755
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current || azuresqldb-current'
ms.openlocfilehash: ff107ac1d65634b3bdbd8230ce757d387f916364
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476568"
---
# <a name="set-ansi_nulls-transact-sql"></a>SET ANSI_NULLS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

指定在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中与 Null 值一起使用等于 (=) 和不等于 (<>) 比较运算符时采用符合 ISO 标准的行为。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>语法

### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>[!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)] 的语法
```syntaxsql
SET ANSI_NULLS { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>[!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] 和 [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)] 的语法
```syntaxsql
SET ANSI_NULLS ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注
当 ANSI_NULLS 为 ON 时，即使 column_name 中包含空值，使用 WHERE column_name = NULL 的 SELECT 语句仍返回零行。 即使 column_name 中包含非空值，使用 WHERE column_name <> NULL 的 SELECT 语句仍返回零行。  
  
当 ANSI_NULLS 为 OFF 时，等于 (=) 和不等于 (<>) 比较运算符不遵守 ISO 标准。 使用 WHERE column_name = NULL 的 SELECT 语句返回 column_name 中包含空值的行。 使用 WHERE column_name <> NULL 的 SELECT 语句返回列中包含非空值的行。 此外，使用 WHERE column_name <> XYZ_value 的 SELECT 语句返回所有不为 XYZ_value 也不为 NULL 的行  。  
  
当 ANSI_NULLS 为 ON 时，所有对 null 值的比较均取值为 UNKNOWN。 当 SET ANSI_NULLS 为 OFF 时，如果数据值为 NULL，则所有数据对空值的比较将取值为 TRUE。 如果未指定 SET ANSI_NULLS，则应用当前数据库的 ANSI_NULLS 选项设置。 有关 ANSI_NULLS 数据库选项的详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。  

下表显示 ANSI_NULLS 的设置如何通过 NULL 值和非 NULL 值影响大量布尔表达式的结果。  
  
|布尔表达式|SET ANSI_NULLS ON|SET ANSI_NULLS OFF|  
|---------------|---------------|------------|  
|NULL = NULL|UNKNOWN|TRUE|  
|1 = NULL|UNKNOWN|FALSE|  
|NULL <> NULL|UNKNOWN|FALSE|  
|1 <> NULL|UNKNOWN|TRUE|  
|NULL > NULL|UNKNOWN|UNKNOWN|  
|1 > NULL|UNKNOWN|UNKNOWN|  
|NULL IS NULL|TRUE|TRUE|  
|1 IS NULL|FALSE|FALSE|  
|NULL IS NOT NULL|FALSE|FALSE|  
|1 IS NOT NULL|TRUE|TRUE|  

仅当某个比较操作数是值为 NULL 的变量或文字 NULL 时，SET ANSI_NULLS ON 才会影响比较。 如果比较双方是列或复合表达式，则该设置不会影响比较。  
  
为使脚本按预期运行，不管 ANSI_NULLS 数据库选项或 SET ANSI_NULLS 的设置如何，请在可能包含空值的比较中使用 IS NULL 和 IS NOT NULL。  
  
在执行分布式查询时应将 ANSI_NULLS 设置为 ON。  
  
针对计算列或索引视图创建或更改索引时，ANSI_NULLS 也必须为 ON。 如果 SET ANSI_NULLS 为 OFF，则针对表（包含计算列或索引视图的索引）的 CREATE、UPDATE、INSERT 和 DELETE 语句将失败。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回一个错误消息，该错误消息会列出所有违反所需值的 SET 选项。 另外，在执行 SELECT 语句时，如果 SET ANSI_NULLS 为 OFF，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 忽略计算列或视图的索引值并解析选择操作，就好像表或视图没有这样的索引一样。  
  
> [!NOTE]  
> ANSI_NULLS 是在处理计算列或索引视图的索引时必须设置为所需值的七个 SET 选项之一。 此外，`ANSI_PADDING`、`ANSI_WARNINGS`、`ARITHABORT`、`QUOTED_IDENTIFIER` 和 `CONCAT_NULL_YIELDS_NULL` 选项还必须设置为 ON，而 `NUMERIC_ROUNDABORT` 必须设置为 OFF。  
  
 进行连接时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口会自动将 ANSI_NULLS 设置为 ON。 该设置可以在 ODBC 数据源、ODBC 连接属性或 OLE DB 连接属性（它们在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例之前在应用程序中设置）中进行配置。 SET ANSI_NULLS 的默认值为 OFF。  
  
ANSI_DEFAULTS 为 ON 时，将启用 ANSI_NULLS。  
  
ANSI_NULLS 的设置是在执行或运行时定义的，而不是在分析时定义的。  
  
要查看此设置的当前设置，请运行以下查询：
  
```sql  
DECLARE @ANSI_NULLS VARCHAR(3) = 'OFF';  
IF ( (32 & @@OPTIONS) = 32 ) SET @ANSI_NULLS = 'ON';  
SELECT @ANSI_NULLS AS ANSI_NULLS;   
```  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
 以下示例使用等于 (`=`) 和不等于 (`<>`) 比较运算符对表中的 `NULL` 值和非 NULL 值进行比较。 该示例还表明，`IS NULL` 不受 `SET ANSI_NULLS` 设置的影响。  
  
```sql  
-- Create table t1 and insert values.  
CREATE TABLE dbo.t1 (a INT NULL);  
INSERT INTO dbo.t1 values (NULL),(0),(1);  
GO  
  
-- Print message and perform SELECT statements.  
PRINT 'Testing default setting';  
DECLARE @varname int;   
SET @varname = NULL;  
  
SELECT a  
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO 
```

现在将 ANSI_NULLS 设置为 ON 并测试。

```sql
PRINT 'Testing ANSI_NULLS ON';  
SET ANSI_NULLS ON;  
GO  
DECLARE @varname int;  
SET @varname = NULL  
  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
```

现在将 ANSI_NULLS 设置为 OFF 并测试。  

```sql
PRINT 'Testing ANSI_NULLS OFF';  
SET ANSI_NULLS OFF;  
GO  
DECLARE @varname int;  
SET @varname = NULL;  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
  
-- Drop table t1.  
DROP TABLE dbo.t1;  
```  
  
## <a name="see-also"></a>另请参阅  
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY (Transact-SQL)](../../t-sql/functions/sessionproperty-transact-sql.md)   
 [= (Equals) (Transact-SQL)](../../t-sql/language-elements/equals-transact-sql.md)   
 [IF...ELSE (Transact-SQL)](../../t-sql/language-elements/if-else-transact-sql.md)   
 [<> (Not Equal To) (Transact-SQL)](../../t-sql/language-elements/not-equal-to-transact-sql-traditional.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS (Transact-SQL)](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)   
 [WHILE (Transact-SQL)](../../t-sql/language-elements/while-transact-sql.md)  
  
