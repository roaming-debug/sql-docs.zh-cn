---
title: 创建格式化文件 (SQL Server) | Microsoft Docs
description: 批量导入或导出 SQL Server 表时，格式化文件允许写入数据文件，很少需要从其他程序编辑或读取数据文件。
ms.custom: ''
ms.date: 02/23/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- format files [SQL Server], creating
ms.assetid: f680b4a0-630f-4052-9c79-d348c1076f7b
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 12e75389d7e5878107f7efe5f4fe88494354cf1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351376"
---
# <a name="create-a-format-file-sql-server"></a>创建格式化文件 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  当对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表进行大容量的数据导入或导出时，您可使用格式化文件提供一个灵活的系统，用于写入需要少量编辑或不需要编辑即可符合其他数据格式的数据文件，或用于从其他软件程序读取数据文件。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持两种类型的格式化文件：非 XML 格式和 XML 格式。 非 XML 格式是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]早期版本支持的原始格式。  
  
 通常，XML 与非 XML 格式化文件可以互换。 但是，建议您为新的格式化文件使用 XML 语法，因为与非 XML 格式化文件相比，格式化文件具有多项优点。  
  
> [!NOTE]  
>  读取格式化文件所用的 **bcp** 实用工具 (Bcp.exe) 的版本必须与创建格式化文件所用的版本相同或更高。 例如，[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] bcp 可以读取由 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bcp 生成的 10.0 版格式化文件，但 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bcp 无法读取由 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] bcp 生成的 11.0 版格式化文件     。  
  
 本主题说明了如何使用 [bcp 实用工具](../../tools/bcp-utility.md) 为特定表创建格式化文件。 格式化文件基于指定的数据类型选项（ **-n**、 **-c**、 **-w** 或 **-N**）以及表或视图分隔符。  
  
## <a name="creating-a-non-xml-format-file"></a>创建非 XML 格式化文件  
 若要使用 **bcp** 命令创建格式化文件，请指定 **format** 参数，并使用 **nul** 而不是数据文件路径。 **format** 选项还需要 **-f** 选项，例如：  
  
 **bcp** _table_or_view_ **format** nul **-f**_format_file_name_  
  
> [!NOTE]  
> 为区分非 XML 格式化文件，我们建议使用 .fmt 作为文件扩展名，例如 MyTable.fmt。  
  
 有关非 XML 格式化文件结构和字段的信息，请参阅 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)早期版本支持的原始格式。  
  
### <a name="examples"></a>示例  
 本部分包含以下示例，演示如何使用 **bcp** 命令创建非 XML 格式化文件：  
  
-   A. 为本机数据创建非 XML 格式化文件  
  
-   B. 为字符数据创建非 XML 格式化文件  
  
-   C. 为 Unicode 本机数据创建非 XML 格式化文件  
  
-   D. 为 Unicode 字符数据创建非 XML 格式化文件  
  
-   F. 使用具有代码页选项的格式化文件  
  
 本示例使用 `HumanResources.Department` 示例数据库中的 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 表。 `HumanResources.Department` 表包含四列： `DepartmentID`、 `Name`、 `GroupName`和 `ModifiedDate`。  
  
#### <a name="a-creating-a-non-xml-format-file-for-native-data"></a>A. 为本机数据创建非 XML 格式化文件  
 以下示例将为 `Department-n.xml`表创建 XML 格式化文件 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]`HumanResources.Department` 。 格式化文件使用本机数据类型。 在命令之后显示生成的格式化文件的内容。  
  
 **bcp** 命令包含以下限定符。  
  
|限定符|说明|  
|----------------|-----------------|  
|**formatnul-f** _format_file_|指定非 XML 格式化文件。|  
|**-n**|指定本机数据类型。|  
|**-T**|指定 **bcp** 实用工具通过使用集成安全性的受信任连接连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果未指定 **-T** ，则必须指定 **-U** 和 **-P** 才能成功登录。|  
  
 在 Windows 命令提示符下，输入以下 `bcp` 命令：  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -T -n -f Department-n.fmt  
