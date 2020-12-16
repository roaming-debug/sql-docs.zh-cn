---
description: 为全文搜索配置和管理非索引字和非索引字表
title: 为全文搜索配置和管理非索引字和非索引字表
ms.date: 02/02/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- stoplists [full-text search]
- full-text search [SQL Server], stoplists
- full-text search [SQL Server], noise words
- noise words [full-text search]
- full-text search [SQL Server], stopwords
- stopwords [full-text search]
ms.assetid: 43b5ce7b-9f09-4443-8a5b-c3da6eb28bcc
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: a7da4fa25883f6deee5c0af8afa7df61b6bf5807
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460130"
---
# <a name="configure-and-manage-stopwords-and-stoplists-for-full-text-search"></a>为全文搜索配置和管理非索引字和非索引字表
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  为了精简全文检索， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供了一种机制，用于去掉那些经常出现但对搜索无益的字符串。 这些去掉的字符串称为“非索引字” 。 在索引创建期间，全文引擎将忽略全文检索中的非索引字。 也就是说全文查询将不搜索非索引字。  
   
**非索引字**。 非索引字可以是特定语言中具有含义的单词。 例如，在英语中，诸如“a”、“and”、“is”和“the”之类的词将被排除在全文检索之外，这是因为已经知道它们对搜索没有用处。 非索引字也可以是没有语言含义的“令牌”。  

**非索引字表**。 使用称为“非索引字表”的对象在数据库中管理非索引字。 “非索引字表”是一个由非索引字组成的列表，这些非索引字在与全文检索关联时会应用于该索引的全文查询。
   
## <a name="use-an-existing-stoplist"></a>使用现有的非索引字表  
 可以通过以下方式使用现有的非索引字表：  
  
-   在数据库中使用系统提供的非索引字表。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为每种支持的语言（即默认情况下与给定断字符关联的每种语言）都附带了一个包含最常用非索引字的系统非索引字表。 可以复制系统非索引字表并通过添加和删除非索引字来自定义自己的副本。  
  
     系统非索引字表安装在 [Resource](../../relational-databases/databases/resource-database.md) 数据库中。  
  
-   在当前服务器实例中使用其他数据库中的现有自定义非索引字表，然后根据需要添加或删除非索引字。  
  
## <a name="create-a-new-stoplist"></a>创建新的非索引字表 
### <a name="create-a-new-stoplist-with-transact-sql"></a>使用 Transact-SQL 创建新的非索引字表
使用 [CREATE FULLTEXT STOPLIST](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md)。

### <a name="create-a-new-stoplist-with-management-studio"></a>使用 Management Studio 创建新的非索引字表
  
1.  在对象资源管理器中，展开服务器。  
  
2.  展开“数据库”，然后展开要在其中创建全文非索引字表的数据库。  
  
3.  展开“存储”，然后右键单击“全文非索引字表”。  
  
4.  选择“新建全文非索引字表”。  
  
5.  输入新的非索引字表的名称。  
  
6.  （可选）将另外某人指定为非索引字表所有者。  
  
7.  从下列创建非索引字表的选项中选择一个：  
  
    -   **创建空非索引字表**  
  
    -   **从系统非索引字表创建**  
  
    -   **从现有全文非索引字表创建**  
  
     有关详细信息，请参阅[新建全文非索引字表（常规页）](/previous-versions/sql/sql-server-2016/cc280518(v=sql.130))。  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="use-a-stoplist-in-full-text-queries"></a>在全文查询中使用非索引字表  
 若要在查询中使用非索引字表，必须将该非索引字表与全文索引关联。 可以在创建全文检索时将非索引字表附加到全文检索中，也可以在以后更改索引来添加非索引字表。  
  
### <a name="create-a-full-text-index-and-associate-a-stoplist-with-it"></a>创建全文索引并将非索引字表与其关联起来
使用 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)。
  
