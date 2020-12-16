---
title: 在批量导入期间保留 Null 或默认值
description: 为了在 SQL Server 中进行批量导入，bcp 和 BULK INSERT 都加载默认值来替换 null 值。 对于这两者，可以选择保留 null 值。
ms.date: 09/20/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], null values
- bulk importing [SQL Server], default values
- data formats [SQL Server], null values
- bulk rowset providers [SQL Server]
- bcp utility [SQL Server], null values
- BULK INSERT statement
- default values
- OPENROWSET function, bulk importing
- data formats [SQL Server], default values
ms.assetid: 6b91d762-337b-4345-a159-88abb3e64a81
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 18a1e26d8e89f5e9913c3d1d645e98d7c7c988af
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473938"
---
# <a name="keep-nulls-or-default-values-during-bulk-import-sql-server"></a>在批量导入期间保留 Null 或默认值 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

默认情况下，将数据导入表中时， [bcp](../../tools/bcp-utility.md) 命令和 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 语句将使用为表中的列定义的所有默认值。  例如，如果数据文件中包含一个空字段，则会加载该列的默认值。  [bcp](../../tools/bcp-utility.md) 命令和 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 语句都允许指定保留 NULL 值。

相反，常规 INSERT 语句会保留空值而不会插入默认值。 INSERT ... SELECT * [FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 语句的基本行为与常规 INSERT 相同，但前者还支持插入默认值的[表提示](../../t-sql/queries/hints-transact-sql-table.md)。

|轮廓|
|---|
|[保留 Null 值](#keep_nulls)<br />[对 INSERT 使用默认值...SELECT * FROM OPENROWSET(BULK...)](#keep_default)<br />[示例测试条件](#etc)<br />&emsp;&#9679;&emsp;[示例表](#sample_table)<br />&emsp;&#9679;&emsp;[示例数据文件](#sample_data_file)<br />&emsp;&#9679;&emsp;[示例非 XML 格式化文件](#nonxml_format_file)<br />[在批量导入期间保留 Null 或使用默认值](#import_data)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 bcp 并保留 NULL 值](#bcp_null)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 bcp 并保留 Null 值](#bcp_null_fmt)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 bcp 和默认值](#bcp_default)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 bcp 和默认值](#bcp_default_fmt)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 BULK INSERT 并保留 Null 值](#bulk_null)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 BULK INSERT 并保留 Null 值](#bulk_null_fmt)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 BULK INSERT 和默认值](#bulk_default)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 BULK INSERT 和默认值](#bulk_default_fmt)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 OPENROWSET(BULK...) 并保留 Null 值](#openrowset__null_fmt)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 OPENROWSET(BULK...) 和默认值](#openrowset__default_fmt)

## <a name="keeping-null-values"></a>保留 Null 值<a name="keep_nulls"></a>  
下列限定符指定在大容量导入操作期间数据文件中的空字段保留其空值，而不继承表列的默认值（如果存在）。  对于 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md)，默认情况下，未在批量加载操作中指定的所有列都会设置为 NULL。
  
|Command|Qualifier|限定符类型|  
|-------------|---------------|--------------------|  
|bcp|-k|开关|  
|BULK INSERT|KEEPNULLS\*|参数|  
|INSERT ... SELECT * FROM OPENROWSET(BULK...)|空值|空值|  
  
\* 对于 [ BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md)，如果默认值不可用，则必须将表列定义为允许 NULL 值。 
  
> [!NOTE]
> 这些限定符通过这些大容量导入命令禁止检查表上的 DEFAULT 定义。  然而，对于任何并发 INSERT 语句，都需要 DEFAULT 定义。
 
## <a name="using-default-values-with-insert--select--from-openrowsetbulk"></a>对 INSERT 使用默认值...SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md)<a name="keep_default"></a>  
对于数据文件中的空字段，相应的表列使用其默认值（如果存在）。  若要使用默认值，请使用表提示 [KEEPDEFAULTS](../../t-sql/queries/hints-transact-sql-table.md)。
 
> [!NOTE]
>  有关详细信息，请参阅 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)、[SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)、[OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md) 和[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)

