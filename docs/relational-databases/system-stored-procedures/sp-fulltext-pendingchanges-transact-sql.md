---
description: sp_fulltext_pendingchanges (Transact-SQL)
title: sp_fulltext_pendingchanges (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_fulltext_pendingchanges_TSQL
- sp_fulltext_pendingchanges
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_pendingchanges
ms.assetid: fee042fe-4781-4a33-a01b-d98fb5629f1b
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fa2b81ce294183b005aa59de141d78945f6b3f04
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97427303"
---
# <a name="sp_fulltext_pendingchanges-transact-sql"></a>sp_fulltext_pendingchanges (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  针对正在使用更改跟踪的指定表返回未处理的更改，例如挂起的插入、更新和删除。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_fulltext_pendingchanges table_id  
```  
  
## <a name="arguments"></a>参数  
 table_id  
 表的 ID。 如果该表未进行全文索引，或未对该表启用更改跟踪，则将返回错误。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**键**|*|指定表中的全文键值。|  
|**DocId**|**bigint**|与键值相对应的内部文档标识符 (DocId) 列。|  
|**状态**|**int**|0 = 将从全文索引中删除行。<br /><br /> 1 = 将对行进行全文索引。<br /><br /> 2 = 行是最新的。<br /><br /> -1 = 行处于过渡（进行了批处理，但未提交）状态或错误状态。|  
|**DocState**|**tinyint**|内部文档标识符 (DocId) 映射状态列的原始转储。|  
  
 <sup>* Key 的数据类型与基表中全文键列的数据类型相同。</sup>  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="remarks"></a>备注  
 如果没有要处理的更改，则返回一个空行集。  
  
 Full-Text 搜索查询不返回 **Status** 值为0的行。 这是因为该行已从基表中删除且正等待从全文索引中删除。  
  
 若要确定某个特定表有多少个挂起的更改，请使用 OBJECTPROPERTYEX 函数的 **TableFullTextPendingChanges** 属性。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的全文搜索和语义搜索存储过程 ](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)   
 [OBJECTPROPERTYEX (Transact-SQL)](../../t-sql/functions/objectpropertyex-transact-sql.md)  
  
  
