---
description: sp_changemergefilter (Transact-SQL)
title: sp_changemergefilter (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changemergefilter_TSQL
- sp_changemergefilter
helpviewer_keywords:
- sp_changemergefilter
ms.assetid: e08fdfdd-d242-4e85-817b-9f7a224fe567
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8081fc9a95452d68540acf3b2a5a412910f75219
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159851"
---
# <a name="sp_changemergefilter-transact-sql"></a>sp_changemergefilter (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改某些合并筛选属性。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_changemergefilter [ @publication= ] 'publication'  
        , [ @article= ] 'article'  
        , [ @filtername= ] 'filtername'  
        , [ @property= ] 'property'  
        , [ @value= ] 'value'  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 项目的名称。 *项目* 是 **sysname**，无默认值。  
  
`[ @filtername = ] 'filtername'` 筛选器的当前名称。 *filtername* 的值为 **sysname**，无默认值。  
  
`[ @property = ] 'property'` 要更改的属性的名称。 *属性* 为 **sysname**，无默认值。  
  
`[ @value = ] 'value'` 指定的属性的新值。 *值* 为 **nvarchar (1000)**，无默认值。  
  
 下表说明项目的属性和这些属性的值。  
  
|properties|值|说明|  
|--------------|-----------|-----------------|  
|**filter_type**|**1**|联接筛选器。<br /><br /> 若要支持 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器，此选项是必需的。|  
||**2**|逻辑记录关系。|  
||**3**|联接筛选器也是一种逻辑记录关系。|  
|**filtername**||筛选器名称。|  
|**join_articlename**||联接项目名。|  
|**join_filterclause**||筛选子句。|  
|**join_unique_key**|true|联接位于唯一键上。|  
||**false**|联接没有位于唯一键上。|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` 确认此存储过程所执行的操作是否会使现有快照失效。 *force_invalidate_snapshot* 是一 **位**，默认值为 **0**。  
  
 **0** 指定对合并项目所做的更改不会导致快照无效。 如果该存储过程检测到更改确实需要新的快照，则会发生错误，并且不进行任何更改。  
  
 **1** 表示对合并项目所做的更改可能会导致快照无效，如果存在需要新快照的现有订阅，则授予将现有快照标记为过时并生成新快照的权限。  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` 确认此存储过程所执行的操作可能需要重新初始化现有订阅。 *force_reinit_subscription* 是一 **位** ，默认值为 **0**。  
  
 **0** 指定对合并项目所做的更改不会导致重新初始化订阅。 如果该存储过程检测到更改将需要重新初始化现有订阅，则会发生错误，并且不进行任何更改。  
  
 **1** 表示对合并项目所做的更改将导致重新初始化现有订阅，并授予重新初始化订阅的权限。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_changemergefilter** 用于合并复制。  
  
 更改对合并项目的筛选需要重新创建快照（如果存在快照）。 这是通过将 **\@ force_invalidate_snapshot** 设置为 **1** 来执行的。 而且，如果该项目有订阅，则需要重新初始化订阅。 这是通过将 **\@ force_reinit_subscription** 设置为 **1** 来完成的。  
  
 若要使用逻辑记录，发布和项目必须满足许多要求。 有关详细信息，请参阅[通过逻辑记录对相关行的更改进行分组](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_changemergefilter**。  
  
## <a name="see-also"></a>另请参阅  
 [更改发布和项目属性](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addmergefilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md)   
 [sp_dropmergefilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md)   
 [sp_helpmergefilter &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpmergefilter-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
