---
description: 创建全文索引时选择语言
title: 创建全文索引时选择语言 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- languages [full-text search]
- full-text indexes [SQL Server], languages
- international considerations [full-text search]
- stemmers [full-text search]
- global considerations [full-text search]
- full-text search [SQL Server], international considerations
- languages [SQL Server], full-text indexes
- word breakers [full-text search]
ms.assetid: 670a5181-ab80-436a-be96-d9498fbe2c09
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 9f8c0860bb5ef874a6095b993478fa9cbc117fc4
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127775"
---
# <a name="choose-a-language-when-creating-a-full-text-index"></a>创建全文索引时选择语言

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  创建全文索引时，需要为索引列指定列级语言。 所指定语言的 [断字符和词干分析器](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md) 将由针对相应列的全文查询使用。 如果要在创建全文索引时选择列语言，有几个事项需要注意。 这些注意事项均与全文引擎如何对文本进行词汇切分再编制其索引有关。  
  
> [!NOTE]  
>  若要为某全文索引列指定列级语言，请在指定此列时使用 LANGUAGE *language_term* 子句。 有关详细信息，请参阅 [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md) 和 [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)。  
  
##  <a name="language-support-in-full-text-search"></a><a name="langsupp"></a> 全文搜索中的语言支持  
 本节简单介绍了断字符和词干分析器，并讨论了全文搜索是如何使用列级语言的 LCID 的。  
  
### <a name="introduction-to-word-breakers-and-stemmers"></a>断字符和词干分析器简介  
 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本包含一系列全新的断字符和词干分析器，这些断字符和词干分析器要明显优于以前在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中提供的断字符和词干分析器。  
  
> [!NOTE]  
>  Microsoft 自然语言组 (MS NLG) 已实现并支持这些新语言组件。  
  
 这些新的断字符具有以下好处：  
  
-   稳定性  
  
     测试表明这些新的断字符在高压查询环境中非常可靠。  
  
-   安全性  
  
     由于语言组件的安全性得到改进，因此默认情况下 SQL Server 中已启用这些新的断字符。 我们极力建议对诸如断字符和筛选器之类的外部组件进行签名以提高 SQL Server 的整体安全性和可靠性。 可以按如下方式配置全文查询以验证是否对这些组件进行了签名：  
  
    ```  
    EXEC sp_fulltext_service 'verify_signature';  
    ```  
  
-   质量  
  
     断字符已经过重新设计，测试表明这些新断字符能够提供比以往断字符更好的语义质量。 这提高了恢复准确性。  
  
