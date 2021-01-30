---
description: sp_articlecolumn (Transact-SQL)
title: sp_articlecolumn (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_articlecolumn
- sp_articlecolumn_TSQL
helpviewer_keywords:
- sp_articlecolumn
ms.assetid: 8abaa8c1-d99e-4788-970f-c4752246c577
author: markingmyname
ms.author: maghan
ms.openlocfilehash: da57a5aa4ae6645c7b1b2ff6377b79160fa110c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203203"
---
# <a name="sp_articlecolumn-transact-sql"></a>sp_articlecolumn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  用于指定项目中包含的列以垂直筛选已发布表中的数据。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_articlecolumn [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @column = ] 'column' ]  
    [ , [ @operation = ] 'operation' ]  
    [ , [ @refresh_synctran_procs = ] refresh_synctran_procs ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @change_active = ] change_actve ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @internal = ] 'internal' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 包含此项目的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 项目的名称。 *项目* 是 **sysname**，无默认值。  
  
`[ @column = ] 'column'` 要添加或删除的列的名称。 *列* 的值为 **sysname**，默认值为 NULL。 如果为 NULL，则发布所有列。  
  
`[ @operation = ] 'operation'` 指定是否在项目中添加或删除列。 *操作* 为 **nvarchar (5)**，默认值为 add。 **添加** 标记列以进行复制。 **drop** 取消对列的标记。  
  
`[ @refresh_synctran_procs = ] refresh_synctran_procs` 指定是否重新生成支持立即更新订阅的存储过程以匹配复制的列数。 *refresh_synctran_procs* 为 **bit**，默认值为 **1**。 如果为 **1**，则重新生成存储过程。  
  
`[ @ignore_distributor = ] ignore_distributor` 指示是否在未连接到分发服务器的情况下执行此存储过程。 *ignore_distributor* 为 **bit**，默认值为 **0**。 如果为 **0**，则必须启用数据库以便发布，并且应刷新项目缓存，以反映项目复制的新列。 如果为 **1**，则允许为驻留在未发布数据库中的项目删除项目列;只应在恢复情况下使用。  
  
`[ @change_active = ] change_active` 允许修改具有订阅的发布中的列。 *change_active* 是 **int** ，默认值为 **0**。 如果为 **0**，则不修改列。 如果为 **1**，则可以从具有订阅的活动项目中添加或删除列。  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` 确认此存储过程所执行的操作是否会使现有快照失效。 *force_invalidate_snapshot* 为一个 **位**，默认值为 **0**。  
  
 **0** 指定对项目所做的更改不会导致快照无效。 如果该存储过程检测到更改确实需要新的快照，则会发生错误，并且不进行任何更改。  
  
 **1** 指定对项目所做的更改可能导致快照无效，如果存在需要新快照的现有订阅，则授予将现有快照标记为过时并生成新快照的权限。  
  
 [**@force_reinit_subscription =** ] *force_reinit_subscription*  
 确认此存储过程所执行的操作可能需要重新初始化现有订阅。 *force_reinit_subscription* 为一个 **位**，默认值为 **0**。  
  
 **0** 指定对项目所做的更改不会导致重新初始化订阅。 如果该存储过程检测到更改将需要重新初始化订阅，则会发生错误，并且不进行任何更改。 **1** 指定对项目所做的更改会导致重新初始化现有订阅，并授予重新初始化订阅的权限。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  *发布服务器* 不应与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器一起使用。  
  
`[ @internal = ] 'internal'` 仅限内部使用。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_articlecolumn** 用于快照复制和事务复制。  
  
 只能使用 **sp_articlecolumn** 筛选取消订阅的项目。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-articlecolumn-transac_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_articlecolumn**。  
  
## <a name="see-also"></a>另请参阅  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [定义和修改列筛选器](../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)   
 [筛选已发布数据](../../relational-databases/replication/publish/filter-published-data.md)   
 [sp_addarticle &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articleview &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)   
 [sp_changearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
