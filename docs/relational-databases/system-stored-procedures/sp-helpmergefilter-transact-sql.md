---
description: sp_helpmergefilter (Transact-SQL)
title: sp_helpmergefilter (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergefilter
- sp_helpmergefilter_TSQL
helpviewer_keywords:
- sp_helpmergefilter
ms.assetid: f133a094-0009-4771-b93b-e86a5c01e40b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 862f23713da4bcd611310149b509e34658766006
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179235"
---
# <a name="sp_helpmergefilter-transact-sql"></a>sp_helpmergefilter (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关合并筛选器的信息。 此存储过程在发布服务器的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpmergefilter [ @publication= ] 'publication'   
    [ , [ @article= ] 'article']  
    [ , [ @filtername= ] 'filtername']  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 项目的名称。 *项目* 的默认值为 **sysname**，默认值为 **%** ，它返回所有项目的名称。  
  
`[ @filtername = ] 'filtername'` 要返回其信息的筛选器的名称。 *filtername* 的值为 **sysname**，默认值为 **%** ，它返回有关在项目或发布上定义的所有筛选器的信息。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**join_filterid**|**int**|联接筛选器的 ID。|  
|**filtername**|**sysname**|筛选器名称。|  
|**join article name**|**sysname**|联接项目名。|  
|**join_filterclause**|**nvarchar (2000)**|用于限定联接的筛选子句。|  
|**join_unique_key**|**int**|表示是否在唯一键上联接。|  
|**base table owner**|**sysname**|基表所有者的名称。|  
|**base table name**|**sysname**|基表的名称。|  
|**join table owner**|**sysname**|与基表联接的表所有者的名称。|  
|**join table name**|**sysname**|与基表联接的表名。|  
|**article name**|**sysname**|与基表联接的表项目名。|  
|**filter_type**|**tinyint**|合并筛选器的类型，可以为下面的一种：<br /><br /> **1** = 仅限联接筛选器<br /><br /> **2** = 逻辑记录关系<br /><br /> **3** = 两个|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpmergefilter** 用于合并复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员和 **db_owner** 固定数据库角色的成员才能执行 **sp_helpmergefilter**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_addmergefilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md)   
 [sp_changemergefilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changemergefilter-transact-sql.md)   
 [sp_dropmergefilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
