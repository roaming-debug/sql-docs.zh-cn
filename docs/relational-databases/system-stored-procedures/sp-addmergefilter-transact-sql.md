---
description: sp_addmergefilter (Transact-SQL)
title: sp_addmergefilter (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergefilter
- sp_addmergefilter_TSQL
helpviewer_keywords:
- sp_addmergefilter
ms.assetid: 4c118cb1-2008-44e2-a797-34b7dc34d6b1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 836f493a3ae4ca6d3f17928af0d5faa367bdd1b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192333"
---
# <a name="sp_addmergefilter-transact-sql"></a>sp_addmergefilter (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  添加新合并筛选以创建基于与另一个表的联接的分区。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addmergefilter [ @publication = ] 'publication'   
        , [ @article = ] 'article'   
        , [ @filtername = ] 'filtername'   
        , [ @join_articlename = ] 'join_articlename'   
        , [ @join_filterclause = ] join_filterclause  
    [ , [ @join_unique_key = ] join_unique_key ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @filter_type = ] filter_type ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 要在其中添加合并筛选器的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 要添加合并筛选器的项目的名称。 *项目* 是 **sysname**，无默认值。  
  
`[ @filtername = ] 'filtername'` 筛选器的名称。 *filtername* 是必需的参数。 *filtername* 的值为 **sysname**，无默认值。  
  
`[ @join_articlename = ] 'join_articlename'`*项目* 所指定的子项目必须使用 *join_filterclause* 指定的联接子句进行联接，以确定子项目中满足合并筛选器条件的行的父项目。 *join_articlename* **sysname**，无默认值。 项目必须位于 *发布* 所指定的发布中。  
  
`[ @join_filterclause = ] join_filterclause`必须用于联接由 *join_article* 指定的 *项目* 和父项目指定的子项目的 join 子句，以便确定符合合并筛选器的行。 *join_filterclause* **(1000) 为 nvarchar**。  
  
`[ @join_unique_key = ] join_unique_key` 指定子 *项目和父项目* *join_article* 之间的联接是一对多、一对一、多对一还是多对多的联接）。 *join_unique_key* 的值为 **int**，默认值为0。 **0** 指示多对一或多对多联接。 " **1** " 指示一对一或一对多联接。 如果联接列在 *join_article* 中形成唯一键，或者 *join_filterclause* 在 *项目* 的外键与 *join_article* 的主键之间，则此值为 **1** 。  
  
> [!CAUTION]  
>  仅当对父项目的基础表中的联接列具有保证唯一性的约束时，才将此参数设置为 **1** 。 如果不正确地将 *join_unique_key* 设置为 **1** ，则可能会导致数据无法收敛。  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` 确认此存储过程所执行的操作是否会使现有快照失效。 *force_invalidate_snapshot* 是一 **位**，默认值为 **0**。  
  
 **0** 指定对合并项目所做的更改不会导致快照无效。 如果存储过程检测到更改确实需要新的快照，则将发生错误，并且不会进行任何更改。  
  
 **1** 指定对合并项目所做的更改可能导致快照无效，如果存在需要新快照的现有订阅，则授予将现有快照标记为过时并生成新快照的权限。  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` 确认此存储过程所执行的操作可能需要重新初始化现有订阅。 *force_reinit_subscription* 为一个 **位**，默认值为0。  
  
 **0** 指定对合并项目所做的更改不会导致重新初始化订阅。 如果该存储过程检测到更改需要重新初始化订阅，则会发生错误，并且不进行任何更改。  
  
 **1** 指定对合并项目所做的更改将导致重新初始化现有订阅，并授予重新初始化订阅的权限。  
  
`[ @filter_type = ] filter_type` 指定要添加的筛选器的类型。 *filter_type* 为 **tinyint**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|仅为联接筛选器。 需要它来支持 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器。|  
|**2**|仅为逻辑记录关系。|  
|**3**|同时为联接筛选器和逻辑记录关系。|  
  
 有关详细信息，请参阅[通过逻辑记录对相关行的更改进行分组](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_addmergefilter** 用于合并复制。  
  
 **sp_addmergefilter** 只能与表项目一起使用。 不支持视图项目和索引视图项目。  
  
 此过程也可用于在两个可能或不可能具有联接筛选器的项目之间添加逻辑关系。 *filter_type* 用于指定要添加的合并筛选器是联接筛选器、逻辑关系还是同时为两者。  
  
 若要使用逻辑记录，发布和项目必须满足许多要求。 有关详细信息，请参阅[通过逻辑记录对相关行的更改进行分组](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)。  
  
 通常，该选项用于具有对已发布的主键表的外键引用的项目，而且该主键表含有在其项目中定义的筛选。 主键行的子集用于确定复制到订阅服务器的外键行。  
  
 当两个已发布项目的源表共享相同的表对象名称时，不能在这两个项目之间添加联接筛选器。 在这种情况下，即使两个表属于不同的架构并具有唯一的项目名，也无法创建联接筛选器。  
  
 当在一个表项目上同时使用参数化行筛选器和联接筛选器时，复制会确定行是否属于某个订阅服务器的分区。 它通过使用 [or](../../t-sql/language-elements/or-transact-sql.md) 运算符) 计算筛选函数或联接筛选器 (，而不是使用 [AND](../../t-sql/language-elements/and-transact-sql.md) 运算符) 来计算两个 (条件的交集。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_addmergefilter](../../relational-databases/replication/codesnippet/tsql/sp-addmergefilter-transa_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addmergefilter**。  
  
## <a name="see-also"></a>另请参阅  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [定义和修改合并项目间的联接筛选器](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Join Filters](../../relational-databases/replication/merge/join-filters.md)   
 [sp_changemergefilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changemergefilter-transact-sql.md)   
 [sp_dropmergefilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md)   
 [sp_helpmergefilter &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpmergefilter-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
