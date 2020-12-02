---
description: 使用全文搜索查询
title: 使用全文搜索查询 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- queries [full-text search], about full-text queries
- queries [full-text search], predicates
- full-text queries [SQL Server], about full-text queries
- full-text search [SQL Server], querying SQL Server
- full-text queries [SQL Server]
- queries [full-text search], functions
ms.assetid: 7624ba76-594b-4be5-ac10-c3ac4a3529bd
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c34b76e3712dceabf407fcece92f223bb97f1563
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91867436"
---
# <a name="query-with-full-text-search"></a>使用全文搜索查询
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
结合 SELECT 语句使用谓词 CONTAINS 和 FREETEXT 以及行集值函数 CONTAINSTABLE 和 FREETEXTTABLE 编写全文查询。 本文提供每个谓词和函数的示例，并帮助用户选择要使用的最佳谓词和函数。

-   要匹配单词和短语，可使用 CONTAINS 和 CONTAINSTABLE。
-   要匹配含义，但不匹配确切的措辞，可使用 FREETEXT 和 FREETEXTTABLE。

## <a name="examples-of-each-predicate-and-function"></a><a name="examples_simple"></a>每个谓词和函数的示例

以下示例使用 AdventureWorks 示例数据库。 有关 AdventureWorks 的最终版本，请参阅[适用于 SQL Server 2016 CTP3 的 AdventureWorks 数据库和脚本](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks)。 要运行示例查询，还需要设置全文搜索。 有关详细信息，请参阅[全文搜索入门](get-started-with-full-text-search.md)。 

### <a name="example---contains"></a>示例 - CONTAINS  
下面的示例查找包含 `"Mountain"` 一词且价格为 `$80.99` 的所有产品：
  
```sql
USE AdventureWorks2012  
GO  
  
SELECT Name, ListPrice  
FROM Production.Product  
WHERE ListPrice = 80.99  
   AND CONTAINS(Name, 'Mountain')  
GO  
```  
  
### <a name="example---freetext"></a>示例 - FREETEXT 
 下面的示例搜索包含与 `vital safety components` 相关的单词的所有文档：
  
```sql
USE AdventureWorks2012  
GO  
  
SELECT Title  
FROM Production.Document  
WHERE FREETEXT (Document, 'vital safety components')  
GO  
```

### <a name="example---containstable"></a>示例 - CONTAINSTABLE  
 对于在词“light”或“lightweight”附近包含词“aluminum”的“Description”列，以下示例返回其所有产品的说明 ID 和说明。 仅返回排名为 2 或更高的行。  
  
```sql
USE AdventureWorks2012  
GO  
  
SELECT FT_TBL.ProductDescriptionID,  
   FT_TBL.Description,   
   KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL INNER JOIN  
   CONTAINSTABLE (Production.ProductDescription,  
      Description,   
      '(light NEAR aluminum) OR  
      (lightweight NEAR aluminum)'  
   ) AS KEY_TBL  
   ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY]  
WHERE KEY_TBL.RANK > 2  
ORDER BY KEY_TBL.RANK DESC;  
GO  
```  
  
### <a name="example---freetexttable"></a>示例 - FREETEXTTABLE  
 以下示例扩展了 FREETEXTTABLE 查询，以便首先返回排名最高的行，然后将每一行的排名添加到选择列表中。 要编写类似查询，必须知道 ProductDescriptionID 是 ProductDescription 表的唯一键列。  
  
```sql 
USE AdventureWorks2012  
GO  
  
SELECT KEY_TBL.RANK, FT_TBL.Description  
FROM Production.ProductDescription AS FT_TBL   
     INNER JOIN  
     FREETEXTTABLE(Production.ProductDescription, Description,  
                    'perfect all-around bike') AS KEY_TBL  
     ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY]  
ORDER BY KEY_TBL.RANK DESC  
GO  
```  
  
下面是同一查询的扩展查询，此查询只返回排名为 10 或更高的行：  
  
```sql  
USE AdventureWorks2012  
GO  
  
SELECT KEY_TBL.RANK, FT_TBL.Description  
FROM Production.ProductDescription AS FT_TBL   
     INNER JOIN  
     FREETEXTTABLE(Production.ProductDescription, Description,  
                    'perfect all-around bike') AS KEY_TBL  
     ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY]  
WHERE KEY_TBL.RANK >= 10  
ORDER BY KEY_TBL.RANK DESC  
GO  
```  

