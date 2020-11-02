---
title: 使用格式化文件批量导入数据
description: 在 SQL Server 中，可在批量导入操作中使用格式化文件。 格式化文件可将数据文件的各字段映射到表的各列。
ms.date: 09/20/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], format files
- format files [SQL Server], importing data using
ms.assetid: 2956df78-833f-45fa-8a10-41d6522562b9
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: b893b6dc0c3ad11225d1144f757f206fcf5500d5
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92678975"
---
# <a name="use-a-format-file-to-bulk-import-data-sql-server"></a>使用格式化文件批量导入数据 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

本主题说明如何在大容量导入操作中使用格式化文件。  格式化文件可将数据文件的各字段映射到表的各列。  有关其他信息，请查看 [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md) 。
  
## <a name="before-you-begin"></a>开始之前
* 对于用于 Unicode 字符数据文件的格式化文件，所有输入字段必须为 Unicode 文本字符串（即固定大小 Unicode 字符串或字符终止 Unicode 字符串）。
* 若要批量导出或导入 [SQLXML](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md) 数据，请在格式化文件中使用下列数据类型之一：
  * SQLCHAR 或 SQLVARCHAR（在客户端代码页或排序规则隐含的代码页中发送数据）
  * SQLNCHAR 或 SQLNVARCHAR（以 Unicode 格式发送数据）
  * SQLBINARY 或 SQLVARBIN（不经任何转换即发送数据）。
* Azure SQL 数据库和 Azure Synapse Analytics 仅支持 [bcp](../../tools/bcp-utility.md)。  有关其他信息，请参阅：
  * [将数据加载到 Azure Synapse Analytics 中](/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading)
  * [将数据从 SQL Server 载入 Azure Synapse Analytics（平面文件）](/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading)
  * [迁移数据](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-develop)

## <a name="example-test-conditions"></a>示例测试条件
本主题中格式化文件示例基于下面定义的表和数据文件。

### <a name="sample-table"></a>示例表
下面的脚本创建一个测试数据库和一个名为 `myFirstImport`的表。  在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.MyFirstImport (
   PersonID smallint,
   FirstName varchar(25),
   LastName varchar(30),
   BirthDate Date
   );
```

### <a name="sample-data-file"></a>示例数据文件
使用记事本创建一个空文件 `D:\BCP\myFirstImport.bcp` 并插入以下数据：
```
1,Anthony,Grosse,1980-02-23
2,Alica,Fatnowna,1963-11-14
3,Stella,Rosenhain,1992-03-02
```

还可通过执行以下 PowerShell 脚本创建和填充数据文件：
```powershell
Clear-Host
# revise directory as desired
$dir = 'D:\BCP\';

$bcpFile = Join-Path -Path $dir -ChildPath 'MyFirstImport.bcp';

# Confirm directory exists
IF ((Test-Path -Path $dir) -eq 0)
{
    Write-Host "The path $dir does not exist; please create or modify the directory.";
    RETURN;
};

# clear content, will error if file does not exist, can be ignored
Clear-Content -Path $bcpFile -ErrorAction SilentlyContinue;

# Add data
Add-Content -Path $bcpFile -Value '1,Anthony,Grosse,1980-02-23';
Add-Content -Path $bcpFile -Value '2,Alica,Fatnowna,1963-11-14';
Add-Content -Path $bcpFile -Value '3,Stella,Rosenhain,1992-03-02';

#Review content
Get-Content -Path $bcpFile;
Notepad.exe $bcpfile;
```

## <a name="creating-the-format-files"></a>创建格式化文件
SQL Server 支持两种类型的格式化文件：非 XML 格式和 XML 格式。  非 XML 格式是 SQL Server 早期版本支持的原始格式。

### <a name="creating-a-non-xml-format-file"></a>创建非 XML 格式化文件
有关详细信息，请查看 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) 。  下面的命令基于 [的架构使用](../../tools/bcp-utility.md) bcp 实用工具 `myFirstImport.fmt`生成非 XML 格式化文件 `myFirstImport`。  若要使用 bcp 命令创建格式化文件，请指定 **format** 参数，并使用 **nul** 而不是数据文件路径。  格式化选项还需要 **-f** 选项。  此外，对于本示例，限定符 **c** 用于指定字符数据， **t,** 用于将逗号指定为 [字段终止符](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)，而 **T** 用于指定使用集成安全性的受信任连接。  在命令提示符处输入以下命令：

```cmd
bcp TestDatabase.dbo.myFirstImport format nul -c -f D:\BCP\myFirstImport.fmt -t, -T

