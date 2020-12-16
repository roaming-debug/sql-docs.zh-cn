---
description: 配置和管理断字符和词干分析器以便搜索 (SQL Server)
title: 配置和管理断字符和词干分析器以便搜索
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- languages [full-text search]
- full-text search [SQL Server], stemmers
- linguistic analysis [full-text search]
- full-text indexes [SQL Server], linguistic analysis
- full-text search [SQL Server], word breakers
- default full-text language option
- stemmers [full-text search]
- conjugating verbs [full-text search]
- word breakers [full-text search]
ms.assetid: d4bdd16b-a2db-4101-a946-583d1c674229
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: 8281024e31d87cb1d02b4032a3d0f939da297232
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460110"
---
# <a name="configure--manage-word-breakers--stemmers-for-search-sql-server"></a>配置和管理断字符和词干分析器以便搜索 (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
断字符和词干分析器用于对所有全文索引数据执行语言分析。 语言分析执行下述两项操作：

-   **查找词边界（断字）**。 “断字符”根据语言的词法规则确定词的边界位置，从而标识各个词。 每个词（也称为标记）使用压缩表示形式插入全文索引以减少其大小。

-   **组合动词（词干分析）**。 词干分析器根据该语言的规则生成特定词的变形形式（例如，“running”、“ran”和“runner”是单词“run”的不同形式）。

## <a name="word-breakers-and-stemmers-are-language-specific"></a>断字符和词干分析器特定于语言

断字符和词干分析器是特定于语言的，并且各语言的语言分析规则也各不相同。 有了特定于语言的断字符，为该语言生成的词就更准确。

使用为 SQL Server 所支持的所有语言提供的断字符和词干分析器时，通常不需要执行任何操作。

-   如果断字符用于整个语系而不是特定的子语言，将使用该语系中的主要语言。 例如，使用法语断字符来处理加拿大法语文本。
-   如果某一特定语言没有可用的断字符，将使用非特定语言断字符。 使用非特定语言断字符时，词将在非特定语言字符（如空格和标点符号）处断开。

## <a name="get-the-list-of-supported-languages"></a>获取支持的语言的列表

若要查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 全文搜索所支持的语言的列表，请使用以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 如果一种语言存在于该列表中，则说明已为该语言注册断字符。 
  
```sql
SELECT * FROM sys.fulltext_languages
```

##  <a name="get-the-list-of-registered-word-breakers"></a><a name="register"></a> 获取已注册断字符的列表

必须注册一种语言的断字符，然后全文搜索才能使用该断字符。 注册断字符后，还可将关联的语言资源（词干分析器、干扰词（非索引字）和同义词库文件）用于全文索引和查询操作。

若要查看已注册断字符组件的列表，请使用以下语句。

```sql
EXEC sp_help_fulltext_system_components 'wordbreaker';  
GO  
```

如需其他选项和详细信息，请参阅 [sp_help_fulltext_system_components &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md)。
 
## <a name="if-you-add-or-remove-a-word-breaker"></a>如果添加或删除断字符  
如果您添加、删除或更改了断字符，则需要刷新为全文索引和查询而支持的 Microsoft Windows 区域设置标识符 (LCID) 列表。 有关详细信息，请参阅 [查看或更改注册的筛选器和断字符](../../relational-databases/search/view-or-change-registered-filters-and-word-breakers.md)。  
  
##  <a name="set-the-default-full-text-language-option"></a><a name="default"></a> 设置默认的全文语言选项  
 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的本地化版本， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将把 **default full-text language** 选项设置为服务器的语言（如果存在合适的匹配项）。 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的非本地化版本， **或** 选项为“英语”。  
  
 创建或修改全文索引时，可以为每个全文索引列指定不同的语言。 如果未指定列的语言，默认值是配置选项“默认全文语言”的值。  
  
