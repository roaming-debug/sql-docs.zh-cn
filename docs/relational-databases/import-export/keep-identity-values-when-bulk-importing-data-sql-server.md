---
title: 批量导入数据时保留标识值
description: 当将包含标识符值的数据批量导入到 SQL Server 实例时，默认情况下，它将分配新值。 可以选择保留原始值。
ms.date: 09/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- identity values [SQL Server], bulk imports
- data formats [SQL Server], identity values
- bulk importing [SQL Server], identity values
ms.assetid: 45894a3f-2d8a-4edd-9568-afa7d0d3061f
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: d523fcf6649b51432b994e6c941169616bb123a8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473918"
---
# <a name="keep-identity-values-when-bulk-importing-data-sql-server"></a>大容量导入数据时保留标识值 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
可以将包含标识值的数据文件批量导入到 Microsoft SQL Server 的实例中。  默认情况下，将忽略导入的数据文件中标识列的值， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动分配唯一值。  这些唯一值基于在表创建期间指定的种子和增量值。

如果该数据文件表中的标识符列不包含值，则使用格式化文件来指定导入数据时应跳过表中的标识符列。  有关详细信息，请参阅 [使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md) 。

|轮廓|
|---|
|[保留标识值](#keep_identity)<br />[示例测试条件](#etc)<br />&emsp;&#9679;&emsp;[示例表](#sample_table)<br />&emsp;&#9679;&emsp;[示例数据文件](#sample_data_file)<br />&emsp;&#9679;&emsp;[示例非 XML 格式化文件](#nonxml_format_file)<br />[示例](#examples)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 bcp 并保留标识值](#bcp_identity)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 bcp 并保留标识值](#bcp_identity_fmt)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 bcp 和生成的标识值](#bcp_default)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 bcp 和生成的标识值](#bcp_default_fmt)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 BULK INSERT 并保留标识值](#bulk_identity)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 BULK INSERT 并保留标识值](#bulk_identity_fmt)<br />&emsp;&#9679;&emsp;[在不使用格式化文件的情况下使用 BULK INSERT 和生成的标识值](#bulk_default)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 BULK INSERT 和生成的标识值](#bulk_default_fmt)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 OPENROWSET 并保留标识值](#openrowset_identity_fmt)<br />&emsp;&#9679;&emsp;[在使用非 XML 格式化文件的情况下使用 OPENROWSET 和生成的标识值](#openrowset_default_fmt)<br /><p>                                                                                                                                                                                                                  </p>|

## <a name="keep-identity-values"></a>保留标识值 <a name="keep_identity"></a>  
若要防止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在将数据行大容量导入到表中时分配标识值，请使用相应的保留标识命令限定符。  在您指定保留标识限定符后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在该数据文件中使用标识值。  这些限定符如下：

|Command|保留标识限定符|限定符类型|  
|-------------|------------------------------|--------------------|  
|bcp|-E|开关|  
|BULK INSERT|KEEPIDENTITY|参数|  
|INSERT ... SELECT * FROM OPENROWSET(BULK...)|KEEPIDENTITY|表提示|  
   
 有关详细信息，请参阅 [bcp 实用工具](../../tools/bcp-utility.md)、[BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)、[OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)、[INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)、[SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md) 和[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。  

> [!NOTE]
>  要创建一个可在多个表中使用的自动递增数字或者可以从应用程序中调用而不引用任何表的自动递增数字，请参阅[序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)。
 
## <a name="example-test-conditions"></a>示例测试条件<a name="etc"></a>  
本主题中的示例基于下面定义的表、数据文件和格式化文件。

### <a name="sample-table"></a>**示例表**<a name="sample_table"></a>
下面的脚本创建一个测试数据库和一个名为 `myIdentity`的表。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myIdentity ( 
   PersonID smallint IDENTITY(1,1) NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date
   );
```
 
### <a name="sample-data-file"></a>**示例数据文件**<a name="sample_data_file"></a>
使用记事本创建一个空文件 `D:\BCP\myIdentity.bcp` ，并插入下面的数据。  
```
3,Anthony,Grosse,1980-02-23
2,Alica,Fatnowna,1963-11-14
1,Stella,Rosenhain,1992-03-02
4,Miller,Dylan,1954-01-05
```
  
还可通过执行以下 PowerShell 脚本创建和填充数据文件：
```powershell
cls
# revise directory as desired
$dir = 'D:\BCP\';

$bcpFile = $dir + 'myIdentity.bcp';

# Confirm directory exists
IF ((Test-Path -Path $dir) -eq 0)
{
    Write-Host "The path $dir does not exist; please create or modify the directory.";
    RETURN;
};

# clear content, will error if file does not exist, can be ignored
Clear-Content -Path $bcpFile -ErrorAction SilentlyContinue;

# Add data
Add-Content -Path $bcpFile -Value '3,Anthony,Grosse,1980-02-23';
Add-Content -Path $bcpFile -Value '2,Alica,Fatnowna,1963-11-14';
Add-Content -Path $bcpFile -Value '1,Stella,Rosenhain,1992-03-02';
Add-Content -Path $bcpFile -Value '4,Miller,Dylan,1954-01-05';

#Review content
Get-Content -Path $bcpFile;
Invoke-Item $bcpFile;
```

### <a name="sample-non-xml-format-file"></a>**示例非 XML 格式化文件**<a name="nonxml_format_file"></a>
SQL Server 支持两种类型的格式化文件：非 XML 格式和 XML 格式。  非 XML 格式是 SQL Server 早期版本支持的原始格式。  有关详细信息，请查看 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) 。  下面的命令基于 [的架构使用](../../tools/bcp-utility.md) bcp 实用工具 `myIdentity.fmt`生成非 XML 格式化文件 `myIdentity`。  若要使用 [bcp](../../tools/bcp-utility.md) 命令创建格式化文件，请指定 **format** 参数，并使用 **nul** 而不是数据文件路径。  格式化选项还需要 **-f** 选项。  此外，对于本示例，限定符 **c** 用于指定字符数据， **t,** 用于将逗号指定为 [字段终止符](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)，而 **T** 用于指定使用集成安全性的受信任连接。  在命令提示符处输入以下命令：
  
```
bcp TestDatabase.dbo.myIdentity format nul -c -f D:\BCP\myIdentity.fmt -t, -T

REM Review file
Notepad D:\BCP\myIdentity.fmt
```
 
> [!IMPORTANT]
> 确保非 XML 格式化文件以回车符/换行符结尾。  否则可能会收到以下错误消息：
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>示例<a name="examples"></a>
下面的示例使用上面创建的数据库、数据文件和格式化文件。
  
### <a name="using-bcp-and-keeping-identity-values-without-a-format-file"></a>**在不使用格式化文件的情况下使用 [bcp](../../tools/bcp-utility.md) 并保留标识值**<a name="bcp_identity"></a>
**-E** 开关。  在命令提示符处输入以下命令：
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -T -c -t, -E

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```

### <a name="using-bcp-and-keeping-identity-values-with-a-non-xml-format-file"></a>**Using [bcp](../../tools/bcp-utility.md) and Keeping Identity Values with a [Non-XML Format File](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bcp_identity_fmt"></a>
**-E** 和 **-f** 开关。  在命令提示符处输入以下命令：
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -f D:\BCP\myIdentity.fmt -T -E

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```
 
### <a name="using-bcp-and-generated-identity-values-without-a-format-file"></a>**在不使用格式化文件的情况下使用 [bcp](../../tools/bcp-utility.md) 和生成的标识值**<a name="bcp_default"></a>
使用默认值。  在命令提示符处输入以下命令：
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -T -c -t,

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```
  
### <a name="using-bcp-and-generated-identity-values-with-a-non-xml-format-file"></a>**Using [bcp](../../tools/bcp-utility.md) and Generated Identity Values with a [Non-XML Format File](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bcp_default_fmt"></a>
使用默认值和 **-f** 开关。  在命令提示符处输入以下命令：
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -f D:\BCP\myIdentity.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```
  
### <a name="using-bulk-insert-and-keeping-identity-values-without-a-format-file"></a>**在不使用格式化文件的情况下使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 并保留标识值**<a name="bulk_identity"></a>
**KEEPIDENTITY** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity; -- for testing
BULK INSERT dbo.myIdentity
    FROM 'D:\BCP\myIdentity.bcp'
    WITH (
        DATAFILETYPE = 'char',  
        FIELDTERMINATOR = ',',  
        KEEPIDENTITY
        );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### <a name="using-bulk-insert-and-keeping-identity-values-with-a-non-xml-format-file"></a>**Using [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) and Keeping Identity Values with a [Non-XML Format File](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bulk_identity_fmt"></a>
**KEEPIDENTITY** 和 **FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity; -- for testing
BULK INSERT dbo.myIdentity
   FROM 'D:\BCP\myIdentity.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myIdentity.fmt',
        KEEPIDENTITY
        );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### <a name="using-bulk-insert-and-generated-identity-values-without-a-format-file"></a>**在不使用格式化文件的情况下使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 和生成的标识值**<a name="bulk_default"></a>
使用默认值。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
BULK INSERT dbo.myIdentity
   FROM 'D:\BCP\myIdentity.bcp'
   WITH (
      DATAFILETYPE = 'char',  
      FIELDTERMINATOR = ','
      );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### <a name="using-bulk-insert-and-generated-identity-values-with-a-non-xml-format-file"></a>**Using [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) and Generated Identity Values with a [Non-XML Format File](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bulk_default_fmt"></a>
使用默认值和 **FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
BULK INSERT dbo.myIdentity
   FROM 'D:\BCP\myIdentity.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myIdentity.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### <a name="using-openrowsetbulk-and-keeping-identity-values-with-a-non-xml-format-file"></a>**Using [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) and Keeping Identity Values with a [Non-XML Format File](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="openrowset_identity_fmt"></a>
**KEEPIDENTITY** 表提示和 **FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
INSERT INTO dbo.myIdentity
WITH (KEEPIDENTITY) 
(PersonID, FirstName, LastName, BirthDate)
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myIdentity.bcp', 
        FORMATFILE = 'D:\BCP\myIdentity.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
 
### <a name="using-openrowsetbulk-and-generated-identity-values-with-a-non-xml-format-file"></a>**Using [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) and Generated Identity Values with a [Non-XML Format File](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="openrowset_default_fmt"></a>
使用默认值和 **FORMATFILE** 参数。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
INSERT INTO dbo.myIdentity
(FirstName, LastName, BirthDate)
    SELECT FirstName, LastName, BirthDate
    FROM OPENROWSET (
        BULK 'D:\BCP\myIdentity.bcp', 
        FORMATFILE = 'D:\BCP\myIdentity.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [在批量导入期间保留 Null 或使用默认值 (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)  
  
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
  
1.  [指定字段终止符和行终止符 (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)  
  
2.  [使用 bcp 指定数据文件中的前缀长度 (SQL Server)](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)  
  
3.  [使用 bcp 指定文件存储类型 (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)  
[用来导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)  
  
