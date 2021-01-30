---
description: sp_mergearticlecolumn (Transact-SQL)
title: sp_mergearticlecolumn (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_mergearticlecolumn
- sp_mergearticlecolumn_TSQL
helpviewer_keywords:
- sp_mergearticlecolumn
ms.assetid: b4f2b888-e094-4759-a472-d893638995eb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b5d8d2f63afd421081058dd1b53ec8084f4b5e13
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185314"
---
# <a name="sp_mergearticlecolumn-transact-sql"></a>sp_mergearticlecolumn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对合并发布进行垂直分区。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_mergearticlecolumn [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @column = ] 'column'  
    [ , [ @operation = ] 'operation'   
    [ , [ @schema_replication = ] 'schema_replication' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]   
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]   
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 发布中项目的名称。 *项目* 是 **sysname**，无默认值。  
  
`[ @column = ] 'column'` 标识要在其中创建垂直分区的列。 *列* 的值为 **sysname**，默认值为 NULL。 如果为 NULL 和 `@operation = N'add'`，默认情况下，源表中的所有列将添加到项目。 将 *操作* 设置为 **drop** 时，*列* 不能为 NULL。 若要从项目中排除列，请执行 **sp_mergearticlecolumn** ，并指定 `@operation = N'drop'` 要从指定 *项目* 中删除的列以及每个列。  
  
`[ @operation = ] 'operation'` 是复制状态。 *操作* 为 **nvarchar (4)**，默认值为 ADD。 **添加** 标记列以进行复制。 **drop** 清除列。  
  
`[ @schema_replication = ] 'schema_replication'` 指定在合并代理运行时将传播架构更改。 *schema_replication* 为 **nvarchar (5)**，默认值为 FALSE。  
  
> [!NOTE]  
>  *Schema_replication* 仅支持 **FALSE** 。  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` 启用或禁用使快照失效的功能。 *force_invalidate_snapshot* 为一个 **位**，默认值为 **0**。  
  
 **0** 指定对合并项目所做的更改不会导致快照无效。  
  
 **1** 指定对合并项目所做的更改可能会导致快照无效，如果是这种情况，则值 **1** 将为新快照提供权限。  
  
`[ @force_reinit_subscription = ]force_reinit_subscription_` 启用或禁用使订阅重新初始化的功能。 *force_reinit_subscription* 是一位，默认值为 **0**。  
  
 **0** 指定对合并项目所做的更改不会导致重新初始化订阅。  
  
 **1** 指定对合并项目所做的更改可能导致订阅重新初始化，如果是这种情况，则值 **1** 将授予重新初始化订阅的权限。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_mergearticlecolumn** 用于合并复制。  
  
 如果正在使用自动标识范围管理，则不能从项目删除标识列。 有关详细信息，请参阅[复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)。  
  
 如果创建初始快照后，应用程序设置了新的垂直分区，则一定会生成新的快照且重新应用于每个订阅。 当下一个已计划的快照和分发或合并代理运行时应用快照。  
  
 如果行跟踪用于冲突检测（默认值），则基表最多可包含 1,024 列，但是必须从项目中筛选列，以便最多发布 246 列。 如果使用列跟踪，则基表最多可包含 246 列。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_AddMergeArticle](../../relational-databases/replication/codesnippet/tsql/sp-mergearticlecolumn-tr_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_mergearticlecolumn**。  
  
## <a name="see-also"></a>另请参阅  
 [定义和修改合并项目间的联接筛选器](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [定义和修改合并项目的参数化行筛选器](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [筛选已发布数据](../../relational-databases/replication/publish/filter-published-data.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