## <a name="example-test-conditions"></a>示例测试条件<a name="etc"></a>  
本主题中的示例基于下面定义的表、数据文件和格式化文件。

### <a name="sample-table"></a>**示例表**<a name="sample_table"></a>
下面的脚本创建一个测试数据库和一个名为 `myNulls`的表。  请注意，第四个表列 ( `Kids`) 具有默认值。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myNulls ( 
   PersonID smallint not null,
   FirstName varchar(25),
   LastName varchar(30),
   Kids varchar(13) DEFAULT 'Default Value',
   BirthDate date
   );
```

### <a name="sample-data-file"></a>**示例数据文件**<a name="sample_data_file"></a>
使用记事本创建一个空文件 `D:\BCP\myNulls.bcp` ，并插入下面的数据。  请注意，在第三条记录（第四列）中没有任何值。

```
1,Anthony,Grosse,Yes,1980-02-23
2,Alica,Fatnowna,No,1963-11-14
3,Stella,Rosenhain,,1992-03-02
```

还可通过执行以下 PowerShell 脚本创建和填充数据文件：

```powershell
cls
# revise directory as desired
$dir = 'D:\BCP\';

$bcpFile = $dir + 'MyNulls.bcp';

# Confirm directory exists
IF ((Test-Path -Path $dir) -eq 0)
{
    Write-Host "The path $dir does not exist; please create or modify the directory.";
    RETURN;
};

# clear content, will error if file does not exist, can be ignored
Clear-Content -Path $bcpFile -ErrorAction SilentlyContinue;

# Add data
Add-Content -Path $bcpFile -Value '1,Anthony,Grosse,Yes,1980-02-23';
Add-Content -Path $bcpFile -Value '2,Alica,Fatnowna,No,1963-11-14';
Add-Content -Path $bcpFile -Value '3,Stella,Rosenhain,,1992-03-02';

#Review content
Get-Content -Path $bcpFile;
Invoke-Item $bcpFile;
```
  
### <a name="sample-non-xml-format-file"></a>**示例非 XML 格式化文件**<a name="nonxml_format_file"></a>
SQL Server 支持两种类型的格式化文件：非 XML 格式和 XML 格式。  非 XML 格式是 SQL Server 早期版本支持的原始格式。  有关详细信息，请查看 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) 。  下面的命令基于 [的架构使用](../../tools/bcp-utility.md) bcp 实用工具 `myNulls.fmt`生成非 XML 格式化文件 `myNulls`。  若要使用 [bcp](../../tools/bcp-utility.md) 命令创建格式化文件，请指定 **format** 参数，并使用 **nul** 而不是数据文件路径。  格式化选项还需要 **-f** 选项。  此外，对于本示例，限定符 **c** 用于指定字符数据， **t,** 用于将逗号指定为 [字段终止符](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)，而 **T** 用于指定使用集成安全性的受信任连接。  在命令提示符处输入以下命令：

```cmd
bcp TestDatabase.dbo.myNulls format nul -c -f D:\BCP\myNulls.fmt -t, -T

REM Review file
Notepad D:\BCP\myNulls.fmt
```

> [!IMPORTANT]
> 确保非 XML 格式化文件以回车符/换行符结尾。  否则可能会收到以下错误消息：
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

 有关创建格式化文件的详细信息，请参阅 [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)。  
  
## <a name="keep-nulls-or-use-default-values-during-bulk-import"></a>在大容量导入期间保留 Null 或使用默认值<a name="import_data"></a>
下面的示例使用上面创建的数据库、数据文件和格式化文件。

### <a name="using-bcp-and-keeping-null-values-without-a-format-file"></a>**在不使用格式化文件的情况下使用 [bcp](../../tools/bcp-utility.md) 并保留 NULL 值**<a name="bcp_null"></a>

**-k** 开关。  在命令提示符处输入以下命令：

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -c -t, -T -k

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```
  
