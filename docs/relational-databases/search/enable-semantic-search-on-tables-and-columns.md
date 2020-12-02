---
description: 对表和列启用语义搜索
title: 对表和列启用语义搜索 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- semantic search [SQL Server], enabling
ms.assetid: 895d220c-6749-4954-9dd3-2ea4c6a321ff
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: d02424e5e33823956977c8b32d1ab4e996df5526
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130929"
---
# <a name="enable-semantic-search-on-tables-and-columns"></a>对表和列启用语义搜索
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  介绍如何启用或禁用包含文档或文本的选定列上的统计语义索引。  
  
 统计语义搜索使用全文搜索创建的索引并创建其他索引。 作为全文搜索上的此依赖关系的结果，您可在定义新的全文索引或更改现有全文索引时创建一个新的语义索引。 可以按本主题中所述使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句或使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的全文索引向导和其他对话框创建新的语义索引。  
  
##  <a name="create-a-semantic-index"></a><a name="BasicEnabling"></a> 创建语义索引  
  
###  <a name="requirements-and-restrictions-for-creating-a-semantic-index"></a><a name="reqenable"></a> 创建语义索引的要求和限制  
  
-   可以对任何支持全文索引的数据库对象创建索引，包括表和索引视图。  
  
-   在您可以启用特定列的语义索引之前，必须满足下列先决条件：  
  
    -   数据库必须具有全文目录。  
  
    -   表必须具有全文索引。  
  
    -   选定列必须参与全文索引。  
  
     您可以同时创建并启用所有这些要求。  
  
-   可以对具有任何支持全文索引的数据类型的列创建语义索引。 有关详细信息，请参阅 [创建和管理全文索引](../../relational-databases/search/create-and-manage-full-text-indexes.md)。  
  
