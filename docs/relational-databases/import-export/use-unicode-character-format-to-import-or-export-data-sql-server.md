---
title: 使用 Unicode 字符格式导入和导出数据
description: 从 SQL Server 实例导出数据时，Unicode 字符数据格式允许使用与客户端不同的代码页。
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- data formats [SQL Server], Unicode character
- Unicode [SQL Server], bulk importing and exporting
ms.assetid: 74342a11-c1c0-4746-b482-7f3537744a70
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 8999c3a94d65ee29d496f891068e158f5f178814
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465568"
---
# <a name="use-unicode-character-format-to-import-or-export-data-sql-server"></a>使用 Unicode 字符格式导入或导出数据 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
使用包含扩展/DBCS 字符的数据文件在多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例之间大容量传输数据时，建议使用 Unicode 字符格式。 从服务器导出数据时，Unicode 字符数据格式允许使用与执行该操作的客户端不同的代码页。 在这种情况下，使用 Unicode 字符格式有下列优点：  
  
* 如果源数据和目标数据的类型为 Unicode，则使用 Unicode 字符格式可以保留所有的字符数据。  
  
* 如果源数据和目标数据不为 Unicode 数据类型，则使用 Unicode 字符格式可以尽可能减少丢失源数据中无法在目标数据中表示的扩展字符。