REM Review file
Notepad D:\BCP\myFirstImport.fmt
```

非 XML 格式化文件 `D:\BCP\myFirstImport.fmt` 应如下所示：
```
13.0
4
1       SQLCHAR             0       7       ","      1     PersonID               ""
2       SQLCHAR             0       25      ","      2     FirstName              SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR             0       30      ","      3     LastName               SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR             0       11      "\r\n"   4     BirthDate              ""
```

> [!IMPORTANT]
> 确保非 XML 格式化文件以回车符/换行符结尾。  否则可能会收到以下错误消息：
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

### <a name="creating-an-xml-format-file"></a>创建 XML 格式化文件
有关详细信息，请查看 [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md) 。  下面的命令使用 [bcp 实用工具](../../tools/bcp-utility.md) 基于 `myFirstImport.xml`的架构创建 xml 格式化文件 `myFirstImport`。 若要使用 bcp 命令创建格式化文件，请指定 **format** 参数，并使用 **nul** 而不是数据文件路径。  format 选项始终要求 **-f** 选项，若要创建 XML 格式化文件，还必须指定 **-x** 选项。  此外，对于本示例，限定符 **c** 用于指定字符数据， **t,** 用于将逗号指定为 [字段终止符](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)，而 **T** 用于指定使用集成安全性的受信任连接。  在命令提示符处输入以下命令：
```cmd
bcp TestDatabase.dbo.myFirstImport format nul -c -x -f D:\BCP\myFirstImport.xml -t, -T

REM Review file
Notepad D:\BCP\myFirstImport.xml
```

XML 格式化文件 `D:\BCP\myFirstImport.xml` 应如下所示：
```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="http://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="11"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="2" NAME="FirstName" xsi:type="SQLVARCHAR"/>
  <COLUMN SOURCE="3" NAME="LastName" xsi:type="SQLVARCHAR"/>
  <COLUMN SOURCE="4" NAME="BirthDate" xsi:type="SQLDATE"/>
 </ROW>
</BCPFORMAT>
```

## <a name="using-a-format-file-to-bulk-import-data"></a>使用格式化文件批量导入数据
下面的示例使用上面创建的数据库、数据文件和格式化文件。

### <a name="using-bcp-and-non-xml-format-file"></a>使用 [bcp](../../tools/bcp-utility.md) 和[非 XML 格式化文件](../../relational-databases/import-export/non-xml-format-files-sql-server.md)
在命令提示符处输入以下命令：
```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.MyFirstImport;"

REM Import data
bcp TestDatabase.dbo.myFirstImport IN D:\BCP\myFirstImport.bcp -f D:\BCP\myFirstImport.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.MyFirstImport"
```


### <a name="using-bcp-and-xml-format-file"></a>使用 [bcp](../../tools/bcp-utility.md) 和 [XML 格式化文件](../../relational-databases/import-export/xml-format-files-sql-server.md)
在命令提示符处输入以下命令：
```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.MyFirstImport;"

REM Import data
bcp TestDatabase.dbo.myFirstImport IN D:\BCP\myFirstImport.bcp -f D:\BCP\myFirstImport.xml -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.MyFirstImport;"
```


### <a name="using-bulk-insert-and-non-xml-format-file"></a>使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 和[非 XML 格式化文件](../../relational-databases/import-export/non-xml-format-files-sql-server.md)
在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
BULK INSERT dbo.myFirstImport   
   FROM 'D:\BCP\myFirstImport.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myFirstImport.fmt');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### <a name="using-bulk-insert-and-xml-format-file"></a>使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 和 [XML 格式化文件](../../relational-databases/import-export/xml-format-files-sql-server.md)
在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
BULK INSERT dbo.myFirstImport   
   FROM 'D:\BCP\myFirstImport.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myFirstImport.xml');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### <a name="using-openrowsetbulk-and-non-xml-format-file"></a>使用 [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 和[非 XML 格式化文件](../../relational-databases/import-export/non-xml-format-files-sql-server.md)    
在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
INSERT INTO dbo.myFirstImport
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myFirstImport.bcp',
        FORMATFILE = 'D:\BCP\myFirstImport.fmt'
        ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### <a name="using-openrowsetbulk-and-xml-format-file"></a>使用 [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 和 [XML 格式化文件](../../relational-databases/import-export/xml-format-files-sql-server.md)
在 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
INSERT INTO dbo.myFirstImport 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myFirstImport.bcp',
        FORMATFILE = 'D:\BCP\myFirstImport.xml'  
       ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```
  
## <a name="more-examples"></a>更多示例
 [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
 [使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
 [使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
 [使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)   
 [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md)  
  [用来导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)
