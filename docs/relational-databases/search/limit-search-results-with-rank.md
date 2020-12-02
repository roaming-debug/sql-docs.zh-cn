---
description: 使用 RANK 限制搜索结果
title: 使用 RANK 限制搜索结果 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- row ranking [full-text search]
- relevance ranking values [full-text search]
- full-text search [SQL Server], rankings
- index rankings [full-text search]
- ranked results [full-text search]
- rankings [full-text search]
- per-row rank values [full-text search]
ms.assetid: 06a776e6-296c-4ec7-9fa5-0794709ccb17
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f68204154f2bbbc3c78d3aeec7e9221a625f355d
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88470125"
---
# <a name="limit-search-results-with-rank"></a>使用 RANK 限制搜索结果
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 和 [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) 函数返回名为 RANK 的列，该列包含从 0 到 1000 的序数值（排名值）。 这些值用来根据返回的行与选择条件的匹配程度对这些行进行排名。 排名值仅表示结果集中各行相关性的相对顺序，值越小，表示相关性越低。 实际的值并不重要，并且每次运行查询时实际值通常都不同。  
  
> [!NOTE]  
>  CONTAINS 和 FREETEXT 谓词不会返回任何排名值。  
  
 符合搜索条件的项通常有很多。 为防止 CONTAINSTABLE 或 FREETEXTTABLE 查询返回太多匹配项，请使用可选的 *top_n_by_rank* 参数，此参数仅返回行的子集。 *top_n_by_rank* 是一个整数值 *n*，它指定仅返回 *n* 个排名值最高的匹配项，并按降序排列。 如果 *top_n_by_rank* 与其他参数组合使用，则查询返回的行数可能会少于实际与所有谓词都匹配的行数。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 按排名对匹配项进行排序，并且最多只返回指定数目的行。 这样可以大幅度提高性能。 例如，对于正常情况下会从一个包含一百万行的表中返回 100,000 行的查询，如果只要求返回前 100 行，此查询的处理速度会更快。  
  
##  <a name="examples-of-using-rank-to-limit-search-results"></a><a name="examples"></a> 使用 RANK 限制搜索结果的示例  
  
### <a name="example-a-searching-for-only-the-top-three-matches"></a>示例 A：仅搜索前三个匹配项  
 下面的示例使用 CONTAINSTABLE 仅返回前三个匹配项。  
  
```  
USE AdventureWorks2012  
GO  
  
SELECT K.RANK, AddressLine1, City  
FROM Person.Address AS A  
  INNER JOIN  
  CONTAINSTABLE(Person.Address, AddressLine1, 'ISABOUT ("des*",  
    Rue WEIGHT(0.5),  
    Bouchers WEIGHT(0.9))',  
    3) AS K  
  ON A.AddressID = K.[KEY]  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
RANK        Address                          City  
----------- -------------------------------- ------------------------------  
172         9005, rue des Bouchers           Paris  
172         5, rue des Bouchers              Orleans  
172         5, rue des Bouchers              Metz  
  
(3 row(s) affected)  
```  
  
  
### <a name="example-b-searching-for-the-top-ten-matches"></a>示例 B：搜索前十个匹配项  
 下面的示例使用 CONTAINSTABLE 返回前 5 个产品的说明，其中 `Description` 列在单词“light”或“lightweight”附近包含字词“aluminum”。  
  
```  
USE AdventureWorks2012  
GO  
  
SELECT FT_TBL.ProductDescriptionID,  
   FT_TBL.Description,   
   KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL INNER JOIN  
   CONTAINSTABLE (Production.ProductDescription,  
      Description,   
      '(light NEAR aluminum) OR  
      (lightweight NEAR aluminum)',  
      5  
   ) AS KEY_TBL  
   ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY]  
GO  
```  
  
  
##  <a name="how-search-query-results-are-ranked"></a><a name="how"></a> 搜索查询结果如何进行排名  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的全文搜索可以生成可选分数（或排名值），该分数（或排名值）指示全文查询返回的数据的相关性。 对于每一行计算此排名值，并且可将此排名值用作排序条件按相关性对给定查询的结果集排序。 此排名值仅指示结果集中各行相关性的相对顺序。 实际的值并不重要，并且每次运行查询时实际值通常都不同。 此排名值在不同的查询之间没有任何意义。  
  
