---
title: 使用格式化文件跳过数据字段
description: 对于字段多于表列的数据文件，可以使用格式化文件。 它将表列映射到相应的数据字段并忽略额外字段。
ms.date: 09/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- format files [SQL Server], skipping data fields
- skipping data fields when importing
ms.assetid: 6a76517e-983b-47a1-8f02-661b99859a8b
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: d939948fe5644b4d8f347bf36a962b52fe22e35c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745407"
---
# <a name="use-a-format-file-to-skip-a-data-field-sql-server"></a>使用格式化文件跳过数据字段 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
数据文件所包含的字段数可能大于表中的列数。 本主题说明了通过修改非 XML 和 XML 格式化文件，将表中的列映射到相应的数据字段并忽略额外字段，从而能够使用具有较多字段的数据文件。  有关其他信息，请查看 [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md) 。

|轮廓|
|---|
|[示例测试条件](#etc)<br />&emsp;&#9679;&emsp;[示例表](#sample_table)<br />&emsp;&#9679;&emsp;[示例数据文件](#sample_data_file)<br />[创建格式化文件](#create_format_file)<br />&emsp;&#9679;&emsp;[创建非 XML 格式化文件](#nonxml_format_file)<br />&emsp;&#9679;&emsp;[修改非 XML 格式化文件](#modify_nonxml_format_file)<br />&emsp;&#9679;&emsp;[创建 XML 格式化文件](#xml_format_file)<br />&emsp;&#9679;&emsp;[修改 XML 格式化文件](#modify_xml_format_file)<br />[使用格式化文件导入数据以跳过数据字段](#import_data)<br />&emsp;&#9679;&emsp;[使用 bcp 和非 XML 格式化文件](#bcp_nonxml)<br />&emsp;&#9679;&emsp;[使用 bcp 和 XML 格式化文件](#bcp_xml)<br />&emsp;&#9679;&emsp;[使用 BULK INSERT 和非 XML 格式化文件](#bulk_nonxml)<br />&emsp;&#9679;&emsp;[使用 BULK INSERT 和 XML 格式化文件](#bulk_xml)<br />&emsp;&#9679;&emsp;[使用 OPENROWSET(BULK...) 和非 XML 格式化文件](#openrowset_nonxml)<br />&emsp;&#9679;&emsp;[使用 OPENROWSET(BULK...) 和 XML 格式化文件](#openrowset_xml)<p>                                                                                                                                                                                                                  </p>|
  
> [!NOTE]
>  可以使用非 XML 或 XML 格式化文件将数据文件批量导入表中，方法是使用 [bcp 实用工具](../../tools/bcp-utility.md)命令、[BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 语句或 INSERT ...SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 语句。 有关详细信息，请参阅[使用格式化文件批量导入数据 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)。

## <a name="example-test-conditions"></a>示例测试条件<a name="etc"></a>  
本主题中修改的格式化文件示例基于下面定义的表和数据文件。
  
### <a name="sample-table"></a>示例表<a name="sample_table"></a>
下面的脚本创建一个测试数据库和一个名为 `myTestSkipField`的表。  在 Microsoft SQL Server Management Studio (SSMS) 中执行以下 Transact-SQL：
 
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE myTestSkipField
   (
   PersonID smallint,
   FirstName varchar(25),
   LastName varchar(30)
   );
```
  
### <a name="sample-data-file"></a>示例数据文件<a name="sample_data_file"></a>
创建一个空文件 `D:\BCP\myTestSkipField.bcp` 并插入以下数据： 
```
1,SkipMe,Anthony,Grosse
2,SkipMe,Alica,Fatnowna
3,SkipMe,Stella,Rosenhain
```
  
## <a name="creating-the-format-files"></a>创建格式化文件<a name="create_format_file"></a>
若要将数据从 `myTestSkipField.bcp` 大容量导入至 `myTestSkipField` 表，则该格式化文件必须进行下列操作：
* 将第一个数据字段映射到第一列 `PersonID`。
* 跳过第二个数据字段。
* 将第三个数据字段映射到第二列 `FirstName`。
* 将第四个数据字段映射到第三列 `LastName`。  

用于创建格式化文件的最简单方法是使用 [bcp 实用工具](../../tools/bcp-utility.md)。  首先，从现有表创建基本格式化文件。  其次，修改基本格式化文件以反映实际数据文件。
  
### <a name="creating-a-non-xml-format-file"></a><a name="nonxml_format_file"></a>创建非 XML 格式化文件 
有关详细信息，请查看 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) 。 下面的命令基于 [的架构使用](../../tools/bcp-utility.md) bcp 实用工具 `myTestSkipField.fmt`生成非 XML 格式化文件 `myTestSkipField`。  此外，限定符 `c` 用于指定字符数据， `t,` 用于将逗号指定为字段终止符，而 `T` 用于指定使用集成安全性的信任连接。  在命令提示符处输入以下命令：

```
bcp TestDatabase.dbo.myTestSkipField format nul -c -f D:\BCP\myTestSkipField.fmt -t, -T
```

### <a name="modifying-the-non-xml-format-file"></a>修改非 XML 格式化文件 <a name="modify_nonxml_format_file"></a>
有关术语，请参阅 [非 XML 格式化文件的结构](../../relational-databases/import-export/non-xml-format-files-sql-server.md#Structure) 。 在记事本中打开 `D:\BCP\myTestSkipField.fmt` 并执行以下修改：
1) 复制 `FirstName` 的整个格式化文件行，并紧接在下一行的 `FirstName` 后面粘贴它。
2) 对于新行和所有后续行，将主机文件字段顺序值增加一。
3) 增加列数值以反映数据文件中的实际字段数。
3) 对于第二个格式化文件行，将服务器列顺序从 `2` 修改为 `0` 。 

比较进行的更改：  
**之前**
```
13.0
3
1       SQLCHAR 0       7       ","      1     PersonID     ""
2       SQLCHAR 0       25      ","      2     FirstName    SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR 0       30      "\r\n"   3     LastName     SQL_Latin1_General_CP1_CI_AS
```
**之后**
```
13.0
4
1       SQLCHAR 0       7       ","      1     PersonID     ""
2       SQLCHAR 0       25      ","      0     FirstName    SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR 0       25      ","      2     FirstName    SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR 0       50      "\r\n"   3     LastName     SQL_Latin1_General_CP1_CI_AS

```

修改的格式化文件现在可反映：
* 4 个数据字段
* `myTestSkipField.bcp` 中的第一个数据字段映射到第一列， `myTestSkipField.. PersonID`
* `myTestSkipField.bcp` 中的第二个数据字段未映射到任何列。
* `myTestSkipField.bcp` 中的第三个数据字段映射到第二列， `myTestSkipField.. FirstName`
* `myTestSkipField.bcp` 中的第四个数据字段映射到第三列， `myTestSkipField.. LastName`

### <a name="creating-an-xml-format-file"></a>创建 XML 格式化文件 <a name="xml_format_file"></a>  
有关详细信息，请查看 [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md) 。  下面的命令使用 [bcp 实用工具](../../tools/bcp-utility.md) 基于 `myTestSkipField.xml`的架构创建 xml 格式化文件 `myTestSkipField`。  此外，限定符 `c` 用于指定字符数据， `t,` 用于将逗号指定为字段终止符，而 `T` 用于指定使用集成安全性的信任连接。  `x` 限定符必须用于生成基于 XML 的格式化文件。  在命令提示符处输入以下命令：

```
bcp TestDatabase.dbo.myTestSkipField format nul -c -x -f D:\BCP\myTestSkipField.xml -t, -T
```

### <a name="modifying-the-xml-format-file"></a>修改 XML 格式化文件 <a name="modify_xml_format_file"></a>
有关术语，请参阅 [XML 格式化文件的架构语法](../../relational-databases/import-export/xml-format-files-sql-server.md#StructureOfXmlFFs) 。  在记事本中打开 `D:\BCP\myTestSkipField.xml` 并执行以下修改：
1) 复制整个第二个字段，并紧接在下一行的第二个字段后面粘贴它。
2) 对于新 FIELD 和每个后续 FIELD，将“FIELD ID”值增加 1。
3) 对于 `FirstName`和 `LastName` 将“COLUMN SOURCE”值增加 1，以反映修改的映射。

比较进行的更改：  
**之前**
```
\<?xml version="1.0"?>
\<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  \<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  \<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  \<COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  \<COLUMN SOURCE="2" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  \<COLUMN SOURCE="3" NAME="LastName" xsi:type="SQLVARYCHAR"/>
 </ROW>
</BCPFORMAT>
```

**之后**
```
\<?xml version="1.0"?>
\<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  \<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  \<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  \<FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  \<COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  \<COLUMN SOURCE="3" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  \<COLUMN SOURCE="4" NAME="LastName" xsi:type="SQLVARYCHAR"/>
 </ROW>
</BCPFORMAT>
```

修改的格式化文件现在可反映：
* 4 个数据字段
* 与 COLUMN 1 对应的 FIELD 1 映射到第一个表列， `myTestSkipField.. PersonID`
* FIELD 2 不与任何 COLUMN 对应，因此不映射到任何表列。
* 与 COLUMN 3 对应的 FIELD 3 映射到第二个表列， `myTestSkipField.. FirstName`
* 与 COLUMN 4 对应的 FIELD 4 映射到第三个表列， `myTestSkipField.. LastName`

## <a name="importing-data-with-a-format-file-to-skip-a-data-field"></a>使用格式化文件导入数据以跳过数据字段<a name="import_data"></a>
下面的示例使用上面创建的数据库、数据文件和格式化文件。

### <a name="using-bcp-and-non-xml-format-file"></a>使用 [bcp](../../tools/bcp-utility.md) 和 [非 XML 格式化文件](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bcp_nonxml"></a>
在命令提示符处输入以下命令：
```
bcp TestDatabase.dbo.myTestSkipField IN D:\BCP\myTestSkipField.bcp -f D:\BCP\myTestSkipField.fmt -T
```

### <a name="using-bcp-and-xml-format-file"></a>使用 [bcp](../../tools/bcp-utility.md) 和 [XML 格式化文件](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bcp_xml"></a>
在命令提示符处输入以下命令：
```
bcp TestDatabase.dbo.myTestSkipField IN D:\BCP\myTestSkipField.bcp -f D:\BCP\myTestSkipField.xml -T
```

### <a name="using-bulk-insert-and-non-xml-format-file"></a>使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 和 [非 XML 格式化文件](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bulk_nonxml"></a>
在 Microsoft SQL Server Management Studio (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myTestSkipField;
BULK INSERT dbo.myTestSkipField   
   FROM 'D:\BCP\myTestSkipField.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myTestSkipField.fmt');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myTestSkipField;
```

### <a name="using-bulk-insert-and-xml-format-file"></a>使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 和 [XML 格式化文件](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bulk_xml"></a>
在 Microsoft SQL Server Management Studio (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myTestSkipField;
BULK INSERT dbo.myTestSkipField   
   FROM 'D:\BCP\myTestSkipField.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myTestSkipField.xml');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myTestSkipField;
```

### <a name="using-openrowsetbulk-and-non-xml-format-file"></a>使用 [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 和 [非 XML 格式化文件](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="openrowset_nonxml"></a>    
在 Microsoft SQL Server Management Studio (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;
GO

TRUNCATE TABLE myTestSkipField;
INSERT INTO dbo.myTestSkipField
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myTestSkipField.bcp',
        FORMATFILE = 'D:\BCP\myTestSkipField.fmt'
        ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myTestSkipField;
```

### <a name="using-openrowsetbulk-and-xml-format-file"></a>使用 [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 和 [XML 格式化文件](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="openrowset_xml"></a>
在 Microsoft SQL Server Management Studio (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myTestSkipField;
INSERT INTO dbo.myTestSkipField 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myTestSkipField.bcp',
        FORMATFILE = 'D:\BCP\myTestSkipField.xml'  
       ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myTestSkipField;
```

  
## <a name="see-also"></a>另请参阅  
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)   
 [使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
  
