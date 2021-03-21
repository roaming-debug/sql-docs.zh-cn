---
title: 使用字符格式导入和导出数据
description: 采用字符格式后，所有列均应用字符数据格式。 与其他程序一起使用或从其他数据库供应商复制到实例时，这很有用。
ms.date: 09/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- data formats [SQL Server], character
- character formats [SQL Server]
ms.assetid: d925e66a-1a73-43cd-bc06-1cbdf8174a4d
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: cb3a2d60145ceeadd1a7d13cb8a6cb788e2a0131
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753647"
---
# <a name="use-character-format-to-import-or-export-data-sql-server"></a>使用字符格式导入或导出数据 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
将数据批量导出到要在其他程序中使用的文本文件时，或从其他程序生成的文本文件批量导入数据时，建议使用字符格式。  

采用字符格式后，所有列均应用字符数据格式。 如果要将数据用于其他程序（如电子表格程序），或需要通过其他数据库供应商（如 Oracle）将数据复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中，则以字符格式存储信息会非常有用。  
  
> [!NOTE]
>  当你在 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例之间批量传输数据，且数据文件包含 Unicode 字符数据，但不包含任何扩展字符或 DBCS 字符时，请使用 Unicode 字符格式。 有关详细信息，请参阅 [使用 Unicode 字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)。
  