> [!NOTE]  
>  在单个全文查询函数子句中列出的所有列必须使用同一语言，除非在查询中指定了 LANGUAGE 选项。 所查询的用于全文索引列的语言确定了对全文查询谓词（[CONTAINS](../../t-sql/queries/contains-transact-sql.md) 和 [FREETEXT](../../t-sql/queries/freetext-transact-sql.md)）以及函数（[CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 和 [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md)）的参数执行的语言分析。  
  
##  <a name="choose-the-language-for-an-indexed-column"></a><a name="lang"></a> 为索引列选择语言  
 创建全文索引时，建议为每个索引列都指定一种语言。 如果未为列指定语言，则将使用系统默认语言。 某列的语言确定使用什么断字符和词干分析器对该列创建索引。 另外，该语言的同义词库文件将由针对相应列的全文查询使用。  
  
 如果要选择用于创建全文索引的列语言，有几个事项需要注意。 这些注意事项均与全文引擎如何对文本进行词汇切分再编制其索引有关。 有关详细信息，请参阅 [创建全文索引时选择语言](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md)。  
  
若要查看特定列的断字符语言，请运行以下语句。
   
```sql 
SELECT language_id AS 'LCID' FROM sys.fulltext_index_columns;
```  

如需其他选项和详细信息，请参阅 [sys.fulltext_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)。

##  <a name="troubleshoot-word-breaking-time-out-errors"></a><a name="tshoot"></a> 排查断字超时错误  
 在多种情况下会发生断字超时错误。 若要了解这些情况以及如何在每种情况下进行响应，请参阅 [MSSQLSERVER_30053](../errors-events/mssqlserver-30053-database-engine-error.md)。

### <a name="info-about-the-mssqlserver_30053-error"></a>有关 MSSQLSERVER_30053 错误的信息
  
|properties|值|
|-|-|
|产品名称|SQL Server|  
|事件 ID|30053|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|FTXT_QUERY_E_WORDBREAKINGTIMEOUT|  
|消息正文|全文查询字符串出现断字超时。 如果断字器长时间处理全文查询字符串或服务器上有大量查询正在运行，则会发生这种情况。 请在负荷较轻的情况下再次尝试运行此查询。|  
  
#### <a name="explanation"></a>说明  
 在以下情况下会发生断字超时错误：  
  
-   查询语言的断字符配置不正确；例如，其注册表设置不正确。  
  
-   特定查询字符串的断字符工作不正常。  
  
-   断字符为特定查询字符串返回过多数据。 过多的数据将被视为潜在的缓冲区溢出攻击，并会关闭承载断字服务的筛选器后台进程 (fdhost.exe)。  
  
-   筛选器后台进程配置不正确。  
  
     最常见的配置问题是密码过期或阻止筛选器后台帐户登录的域策略。  
  
-   服务器实例上运行的查询工作负荷很重；例如，断字器长时间处理全文查询字符串或服务器上有大量查询正在运行。 请注意，此原因发生的可能性最低。  
  
#### <a name="user-action"></a>用户操作  
 针对超时的可能原因选择适当的用户操作，如下所示：  
  
|可能的原因|用户操作|  
|--------------------|-----------------|  
|查询语言的断字符配置不正确。|如果使用的是第三方断字符，则该断字符在操作系统中的注册可能不正确。 在这种情况下，请重新注册断字符。 有关详细信息，请参阅[将搜索功能所使用的断字符还原到以前的版本](revert-the-word-breakers-used-by-search-to-the-previous-version.md)。|  
|特定查询字符串的断字符工作不正常。|如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持断字符，请与 Microsoft 客户服务与支持部门联系。|  
|断字符为特定查询字符串返回过多数据。|如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持断字符，请与 Microsoft 客户服务与支持部门联系。|  
|筛选器后台进程配置不正确。|请确保使用的是当前密码，并且域策略不会阻止筛选器后台帐户登录。|  
|服务器实例上运行的查询工作负荷很重。|请在负荷较轻的情况下再次尝试运行此查询。|  

##  <a name="understand-the-impact-of-updated-word-breakers"></a><a name="impact"></a> 了解已更新断字符的影响  
 每个版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通常都包括新的断字符，这些断字符与早期断字符相比具有更好的语言规则并且更加准确。 新断字符的行为可能与从以前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]导入的全文索引中的断字符行为稍有不同。
 
如果全文目录是在数据库升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的当前版本时导入的，这一点将非常重要。 该全文目录中全文索引使用的一种或多种语言现在可能与新断字符关联。 有关详细信息，请参阅 [全文搜索升级](../../relational-databases/search/upgrade-full-text-search.md)。  
 
## <a name="see-also"></a>另请参阅  
 [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md)    
 [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)   
 [为全文搜索配置和管理非索引字和非索引字表](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   