### <a name="using-bcp-and-keeping-null-values-with-a-non-xml-format-file"></a>**在不使用格式化文件的情况下使用 [bcp](../../tools/bcp-utility.md) 的情况下使用 [bcp](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bcp_null_fmt"></a>
**-k** 和 **-f** 开关。 在命令提示符处输入以下命令：

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -f D:\BCP\myNulls.fmt -T -k

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```

### <a name="using-bcp-and-using-default-values-without-a-format-file"></a>**在不使用格式化文件的情况下使用 [bcp](../../tools/bcp-utility.md) 和默认值**<a name="bcp_default"></a>
在命令提示符处输入以下命令：

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -c -t, -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```
  
### <a name="using-bcp-and-using-default-values-with-a-non-xml-format-file"></a>**在不使用格式化文件的情况下使用 [bcp](../../tools/bcp-utility.md) 的情况下使用 [bcp](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bcp_default_fmt"></a>
**-f** 开关。  在命令提示符处输入以下命令：

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -f D:\BCP\myNulls.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```

### <a name="using-bulk-insert-and-keeping-null-values-without-a-format-file"></a>**在不使用格式化文件的情况下使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 并保留 Null 值**<a name="bulk_null"></a>
**KEEPNULLS** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
USE TestDatabase;
GO
TRUNCATE TABLE dbo.myNulls; -- for testing
BULK INSERT dbo.myNulls
    FROM 'D:\BCP\myNulls.bcp'
    WITH (
        DATAFILETYPE = 'char',  
        FIELDTERMINATOR = ',',  
        KEEPNULLS
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-bulk-insert-and-keeping-null-values-with-a-non-xml-format-file"></a>**在不使用格式化文件的情况下使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 的情况下使用 [bcp](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bulk_null_fmt"></a>
**KEEPNULLS** 和 **FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls; -- for testing
BULK INSERT dbo.myNulls
   FROM 'D:\BCP\myNulls.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNulls.fmt',
        KEEPNULLS
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-bulk-insert-and-using-default-values-without-a-format-file"></a>**在不使用格式化文件的情况下使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 和默认值**<a name="bulk_default"></a>
在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
BULK INSERT dbo.myNulls
   FROM 'D:\BCP\myNulls.bcp'
   WITH (
      DATAFILETYPE = 'char',  
      FIELDTERMINATOR = ','
      );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-bulk-insert-and-using-default-values-with-a-non-xml-format-file"></a>**在不使用格式化文件的情况下使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 的情况下使用 [bcp](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bulk_default_fmt"></a>
**FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
BULK INSERT dbo.myNulls
   FROM 'D:\BCP\myNulls.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNulls.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-openrowsetbulk-and-keeping-null-values-with-a-non-xml-format-file"></a>**在不使用格式化文件的情况下使用 [FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 的情况下使用 [bcp](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="openrowset__null_fmt"></a>
**FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
INSERT INTO dbo.myNulls
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNulls.bcp', 
        FORMATFILE = 'D:\BCP\myNulls.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-openrowsetbulk-and-using-default-values-with-a-non-xml-format-file"></a>**在不使用格式化文件的情况下使用 [FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 的情况下使用 [bcp](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="openrowset__default_fmt"></a>
**KEEPDEFAULTS** 表提示和 **FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
INSERT INTO dbo.myNulls
WITH (KEEPDEFAULTS) 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNulls.bcp', 
        FORMATFILE = 'D:\BCP\myNulls.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [批量导入数据时保留标识值 (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)  
  
-   [准备用于批量导出或导入的数据 (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)  
  
 **使用格式化文件**  
  
-   [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
  
-   [使用格式化文件批量导入数据 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)  
  
-   [使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
-   [使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
 **使用数据格式进行大容量导入或大容量导出**  
  
-   [导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [使用字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [使用本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [使用 Unicode 字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [使用 Unicode 本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
 **在使用 bcp 时指定数据格式以获得兼容性**  
  
-   [指定字段终止符和行终止符 (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)  
  
-   [使用 bcp 指定数据文件中的前缀长度 (SQL Server)](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)  
  
-   [使用 bcp 指定文件存储类型 (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)  
  
  
