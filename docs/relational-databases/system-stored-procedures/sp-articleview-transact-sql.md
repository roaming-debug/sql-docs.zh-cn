---
description: sp_articleview (Transact-SQL)
title: sp_articleview (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_articleview
- sp_articleview_TSQL
helpviewer_keywords:
- sp_articleview
ms.assetid: a3d63fd6-f360-4a2f-8a82-a0dc15f650b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 014ccf37727534d16fa5b16bc280da5b76dfb76e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203200"
---
# <a name="sp_articleview-transact-sql"></a>sp_articleview (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  在垂直或水平筛选表时创建用于定义已发布项目的视图。 该视图用作目标表的架构和数据的筛选源。 只有未订阅的项目才能由此存储过程修改。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_articleview [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @view_name = ] 'view_name']  
    [ , [ @filter_clause = ] 'filter_clause']  
    [ , [ @change_active = ] change_active ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @refreshsynctranprocs = ] refreshsynctranprocs ]  
    [ , [ @internal = ] internal ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 包含项目的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 项目的名称。 *项目* 是 **sysname**，无默认值。  
  
`[ @view_name = ] 'view_name'` 用于定义已发布项目的视图的名称。 *view_name* 为 **nvarchar (386)**，默认值为 NULL。  
  
`[ @filter_clause = ] 'filter_clause'` 用于定义水平筛选器的) 子句 (的限制。 当输入限制子句时，将省略 WHERE 关键字。 *filter_clause* 为 **ntext**，默认值为 NULL。  
  
`[ @change_active = ] change_active` 允许修改具有订阅的发布中的列。 *change_active* 为 **int**，默认值为 **0**。 如果为 **0**，则不更改列。 如果为 **1**，则可以在具有订阅的活动项目上创建或重新创建视图。  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` 确认此存储过程所执行的操作是否会使现有快照失效。 *force_invalidate_snapshot* 为一个 **位**，默认值为 **0**。  
  
 **0** 指定对项目所做的更改不会导致快照无效。 如果该存储过程检测到更改确实需要新的快照，则会发生错误，并且不进行任何更改。  
  
 **1** 指定对项目所做的更改可能导致快照无效，如果存在需要新快照的现有订阅，则授予将现有快照标记为过时并生成新快照的权限。  
  
`[ @force_reinit_subscription = ] _force_reinit_subscription_` 确认此存储过程所执行的操作可能需要重新初始化现有订阅。 *force_reinit_subscription* 是一 **位** ，默认值为 **0**。  
  
 **0** 指定对项目所做的更改不会导致重新初始化订阅。 如果该存储过程检测到更改将需要重新初始化订阅，则会发生错误，并且不进行任何更改。  
  
 **1** 指定对项目所做的更改会导致重新初始化现有订阅，并授予重新初始化订阅的权限。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  在从发布服务器进行发布时，不应使用 *publisher* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
`[ @refreshsynctranprocs = ] refreshsynctranprocs` 是否自动重新创建用于同步复制的存储过程。 *refreshsynctranprocs* 的值为 **bit**，默认值为1。  
  
 **1** 表示重新创建存储过程。  
  
 **0** 表示不重新创建存储过程。  
  
`[ @internal = ] internal` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_articleview** 创建用于定义已发布项目的视图，并将此视图的 ID 插入到 [sysarticles &#40;transact-sql&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md)表的 **sync_objid** 列中，并在 **filter_clause** 列中插入限制子句的文本。 如果复制所有列且没有 **filter_clause**，则 [sysarticles &#40;transact-sql&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md)表中的 **sync_objid** 将设置为基表的 ID，而不需要使用 **sp_articleview** 。  
  
 若要发布垂直筛选的表 (即，若要在) 第一次运行 **sp_addarticle** 但不 *sync_object* 参数的情况下筛选列，请对每个要复制的列运行 [sp_articlecolumn &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) 一次 (定义垂直筛选器) ，然后运行 **sp_articleview** 来创建用于定义已发布项目的视图。  
  
 若要发布水平筛选的表 (也就是说，若要筛选) 的行，请在不带 *筛选器* 参数的情况下运行 [sp_addarticle &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) 。 运行 [sp_articlefilter &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)，同时提供包括 *filter_clause* 在内的所有参数。 然后，运行 **sp_articleview**，同时提供所有参数，包括相同的 *filter_clause*。  
  
 若要发布垂直和水平筛选的表，请运行不带 *sync_object* 或 *Filter* 参数 [&#40;transact-sql&#41;sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) 。 为每个要复制的列运行 [sp_articlecolumn &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) 一次，然后在 [&#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md) 和 **sp_articleview** 中运行 sp_articlefilter。  
  
 如果项目已具有定义已发布项目的视图， **sp_articleview** 会删除现有视图，并自动创建一个新视图。 如果手动创建了视图 (**键入** sysarticles 中 [&#40;transact-sql&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md) 为 **5**) ，则不删除现有视图。  
  
 如果创建自定义筛选器存储过程和一个手动定义已发布项目的视图，则不要运行 **sp_articleview**。 相反，请将它们作为 *筛选器* 和 *sync_object* 参数提供给 [&#40;transact-sql&#41;sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)，以及适当的 *类型* 值。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-articleview-transact-_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_articleview**。  
  
## <a name="see-also"></a>另请参阅  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [定义和修改静态行筛选器](../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)   
 [sp_addarticle &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlefilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)   
 [sp_changearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