### <a name="associate-or-disassociate-a-stoplist-with-an-existing-full-text-index"></a>将非索引字表与现有的全文索引关联起来或取消它们之间的关联
使用 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)。 
  
## <a name="change-the-stopwords-in-a-stoplist"></a>在非索引字表中更改非索引字  
### <a name="add-or-drop-stopwords-from-a-stoplist-with-transact-sql"></a>通过 Transact-SQL 向非索引字表中添加非索引字或从中删除非索引字
使用 [ALTER FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md)。
  
### <a name="add-or-drop-stopwords-from-a-stoplist-with-management-studio"></a>通过 Management Studio 向非索引字表中添加非索引字或从中删除非索引字  
  
1.  在对象资源管理器中，展开服务器。  
  
2.  展开 **“数据库”**，然后展开数据库。  
  
3.  展开 **“存储”**，然后选择 **“全文非索引字表”**。  
  
4.  右键单击要更改其属性的非索引字表，然后选择“属性”。  
  
5.  在“ [全文非索引字表属性](/previous-versions/sql/sql-server-2016/cc280415(v=sql.130)) ”对话框中：  
  
    1.  在 **“操作”** 列表框中，选择下列操作之一： **“添加非索引字”**、 **“删除非索引字”**、 **“删除所有非索引字”** 或 **“清除非索引字表”**。  
  
    2.  如果对选定的操作启用了 **“非索引字”** 文本框，请输入一个非索引字。 该非索引字必须是唯一的，也就是说，在针对所选语言的此非索引字表中还不存在该非索引字。  
  
    3.  如果对选定的操作启用了“全文语言”列表框，请选择一种语言。  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

## <a name="manage-stoplists-and-their-usage"></a>管理非索引字表及其用法
  
### <a name="view-all-the-stopwords-in-a-stoplist"></a>在非索引字表中查看所有非索引字
使用 [sys.fulltext_stopwords &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)。 
  
### <a name="get-info-about-all-the-stoplists-in-the-current-database"></a>获取有关当前数据库中所有非索引字表的信息
使用 [sys.fulltext_stoplists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md) 和 [sys.fulltext_stopwords &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)。
  
### <a name="view-the-tokenization-result-of-a-word-breaker-thesaurus-and-stoplist-combination"></a>查看断字符、同义词库和非索引字表组合的词汇切分结果
使用 [sys.dm_fts_parser &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-parser-transact-sql.md)。

### <a name="suppress-an-error-message-if-stopwords-cause-a-boolean-operation-on-a-full-text-query-to-fail"></a>取消非索引字导致全文查询的布尔操作失败时产生的错误消息
使用[“转换干扰词”服务器配置选项](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md)。 
   
## <a name="more-info-about-stopword-position"></a>有关非索引字位置的详细信息
 尽管全文检索会忽略所包含的非索引字，但它确实会考虑非索引字的位置。 例如，请看下面这个短语：“Instructions are applicable to these Adventure Works Cycles models”。 下表显示了短语中各个词的位置：  
  
|Word|位置|  
|----------|--------------|  
|Instructions|1|  
|are|2|  
|applicable|3|  
|设置为|4|  
|these|5|  
|Adventure|6|  
|Works|7|  
|周期|8|  
|模型|9|  
  
 分别在第 2、第 4 和第 5 个位置的非索引字“are”、“to”和“these”将被排除在全文检索之外。 但是会保留它们的位置信息，从而使短语中其他词的位置不受影响。   
  
## <a name="upgrade-noise-words-from-sql-server-2005"></a>从 SQL Server 2005 升级干扰词  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 干扰词已替换为非索引字。 从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]升级数据库后，将不再使用干扰词文件。 然而，干扰词文件存储在 FTDATA\FTNoiseThesaurusBak 文件夹中，您可以在以后更新或生成对应的非索引字表时使用它们。 有关将干扰词文件升级到非索引字表的信息，请参阅 [升级全文搜索](../../relational-databases/search/upgrade-full-text-search.md)。  
  
  