### <a name="statistics-for-ranking"></a>排名统计信息  
 生成索引后，将收集统计信息以用于排名。 生成全文目录的过程不会直接得出一个索引结构。 而是在对数据创建索引时，由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的全文引擎创建多个中间索引。 全文引擎随后会将这些索引根据需要合并为一个较大的索引。 此过程可以多次重复。 最后，全文引擎将进行“主合并”，将所有中间索引合并为一个较大的主索引。  
  
 在每个中间索引级别都会收集统计信息。 合并索引时，统计信息也将被合并。 某些统计值只有在主合并过程中才能生成。  
  
 对查询结果集排名时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将使用最大中间索引的统计信息。 这取决于是否合并了中间索引。 因此，如果未合并中间索引，则排名统计信息在准确性上会有变化。 这解释了为什么随着全文索引数据的添加、修改和删除，以及较小索引的合并，同样的查询会返回不同的排名结果。  
  
 为了尽量减小索引大小以及尽量降低计算的复杂程度，经常会对统计信息进行舍入。  
  
 下表包含了对计算排名非常重要的一些常用术语和统计值。  
  
 属性  
 行的全文索引列。  
  
 文档  
 在查询中返回的实体。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，此实体对应于一行。 正如一行可以具有多个全文索引列一样，一个文档也可以具有多个属性。  
  
 索引  
 一个或多个文档的单个倒排索引。 此索引可以整个位于内存中或整个位于磁盘上。 许多查询统计信息都与匹配的那个索引相关。  
  
 全文目录  
 当作一个查询实体来处理的中间索引的集合。 目录是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员可以看到的组织结构单位。  
  
 词、标记或项  
 全文引擎中的匹配单位。 文档中的文本流将由特定语言的断字符词汇切分为词或标记。  
  
 出现次数  
 文档属性中的词偏移量取决于断字符。 第一个词出现在位置 1，下一个词出现在位置 2，依此类推。 为避免在短语和邻近查询中出现假正数，句子结尾和段落结尾引入了较大的位置间隔。  
  
 TermFrequency  
 此数值记录了键值在某一行中出现的次数。  
  
 IndexedRowCount  
 索引行总数。 此数值基于中间索引中维护的计数来计算。 此数值在准确性上会有变化。  
  
 KeyRowCount  
 全文目录中包含给定键的总行数。  
  
 MaxOccurrence  
 某一行中的给定属性在全文目录中偏移量最大的位置。  
  
 MaxQueryRank  
 由全文引擎返回的最大排名 1000。  
  
  
