---
description: '访问清单架构 (AccessToSQL) '
title: 访问清单架构 (AccessToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- columns table
- databases table
- exported metadata
- exporting, schemas of exported metadata
- foreign keys table
- forms table
- indexes, table
- inventory schemas
- macros table
- modules table
- queries table
- reference, inventory schemas
- reports table
- schemas of exported metadata
- schemas, exported metadata
- SSMA_Access_InventoryColumns
- SSMA_Access_InventoryDatabases
- SSMA_Access_InventoryForeignKeys
- SSMA_Access_InventoryForms
- SSMA_Access_InventoryIndexes
- SSMA_Access_InventoryMacros
- SSMA_Access_InventoryModules
- SSMA_Access_InventoryQueries
- SSMA_Access_InventoryReports
- SSMA_Access_InventoryTables
- tables, inventory
ms.assetid: fdd3cff2-4d62-4395-8acf-71ea8f17f524
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: bddb9d2329c20203aca602e7aec37f235c4b6850
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077666"
---
# <a name="access-inventory-schemas-accesstosql"></a>访问清单架构 (AccessToSQL) 
以下各节描述了在将访问架构导出到时由 SSMA 创建的表 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="databases"></a>数据库  
数据库元数据将导出到 **SSMA_Access_InventoryDatabases** 表。 此表包含以下列：  
  
|列名|数据类型|说明|  
|---------------|-------------|---------------|  
|**数据库 ID**|**uniqueidentifier**|唯一标识每个数据库的 GUID。 此列也是表的主键。|  
|**DatabaseName**|**nvarchar(4000)**|访问数据库的名称。|  
|**ExportTime**|**datetime**|此元数据由 SSMA 创建的日期和时间。|  
|**文件路径**|**nvarchar(4000)**|Access 数据库的完整路径和文件名。|  
|**FileSize**|**bigint**|Access 数据库的大小（KB）。|  
|**FileOwner**|**nvarchar(4000)**|指定为访问数据库的所有者的 Windows 帐户。|  
|**DateCreated**|**datetime**|创建 Access 数据库的日期和时间。|  
|**DateModified**|**datetime**|上次修改 Access 数据库的日期和时间。|  
|**TablesCount**|**int**|Access 数据库中的表数。|  
|**QueriesCount**|**int**|Access 数据库中的查询数。|  
|**FormsCount**|**int**|Access 数据库中的窗体数。|  
|**ModulesCount**|**int**|Access 数据库中的模块数。|  
|**ReportsCount**|**int**|Access 数据库中的报表数。|  
|**MacrosCount**|**int**|Access 数据库中宏的数目。|  
|**AccessVersion**|**nvarchar(4000)**|数据库的访问版本。|  
|**排序规则**|**nvarchar(4000)**|Access 数据库的排序规则。 排序规则确定数据库如何排序和比较字符串。|  
|**JetVersion**|**nvarchar(4000)**|Jet 数据库引擎版本。 Access 数据库使用底层 Jet 数据库引擎。|  
|**IsUpdatable**|**bit**|指示是否可以更新数据库。 如果值为1，则数据库可更新。 如果该值为0，则数据库为只读。|  
|**QueryTimeout**|**int**|数据库的已配置 ODBC 查询超时值（以秒为单位）。 默认值为 60 秒。|  
  
## <a name="tables"></a>表  
表元数据将导出到 **SSMA_Access_InventoryTables** 表。 此表包含以下列：  
  
|列名|数据类型|说明|  
|---------------|-------------|---------------|  
|**数据库 ID**|**uniqueidentifier**|标识包含此表的数据库。|  
|**TableId**|**uniqueidentifier**|用于唯一标识表的 GUID。 此列也是表的主键。|  
|**TableName**|**nvarchar(4000)**|表的名称。|  
|**RowsCount**|**int**|表中的行数。|  
|**有效性**|**nvarchar(4000)**|定义表的有效输入的规则。 如果不存在验证规则，则字段将包含空字符串。|  
|**LinkedTable**|**nvarchar(4000)**|与表关联的另一个表（如果有）。 链接表允许使用该表添加、删除和更新其他表。|  
|**ExternalSource**|**nvarchar(4000)**|与表关联的数据源（如果有）。 如果表已链接，则它具有在此字段中指定的外部数据源。|  
  
## <a name="columns"></a>列  
列元数据将导出到 **SSMA_Access_InventoryColumns** 表。 此表包含以下列：  
  
|列名|数据类型|说明|  
|---------------|-------------|---------------|  
|**数据库 ID**|**uniqueidentifier**|标识包含此列的数据库。|  
|**TableId**|**uniqueidentifier**|标识包含此列的表。|  
|**ColumnId**|**int**|用于标识列的递增整数。 **ColumnId** 是表的主键。|  
|**ColumnName**|**nvarchar(4000)**|列的名称。|  
|**IsNullable**|**bit**|指定列是否可以包含 null 值。 如果值为1，则列可以包含 null 值。 如果该值为0，则列不能包含 null 值。 请注意，验证规则还可用于防止 null 值。|  
|**DataType**|**nvarchar(4000)**|列的访问数据类型，例如 **文本** 或 **Long**。|  
|**IsAutoIncrement**|**bit**|指定列是否自动增加整数值。 如果值为1，则整数会自动递增。|  
|**序号**|**smallint**|表中列的顺序（从零开始）。|  
|**值**|**nvarchar(4000)**|列的默认值。|  
|**有效性**|**nvarchar(4000)**|用于验证在列中添加或更新的数据的规则。|  
  
## <a name="indexes"></a>索引  
索引元数据将导出到 **SSMA_Access_InventoryIndexes** 表。 此表包含以下列：  
  
|列名|数据类型|说明|  
|---------------|-------------|---------------|  
|**数据库 ID**|**uniqueidentifier**|标识包含此索引的数据库。|  
|**TableId**|**uniqueidentifier**|标识包含此索引的表。|  
|**IndexId**|**int**|用于标识索引的递增整数。 此列是表的主键。|  
|**IndexName**|**nvarchar(4000)**|索引的名称。|  
|**ColumnsIncluded**|**nvarchar(4000)**|列出索引中包含的列。 列名用分号分隔。|  
|**IsUnique**|**bit**|指定索引中的每个项是否都必须是唯一的。 在多列索引中，值的组合必须是唯一的。 如果值为1，则索引强制实施唯一值。|  
|**IsPK**|**bit**|指定在定义主键时是否自动创建索引。|  
|**IsClustered**|**bit**|指定索引是否为聚集索引。 聚集索引对数据的物理存储重新排序。 一个表只能有一个聚集索引。|  
  
## <a name="foreign-keys"></a>Foreign Keys  
外键元数据将导出到 **SSMA_Access_InventoryForeignKeys** 表。 此表包含以下列：  
  
|列名|数据类型|说明|  
|---------------|-------------|---------------|  
|**数据库 ID**|**uniqueidentifier**|标识包含此外键的数据库。|  
|**TableId**|**uniqueidentifier**|标识包含此外键的表。|  
|**ForeignKeyId**|**int**|标识外键的递增整数。 此列是表的主键。|  
|**ForeignKeyName**|**nvarchar(4000)**|索引的名称。|  
|**ReferencedTableId**|**uniqueidentifier**|标识包含源列的表。|  
|**SourceColumns**|**nvarchar(4000)**|列出外键列。|  
|**ReferencedColumns**|**nvarchar(4000)**|列出外键引用的主键列。|  
|**IsCascadeForUpdate**|**bit**|指定如果更新主键值，则还会更新引用该键值的所有行。|  
|**IsCascadeForDelete**|**bit**|指定在删除主键值时，还将删除所有引用该键值的行。|  
|**IsEnforced**|**bit**|指定强制实施外键约束。|  
  
## <a name="queries"></a>查询  
查询元数据将导出到 **SSMA_Access_InventoryQueries** 表。 此表包含以下列：  
  
|列名|数据类型|说明|  
|---------------|-------------|---------------|  
|**数据库 ID**|**uniqueidentifier**|标识包含此查询的数据库。|  
|**QueryId**|**int**|用于标识查询的递增整数。 此列是表的主键。|  
|**QueryName**|**nvarchar(4000)**|查询的名称。|  
|**QueryText**|**nvarchar(4000)**|SQL 查询代码，如 SELECT 语句。|  
|**IsUpdateable**|**bit**|指定查询是可更新还是只读。|  
|**QueryType**|**nvarchar(4000)**|指定查询的类型，例如 **Select** 或 **SetOperation**。|  
|**ExternalSource**|**nvarchar(4000)**|如果查询引用外部数据源，则这是查询使用的连接字符串。|  
  
## <a name="forms"></a>窗体  
表单元数据将导出到 **SSMA_Access_InventoryForms** 表。 此表包含以下列：  
  
|列名|数据类型|说明|  
|---------------|-------------|---------------|  
|**数据库 ID**|**uniqueidentifier**|标识包含此窗体的数据库。|  
|**FormId**|**int**|用于标识窗体的递增整数。 此列是表的主键。|  
|**FormName**|**nvarchar(4000)**|窗体的名称。|  
  
## <a name="macros"></a>宏  
宏元数据将导出到 **SSMA_Access_InventoryMacros** 表。 此表包含以下列：  
  
|列名|数据类型|说明|  
|---------------|-------------|---------------|  
|**数据库 ID**|**uniqueidentifier**|标识包含宏的数据库。|  
|**MacroId**|**int**|用于标识宏的递增整数。 此列是表的主键。|  
|**MacroName**|**nvarchar(4000)**|宏的名称。|  
  
## <a name="reports"></a>报表  
报表元数据将导出到 **SSMA_Access_InventoryReports** 表。 此表包含以下列：  
  
|列名|数据类型|说明|  
|---------------|-------------|---------------|  
|**数据库 ID**|**uniqueidentifier**|标识包含报表的数据库。|  
|**ReportId**|**int**|用于标识报表的递增整数。 此列是表的主键。|  
|**ReportName**|**nvarchar(4000)**|报表的名称。|  
  
## <a name="modules"></a>模块  
模块元数据将导出到 **SSMA_Access_InventoryModules** 表。 此表包含以下列：  
  
|列名|数据类型|说明|  
|---------------|-------------|---------------|  
|**数据库 ID**|**uniqueidentifier**|标识包含模块的数据库。|  
|**ModuleId**|**int**|标识模块的递增整数。 此列是表的主键。|  
|**ModuleName**|**nvarchar(4000)**|模块的名称。|  
  
## <a name="see-also"></a>另请参阅  
[导出 Access 清单](exporting-an-access-inventory-accesstosql.md)  
  
