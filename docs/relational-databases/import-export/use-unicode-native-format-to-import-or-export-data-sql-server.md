---
title: 使用 Unicode 本机格式导入或导出数据 (SQL Server) | Microsoft Docs
description: 使用 Unicode 本机格式在 SQL Server 实例之间批量传输数据，而无需在数据类型与字符格式之间进行转换。
ms.custom: ''
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- Unicode [SQL Server], bulk importing and exporting
- data formats [SQL Server], Unicode native
ms.assetid: a6213308-f3d5-406e-9029-19d8bb3367f3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 49e3681e448e0fb262e3ea1c52650106f15687e3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465488"
---
# <a name="use-unicode-native-format-to-import-or-export-data-sql-server"></a>使用 Unicode 本机格式导入或导出数据 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
当必须将信息从一个 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装复制到另一个时，Unicode 本机格式非常有用。 为非字符数据使用本机格式可以节省时间，并消除与字符格式之间不必要的数据类型转换。 在使用不同代码页的服务器之间大容量传输数据时，为所有字符数据使用 Unicode 字符格式可以防止丢失任何扩展字符。 可以通过任何批量导入方法读取 Unicode 本机格式的数据文件。  
  
 通过使用包含扩展字符或 DBCS 字符的数据文件在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的多个实例之间大容量传输数据时，建议使用 Unicode 本机格式。 对于非字符数据，Unicode 本机格式使用本机（数据库）数据类型。 对于字符数据（如 [char](../../t-sql/data-types/char-and-varchar-transact-sql.md)、 [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)、 [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md)、 [nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)、 [text](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)、 [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)、 [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)和 [ntext](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)），Unicode 本机格式使用 Unicode 字符数据格式。  
  
 在 Unicode 本机格式数据文件中存储为 SQLVARIANT 的 [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) 数据的运算方式与在本机格式数据文件中的运算方式相同，只是 [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) 和 [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) 值需转换为 [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) 和 [nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)，这使得受影响列所需的存储量加倍。 这些值的原始元数据被保留，当批量导入到表列时，这些值将转换回其原始的 [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) 和 [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) 数据类型。  
 
 |本主题内容：|
