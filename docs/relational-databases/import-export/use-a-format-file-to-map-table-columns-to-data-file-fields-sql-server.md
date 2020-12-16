---
title: 使用格式化文件将表列映射到数据文件字段
description: 在 SQL Server 中，非 XML 格式化文件和 XML 格式化文件可以容纳字段排列顺序不同于表列顺序的数据文件。
ms.date: 09/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- mapping columns to fields during import [SQL Server]
- format files [SQL Server], mapping columns to fields
ms.assetid: e7ee4f7e-24c4-4eb7-84d2-41e57ccc1ef1
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 4a3609eb60099d4099a1b5180a629e5bfea8269a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460520"
---
# <a name="use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server"></a>使用格式化文件将表列映射到数据文件字段 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
数据文件中包含的字段的排列顺序可能不同于表中相应列的顺序。 本主题介绍了非 XML 格式化文件和 XML 格式化文件，它们经过修改可容纳字段排列顺序不同于表列顺序的数据文件。 修改后的格式化文件可将数据字段映射到与之相应的表列。  有关其他信息，请查看 [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md) 。

|轮廓|
|---|
|[示例测试条件](#etc)<br />&emsp;&#9679;&emsp;[示例表](#sample_table)<br />&emsp;&#9679;&emsp;[示例数据文件](#sample_data_file)<br />[创建格式化文件](#create_format_file)<br />&emsp;&#9679;&emsp;[创建非 XML 格式化文件](#nonxml_format_file)<br />&emsp;&#9679;&emsp;[修改非 XML 格式化文件](#modify_nonxml_format_file)<br />&emsp;&#9679;&emsp;[创建 XML 格式化文件](#xml_format_file)<br />&emsp;&#9679;&emsp;[修改 XML 格式化文件](#modify_xml_format_file)<br />[使用格式化文件导入数据以将表列映射到数据文件字段](#import_data)<br />&emsp;&#9679;&emsp;[使用 bcp 和非 XML 格式化文件](#bcp_nonxml)<br />&emsp;&#9679;&emsp;[使用 bcp 和 XML 格式化文件](#bcp_xml)<br />&emsp;&#9679;&emsp;[使用 BULK INSERT 和非 XML 格式化文件](#bulk_nonxml)<br />&emsp;&#9679;&emsp;[使用 BULK INSERT 和 XML 格式化文件](#bulk_xml)<br />&emsp;&#9679;&emsp;[使用 OPENROWSET(BULK...) 和非 XML 格式化文件](#openrowset_nonxml)<br />&emsp;&#9679;&emsp;[使用 OPENROWSET(BULK...) 和 XML 格式化文件](#openrowset_xml)|

> [!NOTE]  
>  可以使用非 XML 或 XML 格式化文件将数据文件批量导入表中，方法是使用 [bcp 实用工具](../../tools/bcp-utility.md)命令、[BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 语句或 INSERT ...SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 语句。 有关详细信息，请参阅[使用格式化文件批量导入数据 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)。  

## <a name="example-test-conditions"></a>示例测试条件<a name="etc"></a>  
本主题中修改的格式化文件示例基于下面定义的表和数据文件。

### <a name="sample-table"></a>示例表<a name="sample_table"></a>
下面的脚本创建一个测试数据库和一个名为 `myRemap`的表。  在 Microsoft SQL Server Management Studio (SSMS) 中执行以下 Transact-SQL：
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE myRemap
   (
   PersonID smallint,
   FirstName varchar(25),
   LastName varchar(30),
   Gender char(1)
   );
```

### <a name="sample-data-file"></a>示例数据文件<a name="sample_data_file"></a>
下面的数据按照与表 `FirstName` 中的呈现顺序相反的顺序来呈现 `LastName` 和 `myRemap`。  使用记事本创建一个空文件 `D:\BCP\myRemap.bcp` 并插入以下数据：
```
1,Grosse,Anthony,M
2,Fatnowna,Alica,F
3,Rosenhain,Stella,F
```

## <a name="creating-the-format-files"></a>创建格式化文件<a name="create_format_file"></a>
若要将数据从 `myRemap.bcp` 大容量导入至 `myRemap` 表，则该格式化文件必须进行下列操作：
* 将第一个数据字段映射到第一列 `PersonID`。
* 将第二个数据字段映射到第三列 `LastName`。
* 将第三个数据字段映射到第二列 `FirstName`。
* 将第四个数据字段映射到第四列 `Gender`。

用于创建格式化文件的最简单方法是使用 [bcp 实用工具](../../tools/bcp-utility.md)。  首先，从现有表创建基本格式化文件。  其次，修改基本格式化文件以反映实际数据文件。

### <a name="creating-a-non-xml-format-file"></a>创建非 XML 格式化文件<a name="nonxml_format_file"></a>
有关详细信息，请查看 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) 。 下面的命令基于 [的架构使用](../../tools/bcp-utility.md) bcp 实用工具 `myRemap.fmt`生成非 XML 格式化文件 `myRemap`。  此外，限定符 `c` 用于指定字符数据， `t,` 用于将逗号指定为字段终止符，而 `T` 用于指定使用集成安全性的信任连接。  在命令提示符处输入以下命令：
```
bcp TestDatabase.dbo.myRemap format nul -c -f D:\BCP\myRemap.fmt -t, -T
```
### <a name="modifying-the-non-xml-format-file"></a>修改非 XML 格式化文件 <a name="modify_nonxml_format_file"></a>
有关术语，请参阅 [非 XML 格式化文件的结构](../../relational-databases/import-export/non-xml-format-files-sql-server.md#Structure) 。  在记事本中打开 `D:\BCP\myRemap.fmt` 并执行以下修改：
1.  重新排列格式化文件行的顺序，以便这些行的顺序与 `myRemap.bcp`中的数据相同。
2.  确保主机文件字段顺序值是顺序的。
3.  确保最后一个格式化文件行后面有一个回车符。

比较更改：     
**之前**
```
13.0
4
1       SQLCHAR    0       7       ","      1     PersonID               ""
2       SQLCHAR    0       25      ","      2     FirstName              SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR    0       30      ","      3     LastName               SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR    0       1       "\r\n"   4     Gender                 SQL_Latin1_General_CP1_CI_AS

```
**之后**
```
13.0
4
1       SQLCHAR    0       7       ","      1     PersonID               ""
2       SQLCHAR    0       30      ","      3     LastName               SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR    0       25      ","      2     FirstName              SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR    0       1       "\r\n"   4     Gender                 SQL_Latin1_General_CP1_CI_AS

```
修改的格式化文件现在可反映：
* `myRemap.bcp` 中的第一个数据字段映射到第一列， `myRemap.. PersonID`
* `myRemap.bcp` 中的第二个数据字段映射到第三列， `myRemap.. LastName`
* `myRemap.bcp` 中的第三个数据字段映射到第二列， `myRemap.. FirstName`
* `myRemap.bcp` 中的第四个数据字段映射到第四列， `myRemap.. Gender`

### <a name="creating-an-xml-format-file"></a>创建 XML 格式化文件 <a name="xml_format_file"></a>  
有关详细信息，请查看 [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md) 。  下面的命令使用 [bcp 实用工具](../../tools/bcp-utility.md) 基于 `myRemap.xml`的架构创建 xml 格式化文件 `myRemap`。  此外，限定符 `c` 用于指定字符数据， `t,` 用于将逗号指定为字段终止符，而 `T` 用于指定使用集成安全性的信任连接。  `x` 限定符必须用于生成基于 XML 的格式化文件。  在命令提示符处输入以下命令：
```
bcp TestDatabase.dbo.myRemap format nul -c -x -f D:\BCP\myRemap.xml -t, -T
```
### <a name="modifying-the-xml-format-file"></a>修改 XML 格式化文件 <a name="modify_xml_format_file"></a>
有关术语，请参阅 [XML 格式化文件的架构语法](../../relational-databases/import-export/xml-format-files-sql-server.md#StructureOfXmlFFs) 。  在记事本中打开 `D:\BCP\myRemap.xml` 并执行以下修改：
1. 在格式化文件中声明 \<FIELD> 元素的顺序是这些字段在数据文件中的显示顺序，因此颠倒具有 ID 属性 2 和 3 的 \<FIELD> 元素的顺序。
2. 确保 \<FIELD> ID 属性值是顺序的。
3. \<ROW> 元素中 \<COLUMN> 元素的顺序定义了批量操作将这些元素发送到目标的顺序。  XML 格式化文件为每个 \<COLUMN> 元素分配了一个本地名称，该名称与批量导入操作的目标表中的列没有关系。  \<COLUMN> 元素的顺序与 \<RECORD> 定义中的 \<FIELD> 元素的顺序无关。  每个 \<COLUMN> 元素对应一个 \<FIELD> 元素（其 ID 在 \<COLUMN> 元素的 SOURCE 属性中指定）。  因此，\<COLUMN> SOURCE 的值是需要修订的唯一属性。  颠倒 \<COLUMN> SOURCE 属性 2 和 3 的顺序。

比较更改  
**之前**
```
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="1" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="2" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="3" NAME="LastName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="4" NAME="Gender" xsi:type="SQLCHAR"/>
 </ROW>
</BCPFORMAT>
```
**之后**
```
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="1" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="3" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="2" NAME="LastName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="4" NAME="Gender" xsi:type="SQLCHAR"/>
 </ROW>
</BCPFORMAT>
```
修改的格式化文件现在可反映：
* 与 COLUMN 1 对应的 FIELD 1 映射到第一个表列， `myRemap.. PersonID`
* 与 COLUMN 2 对应的 FIELD 2 重新映射到第三个表列， `myRemap.. LastName`
* 与 COLUMN 3 对应的 FIELD 3 重新映射到第二个表列， `myRemap.. FirstName`
* 与 COLUMN 4 对应的 FIELD 4 映射到第四个表列， `myRemap.. Gender`


## <a name="importing-data-with-a-format-file-to-map-table-columns-to-data-file-field"></a>使用格式化文件导入数据以将表列映射到数据文件字段<a name="import_data"></a>
下面的示例使用上面创建的数据库、数据文件和格式化文件。

### <a name="using-bcp-and-non-xml-format-file"></a>使用 [bcp](../../tools/bcp-utility.md) 和 [非 XML 格式化文件](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bcp_nonxml"></a>
在命令提示符处输入以下命令：
```
bcp TestDatabase.dbo.myRemap IN D:\BCP\myRemap.bcp -f D:\BCP\myRemap.fmt -T
```

### <a name="using-bcp-and-xml-format-file"></a>使用 [bcp](../../tools/bcp-utility.md) 和 [XML 格式化文件](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bcp_xml"></a>
在命令提示符处输入以下命令：
```
bcp TestDatabase.dbo.myRemap IN D:\BCP\myRemap.bcp -f D:\BCP\myRemap.xml -T
```

### <a name="using-bulk-insert-and-non-xml-format-file"></a>使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 和 [非 XML 格式化文件](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bulk_nonxml"></a>
在 Microsoft SQL Server Management Studio (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myRemap;
BULK INSERT dbo.myRemap   
   FROM 'D:\BCP\myRemap.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myRemap.fmt');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```

### <a name="using-bulk-insert-and-xml-format-file"></a>使用 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 和 [XML 格式化文件](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bulk_xml"></a>
在 Microsoft SQL Server Management Studio (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myRemap;
BULK INSERT dbo.myRemap   
   FROM 'D:\BCP\myRemap.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myRemap.xml');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```

### <a name="using-openrowsetbulk-and-non-xml-format-file"></a>使用 [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 和 [非 XML 格式化文件](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="openrowset_nonxml"></a>    
在 Microsoft SQL Server Management Studio (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;
GO

TRUNCATE TABLE myRemap;
INSERT INTO dbo.myRemap
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myRemap.bcp',
        FORMATFILE = 'D:\BCP\myRemap.fmt'
        ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```

### <a name="using-openrowsetbulk-and-xml-format-file"></a>使用 [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) 和 [XML 格式化文件](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="openrowset_xml"></a>
在 Microsoft SQL Server Management Studio (SSMS) 中执行以下 Transact-SQL：
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myRemap;
INSERT INTO dbo.myRemap 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myRemap.bcp',
        FORMATFILE = 'D:\BCP\myRemap.xml'  
       ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```


  
## <a name="see-also"></a>另请参阅  
[bcp 实用工具](../../tools/bcp-utility.md)   
 [使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)   
 [使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
  
