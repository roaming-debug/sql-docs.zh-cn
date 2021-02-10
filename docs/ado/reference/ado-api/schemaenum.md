---
description: SchemaEnum
title: SchemaEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- SchemaEnum
helpviewer_keywords:
- SchemaEnum enumeration [ADO]
ms.assetid: 21c97651-297f-469f-b5b5-c48af72b62a8
author: rothja
ms.author: jroth
ms.openlocfilehash: 5a58781196393471255f2cb00eddd153373e0cd0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051488"
---
# <a name="schemaenum"></a>SchemaEnum
指定 [OpenSchema](./openschema-method.md)方法检索的架构 **记录集** 的类型。  
  
## <a name="remarks"></a>备注  
 有关为每个 ADO 常量返回的函数和列的其他信息，请参阅附录 B： OLE DB 程序员参考中的 [架构行集](/previous-versions/windows/desktop/ms712921(v=vs.85)) 。 下表的 "描述" 部分的括号中列出了每个主题的名称。  
  
 有关为每个 ADO MD 常量返回的函数和列的其他信息，请参阅 OLAP) 文档 (的联机分析处理 OLE DB 中的 [OLAP 对象和架构行集 OLE DB](/previous-versions/windows/desktop/ms723056(v=vs.85)) 中的相关主题。 下表的 "说明" 列中的括号内列出了每个主题的名称。  
  
 您可以通过参考 ADO [DataTypeEnum](./datatypeenum.md) 主题的 Description 列，将 OLE DB 文档中的列的数据类型转换为 ado 数据类型。 例如，OLE DB 的数据类型 **DBTYPE_WSTR** 等效于 ADO 数据类型 **adWChar**。  
  
 ADO 为常量、 **adSchemaDBInfoKeywords** 和 **adSchemaDBInfoLiterals** 生成类似于架构的结果。 ADO 创建一个 **记录集**，然后使用 **IDBInfo：： GetKeywords** 和 **IDBInfo：： GetLiteralInfo** 方法分别返回的值填充每一行。 有关这些方法的其他信息可在 OLE DB 程序员参考的 [IDBInfo](/previous-versions/windows/desktop/ms713663(v=vs.85)) 部分找到。  
  
|返回的常量|Value|说明|约束列|  
|--------------|-----------|-----------------|------------------------|  
|**adSchemaAsserts**|0|返回在目录中定义的由给定用户拥有的断言。<br /><br />  (断言行集) |CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaCatalogs**|1|返回与可从 DBMS 访问的目录关联的物理特性。<br /><br />  (目录行集) |CATALOG_NAME|  
|**adSchemaCharacterSets**|2|返回在目录中定义的、给定用户可以访问的字符集。<br /><br /> CHARACTER_SETS 行集 () |CHARACTER_SET_CATALOG CHARACTER_SET_SCHEMA CHARACTER_SET_NAME|  
|**adSchemaCheckConstraints**|5|返回在目录中定义的由给定用户拥有的 check 约束。<br /><br />  (CHECK_CONSTRAINTS) 行集) |CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaCollations**|3|返回在目录中定义的、给定用户可以访问的字符排序规则。<br /><br />  (排序规则行集) |COLLATION_CATALOG COLLATION_SCHEMA COLLATION_NAME|  
|**adSchemaColumnPrivileges**|13|返回在目录中定义的、给定用户可使用或授予的对表的列的特权。<br /><br /> COLUMN_PRIVILEGES 行集 () |TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME GRANTOR GRANTEE|  
|**adSchemaColumns**|4|返回表的各列 (包括在目录中定义的、给定用户可以访问的视图) 。<br /><br />  (列行集) |TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaColumnsDomainUsage**|11|返回在目录中定义、依赖于在目录中定义的域并由给定用户拥有的列。<br /><br /> COLUMN_DOMAIN_USAGE 行集 () |DOMAIN_CATALOG DOMAIN_SCHEMA DOMAIN_NAME COLUMN_NAME|  
|**adSchemaConstraintColumnUsage**|6|返回由引用约束、唯一约束、CHECK 约束和断言使用的、在目录中定义并由给定用户拥有的列。<br /><br /> CONSTRAINT_COLUMN_USAGE 行集 () |TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaConstraintTableUsage**|7|返回由引用约束、唯一约束、CHECK 约束和在目录中定义并由给定用户拥有的断言使用的表。<br /><br /> CONSTRAINT_TABLE_USAGE 行集 () |TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaCubes**|32|如果提供程序不支持架构) ，则返回有关架构 (或目录中的可用多维数据集的信息。<br /><br />  (多维数据集行集 * ) |CATALOG_NAME SCHEMA_NAME CUBE_NAME|  
|**adSchemaDBInfoKeywords**|30|返回提供程序特定的关键字的列表。<br /><br />  (IDBInfo：： GetKeywords) |\<None>|  
|**adSchemaDBInfoLiterals**|31|返回在文本命令使用的提供程序特定的文本的列表。<br /><br />  (IDBInfo：： GetLiteralInfo) |\<None>|  
|**adSchemaDimensions**|33|返回有关给定多维数据集中的维度的信息。 对于每个维度，都有一行。<br /><br />  (维度行集) |CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_NAME DIMENSION_UNIQUE_NAME|  
|**adSchemaForeignKeys**|27|返回由给定用户在目录中定义的外键列。<br /><br /> FOREIGN_KEYS 行集 () |PK_TABLE_CATALOG PK_TABLE_SCHEMA PK_TABLE_NAME FK_TABLE_CATALOG FK_TABLE_SCHEMA FK_TABLE_NAME|  
|**adSchemaHierarchies**|34|返回有关维度中可用层次结构的信息。<br /><br />  (层次结构行集) |CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_NAME HIERARCHY_UNIQUE_NAME|  
|**adSchemaIndexes**|12|返回在目录中定义的由给定用户拥有的索引。<br /><br />  (索引行集) |INDEX_NAME 类型 TABLE_SCHEMA TABLE_CATALOG TABLE_NAME|  
|**adSchemaKeyColumnUsage**|8|返回在目录中定义的、由给定用户约束为键的列。<br /><br /> KEY_COLUMN_USAGE 行集 () |CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME|  
|**adSchemaLevels**|35|返回有关维度中可用的级别的信息。<br /><br />  (级别行集) |CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_NAME LEVEL_UNIQUE_NAME|  
|**adSchemaMeasures**|36|返回有关可用度量值的信息。<br /><br />  (度量值行集) |CATALOG_NAME SCHEMA_NAME CUBE_NAME MEASURE_NAME MEASURE_UNIQUE_NAME|  
|**adSchemaMembers**|38|返回有关可用成员的信息。<br /><br />  (成员行集) |CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_UNIQUE_NAME LEVEL_NUMBER MEMBER_NAME MEMBER_UNIQUE_NAME MEMBER_CAPTION MEMBER_TYPE 树操作员。 有关详细信息，请参阅联机分析处理 (OLAP) OLE DB。|  
|**adSchemaPrimaryKeys**|28|返回在目录中由给定用户定义的主键列。<br /><br /> PRIMARY_KEYS 行集 () |PK_TABLE_CATALOG PK_TABLE_SCHEMA PK_TABLE_NAME|  
|**adSchemaProcedureColumns**|29|返回有关由过程返回的行集合的列的信息。<br /><br /> PROCEDURE_COLUMNS 行集 () |PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME COLUMN_NAME|  
|**adSchemaProcedureParameters**|26|返回有关过程的参数和返回代码的信息。<br /><br /> PROCEDURE_PARAMETERS 行集 () |PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME PARAMETER_NAME|  
|**adSchemaProcedures**|16|返回在目录中定义的由给定用户拥有的过程。<br /><br />  (过程行集) |PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME PROCEDURE_TYPE|  
|**adSchemaProperties**|37|返回有关维度的每个级别的可用属性的信息。<br /><br />  (属性行集) |CATALOG_NAME SCHEMA_NAME CUBE_NAME DIMENSION_UNIQUE_NAME HIERARCHY_UNIQUE_NAME LEVEL_UNIQUE_NAME MEMBER_UNIQUE_NAME PROPERTY_TYPE PROPERTY_NAME|  
|**adSchemaProviderSpecific**|-1|如果提供程序定义了其自己的非标准架构查询，则使用。|\<Provider specific>|  
|**adSchemaProviderTypes**|22|返回数据提供程序支持的 (基) 数据类型。<br /><br /> PROVIDER_TYPES 行集 () |DATA_TYPE BEST_MATCH|  
|**AdSchemaReferentialConstraints**|9|返回在目录中定义的由给定用户拥有的引用约束。<br /><br /> REFERENTIAL_CONSTRAINTS 行集 () |CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME|  
|**adSchemaSchemata**|17|返回给定用户拥有 (数据库对象) 的架构。<br /><br />  (架构行集) |CATALOG_NAME SCHEMA_NAME SCHEMA_OWNER|  
|**adSchemaSQLLanguages**|18|返回在目录中定义的 SQL 实现处理数据所支持的一致性级别、选项和语言。<br /><br /> SQL_LANGUAGES 行集 () |\<None>|  
|**adSchemaStatistics**|19|返回在目录中定义的由给定用户拥有的统计信息。<br /><br />  (统计信息行集) |TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaTableConstraints**|10|返回在目录中定义的由给定用户拥有的表约束。<br /><br /> TABLE_CONSTRAINTS 行集 () |CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME TABLE_CATALOG TABLE_SCHEMA TABLE_NAME CONSTRAINT_TYPE|  
|**adSchemaTablePrivileges**|14|返回在目录中定义、给定用户可使用或负责授予的对表的特权。<br /><br /> TABLE_PRIVILEGES 行集 () |TABLE_CATALOG TABLE_SCHEMA TABLE_NAME GRANTOR GRANTEE|  
|**adSchemaTables**|20|返回在目录中定义的表（包括视图），给定用户可以访问它们。<br /><br />  (表行集) |TABLE_CATALOG TABLE_SCHEMA TABLE_NAME TABLE_TYPE|  
|**adSchemaTranslations**|21|返回在目录中定义的、给定用户可以访问的字符转换。<br /><br />  (翻译行集) |TRANSLATION_CATALOG TRANSLATION_SCHEMA TRANSLATION_NAME|  
|**adSchemaTrustees**|39|留待将来使用。||  
|**adSchemaUsagePrivileges**|15|返回在目录中定义的、给定用户可使用或授予的对象上的使用权限。<br /><br /> USAGE_PRIVILEGES 行集 () |OBJECT_CATALOG OBJECT_SCHEMA OBJECT_NAME OBJECT_TYPE 授予者|  
|**adSchemaViewColumnUsage**|24|返回在目录中定义并由给定用户拥有的已查看的表所依赖的列。<br /><br /> VIEW_COLUMN_USAGE 行集 () |VIEW_CATALOG VIEW_SCHEMA VIEW_NAME|  
|**adSchemaViews**|23|返回在目录中定义的、给定用户可以访问的视图。<br /><br />  (视图行集) |TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|**adSchemaViewTableUsage**|25|返回已查看的表所依赖、在目录中定义并由给定用户拥有的表。<br /><br /> VIEW_TABLE_USAGE 行集 () |VIEW_CATALOG VIEW_SCHEMA VIEW_NAME|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums|  
|AdoEnums|  
|AdoEnums. CHARACTERSETS|  
|AdoEnums. CHECKCONSTRAINTS|  
|AdoEnums|  
|AdoEnums. COLUMNPRIVILEGES|  
|AdoEnums|  
|AdoEnums. COLUMNSDOMAINUSAGE|  
|AdoEnums. CONSTRAINTCOLUMNUSAGE|  
|AdoEnums. CONSTRAINTTABLEUSAGE|  
|AdoEnums|  
|AdoEnums. DBINFOKEYWORDS|  
|AdoEnums. DBINFOLITERALS|  
|AdoEnums|  
|AdoEnums. FOREIGNKEYS|  
|AdoEnums|  
|AdoEnums|  
|AdoEnums. KEYCOLUMNUSAGE|  
|AdoEnums|  
|AdoEnums|  
|AdoEnums|  
|AdoEnums. PRIMARYKEYS|  
|AdoEnums. PROCEDURECOLUMNS|  
|AdoEnums. PROCEDUREPARAMETERS|  
|AdoEnums|  
|AdoEnums|  
|AdoEnums. PROVIDERSPECIFIC|  
|AdoEnums. PROVIDERTYPES|  
|AdoEnums. REFERENTIALCONTRAINTS|  
|AdoEnums|  
|AdoEnums. SQLLANGUAGES|  
|AdoEnums|  
|AdoEnums. TABLECONSTRAINTS|  
|AdoEnums. TABLEPRIVILEGES|  
|AdoEnums|  
|AdoEnums|  
|AdoEnums|  
|AdoEnums. USAGEPRIVILEGES|  
|AdoEnums. VIEWCOLUMNUSAGE|  
|AdoEnums|  
|AdoEnums. VIEWTABLEUSAGE|  
  
## <a name="applies-to"></a>应用于  
 [OpenSchema 方法](./openschema-method.md)