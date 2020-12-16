---
description: 全文搜索入门
title: 全文搜索入门 | Microsoft Docs
ms.date: 03/31/2020
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text catalogs [SQL Server], creating
- full-text indexes [SQL Server], creating
- full-text search [SQL Server], about
- full-text search [SQL Server], setting up
ms.assetid: 1fa628ba-0ee4-4d8f-b086-c4e52962ca4a
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ffb6aefd828d9e329df6b624669f7d025d6044f7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479518"
---
# <a name="get-started-with-full-text-search"></a>全文搜索入门
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
SQL Server 数据库默认已启用全文搜索。 但是，在运行全文查询之前，必须先创建全文目录，然后在要搜索的表或索引视图中创建全文索引。

## <a name="set-up-full-text-search-in-two-steps"></a>通过两个步骤设置全文搜索
可通过两个基本步骤来设置全文搜索：  
1.  创建全文目录。  
2.  在要搜索的表或索引视图上创建全文索引。 

每个全文索引必须属于一个全文目录。 您可以为每个全文索引创建一个单独的文本目录，也可以将多个全文索引与给定目录关联起来。 全文目录是虚拟对象，不属于任何文件组。 该目录是表示一组全文索引的逻辑概念。

> [!NOTE]
> 这些步骤假设安装 SQL Server 时已安装可选的全文搜索组件。 如果未安装，必须再次运行 SQL Server 安装程序来添加该组件。  

## <a name="set-up-full-text-search-with-a-wizard"></a>使用向导设置全文搜索 
 
若要使用向导设置全文搜索，请参阅[使用全文索引向导](../../relational-databases/search/use-the-full-text-indexing-wizard.md)。

## <a name="set-up-full-text-search-with-transact-sql"></a>使用 Transact-SQL 设置全文搜索 
 下面的示例由两个部分组成，首先在 AdventureWorks 示例数据库中创建名为 `AdvWksDocFTCat` 的全文目录，然后在示例数据库中的 `Document` 表内创建全文索引。 此语句在安装 SQL Server 期间指定的默认目录中创建全文目录。 将在默认目录下创建名为 `AdvWksDocFTCat` 的文件夹。  
  
1.  为了创建名为 `AdvWksDocFTCat`的全文目录，此示例使用了 [CREATE FULLTEXT CATALOG](../../t-sql/statements/create-fulltext-catalog-transact-sql.md) 语句：  
  
    ```sql
    USE AdventureWorks;  
    GO  
    CREATE FULLTEXT CATALOG AdvWksDocFTCat;  
    ```  
    有关详细信息，请参阅[创建和管理全文目录](../../relational-databases/search/create-and-manage-full-text-catalogs.md)。
 
2.  在对 Document 表创建全文索引之前，请确保该表具有唯一的、不可为 Null 的单列索引。 下面的 [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) 语句可对 Document 表的 DocumentID 列创建唯一索引 `ui_ukDoc`：  
  
    ```sql 
    CREATE UNIQUE INDEX ui_ukDoc ON Production.Document(DocumentNode);  
    ```  

3.  使用以下 [DROP FULLTEXT INDEX](../../t-sql/statements/drop-fulltext-index-transact-sql.md) 语句删除 `Document` 表上的现有全文检索。 

    ```sql
    DROP FULLTEXT INDEX ON Production.Document
    GO
    ```

4.  具有唯一键后，即可使用下面的 `Document` CREATE FULLTEXT INDEX [语句对](../../t-sql/statements/create-fulltext-index-transact-sql.md) 表创建全文索引。  
  
    ```sql  
    CREATE FULLTEXT INDEX ON Production.Document  
    (  
        Document                         --Full-text index column name   
            TYPE COLUMN FileExtension    --Name of column that contains file type information  
            Language 2057                 --2057 is the LCID for British English  
    )  
    KEY INDEX ui_ukDoc ON AdvWksDocFTCat --Unique index  
    WITH CHANGE_TRACKING AUTO            --Population type;  
    GO  
  
    ```  
    
  
  
     此示例中定义的 TYPE COLUMN 指定表中的类型列，该列包含“Document”列（为二进制类型）的每一行中文档的类型。 此类型列存储给定行中文档的、由用户提供的文件扩展名 -“.doc”、“.xls”等等。 全文引擎使用给定行中的文件扩展名调用正确的筛选器，以用于分析该行中的数据。 筛选器分析行的二进制数据后，指定的分词系统将分析内容。 （此示例中使用了英国英语的分词系统。）有关详细信息，请参阅 [配置和管理搜索筛选器](../../relational-databases/search/configure-and-manage-filters-for-search.md)。  

    有关详细信息，请参阅[创建和管理全文索引](../../relational-databases/search/create-and-manage-full-text-indexes.md)。

