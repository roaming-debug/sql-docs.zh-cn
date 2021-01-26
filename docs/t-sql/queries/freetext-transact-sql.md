---
description: FREETEXT (Transact-SQL)
title: FREETEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FREETEXT
- FREETEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], meaning matches
- meaning matches [full-text search]
- FREETEXT predicate (Transact-SQL)
- words in predicate [full-text search]
- column searches [full-text search]
ms.assetid: 2f199d3c-440e-4bcf-bdb5-82bb3994005d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c4ed81c783dcd8f3fc850432f89b3f7498c16664
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596231"
---
# <a name="freetext-transact-sql"></a>FREETEXT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  它是一个谓词，用于在 [!INCLUDE[tsql](../../includes/tsql-md.md)] SELECT 语句的 [!INCLUDE[tsql](../../includes/tsql-md.md)] [WHERE 子句](../../t-sql/queries/where-transact-sql.md)中对包含基于字符的数据类型的全文索引列执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 全文搜索。 该谓词将搜索含义与搜索条件中的单词相同但不完全匹配的值。 如果使用 FREETEXT，全文查询引擎将在内部对 freetext_string 执行以下操作，并为每个字词分配权重，再查找匹配项：  
  
-   基于单词边界（断字）将字符串分隔成单独的单词。  
  
-   生成单词的词形变化形式（词干处理）。  
  
-   基于同义词库中的匹配项标识字词的扩展或替换的列表。  
  