|本主题内容：|
|---|
|[使用 Unicode 字符格式的注意事项](#considerations)|
|[使用 Unicode 字符格式、bcp 和格式化文件的特殊注意事项](#special_considerations)|
|[Unicode 字符格式的命令选项](#command_options)|
|[示例测试条件](#etc)<br />&emsp;&#9679;&emsp;[示例表](#sample_table)<br />&emsp;&#9679;&emsp;[示例非 XML 格式化文件](#nonxml_format_file)|
|[示例](#examples)<br />&emsp;&#9679;&emsp;[使用 bcp 和 Unicode 字符格式导出数据](#bcp_widechar_export)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 bcp 和 Unicode 字符格式导入数据](#bcp_widechar_import)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 bcp 和 Unicode 字符格式导入数据](#bcp_widechar_import_fmt)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 BULK INSERT 和 Unicode 字符格式](#bulk_widechar)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 BULK INSERT 和 Unicode 字符格式](#bulk_widechar_fmt)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 OPENROWSET 和 Unicode 字符格式](#openrowset_widechar_fmt)|
|[相关任务](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|
 
## <a name="considerations-for-using-unicode-character-format"></a>使用 Unicode 字符格式的注意事项<a name="considerations"></a>
使用 Unicode 字符格式时，请考虑下列事项：  

* 默认情况下， [bcp 实用工具](../../tools/bcp-utility.md) 使用制表符分隔字符数据字段，并用换行符终止记录。  有关如何指定替换终止符的详细信息，请参阅[指定字段终止符和行终止符 (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)。

* Unicode 字符格式数据文件中存储的 [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) 数据的操作方式与字符格式数据文件中的同类数据的操作方式相同，唯一的不同是该数据存储为 [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) 数据而不是 [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) 数据。 有关字符格式的详细信息，请参阅 [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md)。  

## <a name="special-considerations-for-using-unicode-character-format-bcp-and-a-format-file"></a>使用 Unicode 字符格式、bcp 和格式化文件的特殊注意事项<a name="special_considerations"></a>
Unicode 字符格式数据文件遵循 Unicode 文件的约定。  该文件的前两个字节为十六进制数字 0xFFFE。  这两个字节用作字节顺序标记 (BOM)，指定在文件中高位字节是存储在前面还是后面。  [bcp 实用工具](../../tools/bcp-utility.md) 可能曲解字节顺序标记，并导致部分导入过程失败；可能会收到如下错误消息：
```
Starting copy...
SQLState = 22005, NativeError = 0
Error = [Microsoft][ODBC Driver 13 for SQL Server]Invalid character value for cast specification
```

在以下情况下，字节顺序标记可能被曲解：
* 使用 [bcp 实用工具](../../tools/bcp-utility.md) 和 **-w** 切换表示 Unicode 字符

* 使用格式化文件

* 数据文件中的第一个字段是非字符

考虑以下任意解决方法是否适用于特定  情况：
* 请勿使用格式化文件。  下面提供了此解决方法的一个示例，请参阅 [在不使用格式化文件的情况下使用 bcp 和 Unicode 字符格式导入数据](#bcp_widechar_import)，

* 使用 **-c** 切换而不是 **-w**，

* 使用本机格式重新导出数据，

* 使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 或 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md)。  下面提供了这些解决方法的示例，请参阅 [在使用非 XML 格式化文件的情况下使用 BULK INSERT 和 Unicode 字符格式](#bulk_widechar_fmt) 以及 [在使用非 XML 格式化文件的情况下使用 OPENROWSET 和 Unicode 字符格式](#openrowset_widechar_fmt)，
 
* 手动插入目标表中的第一条记录，然后使用 **-F 2** 切换在第二条记录上开始导入，

* 手动插入数据文件中虚拟的第一条记录，然后使用 **-F 2** 切换在第二条记录上开始导入。  下面提供了此解决方法的一个示例，请参阅 [在使用非 XML 格式化文件的情况下使用 bcp 和 Unicode 字符格式导入数据](#bcp_widechar_import_fmt)，

* 使用临时表，其中第一列为字符数据类型，或

* 重新导出数据并更改数据字段顺序，以便第一个数据字段为字符。  然后使用格式化文件将数据字段重新映射到表中的实际顺序。  例如，请参阅 [使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)。
  
## <a name="command-options-for-unicode-character-format"></a>Unicode 字符格式的命令选项<a name="command_options"></a>  
可以使用 [bcp](../../tools/bcp-utility.md)、[BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 或 [INSERT ... 将 Unicode 字符格式数据导入表中SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md)。对于 [bcp](../../tools/bcp-utility.md) 命令或 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 语句，你可以在语句中指定数据格式。  对于 [INSERT ...SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 语句，必须在格式化文件中指定数据格式。  
  
下列命令选项支持 Unicode 字符格式：  
  
|Command|选项|说明|  
|-------------|------------|-----------------|  
|bcp|**-w**|使用 Unicode 字符格式。|  
|BULK INSERT|DATAFILETYPE **='widechar'**|批量导入数据时使用 Unicode 字符格式。|  
|OPENROWSET|空值|必须使用格式化文件|
  
> [!NOTE]
>  或者，您可以在格式化文件中为每个字段指定格式设置。 有关详细信息，请参阅 [用来导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)。
  
## <a name="example-test-conditions"></a>示例测试条件<a name="etc"></a>  
本主题中的示例基于下面定义的表和格式化文件。

### <a name="sample-table"></a>**示例表**<a name="sample_table"></a>
以下脚本将创建测试数据库、名为 `myWidechar` 的表并用一些初始值填充表。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myWidechar ( 
    PersonID smallint NOT NULL,
    FirstName nvarchar(25) NOT NULL,
    LastName nvarchar(30) NOT NULL,
    BirthDate date,
    AnnualSalary money
);

-- Populate table
INSERT TestDatabase.dbo.myWidechar
VALUES 
(1, N'ϴAnthony', N'Grosse', '02-23-1980', 65000.00),
(2, N'❤Alica', N'Fatnowna', '11-14-1963', 45000.00),
(3, N'☎Stella', N'Rossenhain', '03-02-1992', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myWidechar;
```

### <a name="sample-non-xml-format-file"></a>**示例非 XML 格式化文件**<a name="nonxml_format_file"></a>
SQL Server 支持两种类型的格式化文件：非 XML 格式和 XML 格式。  非 XML 格式是 SQL Server 早期版本支持的原始格式。  有关详细信息，请查看 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) 。  下面的命令基于 [的架构使用](../../tools/bcp-utility.md) bcp 实用工具 `myWidechar.fmt`生成非 XML 格式化文件 `myWidechar`。  若要使用 [bcp](../../tools/bcp-utility.md) 命令创建格式化文件，请指定 **format** 参数，并使用 **nul** 而不是数据文件路径。  格式化选项还需要 **-f** 选项。  此外，对于本示例，限定符 **c** 用于指定字符数据， **T** 用于指定使用集成安全性的受信任连接。  在命令提示符处输入以下命令：

```
bcp TestDatabase.dbo.myWidechar format nul -f D:\BCP\myWidechar.fmt -T -w

REM Review file
Notepad D:\BCP\myWidechar.fmt
```

> [!IMPORTANT]
> 确保非 XML 格式化文件以回车符/换行符结尾。  否则可能会收到以下错误消息：
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`


## <a name="examples"></a>示例<a name="examples"></a>
下面的示例使用上面创建的数据库和格式化文件。

### <a name="using-bcp-and-unicode-character-format-to-export-data"></a>**使用 bcp 和 Unicode 字符格式导出数据**<a name="bcp_widechar_export"></a>
**-w** 切换和 **OUT** 命令。  请注意：此示例中创建的数据文件将用于所有后续示例中。  在命令提示符处输入以下命令：
```
bcp TestDatabase.dbo.myWidechar OUT D:\BCP\myWidechar.bcp -T -w

REM Review results
NOTEPAD D:\BCP\myWidechar.bcp
```

### <a name="using-bcp-and-unicode-character-format-to-import-data-without-a-format-file"></a>**在不使用格式化文件的情况下使用 bcp 和 Unicode 字符格式导入数据**<a name="bcp_widechar_import"></a>
**-w** 切换和 **IN** 命令。  在命令提示符处输入以下命令：
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidechar;"

REM Import data
bcp TestDatabase.dbo.myWidechar IN D:\BCP\myWidechar.bcp -T -w

REM Review results is SSMS
```

### <a name="using-bcp-and-unicode-character-format-to-import-data-with-a-non-xml-format-file"></a>**在使用非 XML 格式化文件的情况下使用 bcp 和 Unicode 字符格式导入数据**<a name="bcp_widechar_import_fmt"></a>
**-w** 和 **-f** 切换以及 **IN** 命令。  由于此示例讲到 bcp、格式化文件、Unicode 字符以及数据文件中的第一个数据字段为非字符，因此需要使用解决方法。  请参阅上面的 [使用 Unicode 字符格式、bcp 和格式化文件的特殊注意事项](#special_considerations)。  数据文件 `myWidechar.bcp` 将通过添加其他记录为“虚拟”记录来进行更改，然后使用 `-F 2` 开关跳过该记录。

在命令提示符中，输入以下命令，然后按照修改步骤操作：
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidechar;"

REM Open data file
Notepad D:\BCP\myWidechar.bcp
REM Copy first record and then paste as new first record.  This additional record is the "dummy" record.
REM Close file.

REM Import data instructing bcp to skip dummy record with the -F 2 switch.
bcp TestDatabase.dbo.myWidechar IN D:\BCP\myWidechar.bcp -f D:\BCP\myWidechar.fmt -T -F 2

REM Review results is SSMS

REM Return data file to original state for usage in other examples
bcp TestDatabase.dbo.myWidechar OUT D:\BCP\myWidechar.bcp -T -w
```
  
### <a name="using-bulk-insert-and-unicode-character-format-without-a-format-file"></a>**在不使用格式化文件的情况下使用 BULK INSERT 和 Unicode 字符格式**<a name="bulk_widechar"></a>
**DATAFILETYPE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidechar; -- for testing
BULK INSERT TestDatabase.dbo.myWidechar
    FROM 'D:\BCP\myWidechar.bcp'
    WITH (
        DATAFILETYPE = 'widechar'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidechar;
```
  
### <a name="using-bulk-insert-and-unicode-character-format-with-a-non-xml-format-file"></a>**在使用非 XML 格式化文件的情况下使用 BULK INSERT 和 Unicode 字符格式**<a name="bulk_widechar_fmt"></a>
**FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidechar; -- for testing
BULK INSERT TestDatabase.dbo.myWidechar
   FROM 'D:\BCP\myWidechar.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myWidechar.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidechar;
```
  
### <a name="using-openrowset-and-unicode-character-format-with-a-non-xml-format-file"></a>**在使用非 XML 格式化文件的情况下使用 OPENROWSET 和 Unicode 字符格式**<a name="openrowset_widechar_fmt"></a>
**FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidechar;  -- for testing
INSERT INTO TestDatabase.dbo.myWidechar
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myWidechar.bcp', 
        FORMATFILE = 'D:\BCP\myWidechar.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myWidechar;
```
 
  
## <a name="related-tasks"></a>相关任务<a name="RelatedTasks"></a>
使用数据格式进行批量导入或批量导出  
-   [导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [使用字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [使用本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [使用 Unicode 本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  