|本主题内容：|
|---|
|[使用字符格式的注意事项](#considerations)|
|[字符格式的命令选项](#command_options)|
|[示例测试条件](#etc)<br />&emsp;&#9679;&emsp;[示例表](#sample_table)<br />&emsp;&#9679;&emsp;[示例非 XML 格式化文件](#nonxml_format_file)<br />|
|[示例](#examples)<br />&emsp;&#9679;&emsp;[使用 bcp 和字符格式导出数据](#bcp_char_export)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 bcp 和字符格式导入数据](#bcp_char_import)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 bcp 和字符格式导入数据](#bcp_char_import_fmt)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 BULK INSERT 和字符格式](#bulk_char)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 BULK INSERT 和字符格式](#bulk_char_fmt)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 OPENROWSET 和字符格式](#openrowset_char_fmt)|
|[相关任务](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|


  
## <a name="considerations-for-using-character-format"></a>使用字符格式的注意事项<a name="considerations"></a>
使用字符格式时，请考虑下列事项：  
  
-   默认情况下， [bcp 实用工具](../../tools/bcp-utility.md) 使用制表符分隔字符数据字段，并用换行符终止记录。  有关如何指定替换终止符的详细信息，请参阅[指定字段终止符和行终止符 (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)。  
  
-   默认情况下，在批量导出或导入字符模式数据之前，将执行下列转换：  
  
    |批量操作的方向|转换|  
    |---------------------------------|----------------|  
    |导出|将数据转换为字符表示形式。 如果进行显式请求，字符列的数据将转换为所请求的代码页。 如果未指定代码页，将通过使用客户端计算机的 OEM 代码页对字符数据进行转换。|  
    |导入|必要时将字符数据转换为本机表示形式，并将字符数据从客户端的代码页转换到目标列的代码页。|  
  
-   为避免在转换期间丢失扩展字符，请使用 Unicode 字符格式或指定代码页。  
  
-   存储在字符格式文件中的所有 [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) 数据都是在不包括元数据的情况下进行存储的。 每个数据值都将按照隐式数据转换规则转换为 [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) 格式。 当数据导入到 [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) 列中时，该数据是以 [char](../../t-sql/data-types/char-and-varchar-transact-sql.md)格式导入的。 而导入到数据类型不是 [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md)的列中时，数据将通过隐式转换从 [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) 格式转换为其他格式。 有关数据转换的详细信息，请参阅[数据类型转换（数据库引擎）](../../t-sql/data-types/data-type-conversion-database-engine.md)。  
  
-   [bcp 实用工具](../../tools/bcp-utility.md) 将 [money](../../t-sql/data-types/money-and-smallmoney-transact-sql.md) 值作为字符格式数据文件导出时，该数据文件小数点后保留四位数字且不带诸如逗号分隔符之类的任何数字分组符号。 例如，包含值 1,234,567.123456 的 [money](../../t-sql/data-types/money-and-smallmoney-transact-sql.md) 列将以字符串 1234567.1235 的形式批量导出到数据文件中。  
  
## <a name="command-options-for-character-format"></a>字符格式的命令选项<a name="command_options"></a>  
可以使用 [bcp](../../tools/bcp-utility.md)、[BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 或 [INSERT ... 将字符格式数据导入表中SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md)。对于 [bcp](../../tools/bcp-utility.md) 命令或 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 语句，你可以在语句中指定数据格式。  对于 [INSERT ...SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 语句，必须在格式化文件中指定数据格式。  
  
下列命令选项支持字符格式：  
  
|Command|选项|说明|  
|-------------|------------|-----------------|  
|bcp|**-c**|让 bcp 实用工具使用字符数据。\*|  
|BULK INSERT|DATAFILETYPE **='char'**|在批量导入数据时使用字符格式。|  
|OPENROWSET|空值|必须使用格式化文件|
  
 \*若要将字符 ( **-c**) 数据加载到与旧版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 客户端兼容的格式中，请使用 **-V** 开关。 有关详细信息，请参阅 [导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)。  
   
> [!NOTE]
>  或者，您可以在格式化文件中为每个字段指定格式设置。 有关详细信息，请参阅 [用来导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)。

## <a name="example-test-conditions"></a>示例测试条件<a name="etc"></a>  
本主题中的示例基于下面定义的表和格式化文件。

### <a name="sample-table"></a>**示例表**<a name="sample_table"></a>
以下脚本将创建测试数据库、名为 `myChar` 的表并用一些初始值填充表。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myChar ( 
   PersonID smallint NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date,
   AnnualSalary money
   );

-- Populate table
INSERT TestDatabase.dbo.myChar
VALUES 
(1, 'Anthony', 'Grosse', '1980-02-23', 65000.00),
(2, 'Alica', 'Fatnowna', '1963-11-14', 45000.00),
(3, 'Stella', 'Rossenhain', '1992-03-02', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myChar;
```

### <a name="sample-non-xml-format-file"></a>**示例非 XML 格式化文件**<a name="nonxml_format_file"></a>
SQL Server 支持两种类型的格式化文件：非 XML 格式和 XML 格式。  非 XML 格式是 SQL Server 早期版本支持的原始格式。  有关详细信息，请查看 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) 。  下面的命令基于 [的架构使用](../../tools/bcp-utility.md) bcp 实用工具 `myChar.fmt`生成非 XML 格式化文件 `myChar`。  若要使用 [bcp](../../tools/bcp-utility.md) 命令创建格式化文件，请指定 **format** 参数，并使用 **nul** 而不是数据文件路径。  格式化选项还需要 **-f** 选项。  此外，对于本示例，限定符 **c** 用于指定字符数据， **T** 用于指定使用集成安全性的受信任连接。  在命令提示符处输入以下命令：

```cmd
bcp TestDatabase.dbo.myChar format nul -f D:\BCP\myChar.fmt -T -c 

REM Review file
Notepad D:\BCP\myChar.fmt
```

> [!IMPORTANT]
> 确保非 XML 格式化文件以回车符/换行符结尾。  否则可能会收到以下错误消息：
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>示例<a name="examples"></a>
下面的示例使用上面创建的数据库和格式化文件。

### <a name="using-bcp-and-character-format-to-export-data"></a>**使用 bcp 和字符格式导出数据**<a name="bcp_char_export"></a>
**-c** 切换和 **OUT** 命令。  请注意：此示例中创建的数据文件将用于所有后续示例中。  在命令提示符处输入以下命令：

```cmd
bcp TestDatabase.dbo.myChar OUT D:\BCP\myChar.bcp -T -c

REM Review results
NOTEPAD D:\BCP\myChar.bcp
```

### <a name="using-bcp-and-character-format-to-import-data-without-a-format-file"></a>**在不使用格式化文件的情况下使用 bcp 和字符格式导入数据**<a name="bcp_char_import"></a>
**-c** 切换和 **IN** 命令。  在命令提示符处输入以下命令：

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myChar;"

REM Import data
bcp TestDatabase.dbo.myChar IN D:\BCP\myChar.bcp -T -c

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myChar;"
```

### <a name="using-bcp-and-character-format-to-import-data-with-a-non-xml-format-file"></a>**在使用非 XML 格式化文件的情况下使用 bcp 和字符格式导入数据**<a name="bcp_char_import_fmt"></a>
**-c** 和 **-f** 切换以及 **IN** 命令。  在命令提示符处输入以下命令：

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myChar;"

REM Import data
bcp TestDatabase.dbo.myChar IN D:\BCP\myChar.bcp -f D:\BCP\myChar.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myChar;"
```

### <a name="using-bulk-insert-and-character-format-without-a-format-file"></a>**在不使用格式化文件的情况下使用 BULK INSERT 和字符格式**<a name="bulk_char"></a>
**DATAFILETYPE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
TRUNCATE TABLE TestDatabase.dbo.myChar; -- for testing
BULK INSERT TestDatabase.dbo.myChar
    FROM 'D:\BCP\myChar.bcp'
    WITH (
        DATAFILETYPE = 'Char'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myChar;
```

### <a name="using-bulk-insert-and-character-format-with-a-non-xml-format-file"></a>**在使用非 XML 格式化文件的情况下使用 BULK INSERT 和字符格式**<a name="bulk_char_fmt"></a>
**FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
TRUNCATE TABLE TestDatabase.dbo.myChar; -- for testing
BULK INSERT TestDatabase.dbo.myChar
   FROM 'D:\BCP\myChar.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myChar.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myChar;
```

### <a name="using-openrowset-and-character-format-with-a-non-xml-format-file"></a>**在使用非 XML 格式化文件的情况下使用 OPENROWSET 和字符格式**<a name="openrowset_char_fmt"></a>
**FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
TRUNCATE TABLE TestDatabase.dbo.myChar;  -- for testing
INSERT INTO TestDatabase.dbo.myChar
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myChar.bcp', 
        FORMATFILE = 'D:\BCP\myChar.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myChar;
```
  
## <a name="related-tasks"></a>相关任务<a name="RelatedTasks"></a>  
使用数据格式进行批量导入或批量导出 
  
-   [导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [使用本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [使用 Unicode 字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [使用 Unicode 本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
  