## <a name="match-words-or-match-meaning"></a>匹配单词或匹配含义

`CONTAINS`/`CONTAINSTABLE` 和 `FREETEXT`/`FREETEXTTABLE` 可用于不同类型的匹配。 以下信息有助于为查询选择最佳的谓词或函数：

### <a name="containscontainstable"></a>CONTAINS/CONTAINSTABLE

-   使用精确或模糊（不太精确）匹配来匹配单个单词和短语。
-   也可以执行以下操作：
    -   指定单词在彼此特定范围内的邻近性。
    -   返回加权匹配项。
    -   使用逻辑运算来组合搜索条件。 有关详细信息，请参阅本文稍后的[使用布尔运算符（AND、OR 和 NOT）](#Using_Boolean_Operators)。

### <a name="freetextfreetexttable"></a>FREETEXT/FREETEXTTABLE

-   匹配指定单词、短语或句子（Freetext 字符串）的含义，但无法匹配确切的措辞。
-   只要在指定列的全文索引中找到任何搜索词或任何搜索词的任何形式，就会生成匹配项。

## <a name="compare-predicates-and-functions"></a>比较谓词和函数

谓词 `CONTAINS`/`FREETEXT` 和行集值函数 `CONTAINSTABLE`/`FREETEXTTABLE` 具有不同的语法和选项。 以下信息有助于为查询选择最佳的谓词或函数：

### <a name="predicates-contains-and-freetext"></a>谓词 CONTAINS 和 FREETEXT

**使用情况**。 在 SELECT 语句的 WHERE 或 HAVING 子句中使用全文 **谓词** CONTAINS 和 FREETEXT。

**结果**。 CONTAINS 和 FREETEXT 谓词返回 TRUE 或 FALSE 值，指示给定的行是否与全文查询匹配。 匹配的行在结果集中返回。

**更多选项**。 可将这些谓词与 LIKE 和 BETWEEN 等其他任何 [!INCLUDE[tsql](../../includes/tsql-md.md)] 谓词结合使用。

可以指定要搜索的表中的单个列、一组列或所有列。

此外，还可以指定语言，以便全文查询使用此语言的资源进行断字和词干分析、同义词库查找以及干扰词删除。

可以在 CONTAINS 或 FREETEXT 谓词中使用由四部分组成的名称对链接服务器上的目标表的全文索引列进行查询。 若要准备远程服务器以接收全文查询，请在远程服务器上的目标表和列上创建全文索引，然后将该远程服务器添加为链接服务器。

**详细信息**。 有关这些谓词的语法和参数的详细信息，请参阅 [CONTAINS](../../t-sql/queries/contains-transact-sql.md) 和 [FREETEXT](../../t-sql/queries/freetext-transact-sql.md)。

### <a name="rowset-valued-functions-containstable-and-freetexttable"></a>行集值函数 CONTAINSTABLE 和 FREETEXTTABLE

**使用情况**。 在 SELECT 语句的 FROM 子句中使用全文 **函数** CONTAINSTABLE 和 FREETEXTTABLE，就像使用普通的表名一样。

使用其中的任一函数时，必须指定要搜索的基表。 与谓词一样，可以指定搜索表中的单个列、一组列或所有列；此外，还可以指定给定的全文查询使用的资源的语言。

通常，必须将 CONTAINSTABLE 或 FREETEXTTABLE 的结果与基表联接。 要联接该表，必须知道唯一键列名。 该列出现在每个启用全文的表中，用于强制表的唯一行（“唯一键列”）。 有关键列的详细信息，请参阅[创建和管理全文索引](../../relational-databases/search/create-and-manage-full-text-indexes.md)。

**结果**。 这些函数返回与全文查询匹配的、包含零行、一行或多行的表。 返回的表只包含与该函数的全文搜索条件中指定的选择条件相匹配的基表的行。

使用这些函数之一的查询还针对返回的每个行返回相关性排名值 (RANK) 和全文键 (KEY)，如下所示：

-   **KEY** 列 KEY 列返回所返回行的唯一值。 可使用 KEY 列指定选择条件。
-   **RANK** 列 RANK 列返回每一行的排名值  ，此值指示该行与选择条件相匹配的程度。 行中文本或文档的排名值越高，该行与给定的全文查询的相关性就越高。 不同行的排名可以相同。 可以通过指定可选的 top_n_by_rank 参数限制返回的匹配项的数目。 有关详细信息，请参阅 [使用 RANK 限制搜索结果](../../relational-databases/search/limit-search-results-with-rank.md)。

**详细信息**。 有关这些函数的语法和参数的详细信息，请参阅 [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 和 [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md)。

## <a name="specific-types-of-searches"></a><a name="examples_specific"></a>特定搜索类型

###  <a name="search-for-a-specific-word-or-phrase-simple-term"></a><a name="Simple_Term"></a>搜索特定的单词或短语（简单词）  
 可以使用 [CONTAINS](../../t-sql/queries/contains-transact-sql.md)、[CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md)、[FREETEXT](../../t-sql/queries/freetext-transact-sql.md) 或 [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) 在表中搜索特定单词或短语。 例如，如果要在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的“ProductReview”表中进行搜索，以查找关于某种产品的包含“learning curve”短语的所有注释，可以使用 CONTAINS 谓词，如下所示：  
  
```sql
USE AdventureWorks2012  
GO  
  
SELECT Comments  
FROM Production.ProductReview  
WHERE CONTAINS(Comments, '"learning curve"')  
GO  
```  
  
搜索条件（在本例中为“learning curve”）可以很复杂，也可以由一个或多个词组成。

#### <a name="more-info-about-simple-term-searches"></a>有关简单词搜索的详细信息

在全文搜索中，*单词*（或 *标记*）是其边界由相应的断字符标识、遵循指定语言的语言规则的字符串。 有效的 *短语* 由多个单词组成，单词之间可以有标点符号也可以没有标点符号。

例如，“croissant”是一个词，“café au lait”是一个短语。 这样的词和短语称为“简单词”。

[CONTAINS](../../t-sql/queries/contains-transact-sql.md) 和 [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 查找短语的完全匹配项。 [FREETEXT](../../t-sql/queries/freetext-transact-sql.md) 和 [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) 将短语拆分为几个词。

###  <a name="search-for-a-word-with-a-prefix-prefix-term"></a><a name="Prefix_Term"></a>搜索带有某个前缀的单词（前缀词）  
 可以使用 [CONTAINS](../../t-sql/queries/contains-transact-sql.md) 或 [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 来搜索具有指定前缀的词或短语。 将返回列中所有包含以指定前缀开头的文本的项。 例如，要搜索包含前缀 `top`- 的所有行，如 `top``ple`、 `top``ping`和 `top`。 该查询如以下示例所示：  
  
```sql  
USE AdventureWorks2012  
GO  
  
SELECT Description, ProductDescriptionID  
FROM Production.ProductDescription  
WHERE CONTAINS (Description, '"top*"' )  
GO  
```  
  
 将会返回所有与星号 (*) 之前指定的文本相匹配的文本。 如果未在文本和星号前后加上双引号标记（如 `CONTAINS (DESCRIPTION, 'top*')`），则全文搜索将不把星号当作通配符。  
  
 当前缀词是短语时，组成该短语的每个标记均被看作是单独的前缀词。 将返回包含以这些前缀词开头的词的所有行。 例如，前缀词“light bread\*”将查找带有“light breaded”、“lightly breaded”或“light bread”文本的行，但不会返回“lightly toasted bread”。

#### <a name="more-info-about-prefix-searches"></a>有关前缀搜索的详细信息

*前缀词* 指附加到一个单词的前面以生成一个派生词或变形的字符串。

-   对于单个前缀词，以指定词开头的任何词将是结果集的一部分。 例如，词“auto*”与“automatic”、“automobile”等匹配。

-   如果是短语，则该短语内的每个词都被看作是一个前缀。 例如，词“auto tran\*”与“automatic transmission”和“automobile transducer”匹配，但与“automatic motor transmission”不匹配。

[CONTAINS](../../t-sql/queries/contains-transact-sql.md) 和 [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 支持前缀搜索。
  
###  <a name="search-for-inflectional-forms-of-a-specific-word-generation-term"></a><a name="Inflectional_Generation_Term"></a>搜索特定单词的变形（派生词）  
可以使用 [CONTAINS](../../t-sql/queries/contains-transact-sql.md)、 [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md)、 [FREETEXT](../../t-sql/queries/freetext-transact-sql.md)或 [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) 搜索动词的所有不同时态和语态形式或搜索名词的单数和复数形式（变形搜索）或者搜索特定词的同义词形式（同义词库搜索）。  
  
以下示例在 `AdventureWorks` 数据库的 `ProductReview` 表的 `Comments` 列搜索“foot”的任意变形（“foot”、“feet”等）： 
  
```sql  
USE AdventureWorks2012  
GO  
  
SELECT Comments, ReviewerName  
FROM Production.ProductReview  
WHERE CONTAINS (Comments, 'FORMSOF(INFLECTIONAL, "foot")')  
GO  
```  
  
全文搜索使用 *词干分析器*，它允许你搜索某个动词的不同时态和语态形式，或搜索某个名词的单数和复数形式。 有关词干分析器的详细信息，请参阅 [配置和管理断字符和词干分析器以便搜索](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)。  

#### <a name="more-info-about-generation-term-searches"></a>有关派生词搜索的详细信息

*变形* 是动词的不同时态和语态形式，或是名词的单数和复数形式。

例如，搜索词“drive”的变形。 如果表中不同的行包含词“drive”、“drives”、“drove”、“driving”和“driven”，则这些词都会出现在结果集中，原因是它们每一个都可以从词 drive 变形而来。

[FREETEXT](../../t-sql/queries/freetext-transact-sql.md) 和 [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) 默认情况下查找所有指定词的变形。 [CONTAINS](../../t-sql/queries/contains-transact-sql.md) 和 [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 支持可选的 `INFLECTIONAL` 参数。

### <a name="search-for-synonyms-of-a-specific-word"></a>搜索特定词的同义词

*同义词库* 为词定义用户指定的同义词。 有关同义词库文件的详细信息，请参阅[为全文搜索配置和管理同义词库文件](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)。

例如，如果将项“{car, automobile, truck, van}”添加到同义词库，则可以搜索单词“car”的同义词库形式。 所查询表中所有包括单词“automobile”、“truck”、“van”或“car”的行都会出现在结果集中，因为所有这些单词都属于包含单词“car”的同义词扩展集。

[FREETEXT](../../t-sql/queries/freetext-transact-sql.md) 和 [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) 默认情况下使用同义词库。 [CONTAINS](../../t-sql/queries/contains-transact-sql.md) 和 [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 支持可选的 `THESAURUS` 参数。

### <a name="search-for-a-word-near-another-word"></a>搜索与另一个词相邻的词

邻近词表示词或短语彼此相邻。 还可以指定在第一个搜索词与最后一个搜索之间最多可以有几个非搜索词。 此外，可以以任意顺序或您指定的顺序搜索词或短语。

例如，查找词“ice”与“hockey”邻近或短语“ice skating”与“ice hockey”邻近的行。 

[CONTAINS](../../t-sql/queries/contains-transact-sql.md) 和 [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md)

有关邻近搜索的详细信息，请参阅[使用 NEAR 搜索与另一个词邻近的词](search-for-words-close-to-another-word-with-near.md)。

###  <a name="search-for-words-or-phrases-using-weighted-values-weighted-term"></a><a name="Weighted_Term"></a>使用加权值搜索单词或短语（加权词）  
您可以使用 [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 来搜索词或短语并指定加权值。 加权值用介于 0.0 到 1.0 之间的一个数字来表示，用于指示一组词和短语中的每个词和短语的重要程度。 加权值的最低值是 0.0，最高值是 1.0。  
  
下例所示的查询使用加权值搜索所有符合以下条件的客户地址：地址中任何以字符串“Bay”开头的文本包含“Street”或“View”。 在结果中，对那些包含较多指定词的行赋予较高的排名。  
  
```sql  
USE AdventureWorks2012  
GO  
  
SELECT AddressLine1, KEY_TBL.RANK   
FROM Person.Address AS Address INNER JOIN  
CONTAINSTABLE(Person.Address, AddressLine1, 'ISABOUT ("Bay*",   
         Street WEIGHT(0.9),   
         View WEIGHT(0.1)  
         ) ' ) AS KEY_TBL  
ON Address.AddressID = KEY_TBL.[KEY]  
ORDER BY KEY_TBL.RANK DESC  
GO  
```  
  
 加权词可以与任意简单词、前缀词、派生词或邻近词一起使用。

#### <a name="more-info-about-weighted-term-searches"></a>有关加权词搜索的详细信息

在加权词搜索中，加权值指示一组单词和短语中的每个单词和短语的重要程度。 加权值的最低值是 0.0，最高值是 1.0。

例如，在某个搜索多个词条的查询中，可以为每个搜索单词指定一个加权值，用于指示它相对于搜索条件中其他单词的重要性。 此查询类型的结果将按指定给搜索单词的相对权重首先返回最相关的行。 结果集由包含任何指定词（或它们之间的内容）的文档或行组成；但是，由于与不同搜索词关联的加权值的不同，某些结果将被视为比其他结果更相关。

[CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 支持加权词搜索。

##  <a name="use-and-or-and-not-boolean-operators"></a><a name="Using_Boolean_Operators"></a>使用 AND、OR 和 NOT（布尔运算符）
 
CONTAINS 谓词和 CONTAINSTABLE 函数使用相同的搜索条件。 它们都支持使用布尔运算符（AND、OR、AND NOT）将多个搜索词组合起来，以执行逻辑运算。 例如，可以使用 AND 查找既包含“latte”又包含“New York-style bagel”的行。 例如，可以使用 AND NOT 查找包含“bagel”但不包含“cream cheese”的行。  
  
相反，FREETEXT 和 FREETEXTTABLE 将布尔值项视为词来搜索。  
  
 有关将 CONTAINS 与其他使用逻辑运算符 AND、OR 和 NOT 的谓词结合使用的信息，请参阅 [搜索条件 (Transact-SQL)](../../t-sql/queries/search-condition-transact-sql.md)。  
  
### <a name="example"></a>示例  
 下面的示例使用 CONTAINS 谓词搜索说明 ID 不等于 5 并且既包含单词“Aluminum”又包含单词“spindle”的说明。 该搜索条件使用 AND 布尔运算符。 下面的示例使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的 ProductDescription 表。
  
```sql  
USE AdventureWorks2012  
GO  
  
SELECT Description  
FROM Production.ProductDescription  
WHERE ProductDescriptionID <> 5 AND  
   CONTAINS(Description, 'aluminum AND spindle')  
GO  
```  
  
##  <a name="case-stopwords-language-and-thesaurus"></a><a name="Additional_Considerations"></a>大小写、非索引字、语言和同义词库

 编写全文查询时，还可以指定以下选项：
  
-   **区分大小写**。 全文搜索查询不区分大小写。 但是，在日语中，有许多表示语音的拼字法，其中拼字规范化这一概念与不区分大小写类似，如 kana = 不区分。 这种拼字规范化不受支持。  

-   **非索引字**。 当定义全文查询时，全文引擎会去除搜索条件中的非索引字（也称为干扰词）。 非索引字是可能经常出现但通常对搜索特定文本没有帮助的词，如“a”、“and”、“is”或“the”。 非索引字在非索引字表中列出。 每个全文索引都与一个特定的非索引字表相关联，该非索引字表决定在进行索引时省略查询或索引中的哪些非索引字。 有关详细信息，请参阅[为全文搜索配置和管理非索引字和非索引字表](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)。  

-   使用 **LANGUAGE** 选项指定 **语言**。 许多查询词在很大程度上依赖于断字符行为。 为了确保您使用的是正确的断字符（和词干分析器）以及同义词库文件，我们建议您指定 LANGUAGE 选项。 有关详细信息，请参阅 [创建全文索引时选择语言](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md)。  
  
-   **同义词库**。 FREETEXT 和 FREETEXTTABLE 查询默认情况下使用同义词库。 CONTAINS 和 CONTAINSTABLE 支持可选的 THESAURUS 参数。 有关详细信息，请参阅[为全文搜索配置和管理同义词库文件](configure-and-manage-thesaurus-files-for-full-text-search.md)。
  
##  <a name="check-the-tokenization-results"></a><a name="tokens"></a>检查词汇切分结果

在查询中应用给定的断字符、同义词库和非索引字表组合后，可以使用 sys.dm_fts_parser 动态管理视图查看全文搜索如何切分结果。 有关详细信息，请参阅[sys.dm_fts_parser (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-parser-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [CONTAINS (Transact-SQL)](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE (Transact-SQL)](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXT (Transact-SQL)](../../t-sql/queries/freetext-transact-sql.md)   
 [FREETEXTTABLE (Transact-SQL)](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [创建全文搜索查询 (Visual Database Tools)](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [改进全文查询的性能](../../relational-databases/search/improve-the-performance-of-full-text-queries.md)