-   你可为 **varbinary(max)** 列指定支持全文索引的任何文档类型。 有关详细信息，请参阅本主题中的 [如何确定可对哪些文档类型编制索引](#doctypes) 。  
  
-   语义索引为你选择的列创建两种类型的索引：关键短语索引和文档相似性索引。 启用语义索引时，您无法只选择其中一种索引类型。 但是，您可以单独查询这两种索引。 有关详细信息，请参阅 [使用语义搜索在文档中查找关键短语](../../relational-databases/search/find-key-phrases-in-documents-with-semantic-search.md) 和 [使用语义搜索查找相似和相关文档](../../relational-databases/search/find-similar-and-related-documents-with-semantic-search.md)。  
  
-   如果未显式指定语义索引的 LCID，则仅将主要语言及其关联的语言统计信息用于语义索引编制。  
  
-   如果为缺少语言模型的列指定语言，创建索引将失败并返回错误消息。  
  
##  <a name="create-a-semantic-index-when-there-is-no-full-text-index"></a><a name="HowToEnableCreate"></a> 在没有全文索引时创建语义索引  
 使用 **CREATE FULLTEXT INDEX** 语句创建新的全文索引时，可以通过指定关键字 **STATISTICAL_SEMANTICS** 作为列定义的一部分在列级别启用语义索引。 还可以在使用全文索引向导创建新的全文索引时启用语义索引。  
  
 ### <a name="create-a-new-semantic-index-by-using-transact-sql"></a>使用 Transact-SQL 创建新的语义索引  
 
 为要创建语义索引的每个列调用 **CREATE FULLTEXT INDEX** 语句并指定 **STATISTICAL_SEMANTICS**。 有关此语句的所有选项的详细信息，请参阅 [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md)。  
  
 **示例 1：创建一个唯一索引、全文索引和语义索引**  
  
 以下示例创建一个默认全文目录 **ft**.。然后，该示例对 AdventureWorks2012 示例数据库的 **HumanResources.JobCandidate** 表的 **JobCandidateID** 列创建一个唯一索引。 需要将此唯一索引用作全文索引的键列。 然后，该示例在 **Resume** 列上创建一个全文索引和语义索引。  
  
```sql  
CREATE FULLTEXT CATALOG ft AS DEFAULT  
GO  
  
CREATE UNIQUE INDEX ui_ukJobCand  
    ON HumanResources.JobCandidate(JobCandidateID)  
GO  
  
CREATE FULLTEXT INDEX ON HumanResources.JobCandidate  
    (Resume  
        Language 1033  
        Statistical_Semantics  
    )   
    KEY INDEX JobCandidateID   
    WITH STOPLIST = SYSTEM  
GO  
```  
  
 **示例 2：对具有延迟索引填充的几个列创建一个全文索引和语义索引**  
  
 以下示例在 AdventureWorks2012 示例数据库中创建一个全文目录 **documents_catalog**。 然后，该示例创建一个使用该新目录的全文索引。 全文索引针对 **Production.Document** 表的 **Title**、 **DocumentSummary** 和 **Document** 列创建，而语义索引仅针对 **Document** 列创建。 此全文索引使用新创建的全文目录和现有的唯一键索引 **PK_Document_DocumentID**。 根据建议，此索引键在整数列 **DocumentID** 上创建。 该示例指定英语的 LCID 1033，这是列中数据的语言。  
  
 该示例还指定关闭更改跟踪并且不进行填充。 随后，在非峰值时间，该示例使用 **ALTER FULLTEXT INDEX** 语句对新索引开始进行完全填充，并启用自动更改跟踪。  
  
```sql  
CREATE FULLTEXT CATALOG documents_catalog  
GO  
  
CREATE FULLTEXT INDEX ON Production.Document  
    (   
    Title  
        Language 1033,   
    DocumentSummary  
        Language 1033,   
    Document   
        TYPE COLUMN FileExtension  
        Language 1033  
        Statistical_Semantics  
    )  
    KEY INDEX PK_Document_DocumentID  
        ON documents_catalog  
        WITH CHANGE_TRACKING OFF, NO POPULATION  
GO  
```  
  
 随后，在非峰值时间，填充索引：  
  
```sql  
ALTER FULLTEXT INDEX ON Production.Document SET CHANGE_TRACKING AUTO  
GO  
```  
  
### <a name="create-a-new-semantic-index-by-using-sql-server-management-studio"></a>使用 SQL Server Management Studio 创建新的语义索引  
 运行全文索引向导并在“选择表列” 页为每个要创建语义索引的列启用“统计语义” 。 有关详细信息，包括有关如何启动全文索引向导的信息，请参阅 [使用全文索引向导](../../relational-databases/search/use-the-full-text-indexing-wizard.md)。  
  
##  <a name="create-a-semantic-index-when-there-is-an-existing-full-text-index"></a><a name="HowToEnableAlter"></a> 在存在现有全文索引时创建语义索引  
 在使用 **ALTER FULLTEXT INDEX** 语句更改现有全文索引时，可以添加语义索引。 您还可在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中使用各种对话框添加语义索引。  
  
### <a name="add-a-semantic-index-by-using-transact-sql"></a>使用 Transact-SQL 添加语义索引  
 为每个要添加语义索引的列使用以下所述的选项调用 **ALTER FULLTEXT INDEX** 语句。 有关此语句的所有选项的详细信息，请参阅 [ ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)。  
  
 除非你特别指定，否则在调用 **ALTER** 后，将重新填充全文索引和语义索引。  
  
-   若要仅将全文索引添加到列，请使用 **ADD** 语法。  
  
-   若要同时将全文索引和语义索引添加到列，请使用带 **STATISTICAL_SEMANTICS** 选项的 **ADD** 语法。  
  
-   若要将语义索引添加到已启用全文索引的列，请使用 **ADD STATISTICAL_SEMANTICS** 选项。 在单个 **ALTER** 语句中只能将语义索引添加到一个列。  
  
 **示例：将语义索引添加到已有全文索引的列**  
  
 以下示例更改 AdventureWorks2012 示例数据库的 **Production.Document** 表的现有全文索引。 该示例对 **Production.Document** 表的 **Document** 列添加语义索引，该列已有全文索引。 该示例指定将不自动重新填充索引。  
  
```sql  
ALTER FULLTEXT INDEX ON Production.Document  
    ALTER COLUMN Document  
        ADD Statistical_Semantics  
    WITH NO POPULATION  
GO  
```  
  
### <a name="add-a-semantic-index-by-using-sql-server-management-studio"></a>使用 SQL Server Management Studio 添加语义索引  
 可以在“全文索引属性”对话框的“全文索引列”页上更改启用语义索引和全文索引的列。 有关详细信息，请参阅 [管理全文索引](./create-and-manage-full-text-indexes.md)。  

## <a name="alter-a-semantic-index"></a>更改语义索引
  
###  <a name="requirements-and-restrictions-for-altering-an-existing-index"></a><a name="addreq"></a> 更改现有索引的要求和限制  
  
-   正在填充索引时，不能更改现有索引。 有关监视索引填充进度的详细信息，请参阅 [管理和监视语义搜索](../../relational-databases/search/manage-and-monitor-semantic-search.md)。  
  
-   在 **ALTER FULLTEXT INDEX** 语句的单次调用中，不能将索引添加到列以及更改或删除同一列的索引。  
  
##  <a name="drop-a-semantic-index"></a><a name="dropping"></a> 删除语义索引  
在使用 **ALTER FULLTEXT INDEX** 语句更改现有全文索引时，可以删除语义索引。 您还可在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中使用各种对话框删除语义索引。  
  
 ### <a name="drop-a-semantic-index-by-using-transact-sql"></a>使用 Transact-SQL 删除语义索引  
要仅从一个或多个列删除语义索引，请使用 ALTER COLUMNcolumn\_nameDROP STATISTICAL_SEMANTICS 选项调用 ALTER FULLTEXT INDEX 语句。 可以在单个 **ALTER** 语句中从多个列删除索引。  
  
```sql  
USE database_name  
GO  

ALTER FULLTEXT INDEX  
    ALTER COLUMN column_name  
    DROP STATISTICAL_SEMANTICS  
GO  
```  
  
要从一列同时删除全文索引和语义索引，请使用 ALTER COLUMNcolumn\_nameDROP 选项调用 ALTER FULLTEXT INDEX 语句。  
  
```sql  
USE database_name  
GO  
  
ALTER FULLTEXT INDEX  
    ALTER COLUMN column_name  
    DROP  
GO  
```  
  
 ### <a name="drop-a-semantic-index-by-using-sql-server-management-studio"></a>使用 SQL Server Management Studio 删除语义索引  
 可以在“全文索引属性”对话框的“全文索引列”页上更改启用语义索引和全文索引的列。 有关详细信息，请参阅 [管理全文索引](./create-and-manage-full-text-indexes.md)。  
  
###  <a name="requirements-and-restrictions-for-dropping-a-semantic-index"></a><a name="dropreq"></a> 删除语义索引的要求和限制  
  
-   保留语义索引时不能从列删除全文索引。 对于文档相似性结果，语义索引依赖于全文索引。  
  
-   在从表中启用语义索引的最后一个列中删除语义索引时，不能指定 **NO POPULATION** 选项。 需要一个填充周期来删除以前编制索引的结果。  
  
## <a name="check-whether-semantic-search-is-enabled-on-database-objects"></a>检查是否在数据库对象上启用了语义搜索  
### <a name="is-semantic-search-enabled-for-a-database"></a>是否为数据库启用了语义搜索？
  
 查询 [DATABASEPROPERTYEX (Transact-SQL)](../../t-sql/functions/databasepropertyex-transact-sql.md) 元数据函数的 **IsFullTextEnabled** 属性。  
  
 返回值 1 表示为数据库启用了全文搜索和语义搜索；返回值 0 表示未启用它们。  
  
```sql  
SELECT DATABASEPROPERTYEX('database_name', 'IsFullTextEnabled')  
GO  
```  
  
### <a name="is-semantic-search-enabled-for-a-table"></a>是否为表启用了语义搜索？  
 
 查询 [OBJECTPROPERTYEX (Transact-SQL)](../../t-sql/functions/objectpropertyex-transact-sql.md) 元数据函数的 **TableFullTextSemanticExtraction** 属性。  
  
 返回值 1 表示为表启用了语义搜索；返回值 0 表示未启用它。  
  
```scr  
SELECT OBJECTPROPERTYEX(OBJECT_ID('table_name'), 'TableFullTextSemanticExtraction')  
GO  
```  
  
 ### <a name="is-semantic-search-enabled-for-a-column"></a>是否为列启用了语义搜索？
   
 若要确定是否为特定列启用了语义搜索：  
  
-   查询 [COLUMNPROPERTY (Transact-SQL)](../../t-sql/functions/columnproperty-transact-sql.md) 元数据函数的 **StatisticalSemantics** 属性。  
  
     返回值 1 表示为列启用了语义搜索；返回值 0 表示未启用它。  
  
    ```sql  
    SELECT COLUMNPROPERTY(OBJECT_ID('table_name'), 'column_name', 'StatisticalSemantics')  
    GO  
    ```  
  
-   查询目录视图 [sys.fulltext_index_columns (Transact SQL)](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md) 以获取全文索引。  
  
     **statistical_semantics** 列中的值 1 表示除了启用全文索引编制外，还为指定的列启用了语义索引编制。  
  
    ```sql  
    SELECT * FROM sys.fulltext_index_columns WHERE object_id = OBJECT_ID('table_name')  
    GO  
    ```  
  
-   在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 的对象资源管理器中，右键单击一个列，然后选择“属性”。 在 **“列属性”** 对话框的 **“常规”** 页上，查看 **“统计语义”** 属性的值。  
  
     值 True 表示除了启用全文索引外，还为指定的列启用了语义索引。  
  
## <a name="determine-what-can-be-indexed-for-semantic-search"></a>确定可对哪些内容编制索引以进行语义搜索  
  
###  <a name="check-which-languages-are-supported-for-semantic-search"></a><a name="HowToCheckLanguages"></a> 检查语义搜索支持的语言  
  
> [!IMPORTANT]  
>  语义索引比全文索引支持的语言少。 因此，您可以对某些列编制索引以进行全文搜索，但不能进行语义搜索。  
  
 查询目录视图 [sys.fulltext_semantic_languages (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-semantic-languages-transact-sql.md)。  
  
```sql  
SELECT * FROM sys.fulltext_semantic_languages  
GO  
```  
  
 语义索引支持以下语言。 此列表按 LCID 列出了目录视图 [sys.fulltext_semantic_languages (Transact SQL)](../../relational-databases/system-catalog-views/sys-fulltext-semantic-languages-transact-sql.md) 的输出。  
  
|语言|LCID|  
|--------------|----------|  
|德语|1031|  
|英语(美国)|2052|  
|法语|1036|  
|意大利语|1040|  
|葡萄牙语（巴西）|1046|  
|俄语|1049|  
|瑞典语|1053|  
|英语(英国)|2057|  
|葡萄牙语(葡萄牙)|2070|  
|西班牙语|3082|  
  
###  <a name="determine-which-document-types-can-be-indexed"></a><a name="doctypes"></a> 确定可对哪些文档类型编制索引  
 查询目录视图 [sys.fulltext_document_types (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md)。  
  
 如果您要为其编制索引的文档类型不在所支持类型的列表中，则可能必须查找、下载和安装其他筛选器。 有关详细信息，请参阅 [查看或更改注册的筛选器和断字符](../../relational-databases/search/view-or-change-registered-filters-and-word-breakers.md)。  
  
##  <a name="best-practice-consider-creating-a-separate-filegroup-for-the-full-text-and-semantic-indexes"></a><a name="BestPracticeFilegroup"></a> 最佳做法：考虑为全文索引和语义索引创建单独的文件组  
 如果磁盘空间分配成问题，请考虑为全文索引和语义索引创建单独的文件组。 在全文索引所在的文件组中创建语义索引。 完全填充的语义索引可能包含大量数据。  
 
##  <a name="issue-searching-on-specific-column-returns-no-results"></a><a name="IssueNoResults"></a> 问题：搜索特定列时未返回结果  
 **是否为 Unicode 语言指定了非 Unicode LCID？**  
 可能对非 Unicode 列类型启用了语义索引，而该列的 LCID 用于只有 Unicode 词的语言，如用于俄语的 LCID 1049。 在这种情况下，将不从此列的语义索引返回结果。  
  