> [!NOTE]  
>  有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持的全文搜索形式的信息，请参阅[使用全文搜索查询](../../relational-databases/search/query-with-full-text-search.md)。  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到[当前版本](../../sql-server/what-s-new-in-sql-server-2016.md)）。
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
FREETEXT ( { column_name | (column_list) | * }   
          , 'freetext_string' [ , LANGUAGE language_term ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 column_name  
 FROM 子句中指定表的一个或多个全文索引列的名称。 列可以是 char、varchar、nchar、nvarchar、text、ntext、image、xml、varbinary 或 varbinary(max) 类型          。  
  
 column_list  
 指示可以指定多个列（以逗号分隔）。 column_list 必须用括号括起来。 除非指定 language_term，否则 column_list 中所有列的语言必须相同 。  
  
 \*  
 指定所有注册全文搜索的列均应用于搜索给定的 freetext_string。 如果 FROM 子句中有多个表，那么 \* 必须由表名限定。 除非指定 language_term，否则表的所有列的语言必须相同。  
  
 *freetext_string*  
 要在 column_name 中搜索的文本。 可以输入任何文本，包括单词、短语或句子。 只要在全文索引中找到任何术语或术语格式，就会生成匹配项。  
  
 与 AND 作为关键字的 CONTAINS 和 CONTAINSTABLE 搜索条件不同，当在 freetext_string 中使用单词“and”时，会将它视为干扰词或[非索引字](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)，因此会丢弃它。  
  
 不允许使用 WEIGHT、FORMSOF、通配符、NEAR 和其他语法。 系统将通过同义词库对 freetext_string 进行断字处理、词干分析，然后执行同义词库查询。  
  
 freetext_string 是 nvarchar。 将另一个字符数据类型用作输入时，将发生隐式转换。 不能使用大型字符串数据类型 nvarchar(max) 和 varchar(max)。 在下面的示例中，`@SearchWord` 变量（被定义为 `varchar(30)`）导致 `FREETEXT` 谓词中发生隐式转换。  
  
```sql
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord ='performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```  
  
 由于“参数截取”跨转换无效，因此请使用 nvarchar 以获得更好性能。 本示例将 `@SearchWord` 声明为 `nvarchar(30)`。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30)  
SET @SearchWord = N'performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```  
  
 对于生成非最佳计划的情况，还可以使用 OPTIMIZE FOR 查询提示。  
  
 LANGUAGE language_term  
 特定的语言，查询时，其资源将用于断字、词干分析、同义词库查询以及非索引字删除。 此参数是可选的，可以将其指定为与语言区域设置标识符 (LCID) 对应的字符串、整数或十六进制值。 如果指定了 language_term，则它表示的语言将应用于搜索条件的所有元素。 如果未指定值，则使用该列的全文语言。  
  
 如果将不同语言的文档一起作为二进制大型对象 (BLOB) 存储在单个列中，则指定文档的区域设置标识符 (LCID) 将决定对其内容编制索引时使用哪种语言。 在对这种列进行查询时，指定 LANGUAGE language_term 可增大找到有效匹配项的可能性。  
  
 如果指定为字符串，language_term 将对应于 [sys.syslanguages (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) 兼容性视图中的 alias 列值。  字符串必须用单引号引起来，如 'language_term'。 如果指定为整数，则 language_term 就是标识该语言的实际 LCID。 如果指定为十六进制值，则 language_term 将以 0x 开头，后跟 LCID 的十六进制值。 十六进制值不能超过八位（包括前导零在内）。  
  
 如果该值是双字节字符集 (DBCS) 格式，则 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将其转换为 Unicode 格式。  
  
 如果指定的语言无效，或者未安装对应于该语言的资源，[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将返回错误。 若要使用非特定语言资源，请将 0x0 指定为 language_term。  
  
## <a name="general-remarks"></a>一般备注  
 全文谓词和函数作用于 FROM 谓词所示的单个表。 若要对多个表进行搜索，请在 FROM 子句中使用联接表，以搜索由两个或更多个表的乘积构成的结果集。  
  
使用 FREETEXT 的全文查询没有使用 CONTAINS 的全文查询精度高。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 全文搜索引擎识别重要的字词和短语。 保留关键字或通配符字符都不具有特殊含义，而它们指定在 CONTAINS 谓词的 \<contains_search_condition> 参数中时则通常具有含义。
  
 当数据库兼容级别设为 100 时，不允许在 [OUTPUT 子句](../../t-sql/queries/output-clause-transact-sql.md)中使用全文谓词。  
  
> [!NOTE]  
>  FREETEXTTABLE 函数用来搜索的匹配项的种类与 FREETEXT 谓词相同。 与常规表名称类似，可以在 SELECT 语句的 [FROM 子句](../../t-sql/queries/from-transact-sql.md)中引用此函数。 有关详细信息，请参阅 [FREETEXTTABLE (Transact-SQL)](../../relational-databases/system-functions/freetexttable-transact-sql.md)。  
  
## <a name="querying-remote-servers"></a>查询远程服务器  
 可以在 [CONTAINS](../../t-sql/queries/contains-transact-sql.md) 或 FREETEXT 谓词中使用由四部分组成的名称对链接服务器上的目标表的全文索引列进行查询。 若要准备远程服务器以接收全文查询，请在远程服务器上的目标表和列上创建全文索引，然后将该远程服务器添加为链接服务器。  
  
## <a name="comparison-of-like-to-full-text-search"></a>LIKE 与全文搜索的比较  
 与全文搜索不同， [LIKE](../../t-sql/language-elements/like-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] 谓词仅对字符模式有效。 另外，不能使用 LIKE 谓词来查询格式化的二进制数据。 此外，对大量非结构化的文本数据执行 LIKE 查询要比对相同数据执行同样的全文查询慢得多。 对数百万行文本数据进行的 LIKE 查询可能需要几分钟的时间才能返回结果；而对于同样的数据，全文查询只需要几秒甚至更少的时间，具体取决于返回的行数。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-freetext-to-search-for-words-containing-specified-character-values"></a>A. 使用 FREETEXT 搜索包含指定字符值的单词  
 以下示例搜索包含与 vital、safety、components 相关的单词的所有文档。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Title  
FROM Production.Document  
WHERE FREETEXT (Document, 'vital safety components' );  
GO  
```  
  
### <a name="b-using-freetext-with-variables"></a>B. 通过变量使用 FREETEXT  
 以下示例使用变量替代具体的搜索词。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30);  
SET @SearchWord = N'high-performance';  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [全文搜索入门](../../relational-databases/search/get-started-with-full-text-search.md)   
 [创建和管理全文目录](../../relational-databases/search/create-and-manage-full-text-catalogs.md)   
 [CREATE FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [创建和管理全文索引](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [使用全文搜索查询](../../relational-databases/search/query-with-full-text-search.md)   
 [创建全文搜索查询 (Visual Database Tools)](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [CONTAINS (Transact-SQL)](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE (Transact-SQL)](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [FREETEXTTABLE (Transact-SQL)](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
  
