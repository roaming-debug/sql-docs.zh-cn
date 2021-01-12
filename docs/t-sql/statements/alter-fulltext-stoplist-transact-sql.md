---
description: ALTER FULLTEXT STOPLIST (Transact-SQL)
title: ALTER FULLTEXT STOPLIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER FULLTEXT STOPLIST
- ALTER_FULLTEXT_STOPLIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- stoplists [full-text search]
- full-text search [SQL Server], stoplists
- ALTER FULLTEXT STOPLIST statement
- full-text search [SQL Server], stopwords
- stopwords [full-text search]
ms.assetid: f6ad87d5-6a34-435a-8456-8244947c5c83
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 12e90f8d8b2d0b35ecd406af54f55b6ea96cb4c6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98082308"
---
# <a name="alter-fulltext-stoplist-transact-sql"></a>ALTER FULLTEXT STOPLIST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  在当前数据库的默认全文非索引字表中插入或删除非索引字。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER FULLTEXT STOPLIST stoplist_name  
{   
        ADD [N] 'stopword' LANGUAGE language_term    
  | DROP   
    {  
        'stopword' LANGUAGE language_term   
      | ALL LANGUAGE language_term   
      | ALL  
     }  
;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 stoplist_name  
 要更改的非索引字表的名称。 stoplist_name 最多可以包含 128 个字符。  
  
 **'** *stopword* **'**  
 一个字符串，它可能是在指定的语言中具有语义的词或没有语义的标记。 stopword 不能超过最大标记长度（64 个字符）。 非索引字可被指定为 Unicode 字符串。  
  
 LANGUAGE language_term  
 指定与要添加或删除的 stopword 关联的语言。  
  
 可将 language_term 指定为与语言区域设置标识符 (LCID) 对应的字符串、整数或十六进制值，如下所示：  
  
|格式|说明|  
|------------|-----------------|  
|String|language_term 对应于 [sys.syslanguages (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) 兼容性视图中 alias 列的值。 字符串必须用单引号引起来，例如 'language_term'。|  
|Integer|language_term 是语言的 LCID。|  
|十六进制|language_term 将以 0x 开头，后面跟有 LCID 的十六进制值。 十六进制值不能超过八位（包括前导零在内）。 如果该值是双字节字符集 (DBCS) 格式，则 SQL Server 将其转换为 Unicode 格式。|  
  
 ADD **'** _stopword_*_'_* LANGUAGE *language_term*  
 将非索引字添加到由 LANGUAGE language_term 指定的语言的非索引字表中。  
  
 如果指定的关键字及语言 LCID 值组合在 STOPLIST 中不是唯一的，则将返回错误。  如果 LCID 值与已注册的语言不对应，则会产生一个错误。  
  
 DROP { **'** _stopword_*_'_* LANGUAGE *language_term* | ALL LANGUAGE *language_term* | ALL }  
 从非索引字表中删除非索引字。  
  
 **'** *stopword* **'** LANGUAGE *language_term*  
 删除由 language_term 指定的语言的指定非索引字。  
  
 ALL LANGUAGE *language_term*  
 删除由 language_term 指定的语言的所有非索引字。  
  
 ALL  
 删除非索引字表中的所有非索引字。  
  
## <a name="remarks"></a>备注  
 只有在兼容级别为 100 或更高时，才支持 CREATE FULLTEXT STOPLIST。 在 80 和 90 兼容级别下，会始终为数据库分配系统非索引字表。  
  
## <a name="permissions"></a>权限  
 若要将非索引字表指定为数据库的默认非索引字表，则需要 ALTER DATABASE 权限。 另外，若要更改非索引字表，则必须是非索引字表的所有者或者具有 db_owner 或 db_ddladmin 固定数据库角色的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例将更改名为 `CombinedFunctionWordList` 的非索引字表，首先为西班牙语添加单词“en”，然后为法语添加单词“en”。  
  
```sql  
ALTER FULLTEXT STOPLIST CombinedFunctionWordList ADD 'en' LANGUAGE 'Spanish';  
ALTER FULLTEXT STOPLIST CombinedFunctionWordList ADD 'en' LANGUAGE 'French';  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE FULLTEXT STOPLIST (Transact-SQL)](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md)   
 [DROP FULLTEXT STOPLIST (Transact-SQL)](../../t-sql/statements/drop-fulltext-stoplist-transact-sql.md)   
 [为全文搜索配置和管理非索引字和非索引字表](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [sys.fulltext_stoplists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)   
 [sys.fulltext_stopwords (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)   
 [为全文搜索配置和管理非索引字和非索引字表](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)  
  
  
