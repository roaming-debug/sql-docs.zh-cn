---
title: 非 XML 格式化文件 (SQL Server) | Microsoft Docs
description: SQL Server 2019 支持使用非 XML 格式化文件和 XML 格式化文件进行批量导入和导出。 了解非 XML 格式化文件及其优点。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- non-XML format files
- format files [SQL Server], non-XML format files
- bulk importing [SQL Server], format files
ms.assetid: f566db3e-0a3b-4a61-9c84-49f8d42f5760
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ede937193a50bc314c00562631925a07a4a4b15c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351328"
---
# <a name="non-xml-format-files-sql-server"></a>非 XML 格式化文件 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中，大容量导出和导入支持两种类型的格式化文件：非 XML 格式化文件  和 XML 格式化文件  。  
  
 **本主题内容：**  
  
-   [优点](#Benefits)  
  
-   [非 XML 格式化文件的结构](#Structure)  
  
-   [非 XML 格式化文件的示例](#Examples)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="benefits-of-non-xml-format-files"></a><a name="Benefits"></a> 非 XML 格式化文件的优点  
  
-   通过在 **bcp** 命令中指定 **format** 选项，可以自动创建非 XML 格式化文件。  
  
-   当在 **bcp** 命令中指定某个现有的格式化文件时，该命令就使用记录在该格式化文件中的值，而不提示你输入文件存储类型、前缀长度、字段长度或字段终止符。  
  
-   您可为特定的数据类型（例如字符数据或本机数据）创建格式化文件。  
  
-   还可以创建非 XML 格式化文件，这种文件包含为每个数据字段交互式指定的属性。 有关详细信息，请参阅[在使用 bcp 时指定数据格式以获得兼容性 (SQL Server)](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)。  
  
> [!NOTE]  
>  XML 格式化文件与非 XML 格式化文件相比具有一些优点。 有关详细信息，请参阅 [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md)。  
  
##  <a name="structure-of-non-xml-format-files"></a><a name="Structure"></a> 非 XML 格式化文件的结构  
 非 XML 格式化文件是具有特殊结构的文本文件。 非 XML 格式化文件包含了有关每个表列的文件存储类型、前缀长度、字段长度和字段终止符的信息。  
  
 下图显示了一个示例非 XML 格式化文件的格式化文件字段。  
  
 ![标识非 XML 格式文件的字段](../../relational-databases/import-export/media/mydepart-fmt-ident-c.gif)
  
  “版本”和  “列数”字段仅出现一次。 下表对其意义进行了说明。  
  
|格式化文件字段|说明|  
|------------------------|-----------------|  
|版本|**bcp** 实用工具的版本号：<br /><br /> 9.0 = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> 10.0 = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]<br /><br /> 11.0 = [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]<br /><br /> 12.0 = [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]<br /><br /> 该版本号仅可由 **bcp** 识别，而无法由 [!INCLUDE[tsql](../../includes/tsql-md.md)]识别。<br /><br /> <br /><br /> 注意：读取格式化文件所用的 bcp 实用工具 (Bcp.exe) 的版本必须与创建格式化文件所用的版本相同或更高  。 例如， [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]**bcp** 可以读取由 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]**bcp** 生成的 10.0 版格式化文件，但 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]**bcp** 无法读取由 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]**bcp** 生成的 12.0 版格式化文件。|  
|列数|数据文件中字段的数目。 该数目必须在所有行中都相同。|  
  
 其他格式化文件字段说明需要大容量导入或导出的数据字段。 每个数据字段都需在格式化文件中占单独一行。 每个格式化文件行均包含下表中说明的格式化文件字段的值。  
  
|格式化文件字段|说明|  
|------------------------|-----------------|  
|**宿主文件字段顺序**|用以表示数据文件中每个字段的位置的数字。 行中的第一个字段为 1，依此类推。|  
|**宿主文件数据类型**|表示存储在数据文件的给定字段中的数据类型。 对于 ASCII 数据文件，使用 SQLCHAR；对于本机格式数据文件，使用默认的数据类型。 有关详细信息，请参阅 [使用 bcp 指定文件存储类型 (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)。|  
|**前缀长度**|字段长度前缀字符的数目。 有效前缀长度是 0、1、2、4 和 8。 若要避免指定长度前缀，将其设置为 0。 如果字段包含 NULL 数据值，则必须指定长度前缀。 有关详细信息，请参阅 [使用 bcp 指定数据文件中的前缀长度 (SQL Server)](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)。|  
|**宿主文件数据长度**|数据文件的特定字段中所存储的数据类型的最大长度（按字节计）。<br /><br /> 如果您正在为带分隔符的文本文件创建非 XML 格式化文件，则可以将每个数据字段的宿主文件数据长度指定为 0。 当带分隔符的文本文件的前缀长度为 0 并导入终止符时，可忽略字段长度值，因为字段所使用的存储空间等于数据加上终止符的长度。<br /><br /> 有关详细信息，请参阅 [使用 bcp 指定字段长度 (SQL Server)](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)。|  
|**终止符**|用来分隔数据文件中各字段的分隔符。 常用的终止符为逗号 (,)、制表符 (\t) 和行结束符 (\r\n)。 有关详细信息，请参阅 [指定字段终止符和行终止符 (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)。|  
|**服务器列顺序**|列在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中显示的顺序。 例如，如果数据文件的第四个字段映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中的第六列，则第四个字段的服务器列顺序为 6。<br /><br /> 若要阻止表中的某个列接收数据文件中的任何数据，则可以将服务器列顺序值设置为 0。|  
|**服务器列名**|从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中复制的列名。 无需使用字段的实际名称，但格式化文件中的字段不得为空。|  
|**列排序规则**|排序规则用于在数据文件中存储字符和 Unicode 数据。|  
  
> [!NOTE]  
>  您可以修改格式化文件，以便从字段编号或顺序与表列的编号或顺序不同的数据文件进行大容量导入。 有关详细信息，请参阅本主题后面的 [相关任务](#RelatedTasks) 列表。  
  
##  <a name="example-of-a-non-xml-format-file"></a><a name="Examples"></a> 非 XML 格式化文件的示例  
 下面的示例显示了一个以前创建的非 XML 格式化文件 (`myDepartmentIdentical-f-c.fmt`)。 该文件描述了 `HumanResources.Department` 示例数据库中的 `AdventureWorks2012` 表中每一列的字符数据字段。  
  
 生成的格式化文件 `myDepartmentIdentical-f-c.fmt`包含以下信息：  
  
```  
12.0  
4  
1       SQLCHAR       0       7       "\t"     1     DepartmentID     ""  
2       SQLCHAR       0       100     "\t"     2     Name             SQL_Latin1_General_CP1_CI_AS  
3       SQLCHAR       0       100     "\t"     3     GroupName        SQL_Latin1_General_CP1_CI_AS  
4       SQLCHAR       0       24      "\r\n"   4     ModifiedDate     ""  
```  
  
> [!NOTE]  
>  有关显示与该非 XML 格式化文件示例相关的格式化文件字段的说明，请参阅本主题前面的 [非 XML 格式化文件的结构](#Structure)。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
  
-   [使用格式化文件批量导入数据 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)  
  
-   [使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)   
 [XML 格式化文件 (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md)   
 [用来导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)  
  
  
