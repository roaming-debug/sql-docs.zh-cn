---
description: 创建和管理全文索引
title: 创建和管理全文索引 | Microsoft Docs
ms.custom: ''
ms.date: 03/31/2020
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text indexes [SQL Server], about
ms.assetid: f8a98486-5438-44a8-b454-9e6ecbc74f83
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 501be7ed93ec7be0edd55d95bf7bd0b5bce229d8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460090"
---
# <a name="create-and-manage-full-text-indexes"></a>创建和管理全文索引
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
本主题介绍了如何在 SQL Server 中创建、填充和管理全文索引。
  
## <a name="prerequisite---create-a-full-text-catalog"></a>先决条件 - 创建全文目录
必须具有全文目录，然后才能创建全文索引。 目录是包含一个或多个全文索引的虚拟容器。 有关详细信息，请参阅[创建和管理全文目录](../../relational-databases/search/create-and-manage-full-text-catalogs.md)。
  
##  <a name="create-alter-or-drop-a-full-text-index"></a><a name="tasks"></a> 创建、更改或删除全文索引  
### <a name="create-a-full-text-index"></a>创建全文索引  
  
-   [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md)  
  
### <a name="alter-a-full-text-index"></a>更改全文索引
  
-   [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
### <a name="drop-a-full-text-index"></a>删除全文索引 
  
-   [DROP FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/drop-fulltext-index-transact-sql.md)

## <a name="populate-a-full-text-index"></a>填充全文索引
创建和维护全文索引的过程称为“填充”  （也称为“爬网”  ）。 有三种类型的全文索引填充：
-   完全填充
-   基于更改跟踪的填充
-   基于时间戳的增量填充。

有关详细信息，请参阅[填充全文索引](../../relational-databases/search/populate-full-text-indexes.md)。

##  <a name="view-the-properties-of-a-full-text-index"></a><a name="view"></a> 查看全文索引的属性
### <a name="view-the-properties-of-a-full-text-index-with-transact-sql"></a>使用 Transact-SQL 查看全文索引的属性

|目录视图或动态管理视图|说明|  
|----------------------------------------|-----------------|  
|[sys.fulltext_index_catalog_usages (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-index-catalog-usages-transact-sql.md)|对于全文索引引用的每个全文目录，返回与其对应的一行。|  
|[sys.fulltext_index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)|对构成全文索引的每列都包含一行。|  
|[sys.fulltext_index_fragments (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md)|全文索引使用内部表（称为“全文索引片断”）来存储倒排索引数据。 可以使用此视图来查询有关这些片断的元数据。 在此视图中，每个全文索引片断在每个包含全文索引的表中各占一行。|  
|[sys.fulltext_indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)|表对象的每个全文索引各占一行。|  
|[sys.dm_fts_index_keywords (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)|返回有关指定表的全文索引内容的信息。|  
|[sys.dm_fts_index_keywords_by_document (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)|返回有关指定表的文档级全文索引内容的信息。 给定关键字可以出现在几个文档中。|  
|[sys.dm_fts_index_population (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)|返回有关当前正在进行的全文索引填充的信息。|  
 
### <a name="view-the-properties-of-a-full-text-index-with-management-studio"></a>使用 Management Studio 查看全文索引的属性 
1.  在 Management Studio 的对象资源管理器中展开服务器。  
  
2.  展开“数据库”，然后展开包含全文索引的数据库。  
  
3.  展开 **“表”** 。  
  
4.  右键单击对其定义了全文索引的表，选择“全文索引”，然后在“全文索引”上下文菜单中单击“属性”。 此时将打开“全文索引属性”  对话框。  
  
5.  在 **“选择页”** 窗格中，您可以选择下列页中的任一页：  
  
    |页|说明|  
    |----------|-----------------|  
    |**常规**|显示全文索引的基本属性。 这些基本属性包括若干个可修改属性和多个不可更改属性，后者如数据库名称、表名和全文键列的名称。 可修改属性包括：<br /><br /> **全文索引非索引字表**<br /><br /> **全文索引已启用**<br /><br /> **更改跟踪**<br /><br /> **搜索属性列表**|  
    |**“列”**|显示可用于全文索引的表列。 对于选中的列，均会创建全文索引。 您可以根据需要选择将任意数目的可用列包括在全文索引中。 有关详细信息，请参阅[填充全文索引](populate-full-text-indexes.md)。|
    |**计划**|使用此页可以创建或管理 SQL Server 代理作业的计划，该作业用于启动全文索引填充的表增量填充。 有关详细信息，请参阅[填充全文索引](../../relational-databases/search/populate-full-text-indexes.md)。<br /><br /> 注意：在退出“全文检索属性”对话框之后，所有新创建的计划都将与 SQL Server 代理作业（对 database_name.table_name 启动表增量填充）相关联    。|  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] 以保存任何更改并退出“全文索引属性”对话框。   
  
##  <a name="view-the-properties-of-indexed-tables-and-columns"></a><a name="props"></a> 查看索引表和列的属性  
 一些 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数（例如 OBJECTPROPERTYEX）可用来获取各种全文索引属性的值。 此信息可用于全文搜索的管理和故障排除。  
  
 下表列出了与索引表和列相关的全文属性及其相关 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数。  
  
|properties|说明|函数|  
|--------------|-----------------|--------------|  
|**FullTextTypeColumn**|表中的 TYPE COLUMN，其中包含列的文档类型信息。|[COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md)|  
|**IsFulltextIndexed**|列是否启用了全文索引。|COLUMNPROPERTY|  
|**IsFulltextKey**|索引是否为表的全文键。|[INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md)|  
|**TableFulltextBackgroundUpdateIndexOn**|表是否具有全文后台更新索引。|[OBJECTPROPERTYEX](../../t-sql/functions/objectpropertyex-transact-sql.md)|  
|**TableFulltextCatalogId**|表的全文索引数据所在的全文目录 ID。|OBJECTPROPERTYEX|  
|**TableFulltextChangeTrackingOn**|表是否启用了全文更改跟踪。|OBJECTPROPERTYEX|  
|**TableFulltextDocsProcessed**|自开始全文检索以来所处理的行数。|OBJECTPROPERTYEX|  
|**TableFulltextFailCount**|全文搜索未编制索引的行数。|OBJECTPROPERTYEX|  
|**TableFulltextItemCount**|成功编制了全文索引的行数。|OBJECTPROPERTYEX|  
|**TableFulltextKeyColumn**|全文唯一键列的列 ID。|OBJECTPROPERTYEX|  
|**TableFullTextMergeStatus**|具有全文索引的表当前是否正在合并。|OBJECTPROPERTYEX|  
|**TableFulltextPendingChanges**|要处理的挂起更改跟踪项的数目。|OBJECTPROPERTYEX|  
|**TableFulltextPopulateStatus**|全文表的填充状态。|OBJECTPROPERTYEX|  
|**TableHasActiveFulltextIndex**|表是否具有活动的全文索引。|OBJECTPROPERTYEX|  
  
##  <a name="get-info-about-the-full-text-key-column"></a><a name="key"></a> 获取关于全文键列的信息  
 通常情况下，CONTAINSTABLE 或 FREETEXTTABLE 行集值函数的结果需要与基表相联接。 在这样的情况下，需要知道唯一键列名称。 可以查询给定的唯一索引是否作为全文键使用，并且可以获取全文键列的标识符。  
  
### <a name="determine-whether-a-given-unique-index-is-used-as-the-full-text-key-column"></a>确定给定的唯一索引是否用作全文键列  
  
使用 [SELECT](../../t-sql/queries/select-transact-sql.md) 语句调用 [INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md) 函数。 在此函数的调用过程中，使用 OBJECT_ID 函数将表名 (*table_name*) 转换为表 ID，指定该表的唯一索引的名称，然后指定 **IsFulltextKey** 索引属性，如下所示：  
  
```sql  
SELECT INDEXPROPERTY( OBJECT_ID('table_name'), 'index_name',  'IsFulltextKey' );  
```  
  
 如果使用此索引来强制实现全文键列的唯一性，此语句返回 1，否则返回 0。  
  
 **示例**  
  
 下例查询 `PK_Document_DocumentNode` 索引是否用于强制实现全文键列的唯一性，如下所示：  
  
```sql  
USE AdventureWorks  
GO  
SELECT INDEXPROPERTY ( OBJECT_ID('Production.Document'), 'PK_Document_DocumentNode',  'IsFulltextKey' )  
```  
  
 如果使用 `PK_Document_DocumentNode` 索引来强制实现全文键列的唯一性，则此示例返回 1。 否则，它返回 0 或 NULL。 NULL 表示您使用的是无效索引名称，索引名称与表不对应，或表不存在，等等。  
  
### <a name="find-the-identifier-of-the-full-text-key-column"></a>查找全文键列的标识符  
  
每个启用全文的表都有一个列，该列用于强制实现表中行的唯一性（“唯一键列”）  。 从 OBJECTPROPERTYEX 函数获取的 **TableFulltextKeyColumn** 属性包含唯一键列的列 ID。  
 
若要获取此标识符，可以使用 SELECT 语句调用 OBJECTPROPERTYEX 函数。 使用 OBJECT_ID 函数将表名 (*table_name*) 转换为表 ID，并指定 **TableFulltextKeyColumn** 属性，如下所示：  
  
```sql  
SELECT OBJECTPROPERTYEX(OBJECT_ID( 'table_name'), 'TableFulltextKeyColumn' ) AS 'Column Identifier';  
```  
  
 **示例**  
  
 下例返回全文键列的标识符或 NULL。 NULL 表示您使用的是无效索引名称，索引名称与表不对应，或表不存在，等等。  
  
```sql
USE AdventureWorks;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID('Production.Document'), 'TableFulltextKeyColumn');  
GO  
```  
  
 下例说明如何使用唯一键列的标识符获取列的名称。  
  
```sql  
USE AdventureWorks;  
GO  
DECLARE @key_column sysname  
SET @key_column = Col_Name(Object_Id('Production.Document'),  
ObjectProperty(Object_id('Production.Document'),  
'TableFulltextKeyColumn')   
)  
SELECT @key_column AS 'Unique Key Column';  
GO  
```  
  
 此示例返回名为“`Unique Key Column`”的结果集列，其中显示一行，行内包含 Document 表的唯一键列 DocumentNode 的名称。 请注意，如果此查询包含无效的索引名称，索引名称与表不对应或表不存在等，它将返回 NULL。  

## <a name="index-varbinarymax-and-xml-columns"></a>索引 varbinary(max) 和 xml 列  
 如果 **varbinary(max)** 、 **varbinary** 或 **xml** 列是全文索引列，则与任何其他全文索引列一样，可以使用全文谓词（CONTAINS 和 FREETEXT）以及函数（CONTAINSTABLE 和 FREETEXTTABLE）来查询该列。
   
### <a name="index-varbinarymax-or-varbinary-data"></a>索引 varbinary(max) 或 varbinary 数据  
 单个 **varbinary(max)** 或 **varbinary** 列可存储多种类型的文档。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持安装了相应筛选器并且在操作系统中可用的任何文档类型。 每个文档的文档类型由该文档的文件扩展名标识。 例如，对于 .doc 文件扩展名，全文搜索将使用支持 Microsoft Word 文档的筛选器。 有关可用文档类型的列表，请查询 [sys.fulltext_document_types](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md) 目录视图。  
  
请注意，全文引擎可以利用操作系统中安装的现有筛选器。 在您可以使用操作系统筛选器、断字符和词干分析器之前，您必须将它们加载到服务器实例中，如下所示：  
  
```sql  
EXEC sp_fulltext_service @action='load_os_resources', @value=1  
```  
  
若要对 **varbinary(max)** 列创建全文索引，全文引擎需要访问 **varbinary(max)** 列中文档的文件扩展名。 此信息必须存储在一个称为“类型列”的表列中，该列必须与全文索引中的 **varbinary(max)** 列相关联。 在为文档创建索引时，全文引擎将使用类型列中的文件扩展名来标识要使用的筛选器。  
   
### <a name="index-xml-data"></a>索引 xml 数据  
 **xml** 数据类型列仅存储 XML 文档和片段，并且只有 XML 筛选器用于此类文档。 因此，无需类型列。 在 **xml** 列上，全文索引会为 XML 元素的内容创建索引，但会忽略 XML 标记。 不为数值的属性值都会进行全文索引。 元素标记用作标记边界。 支持包含多种语言的格式正确的 XML 或 HTML 文档和片段。  
  
 有关 **xml** 列的索引编制和查询的详细信息，请参阅 [结合使用全文搜索和 XML 列](../../relational-databases/xml/use-full-text-search-with-xml-columns.md)。  
  
##  <a name="disable-or-re-enable-full-text-indexing-for-a-table"></a><a name="disable"></a> 为表禁用或重新启用全文索引   
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，默认情况下所有由用户创建的数据库都启用了全文索引。 另外，在为表创建全文索引并将列添加到索引之后，就会自动为单个表启用全文索引。 从表的全文索引中删除最后一列时，会自动为表禁用全文索引。  
  
 对于具有全文索引的表，可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]手动为表禁用或重新启用全文索引。  

1.  展开服务器组，展开  “数据库”，再展开包含要为其启用全文索引的表的数据库。  
  
2.  展开“表”  ，然后右键单击要为其禁用或重新启用全文索引的表。  
  
3.  选择  “全文索引”，然后单击  “禁用全文索引”或  “启用全文索引”。  
  
##  <a name="remove-a-full-text-index-from-a-table"></a><a name="remove"></a> 从表中删除全文索引  
  
1.  在对象资源管理器中，右键单击要删除的全文索引所在的表。  
  
2.  选择  “删除全文索引”。  
  
3.  在出现提示时，单击  “确定”，确认是否要删除该全文索引。  
  
  