|---|
|[Unicode 本机格式的命令选项](#command_options)|
|[示例测试条件](#etc)<br />&emsp;&#9679;&emsp;[示例表](#sample_table)<br />&emsp;&#9679;&emsp;[示例非 XML 格式化文件](#nonxml_format_file)|
|[示例](#examples)<br />&emsp;&#9679;&emsp;[使用 bcp 和 Unicode 本机格式导出数据](#bcp_widenative_export)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 bcp 和 Unicode 本机格式导入数据](#bcp_widenative_import)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 bcp 和 Unicode 本机格式导入数据](#bcp_widenative_import_fmt)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 BULK INSERT 和 Unicode 本机格式](#bulk_widenative)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 BULK INSERT 和 Unicode 本机格式](#bulk_widenative_fmt)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 OPENROWSET 和 Unicode 本机格式](#openrowset_widenative_fmt)|
|[相关任务](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|
  
## <a name="command-options-for-unicode-native-format"></a>Unicode 本机格式的命令选项<a name="command_options"></a>  
可以使用 [bcp](../../tools/bcp-utility.md)、[BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 或 [INSERT ... 将 Unicode 本机格式数据导入表中SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md)。对于 [bcp](../../tools/bcp-utility.md) 命令或 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 语句，你可以在语句中指定数据格式。  对于 [INSERT ...SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 语句，必须在格式化文件中指定数据格式。  
  
下列命令选项支持 Unicode 本机格式：  
  
|Command|选项|说明|  
|-------------|------------|-----------------|  
|bcp|**-N**|使 **bcp** 实用工具使用 Unicode 本机格式，将为所有非字符数据使用本机（数据库）数据类型，为所有字符（**char**、 **nchar**、 **varchar**、 **nvarchar**、 **text** 和 **ntext**）数据使用 Unicode 字符数据格式。|  
|BULK INSERT|DATAFILETYPE **='widenative'**|批量导入数据时使用 Unicode 本机格式。|  
|OPENROWSET|空值|必须使用格式化文件|
    
> [!NOTE]
>  或者，您可以在格式化文件中为每个字段指定格式设置。 有关详细信息，请参阅 [用来导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)。
  
## <a name="example-test-conditions"></a>示例测试条件<a name="etc"></a>  
本主题中的示例基于下面定义的表和格式化文件。

### <a name="sample-table"></a>**示例表**<a name="sample_table"></a>
以下脚本将创建测试数据库、名为 `myWidenative` 的表并用一些初始值填充表。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myWidenative ( 
    PersonID smallint NOT NULL,
    FirstName nvarchar(25) NOT NULL,
    LastName nvarchar(30) NOT NULL,
    BirthDate date,
    AnnualSalary money
);

-- Populate table
INSERT TestDatabase.dbo.myWidenative
VALUES 
(1, N'ϴAnthony', N'Grosse', '02-23-1980', 65000.00),
(2, N'❤Alica', N'Fatnowna', '11-14-1963', 45000.00),
(3, N'☎Stella', N'Rossenhain', '03-02-1992', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="sample-non-xml-format-file"></a>**示例非 XML 格式化文件**<a name="nonxml_format_file"></a>
SQL Server 支持两种类型的格式化文件：非 XML 格式和 XML 格式。  非 XML 格式是 SQL Server 早期版本支持的原始格式。  有关详细信息，请查看 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) 。  下面的命令基于 [的架构使用](../../tools/bcp-utility.md) bcp 实用工具 `myWidenative.fmt`生成非 XML 格式化文件 `myWidenative`。  若要使用 [bcp](../../tools/bcp-utility.md) 命令创建格式化文件，请指定 **format** 参数，并使用 **nul** 而不是数据文件路径。  格式化选项还需要 **-f** 选项。  此外，对于本示例，限定符 **c** 用于指定字符数据， **T** 用于指定使用集成安全性的受信任连接。  在命令提示符处输入以下命令：

```
bcp TestDatabase.dbo.myWidenative format nul -f D:\BCP\myWidenative.fmt -T -N

REM Review file
Notepad D:\BCP\myWidenative.fmt
```

> [!IMPORTANT]
> 确保非 XML 格式化文件以回车符/换行符结尾。  否则可能会收到以下错误消息：
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>示例<a name="examples"></a>
下面的示例使用上面创建的数据库和格式化文件。

### <a name="using-bcp-and-unicode-native-format-to-export-data"></a>**使用 bcp 和 Unicode 本机格式导出数据**<a name="bcp_widenative_export"></a>
**-N** 切换和 **OUT** 命令。  请注意：此示例中创建的数据文件将用于所有后续示例中。  在命令提示符处输入以下命令：
```
bcp TestDatabase.dbo.myWidenative OUT D:\BCP\myWidenative.bcp -T -N

REM Review results
NOTEPAD D:\BCP\myWidenative.bcp
```

### <a name="using-bcp-and-unicode-native-format-to-import-data-without-a-format-file"></a>**在不使用格式化文件的情况下使用 bcp 和 Unicode 本机格式导入数据**<a name="bcp_widenative_import"></a>
**-N** 切换和 **IN** 命令。  在命令提示符处输入以下命令：
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidenative;"

REM Import data
bcp TestDatabase.dbo.myWidenative IN D:\BCP\myWidenative.bcp -T -N

REM Review results is SSMS
```

### <a name="using-bcp-and-unicode-native-format-to-import-data-with-a-non-xml-format-file"></a>**在使用非 XML 格式化文件的情况下使用 bcp 和 Unicode 本机格式导入数据**<a name="bcp_widenative_import_fmt"></a>
**-N** 和 **-f** 切换以及 **IN** 命令。  在命令提示符处输入以下命令：
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidenative;"

REM Import data
bcp TestDatabase.dbo.myWidenative IN D:\BCP\myWidenative.bcp -f D:\BCP\myWidenative.fmt -T

REM Review results is SSMS
```

### <a name="using-bulk-insert-and-unicode-native-format-without-a-format-file"></a>**在不使用格式化文件的情况下使用 BULK INSERT 和 Unicode 本机格式**<a name="bulk_widenative"></a>
**DATAFILETYPE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative; -- for testing
BULK INSERT TestDatabase.dbo.myWidenative
    FROM 'D:\BCP\myWidenative.bcp'
    WITH (
        DATAFILETYPE = 'widenative'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="using-bulk-insert-and-unicode-native-format-with-a-non-xml-format-file"></a>**在使用非 XML 格式化文件的情况下使用 BULK INSERT 和 Unicode 本机格式**<a name="bulk_widenative_fmt"></a>
**FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative; -- for testing
BULK INSERT TestDatabase.dbo.myWidenative
   FROM 'D:\BCP\myWidenative.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myWidenative.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="using-openrowset-and-unicode-native-format-with-a-non-xml-format-file"></a>**在使用非 XML 格式化文件的情况下使用 OPENROWSET 和 Unicode 本机格式**<a name="openrowset_widenative_fmt"></a>
**FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative;  -- for testing
INSERT INTO TestDatabase.dbo.myWidenative
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myWidenative.bcp', 
        FORMATFILE = 'D:\BCP\myWidenative.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

## <a name="related-tasks"></a>相关任务<a name="RelatedTasks"></a>
使用数据格式进行批量导入或批量导出  
-   [导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [使用字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [使用本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [使用 Unicode 字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
  
  
