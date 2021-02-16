---
title: 格式化文件以导入和导出数据
description: 当批量导入到 SQL Server 表或从表中批量导出时，格式化文件可以存储相对于表的数据文件的字段格式信息。
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk exporting [SQL Server], format files
- bulk importing [SQL Server], format files
- format files [SQL Server]
ms.assetid: b7b97d68-4336-4091-aee4-1941fab568e3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 35650bcd31127dcf3eaad46be4c958bc8f94547e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351378"
---
# <a name="format-files-to-import-or-export-data-sql-server"></a>格式化文件以导入或导出数据 (SQL Server)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
当向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中大容量导入数据或从该表中大容量导出数据时，可以使用格式化文件  存储大容量导入数据或大容量导出数据所需的所有格式信息。 这包括数据文件中相对于该表的各字段的格式信息。

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 支持两种格式化文件类型：XML 格式和非 XML 格式化文件。 XML 格式化文件和非 XML 格式化文件在一个数据文件中包含每个字段的说明，并且 XML 格式化文件还包含相应表列的说明。 通常，XML 与非 XML 格式化文件可以互换。 但是，建议您为新的格式化文件使用 XML 语法，因为与非 XML 格式化文件相比，格式化文件具有多项优点。 有关详细信息，请参阅 [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md)。

## <a name="benefits-of-format-files"></a><a name="Benefits"></a> 格式化文件的优点

- 为编写数据文件提供了一个灵活的系统，用户只需进行极少的编辑甚至无需编辑即可编写出符合其他数据格式的数据文件，或从其他软件读取数据文件。
- 使您可以大容量导入数据，而不必添加或删除不需要的数据或者重新排列数据文件中现有数据的顺序。 当数据文件中的字段和表中的列存在不匹配的情况时，格式化文件尤其有用。

## <a name="examples-of-format-files"></a><a name="ExamplesOfFFs"></a> 格式化文件的示例

下面的示例说明了非 XML 格式化文件和 XML 格式化文件的布局。 这些格式化文件对应于 `HumanResources.myTeam` 示例数据库中的 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 表。 该表包含四列： `EmployeeID`、 `Name`、 `Title`和 `ModifiedDate`。

> [!NOTE]
> 有关该表以及如何创建该表的信息，请参阅 [HumanResources.myTeam 示例表 (SQL Server)](../../relational-databases/import-export/humanresources-myteam-sample-table-sql-server.md)。

### <a name="a-using-a-non-xml-format-file"></a>A. 使用非 XML 格式化文件

下面的非 XML 格式化文件为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表使用 `HumanResources.myTeam` 本机数据格式。 此格式化文件是用下面的 `bcp` 命令创建的。

```cmd
bcp AdventureWorks.HumanResources.myTeam format nul -f myTeam.Fmt -n -T
The contents of this format file are as follows: 9.0
4
1       SQLSMALLINT   0       2       ""   1     EmployeeID               ""  
2       SQLNCHAR      2       100     ""   2     Name                     SQL_Latin1_General_CP1_CI_AS  
3       SQLNCHAR      2       100     ""   3     Title                    SQL_Latin1_General_CP1_CI_AS  
4       SQLNCHAR      2       100     ""   4     Background               SQL_Latin1_General_CP1_CI_AS  
```  

有关详细信息，请参阅 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)早期版本支持的原始格式。

### <a name="b-using-an-xml-format-file"></a>B. 使用 XML 格式化文件

下面的 XML 格式化文件为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表使用 `HumanResources.myTeam` 本机数据格式。 此格式化文件是用下面的 `bcp` 命令创建的。

```cmd
bcp AdventureWorks.HumanResources.myTeam format nul -f myTeam.Xml -x -n -T
```

格式化文件包含：

```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<RECORD>
  <FIELD ID="1" xsi:type="NativePrefix" LENGTH="1"/>
  <FIELD ID="2" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="EmployeeID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="2" NAME="Name" xsi:type="SQLNVARCHAR"/>
  <COLUMN SOURCE="3" NAME="Title" xsi:type="SQLNVARCHAR"/>
  <COLUMN SOURCE="4" NAME="Background" xsi:type="SQLNVARCHAR"/>
</ROW>
</BCPFORMAT>
```

有关详细信息，请参阅 [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md)。 

## <a name="when-is-a-format-file-required"></a><a name="WhenFFrequired"></a> 何时需要使用格式化文件？

- INSERT ... SELECT * FROM OPENROWSET(BULK...) 语句始终要求使用格式化文件。
- 对于 **bcp** 或 BULK INSERT，在简单的情况下，请视情况选用格式化文件，在极少数的情况下才必须使用。 但是，对于复杂的大容量导入情况，通常都会需要格式化文件。

在以下情况下，必须使用格式化文件：

- 具有不同架构的多个表使用同一数据文件作为数据源。
- 数据文件中的字段数不同于目标表中的列数；例如：

  - 目标表中至少包含一个定义了默认值或允许为 NULL 的列。
  - 用户不具有对目标表的一个或多个列的 SELECT/INSERT 权限。
  - 具有不同架构的两个或多个表使用同一个数据文件。

- 数据文件和表的列顺序不同。
- 数据文件列的终止字符或前缀长度不同。

> [!NOTE]
> 在缺少格式化文件的情况下，如果 **bcp** 命令指定了数据格式开关（ **-n**、 **-c**、 **-w** 或 **-N**），或者 BULK INSERT 操作指定了 DATAFILETYPE 选项，那么指定的数据格式将用作解释数据文件字段的默认方法。

## <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务

- [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)
- [使用格式化文件批量导入数据 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)
- [使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>另请参阅

- [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)
- [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md)
- [用于批量导入或导出的数据格式 (SQL Server)](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md)