##  <a name="choose-options-for-a-full-text-index"></a><a name="options"></a>选择全文检索的选项 
  
### <a name="choose-a-language"></a>选择语言  
 有关选择列语言的信息，请参阅 [创建全文索引时选择语言](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md)。  
  
### <a name="choose-a-filegroup"></a>选择文件组  
 生成全文本索引的过程会消耗相当多的 I/O 资源。 简而言之，该过程包括从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 读取数据，然后将筛选的数据传播到全文索引。 最佳做法是将全文索引置于最适于最大程度地提高 I/O 性能的数据库文件组中，或者将全文索引置于另一个卷的其他文件组中。
  
### <a name="choose-a-full-text-catalog"></a>选择全文目录   
 
 建议将具有相同更新特征的表（如更改次数少的与更改次数多的，或者在一天中某个特定时段内频繁更改的表）关联在一起，并置于同一全文目录下。 通过设置全文目录填充计划，会使全文索引与表保持同步，且在数据库活动较多时不会对数据库服务器的资源使用产生负面影响。  
  
 遵循以下指南：  
  
-   如果创建索引的表有数百万行，请将该表分配到其自身的全文目录。  
  
-   考虑要进行全文索引的表中发生的更改量以及总行数。 如果要更改的总行数与上次全文填充期间表中出现的行数合起来达到了数百万行，请将该表分配到其自身的全文目录。  

### <a name="associate-a-unique-index"></a>关联唯一索引
始终选择可用于全文唯一键的最小唯一索引。 （最好是 4 个字节、基于整数的索引。）这将显著减少文件系统中 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Search 服务所需要的资源。 如果主键较大（超过 100 个字节），可以考虑选择表中的另一个唯一索引（或创建另一个唯一索引）来作为全文唯一键。 否则，如果全文唯一键的大小超过所允许的最大值（900 个字节），全文填充将无法继续进行。  
 
### <a name="associate-a-stoplist"></a>关联非索引字表   
  “非索引字表”  是非索引字（也称为“干扰词”）的列表。 非索引字表与每个全文索引相关联，因而该非索引字表中的词会应用于对该索引的全文查询。 默认情况下，系统非索引字表与新的全文索引相关联。 也可以创建和使用你自己的非索引字表。   
  
 例如，下面的 [CREATE FULLTEXT STOPLIST](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句可通过从系统非索引字表进行复制来创建名为 myStoplist 的新全文非索引字表：  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist FROM SYSTEM STOPLIST;  
GO  
```  
  
 下面的 [ALTER FULLTEXT STOPLIST](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句可更改名为 myStoplist 的非索引字表，首先为西班牙语添加词“en”，再为法语添加词“en”：  
  
```sql  
ALTER FULLTEXT STOPLIST myStoplist ADD 'en' LANGUAGE 'Spanish';  
ALTER FULLTEXT STOPLIST myStoplist ADD 'en' LANGUAGE 'French';  
GO  
```  
有关详细信息，请参阅 [为全文搜索配置和管理非索引字和非索引字表](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)。

## <a name="update-a-full-text-index"></a>更新全文索引  
 与普通 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 索引一样，全文索引可以在相关表中的数据修改时自动更新。 此选项为默认行为。 另外，还可以手动或在预定的间隔更新全文索引。 填充全文索引可能很耗时且要占用大量资源。 因此，索引更新通常作为异步进程执行，该进程在后台运行，在基表中进行修改后使全文索引保持最新。 
 
在基表中进行每次更改后立即更新全文索引也可能会占用大量的资源。 因此，如果更新/插入/删除操作非常频繁，你可能会发现查询性能有所降低。 如果出现这种情况，可以考虑制定一个手动的更改跟踪更新计划，以便按一定的间隔更新大量的更改，从而避免与查询争用资源。  
  
有关详细信息，请参阅[填充全文索引](../../relational-databases/search/populate-full-text-indexes.md)。 

## <a name="next-steps"></a>后续步骤
设置 SQL Server 全文搜索后，便可以开始运行全文查询。 有关详细信息，请参阅[使用全文搜索查询](../../relational-databases/search/query-with-full-text-search.md)。
