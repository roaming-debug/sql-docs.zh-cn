---
description: sys.fulltext_languages (Transact-SQL)
title: sys.fulltext_languages (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fulltext_languages
- sys.fulltext_languages_TSQL
- fulltext_languages
- fulltext_languages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- languages [full-text search]
- sys.fulltext_languages catalog view
ms.assetid: 2ed6b53d-1cf2-4763-9d58-36ea24a610ef
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 47032ad75fb96842f278533e184307b99017c798
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191495"
---
# <a name="sysfulltext_languages-transact-sql"></a>sys.fulltext_languages (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中注册了断字符的每种语言在此目录视图中均存在对应的一行。 每一行都显示了语言的 LCID 和名称。 为语言注册断字符后，其其他语言资源（词干分析器、干扰词 (非索引字) 和同义词库文件）将可用于全文索引/查询操作。 可以在全文查询和全文索引语句中指定 **name** 或 **lcid** 的值 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。  
   
|列|数据类型|说明|  
|------------|---------------|-----------------|  
|**lcid**|**int**|语言的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 区域设置标识符 (LCID)。|  
|name|**sysname**|是 [sys.sys语言](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) 的别名值，该别名与 **lcid** 的值或数字 lcid 的字符串表示形式相对应。|  
  
## <a name="values-returned-for-default-languages"></a>针对默认语言返回的值  
 下表显示在默认情况下为其注册了断字符的语言的值。  
  
|语言|LCID|  
|--------------|----------|  
|阿拉伯语|1025|  
|孟加拉语（印度）|1093|  
|英语（英国）|2057|  
|保加利亚语|1026|  
|加泰罗尼亚语|1027|  
|中文（香港特别行政区）|3076|  
|中文（澳门特别行政区）|5124|  
|中文（新加坡）|4100|  
|克罗地亚语|1050|  
|捷克语|1029|  
|丹麦语|1030|  
|荷兰语|1043|  
|英语|2052|  
|法语|1036|  
|德语|1031|  
|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 希腊语|1032|  
|古吉拉特语|1095|  
|希伯来语|1037|  
|Hindi|1081|  
|冰岛语|1039|  
|印度尼西亚语|1057|  
|意大利语|1040|  
|日语|1041|  
|卡纳达语|1099|  
|韩语|1042|  
|拉脱维亚语|1062|  
|立陶宛语|1063|  
|马来语（马来西亚）|1086|  
|马拉雅拉姆语|1100|  
|马拉地语|1102|  
|中立|0|  
|挪威语（博克马尔语）|1044|  
|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 波兰语|1045|  
|葡萄牙语（巴西）|1046|  
|葡萄牙语(葡萄牙)|2070|  
|旁遮普语|1094|  
|罗马尼亚语|1048|  
|俄语|1049|  
|塞尔维亚语（西里尔）|3098|  
|塞尔维亚语（拉丁）|2074|  
|简体中文|2052|  
|斯洛伐克语|1051|  
|斯洛文尼亚语|1060|  
|西班牙语|3082|  
|瑞典语|1053|  
|泰米尔语|1097|  
|泰卢固语|1098|  
|泰语|1054|  
|繁体中文|1028|  
|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 土耳其语|1055|  
|乌克兰语|1058|  
|乌尔都语|1056|  
|越南语|1066|  
  
## <a name="remarks"></a>备注  
 若要更新在全文搜索中注册的语言的列表，请使用 [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)"**update_languages**"。  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>另请参阅  
 [sp_fulltext_load_thesaurus_file &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-load-thesaurus-file-transact-sql.md)   
 [sp_fulltext_service (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)   
 [配置和管理断字符和词干分析器以便搜索](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)   
 [为 Full-Text 搜索配置和管理同义词库文件](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)   
 [为全文搜索配置和管理非索引字和非索引字表](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [升级全文搜索](../../relational-databases/search/upgrade-full-text-search.md)  
  
  