```  
  
 生成的格式化文件 `Department-n.fmt`包含以下信息：  
  
```  
12.0  
4  
1  SQLSMALLINT   0       2       ""   1     DepartmentID         ""  
2  SQLNCHAR      2       100     ""   2     Name                 SQL_Latin1_General_CP1_CI_AS  
3  SQLNCHAR      2       100     ""   3     GroupName            SQL_Latin1_General_CP1_CI_AS  
4  SQLDATETIME   0       8       ""   4     ModifiedDate         ""  
```  
  
 有关详细信息，请参阅 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)早期版本支持的原始格式。  
  
#### <a name="b-creating-a-non-xml-format-file-for-character-data"></a>B. 为字符数据创建非 XML 格式化文件  
 以下示例将为 `Department.fmt`表创建 XML 格式化文件 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]`HumanResources.Department` 。 格式化文件使用字符数据格式和非默认字段终止符 (`,`)。 在命令之后显示生成的格式化文件的内容。  
  
 **bcp** 命令包含以下限定符。  
  
|限定符|说明|  
|----------------|-----------------|  
|**formatnul-f** _format_file_|指定非 XML 格式化文件。|  
|**-c**|指定字符数据。|  
|**-T**|指定 **bcp** 实用工具通过使用集成安全性的受信任连接连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果未指定 **-T** ，则必须指定 **-U** 和 **-P** 才能成功登录。|  
  
 在 Windows 命令提示符下，输入以下 `bcp` 命令：  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -c -f Department-c.fmt -T  
```  
  
 生成的格式化文件 `Department-c.fmt`包含以下信息：  
  
```  
12.0  
4  
1  SQLCHAR       0       7       "\t"     1     DepartmentID            ""  
2  SQLCHAR       0       100     "\t"     2     Name                    SQL_Latin1_General_CP1_CI_AS  
3  SQLCHAR       0       100     "\t"     3     GroupName               SQL_Latin1_General_CP1_CI_AS  
4  SQLCHAR       0       24      "\r\n"   4     ModifiedDate            ""  
```  
  
 有关详细信息，请参阅 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)早期版本支持的原始格式。  
  
#### <a name="c-creating-a-non-xml-format-file-for-unicode-native-data"></a>C. 为 Unicode 本机数据创建非 XML 格式化文件  
 若要为 `HumanResources.Department` 表中的 Unicode 本机数据创建非 XML 格式化文件，请使用以下命令：  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -T -N -f Department-n.fmt  
```  
  
 有关如何使用 Unicode 本机数据的详细信息，请参阅[使用 Unicode 本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)。  
  
#### <a name="d-creating-a-non-xml-format-file-for-unicode-character-data"></a>D. 为 Unicode 字符数据创建非 XML 格式化文件  
 若要为 `HumanResources.Department` 表中使用默认终止符的 Unicode 字符数据创建非 XML 格式化文件，请使用以下命令：  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -T -w -f Department-w.fmt  
```  
  
 有关如何使用 Unicode 字符数据的详细信息，请参阅[使用 Unicode 字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)。  
  
#### <a name="f-using-a-format-file-with-the-code-page-option"></a>F. 使用具有代码页选项的格式化文件  
如果使用 bcp 命令（即使用 `bcp format`）创建格式化文件，排序规则/代码页的相关信息将写入格式化文件。   
具有 5 列的表的以下示例格式化文件包括了排序规则。  
  
```  
13.0  
5  
1  SQLCHAR         0       0       "**\t**"         1     c_0          Cyrillic_General_CS_AS  
2  SQLCHAR         0       0       "**\t**"         2     c_1          Cyrillic_General_CS_AS  
3  SQLCHAR         0       3000    "**\t**"         3     c_2          Cyrillic_General_CS_AS  
4  SQLCHAR         0       5       "**\t**"         4     c_3          ""  
5  SQLCHAR         0       41      "!!!\r\r\n"      5     c_4          ""  
  
```  
  
 如果尝试使用 `bcp in -c -C65001 -f format_file` ..." 或 "`BULK INSERT`/`OPENROWSET` ... `FORMATFILE='format_file' CODEPAGE=65001` ..." 将数据导入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，有关排序规则/代码页的信息会优先于 65001 选项。  
因此，如要生成格式化文件，必须在开始将数据导回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]之前，手动从生成的格式化文件中删除排序规则信息。  
以下是不具有排序规则信息的格式化文件的示例。  
  
```  
13.0  
5  
1  SQLCHAR         0       0       "**\t**"         1     c_0              ""  
2  SQLCHAR         0       0       "**\t**"         2     c_1              ""  
3  SQLCHAR         0       3000    "**\t**"         3     c_2              ""  
4  SQLCHAR         0       5       "**\t**"         4     c_3              ""  
5  SQLCHAR         0       41      "!!!\r\r\n"      5     c_4              ""  
```  
  
## <a name="creating-an-xml-format-file"></a>创建 XML 格式化文件  
 若要使用 **bcp** 命令创建格式化文件，请指定 **format** 参数，并使用 **nul** 而不是数据文件路径。 **format** 选项始终要求 **-f** 选项，若要创建 XML 格式化文件，还必须指定 **-x** 选项，例如：  
  
 **bcp** _table_or_view_ **format nul-f** _format_file_name_ **-x**  
  
> [!NOTE]  
> 为区分 XML 格式化文件，我们建议使用 .xml 作为文件扩展名，例如 MyTable.xml。  
  
 有关 XML 格式化文件结构和字段的信息，请参阅 [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md)早期版本支持的原始格式。  
  
