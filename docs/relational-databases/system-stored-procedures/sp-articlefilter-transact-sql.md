---
description: sp_articlefilter (Transact-SQL)
title: sp_articlefilter (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_articlefilter_TSQL
- sp_articlefilter
helpviewer_keywords:
- sp_articlefilter
ms.assetid: 4c3fee32-a43f-4757-a029-30aef4696afb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6e39f67440e57c2c4a725db45e6e8d8deda3c8b3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203251"
---
# <a name="sp_articlefilter-transact-sql"></a>sp_articlefilter (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  基于表项目筛选发布的数据。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_articlefilter [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @filter_name = ] 'filter_name' ]  
    [ , [ @filter_clause = ] 'filter_clause' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 包含项目的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 项目的名称。 *项目* 是 **sysname**，无默认值。  
  
`[ @filter_name = ] 'filter_name'` 要从 *filter_name* 创建的筛选存储过程的名称。 *filter_name* 为 **nvarchar (386)**，默认值为 NULL。 您必须为项目筛选指定唯一的名称。  
  
`[ @filter_clause = ] 'filter_clause'` 用于定义水平筛选器的) 子句 (的限制。 当输入限制子句时，将省略关键字 WHERE。 *filter_clause* 为 **ntext**，默认值为 NULL。  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` 确认此存储过程所执行的操作是否会使现有快照失效。 *force_invalidate_snapshot* 为一个 **位**，默认值为 **0**。  
  
 **0** 指定对项目所做的更改不会导致快照无效。 如果该存储过程检测到更改确实需要新的快照，则会发生错误，并且不进行任何更改。  
  
 **1** 指定对项目所做的更改可能导致快照无效，如果存在需要新快照的现有订阅，则授予将现有快照标记为过时并生成新快照的权限。  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` 确认此存储过程所执行的操作可能需要重新初始化现有订阅。 *force_reinit_subscription* 为一个 **位**，默认值为 **0**。  
  
 **0** 指定对项目所做的更改不会导致重新初始化订阅。 如果该存储过程检测到更改将需要重新初始化订阅，则会发生错误，并且不进行任何更改。  
  
 **1** 指定对项目所做的更改会导致重新初始化现有订阅，并授予重新初始化订阅的权限。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  *发布服务器* 不应与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器一起使用。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_articlefilter** 用于快照复制和事务复制。  
  
 为包含现有订阅的项目执行 **sp_articlefilter** 需要重新初始化这些订阅。  
  
 **sp_articlefilter** 创建筛选器，将筛选存储过程的 ID 插入到 [sysarticles &#40;transact-sql&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md)表的 "**筛选器**" 列中，然后在 " **filter_clause** " 列中插入限制子句的文本。  
  
 若要创建包含水平筛选器的项目，请执行不带 *filter* 参数 [&#40;transact-sql&#41;sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) 。 执行 **sp_articlefilter**，提供包括 *filter_clause* 在内的所有参数，然后 [&#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)中执行 sp_articleview，同时提供包括相同 *filter_clause* 的所有参数。 如果筛选器已经存在，并且 **sysarticles** 中的 **类型** 为 **1** (基于日志的项目) ，则将删除上一个筛选器并创建新的筛选器。  
  
 如果未提供 *filter_name* 和 *filter_clause* ，则删除上一个筛选器，并将筛选器 ID 设置为 **0**。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-articlefilter-transac_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_articlefilter**。  
  
## <a name="see-also"></a>另请参阅  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [定义和修改静态行筛选器](../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)   
 [sp_addarticle &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articleview &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)   
 [sp_changearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
