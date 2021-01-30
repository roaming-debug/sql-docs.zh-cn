---
description: sp_droparticle (Transact-SQL)
title: sp_droparticle (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_droparticle_TSQL
- sp_droparticle
helpviewer_keywords:
- sp_droparticle
ms.assetid: 09fec594-53f4-48a5-8edb-c50731c7adb2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 42b89405015235d5c469b2443995ea5038e3cd5f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200683"
---
# <a name="sp_droparticle-transact-sql"></a>sp_droparticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  从快照发布或事务发布中删除一个项目。 如果项目存在一个或多个订阅，则不能删除该项目。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_droparticle [ @publication= ] 'publication'  
        , [ @article= ] 'article'  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @force_invalidate_snapshot= ] force_invalidate_snapshot ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @from_drop_publication = ] from_drop_publication ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 包含要删除的项目的发布名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 要删除的项目的名称。 *项目* 是 **sysname**，无默认值。  
  
`[ @ignore_distributor = ] ignore_distributor` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` 确认此存储过程所执行的操作是否会使现有快照失效。 *force_invalidate_snapshot* 为一个 **位**，默认值为 **0**。  
  
 **0** 指定对项目所做的更改不会导致快照无效。 如果该存储过程检测到更改确实需要新的快照，则会发生错误，并且不进行任何更改。  
  
 **1** 指定对项目所做的更改可能导致快照无效，如果存在需要新快照的现有订阅，则授予将现有快照标记为过时并生成新快照的权限。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  更改发布服务器上的项目属性时，不应使用 *publisher* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
`[ @from_drop_publication = ] from_drop_publication` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_droparticle** 用于快照复制和事务复制。  
  
 对于水平筛选的项目， **sp_droparticle** 会检查 [sysarticles &#40;transact-sql&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md)表中项目的 **类型** 列，以确定是否也应删除视图或筛选器。 如果视图或筛选是自动生成的，则将其与项目一起删除。 如果是手动创建的，则不将其删除。  
  
 执行 **sp_droparticle** 从发布中删除项目时，不会从发布数据库中删除该对象，也不会从订阅数据库中删除相应的对象。 如果需要，请使用 `DROP <object>` 手动删除这些对象。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_droparticle](../../relational-databases/replication/codesnippet/tsql/sp-droparticle-transact-_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_droparticle**。  
  
## <a name="see-also"></a>另请参阅  
 [删除项目](../../relational-databases/replication/publish/delete-an-article.md)   
 [向现有发布添加项目和从中删除项目](../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)   
 [sp_addarticle &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_helparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