### <a name="examples"></a>示例  
 本部分包含以下示例，演示如何使用 **bcp** 命令创建 XML 格式化文件：  
  
-   A. 为字符数据创建 XML 格式化文件  
-   B. 为本机数据创建 XML 格式化文件  
  
 本示例使用 `HumanResources.Department` 示例数据库中的 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 表。 `HumanResources.Department` 表包含四列： `DepartmentID`、 `Name`、 `GroupName`和 `ModifiedDate`。  
  
> [!NOTE]  
>  [!INCLUDE[ssSampleDBdesc](../../includes/sssampledbdesc-md.md)]  
  
#### <a name="a-creating-an-xml-format-file-for-character-data"></a>A. 为字符数据创建 XML 格式化文件  
 以下示例将为 `Department.xml`表创建 XML 格式化文件 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]`HumanResources.Department` 。 格式化文件使用字符数据格式和非默认字段终止符 (`,`)。 在命令之后显示生成的格式化文件的内容。  
  
 **bcp** 命令包含以下限定符。  
  
|限定符|说明|  
|----------------|-----------------|  
|**formatnul-f** _format_file_ **-x**|指定 XML 格式化文件。|  
|**-c**|指定字符数据。|  
|**-t** `,`|将逗号 ( **,** ) 指定为字段终止符。<br /><br /> 注意：如果数据文件使用默认的字段终止符 (`\t`)，则不需要 -t 开关  。|  
|**-T**|指定 **bcp** 实用工具通过使用集成安全性的受信任连接连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果未指定 **-T** ，则必须指定 **-U** 和 **-P** 才能成功登录。|  
  
 在 Windows 命令提示符下，输入以下 `bcp` 命令：  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -c -x -f Department-c.xml -t, -T  
```  
  
 生成的格式化文件 `Department-c.xml`包含以下 XML 元素：  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
 <RECORD>  
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>  
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="24"/>  
 </RECORD>  
 <ROW>  
  <COLUMN SOURCE="1" NAME="DepartmentID" xsi:type="SQLSMALLINT"/>  
  <COLUMN SOURCE="2" NAME="Name" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="3" NAME="GroupName" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="4" NAME="ModifiedDate" xsi:type="SQLDATETIME"/>  
 </ROW>  
</BCPFORMAT>  
```  
  
 有关此格式化文件语法的详细信息，请参阅 [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md)。 有关字符数据的详细信息，请参阅[使用字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)。  
  
#### <a name="b-creating-an-xml-format-file-for-native-data"></a>B. 为本机数据创建 XML 格式化文件  
 以下示例将为 `Department-n.xml`表创建 XML 格式化文件 `HumanResources.Department` 。 格式化文件使用本机数据类型。 在命令之后显示生成的格式化文件的内容。  
  
 **bcp** 命令包含以下限定符。  
  
|限定符|说明|  
|----------------|-----------------|  
|**formatnul-f** _format_file_ **-x**|指定 XML 格式化文件。|  
|**-n**|指定本机数据类型。|  
|**-T**|指定 **bcp** 实用工具通过使用集成安全性的受信任连接连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果未指定 **-T** ，则必须指定 **-U** 和 **-P** 才能成功登录。|  
  
 在 Windows 命令提示符下，输入以下 `bcp` 命令：  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -x -f Department-n.xml -n -T  
```  
  
 生成的格式化文件 `Department-n.xml`包含以下 XML 元素：  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
 <RECORD>  
  <FIELD ID="1" xsi:type="NativeFixed" LENGTH="2"/>  
  <FIELD ID="2" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="3" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="4" xsi:type="NativeFixed" LENGTH="8"/>  
 </RECORD>  
 <ROW>  
  <COLUMN SOURCE="1" NAME="DepartmentID" xsi:type="SQLSMALLINT"/>  
  <COLUMN SOURCE="2" NAME="Name" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="3" NAME="GroupName" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="4" NAME="ModifiedDate" xsi:type="SQLDATETIME"/>  
 </ROW>  
</BCPFORMAT>  
```  
  
 有关此格式化文件语法的详细信息，请参阅 [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md)。 有关如何使用本机数据的详细信息，请参阅[使用本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)。  
  
## <a name="mapping-data-fields-to-table-columns"></a>将数据字段映射到表列  
 如同使用 **bcp** 创建一样，格式化文件按顺序说明所有的表列。 可以修改格式化文件以重新安排或忽略表列。 这便于您针对字段未直接映射到表列的数据文件来自定义格式化文件。 有关详情，请参阅以下主题：  
  
-   [使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)   
 [使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)   
 [使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)   
 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)   
 [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md)  
  
  
