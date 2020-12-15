---
description: CONTAINSTABLE (Transact-SQL)
title: CONTAINSTABLE (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 07/24/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- CONTAINSTABLE
- CONTAINSTABLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- precise or fuzzy (less precise) matches [full-text search]
- fuzzy (less precise) word or phrase search [full-text search]
- word searches [full-text search]
- weighted values [full-text search]
- values [SQL Server], ranked
- LANGUAGE option
- NEAR option [full-text search]
- RANK column
- phrase searches [full-text search]
- conditions [SQL Server], CONTAINSTABLE
- relevance ranking values [full-text search]
- proximity searches [full-text search]
- CONTAINSTABLE function (Transact-SQL)
- ranked results [full-text search]
- rankings [full-text search]
- less precise (fuzzy) searches [full-text search]
ms.assetid: e580c210-cf57-419d-9544-7f650f2ab814
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8222058b5e26dd8e2d1c932705889ec0ee6d68c6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482525"
---
# <a name="containstable-transact-sql"></a>CONTAINSTABLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回一个表，该表包含零行、一行或多行，其中包含对单个词和短语的精确或模糊 (不太精确的) 匹配项、在一定程度上与单词之间的邻近性或加权匹配项。 CONTAINSTABLE 用在 SELECT 语句的 [FROM 子句](../../t-sql/queries/from-transact-sql.md) 中 [!INCLUDE[tsql](../../includes/tsql-md.md)] ，并作为常规表名称引用。 它 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对包含基于字符的数据类型的全文索引列执行全文搜索。  
  
 CONTAINSTABLE 适用于与 [contains 谓词](../../t-sql/queries/contains-transact-sql.md) 相同的匹配项，并使用与 contains 相同的搜索条件。  
  
 但与 CONTAINS 不同，使用 CONTAINSTABLE 的查询对每一行返回一个相关性排名值 (RANK) 和全文键 (KEY)。  有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持的全文搜索形式的信息，请参阅[使用全文搜索查询](../../relational-databases/search/query-with-full-text-search.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
CONTAINSTABLE   
( table , { column_name | ( column_list ) | * } , ' <contains_search_condition> '   
     [ , LANGUAGE language_term]   
  [ , top_n_by_rank ]   
)   
  
<contains_search_condition> ::=   
    { <simple_term>   
    | <prefix_term>   
    | <generation_term>   
    | <generic_proximity_term>   
    | <custom_proximity_term>   
    |  <weighted_term>   
    }   
    | { ( <contains_search_condition> )   
    { { AND | & } | { AND NOT | &! } | { OR | | } }   
     <contains_search_condition> [ ...n ]   
    }  
  
<simple_term> ::=   
     { word | "phrase" }  
<prefix term> ::=   
     { "word*" | "phrase*" }   
<generation_term> ::=   
     FORMSOF ( { INFLECTIONAL | THESAURUS } , <simple_term> [ ,...n ] )   
  
<generic_proximity_term> ::=   
     { <simple_term> | <prefix_term> } { { { NEAR | ~ }   
     { <simple_term> | <prefix_term> } } [ ...n ] }  
  
<custom_proximity_term> ::=   
  NEAR (   
     {  
        { <simple_term> | <prefix_term> } [ ,...n ]  
     |  
        ( { <simple_term> | <prefix_term> } [ ,...n ] )   
      [, <maximum_distance> [, <match_order> ] ]  
     }  
       )   
  
      <maximum_distance> ::= { integer | MAX }  
      <match_order> ::= { TRUE | FALSE }   
  
<weighted_term> ::=   
     ISABOUT  
    ( { {   
  <simple_term>   
  | <prefix_term>   
  | <generation_term>   
  | <proximity_term>   
  }   
   [ WEIGHT ( weight_value ) ]   
   } [ ,...n ]   
    )  
  
```  
  
## <a name="arguments"></a>参数  
 *table*  
 已进行了全文索引的表的名称。 *table* 可以是由一个、两个、三个或四个部分组成的数据库对象名称。 查询视图时，仅能涉及一个全文索引的基表。  
  
 *表* 不能指定服务器名称，并且不能用于对链接服务器的查询。  
  
 column_name  
 是为进行全文搜索而编制了索引的一个或多个列的名称。 列可以是 char、varchar、nchar、nvarchar、text、ntext、image、xml、varbinary 或 varbinary(max) 类型          。  
  
 column_list  
 指示可以指定多个列（以逗号分隔）。 column_list 必须用括号括起来。 除非指定 language_term，否则 column_list 中所有列的语言必须相同 。  
  
 \*  
 指定应使用 *表* 中的所有全文索引列来搜索给定的搜索条件。 除非指定 language_term，否则表的所有列的语言必须相同。  
  
 LANGUAGE language_term  
 一种语言，其资源将用于断字、词干分析、同义词库和干扰词 (或 [非索引字](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)) 删除作为查询的一部分。 此参数是可选的，可以将其指定为与语言区域设置标识符 (LCID) 对应的字符串、整数或十六进制值。 如果指定了 language_term，则它表示的语言将应用于搜索条件的所有元素。 如果未指定值，则使用该列的全文语言。  
  
 如果将不同语言的文档一起作为二进制大型对象 (BLOB) 存储在单个列中，则指定文档的区域设置标识符 (LCID) 将决定对其内容编制索引时使用哪种语言。 在对这种列进行查询时，指定 LANGUAGElanguage_term 可增大找到有效匹配项的可能性。  
  
 指定为字符串时， *language_term* 对应于 [sys.sys语言](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md)兼容性视图中的 **alias** 列值。  字符串必须用单引号引起来，如 'language_term'。 如果指定为整数，则 language_term 就是标识该语言的实际 LCID。 如果指定为十六进制值，则 language_term 将以 0x 开头，后跟 LCID 的十六进制值。 十六进制值不能超过八位（包括前导零在内）。  
  
 如果该值是双字节字符集 (DBCS) 格式，则 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将其转换为 Unicode 格式。  
  
 如果指定的语言无效，或者未安装对应于该语言的资源，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回错误。 若要使用非特定语言资源，请将 0x0 指定为 language_term。  
  
 *top_n_by_rank*  
 指定仅返回 *n* 个排名最高的匹配项（降序）。 仅当指定了整数值 *n* 时才适用。 如果 *top_n_by_rank* 与其他参数组合使用，则查询返回的行数可能会少于实际与所有谓词都匹配的行数。 *top_n_by_rank* 允许通过只调用最相关的命中来提高查询性能。  
  
 <contains_search_condition>  
 指定要在 column_name 中搜索的文本和匹配条件。 有关搜索条件的信息，请参阅 [CONTAINS &#40;transact-sql&#41;](../../t-sql/queries/contains-transact-sql.md)。  
  
## <a name="remarks"></a>备注  
 全文谓词和函数作用于 FROM 谓词所示的单个表。 若要对多个表进行搜索，请在 FROM 子句中使用联接表，以搜索由两个或更多个表的乘积构成的结果集。  
  
 返回的表中有一个名为 **key** 的列，其中包含全文键值。 每个全文索引表都有一个列，其值保证是唯一的， **键** 列中返回的值是与 contains 搜索条件中指定的选择条件匹配的行的全文键值。 从 OBJECTPROPERTYEX 函数获取的 **TableFulltextKeyColumn** 属性提供此唯一键列的标识。 若要获取与全文索引的全文键关联的列的 ID，请使用 **sys.fulltext_indexes**。 有关详细信息，请参阅 [&#40;transact-sql&#41;sys.fulltext_indexes ](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)。  
  
 若要从原始表中获得所需要的行，请指定与 CONTAINSTABLE 行的联接。 使用 CONTAINSTABLE 的 SELECT 语句的 FROM 子句的典型形式为：  
  
```  
SELECT select_list  
FROM table AS FT_TBL INNER JOIN  
   CONTAINSTABLE(table, column, contains_search_condition) AS KEY_TBL  
   ON FT_TBL.unique_key_column = KEY_TBL.[KEY];  
```  
  
 CONTAINSTABLE 生成的表包含一个名为 **RANK** 的列。 " **排名** " 列是从0到 1000) 的 (值，每行表示行与选择条件的匹配程度。 在 SELECT 语句中，此排名值通常按照下列方法之一使用：  
  
-   在 ORDER BY 子句中返回排名最高的行作为表的第一行。  
  
-   在选择列表中查看分配给每一行的排名值。  
  
## <a name="permissions"></a>权限  
 只有对表或被引用表的列具有适当 SELECT 权限的用户才具有执行权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-simple-example"></a>A. 简单示例  
 下面的示例创建并填充两个列的简单表，其中列出了3个县以及其标志中的颜色。 它创建并填充表的全文目录和索引。 然后演示了 **CONTAINSTABLE** 语法。 此示例演示当搜索值超过多次时排名值的增长方式。 在上一个查询中，同时包含绿色和黑色的坦桑尼亚的排名高于意大利，只包含其中一种查询的颜色。  
  
```  
CREATE TABLE Flags (Country nvarchar(30) NOT NULL, FlagColors varchar(200));  
CREATE UNIQUE CLUSTERED INDEX FlagKey ON Flags(Country);  
INSERT Flags VALUES ('France', 'Blue and White and Red');  
INSERT Flags VALUES ('Italy', 'Green and White and Red');  
INSERT Flags VALUES ('Tanzania', 'Green and Yellow and Black and Yellow and Blue');  
SELECT * FROM Flags;  
GO  
  
CREATE FULLTEXT CATALOG TestFTCat;  
CREATE FULLTEXT INDEX ON Flags(FlagColors) KEY INDEX FlagKey ON TestFTCat;  
GO   
  
SELECT * FROM Flags;  
SELECT * FROM CONTAINSTABLE (Flags, FlagColors, 'Green') ORDER BY RANK DESC;  
SELECT * FROM CONTAINSTABLE (Flags, FlagColors, 'Green or Black') ORDER BY RANK DESC;  
```  
  
### <a name="b-returning-rank-values"></a>B. 返回排名值  
 下面的示例搜索包含词“frame”、“wheel”或“tire”的所有产品名称，并为每个词指定了不同的权重。 对于满足这些搜索条件的每个返回行，都将显示匹配的相关程度（排名值）。 此外，排名最高的行将首先返回。  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.Name, KEY_TBL.RANK  
    FROM Production.Product AS FT_TBL   
        INNER JOIN CONTAINSTABLE(Production.Product, Name,   
        'ISABOUT (frame WEIGHT (.8),   
        wheel WEIGHT (.4), tire WEIGHT (.2) )' ) AS KEY_TBL  
            ON FT_TBL.ProductID = KEY_TBL.[KEY]  
ORDER BY KEY_TBL.RANK DESC;  
GO  
```  
  
### <a name="c-returning-rank-values-greater-than-a-specified-value"></a>C. 返回大于指定值的排名值  
  
||  
|-|  
|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。|  
  
 下面的示例使用 NEAR 搜索 `bracket` 表中彼此相近的“`reflector`”和“`Production.Document`”。 仅返回排名值为 50 或排名值更高的行。  
  
```  
USE AdventureWorks2012  
GO  
  
SELECT DocumentNode, Title, DocumentSummary  
FROM Production.Document AS DocTable   
INNER JOIN CONTAINSTABLE(Production.Document, Document,  
  'NEAR(bracket, reflector)' ) AS KEY_TBL  
  ON DocTable.DocumentNode = KEY_TBL.[KEY]  
WHERE KEY_TBL.RANK > 50  
ORDER BY KEY_TBL.RANK DESC;  
GO  
```  
  
> [!NOTE]  
>  如果全文查询没有指定一个整数作为最大距离，则只包含相隔距离大于 100 个逻辑词的匹配项的文档将不满足 NEAR 要求，其排名将为 0。  
  
### <a name="d-returning-top-5-ranked-results-using-top_n_by_rank"></a>D. 使用 top_n_by_rank 返回排名前 5 位的结果  
 下面的示例返回前 5 个产品的说明，其中 `Description` 列在单词“light”或“lightweight”附近包含字词“aluminum”。  
  
```  
USE AdventureWorks2012;  
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
   ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY];  
GO  
```  
  
 `GO`  
  
### <a name="e-specifying-the-language-argument"></a>E. 指定 LANGUAGE 参数  
 以下示例说明了如何使用 `LANGUAGE` 参数。  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.ProductDescriptionID,  
   FT_TBL.Description,   
   KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL INNER JOIN  
   CONTAINSTABLE (Production.ProductDescription,  
      Description,   
      '(light NEAR aluminum) OR  
      (lightweight NEAR aluminum)',  
      LANGUAGE N'English',  
      5  
   ) AS KEY_TBL  
   ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY];  
GO  
```  
  
> [!NOTE]  
>  使用 top_n_by_rank 不需要 *language_term* argumentis 语言 *。*  
  
## <a name="see-also"></a>另请参阅  
 [限制搜索结果排名](../../relational-databases/search/limit-search-results-with-rank.md)   
 [使用全文搜索查询](../../relational-databases/search/query-with-full-text-search.md)   
 [创建全文搜索查询 (Visual Database Tools)](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [CONTAINS (Transact-SQL)](../../t-sql/queries/contains-transact-sql.md)   
 [使用全文搜索查询](../../relational-databases/search/query-with-full-text-search.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)  
  