### <a name="rank-computation-issues"></a>排名计算问题  
 计算排名的过程，取决于一系列因素。  不同语言的断字符对文本进行的词汇切分也不同。 例如，字符串“dog-house”可以被一种断字符断为“dog”和“house”而被另一种断字符断为“dog-house”。 这意味着匹配和排名将根据所指定语言而有所不同，因为不仅词不同，而且文档长度也不同。 文档长度的差异可能会影响所有查询的排名。  
  
 诸如 **IndexRowCount** 之类的统计信息可能会大不相同。 例如，如果一个目录的主索引有二十亿行，那么对一个新文档的索引将被编制为内存中的中间索引，而基于该内存中索引内的文档数对该文档的排名可能与主索引中的文档排名不同。 因此，建议在完成产生大量要创建索引或重新创建索引的行的任意填充后，使用 ALTER FULLTEXT CATALOG ... REORGANIZE [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句将这些索引合并为一个主索引。 全文引擎也会根据参数（例如中间索引的数目和大小）自动合并索引。  
  
 **MaxOccurrence** 值被规范化到 32 个范围的其中一个内。 举例来说，这意味着 50 个字长的文档与 100 个字长的文档的处理方式相同。 下面是用于规范化的表。 由于这两个文档的长度位于相邻表值 32 与 128 之间的范围内，因此将认为它们具有相同的有效长度 128 (32 < **docLength** <= 128)。  
  
```  
{ 16, 32, 128, 256, 512, 725, 1024, 1450, 2048, 2896, 4096, 5792, 8192, 11585,   
16384, 23170, 28000, 32768, 39554, 46340, 55938, 65536, 92681, 131072, 185363,   
262144, 370727, 524288, 741455, 1048576, 2097152, 4194304 };  
  
```  
  
  
### <a name="ranking-of-containstable"></a>CONTAINSTABLE 排名  
 [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 排名使用以下算法：  
  
```  
StatisticalWeight = Log2( ( 2 + IndexedRowCount ) / KeyRowCount )  
Rank = min( MaxQueryRank, HitCount * 16 * StatisticalWeight / MaxOccurrence )  
```  
  
 短语匹配项的排名方式与各个键类似，只不过要估计 **KeyRowCount** （包含该短语的行数），并且此值可能会比实际值大。  
  
 **NEAR 的排名**  
  
 通过使用 NEAR 选项，CONTAINSTABLE 支持查询彼此接近的两个或更多个搜索词。 每个返回行的排名值基于以下几个参数。 一个主要排名因素是相对于文档长度的匹配项总数（或“命中数”）。 因此举例来说，如果一个 100 个字长的文档和一个 900 个字长的文档中包含相同的匹配项，那么 100 个字长的文档的排名更靠前。  
  
 根据匹配项中第一个与最后一个搜索词之间的距离，一行中每个匹配项的总长度也会提高该行的排名。 距离越小，该匹配项对该行排名值的贡献就越大。 如果全文查询没有指定一个整数作为最大距离，只有包含的匹配项的相隔距离大于 100 个逻辑词时，文档才会得到 0 排名。  
  
 **ISABOUT 排名**  
  
 CONTAINSTABLE 使用 ISABOUT 选项支持查询加权词。 按照传统信息检索系统的说法，ISABOUT 表示向量空间查询。 所使用的默认排名算法为广为人知的公式 Jaccard。 将根据查询中的每个词计算排名，然后按如下描述将这些排名相结合。  
  
```  
ContainsRank = same formula used for CONTAINSTABLE ranking of a single term (above).  
Weight = the weight specified in the query for each term. Default weight is 1.  
WeightedSum = Σ[key=1 to n] ContainsRankKey * WeightKey  
Rank =  ( MaxQueryRank * WeightedSum ) / ( ( Σ[key=1 to n] ContainsRankKey^2 )   
      + ( Σ[key=1 to n] WeightKey^2 ) - ( WeightedSum ) )  
  
```  
  
  
### <a name="ranking-of-freetexttable"></a>FREETEXTTABLE 排名  
 [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) 排名基于 OKAPI BM25 排名公式计算。 FREETEXTTABLE 查询将通过派生词（原始查询词的变形）向查询中添加词，这些词将被作为单独的、与派生出它们的词没有特殊联系的词来处理。 同义词库功能派生出的同义词将被当作单独的、具有同等加权值的词来处理。 查询中的每个词都会对排名产生影响。  
  
```  
Rank = Σ[Terms in Query] w ( ( ( k1 + 1 ) tf ) / ( K + tf ) ) * ( ( k3 + 1 ) qtf / ( k3 + qtf ) ) )  
Where:   
w is the Robertson-Sparck Jones weight.   
In simplified form, w is defined as:   
w = log10 ( ( ( r + 0.5 ) * ( N - R + r + 0.5 ) ) / ( ( R - r + 0.5 ) * ( n - r + 0.5 ) )  
N is the number of indexed rows for the property being queried.   
n is the number of rows containing the word.   
K is ( k1 * ( ( 1 - b ) + ( b * dl / avdl ) ) ).   
dl is the property length, in word occurrences.   
avdl is the average length of the property being queried, in word occurrences.   
k1, b, and k3 are the constants 1.2, 0.75, and 8.0, respectively.   
tf is the frequency of the word in the queried property in a specific row.   
qtf is the frequency of the term in the query.   
```  
  
  
## <a name="see-also"></a>另请参阅  
 [使用全文搜索查询](../../relational-databases/search/query-with-full-text-search.md)  
  
  
