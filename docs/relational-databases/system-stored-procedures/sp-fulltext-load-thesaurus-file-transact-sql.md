---
description: sp_fulltext_load_thesaurus_file (Transact-SQL)
title: sp_fulltext_load_thesaurus_file (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_load_thesaurus_file
- sp_fulltext_load_thesaurus_file_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_load_thesaurus_file
- full-text indexes [SQL Server], thesaurus files
- thesaurus [full-text search], editing
ms.assetid: 73a309c3-6d22-42dc-a6fe-8a63747aa2e4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 129bc6385628ef44d5103ddbac5f8ee7f51889ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185413"
---
# <a name="sp_fulltext_load_thesaurus_file-transact-sql"></a>sp_fulltext_load_thesaurus_file (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  使服务器实例从指定 LCID 的语言的对应同义词库文件中分析并加载数据。 在更新同义词库文件后，此存储过程非常有用。 执行 **sp_fulltext_load_thesaurus_file** 会导致重新编译使用指定 LCID 的同义词库的全文查询。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sys.sp_fulltext_load_thesaurus_file lcid [ , @loadOnlyIfNotLoaded  = action ]   
```  
  
## <a name="arguments"></a>参数  
 *lcid*  
 映射某种语言的区域设置标识符 (LCID) 的整数，您要为该语言加载同义词库 XML 定义。 若要获取服务器实例上可用语言的 Lcid，请使用 [sys.fulltext_languages &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md) 目录视图。  
  
 **\@ loadOnlyIfNotLoaded**  =  *操作*  
 指定是否即使在同义词库文件已加载的情况下也将它加载到内部同义词库表中。 *操作* 是以下其中之一：  
  
|值|定义|  
|-----------|----------------|  
|**0**|无论同义词库文件是否已加载都加载它。 这是 **sp_fulltext_load_thesaurus_file** 的默认行为。|  
|1|只有在同义词库文件尚未加载的情况下才加载它。|  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 同义词库文件是使用同义词库的全文查询自动加载的。 为了避免这种第一次对全文查询的性能影响，我们建议您执行 **sp_fulltext_load_thesaurus_file**。  
  
 使用 [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)"**update_languages**" 更新在全文搜索中注册的语言的列表。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或系统管理员才能执行 **sp_fulltext_load_thesaurus_file** 存储过程。  
  
 只有系统管理员能够更新、修改或删除同义词库文件。  
  
## <a name="examples"></a>示例  
  
### <a name="a-load-a-thesaurus-file-even-if-it-is-already-loaded"></a>A. 即使在同义词库文件已加载的情况下也加载它  
 下面的示例分析并加载英语同义词库文件。  
  
```sql
EXEC sys.sp_fulltext_load_thesaurus_file 1033;
```  
  
### <a name="b-load-a-thesaurus-file-only-if-it-is-not-yet-loaded"></a>B. 只有在同义词库文件尚未加载的情况下才加载它  
 下面的示例分析并加载阿拉伯语同义词库文件（除非它已经加载）。  
  
```sql
EXEC sys.sp_fulltext_load_thesaurus_file 1025, @loadOnlyIfNotLoaded = 1;
```  

## <a name="see-also"></a>另请参阅

[FULLTEXTSERVICEPROPERTY (Transact-SQL)](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)  
[系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
[为全文搜索配置和管理同义词库文件](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)