-   对于许多语言来说，断字符已包含在 SQL Server 中，并在默认情况下处于启用状态。  
  
 有关 SQL Server 中已包含其断字符和词干分析器的语言的列表，请参阅 [sys.fulltext_languages (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)。  
  
  
### <a name="how-full-text-search-uses-the-name-of-the-column-level-language"></a>全文搜索使用列级语言名称的方式  
 创建全文索引时，需要为每一列指定有效的语言名称。 如果语言名称有效但却未由 [sys.fulltext_languages (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md) 目录视图返回，则全文搜索将退而使用相同语系中最接近的可用语言名称（如果有）。 否则，全文搜索将退而使用非特定语言断字符。 这种回退行为可能会影响恢复的准确性。 因此，我们极力建议您在创建全文索引时为每一列指定有效且可用的语言名称。  
  
> [!NOTE]  
>  将对可创建全文索引的所有数据类型（例如 **char** 或 **nchar**）使用 LCID。 如果将 **char**、 **varchar** 或 **text** 类型的列的排序顺序设置为不同于 LCID 所标识语言的语言设置，则在对这些列进行全文索引和查询时，始终使用该 LCID。  
  
  
##  <a name="word-breaking"></a><a name="breaking"></a> 断字  
 可使用断字符基于词的边界对要创建索引的文本进行标记，词的边界则取决于特定语言。 因此，断字行为因语言而异。 如果使用一种语言 x 对许多语言 {x, y, and z} 创建索引，则某些行为可能会导致意外结果。 例如，破折号 (-) 或逗号 (,) 可能是在一种语言中被丢弃而在另一种语言中却不会被丢弃的断字元素。 在极少数情况下，也可能会出现意外的词干分析行为，原因是给定字词的词干分析可能因语言而异。 例如，在英语中，词的边界通常是空格或某些标点符号。 在其他语言（例如德语）中，字词或字符有可能组合在一起。 因此，选择的列级语言应当为要存储在相应列的各行中的语言。  
  
### <a name="western-languages"></a>西方语言  
 对于西方语系，如果您不确定将在列中存储哪种语言或者您期望存储多种语言，则常规解决方法是使用可能在列中存储的最复杂语言的断字符。 例如，您可能希望在单个列中存储英语、西班牙语和德语内容。 这三种西方语言拥有非常相似的断字模式，其中德语模式是最复杂的。 因此，这种情况下的明智选择是使用德语断字符，它应能正确地处理英语和西班牙语文本。 与此不同，英语断字符则可能会由于德语中的复合词而无法完美地处理德语文本。  
  
 请注意，使用某一语系中最复杂语言的断字符不能保证针对此语系中的每一种语言都能够完美地创建索引。 也可能存在以下极端情况：最复杂语言的断字符无法正确处理使用其他语言书写的文本。  
  
  
### <a name="non-western-languages"></a>非西方语言  
 对于非西方语言（例如，中文、日语、印地语等等），由于语言原因上述解决方法不一定会起作用。 对于非西方语言，请考虑使用以下解决方法之一：  
  
-   对于其他不同语系的语言  
  
     如果一个列中可能包含显著不同的语言（例如，西班牙语和日语），请考虑将这些不同语言的内容存储在不同的列中。 这样，您将能够对每一列使用特定语言的断字符。 如果选择此解决办法，但却不知道查询时的查询语言，则可能需要对两个列都发出查询，以确保查询能够找到正确的行或文档。  
  
-   对于二进制内容（例如 Microsoft Word 文档）  
  
     当创建索引的内容为 **binary** 类型时，用来对文本内容进行处理然后将其发送给断字符的全文搜索筛选器可能会使用该二进制文件中现有的特定语言标记。 在这种情况下，当创建索引时，筛选器将发出文档或文档某片段的正确 LCID。 然后，全文引擎将使用此 LCID 调用相应语言的断字符。 不过，在为多语言内容创建索引后，我们建议您验证是否为这些内容正确地创建了索引。  
  
-   对于纯文本内容  
  
     当内容为纯文本时，您可以将其转换为 **xml** 数据类型，并添加用来表示与每个特定文档或文档部分相对应的语言的语言标记。 不过，若要使此方法可行，您需要在创建全文索引之前知道相应的语言。  
  
  
##  <a name="stemming"></a><a name="stemming"></a> 词干分析  
 选择列级语言时的另外一个注意事项是词干分析。 全文查询中的 *词干分析* 是指搜索特定语言中某个词的所有词干派生形式（变形）的过程。 当使用一般断字符处理多种语言时，词干分析过程仅对为相应列指定的语言起作用，对于此列中的其他语言则不起作用。 例如，德语词干分析器对于英语或西班牙语（等语言）不起作用。 这可能会影响恢复操作，具体取决于你在查询时选择使用的语言。  
  
  
##  <a name="effect-of-column-type-on-full-text-search"></a><a name="type"></a> 列类型对全文搜索的影响  
 选择语言时的另一个注意事项与数据的表示方式有关。 对于未存储在 **varbinary(max)** 列中的数据，不会执行专门的筛选， 而一般通过断字组件按原样传递该文本。  
  
 此外，断字符主要用于处理书面文本。 因此，如果文本中包含任何类型的标记（例如 HTML），则在索引和搜索过程中可能无法获得很好的语言准确性。 在这种情况下，你有两个选择 - 首选方法是只将文本数据存储在 varbinary(max) 列中，并指示数据文档类型，以便对其进行筛选。 如果不能使用此方法，那么可以考虑使用非特定语言断字符，并且（如果可能）将标记数据（例如 HTML 中的“br”）添加到干扰词列表中。  
  
> [!NOTE]  
>  当指定非特定语言时，基于语言的词干分析将不起作用。  
  
  
##  <a name="specifying-a-non-default-column-level-language-in-a-full-text-query"></a><a name="nondef"></a> 在全文查询中指定非默认列级语言  
 默认情况下，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，全文搜索将使用在全文子句中包括的为每一列指定的语言来分析查询词。 若要覆盖此行为，请指定查询时使用的非默认语言。 对于那些已安装相应资源的受支持语言，可以使用 *CONTAINS* 、 [CONTAINSTABLE](../../t-sql/queries/contains-transact-sql.md)、 [FREETEXT](../../relational-databases/system-functions/containstable-transact-sql.md)或 [FREETEXTTABLE](../../t-sql/queries/freetext-transact-sql.md)查询的 LANGUAGE [language_term](../../relational-databases/system-functions/freetexttable-transact-sql.md) 子句来指定要用于对查询词进行断字、词干分析、同义词库和非索引字处理的语言。  
  
  
## <a name="see-also"></a>另请参阅  
 [CONTAINS (Transact-SQL)](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE (Transact-SQL)](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [FREETEXT (Transact-SQL)](../../t-sql/queries/freetext-transact-sql.md)   
 [FREETEXTTABLE (Transact-SQL)](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [配置和管理搜索筛选器](../../relational-databases/search/configure-and-manage-filters-for-search.md)   
 [sp_fulltext_service (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)   
 [sys.fulltext_languages (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)   
 [配置和管理断字符和词干分析器以便搜索](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)  
  
  
