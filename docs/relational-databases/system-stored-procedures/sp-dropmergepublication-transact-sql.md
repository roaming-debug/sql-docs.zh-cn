---
description: sp_dropmergepublication (Transact-SQL)
title: sp_dropmergepublication (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropmergepublication
- sp_dropmergepublication_TSQL
helpviewer_keywords:
- sp_dropmergepublication
ms.assetid: 9e1cb96e-5889-4f97-88cd-f60cf313ce68
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 35024724255eea714de42f8fb3a974188730fcef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208220"
---
# <a name="sp_dropmergepublication-transact-sql"></a>sp_dropmergepublication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除合并发布及其关联的快照代理。 删除合并发布之前必须删除全部的订阅。 发布中的项目将自动删除。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dropmergepublication [ @publication= ] 'publication'   
    [ , [ @ignore_distributor = ] ignore_distributor ]   
    [ , [ @reserved = ] reserved ]  
    [ , [ @ignore_merge_metadata = ] ignore_merge_metadata ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 要删除的发布的名称。 *发布* 为 **sysname**，无默认值。 如果为 **all**，则删除所有现有合并发布以及与它们关联的快照代理作业。 如果为 *发布* 指定了一个特定值，则只会删除该发布及其关联的快照代理作业。  
  
`[ @ignore_distributor = ] ignore_distributor` 用于在不清除分发服务器上清理任务的情况下删除发布。 *ignore_distributor* 为 **bit**，默认值为 **0**。 重新安装分发服务器时也将使用此参数。  
  
`[ @reserved = ] reserved` 保留供将来使用。 *reserved* 为 **bit**，默认值为 **0**。  
  
`[ @ignore_merge_metadata = ] ignore_merge_metadata` 仅限内部使用。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_dropmergepublication** 用于合并复制。  
  
 **sp_dropmergepublication** 以递归方式删除与发布关联的所有项目，然后删除该发布本身。 如果发布包含一个或更多对它的订阅，则不能删除。 有关如何删除订阅的信息，请参阅 [删除推送订阅](../../relational-databases/replication/delete-a-push-subscription.md) 和 [删除请求订阅](../../relational-databases/replication/delete-a-pull-subscription.md)。  
  
 执行删除发布的 **sp_dropmergepublication** 不会删除发布数据库中的已发布对象，也不会删除订阅数据库中的相应对象。 \<object>如有必要，请使用 DROP 手动删除这些对象。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_dropmergepublication](../../relational-databases/replication/codesnippet/tsql/sp-dropmergepublication-_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_dropmergepublication**。  
  
## <a name="see-also"></a>另请参阅  
 [删除发布](../../relational-databases/replication/publish/delete-a-publication.md)   
 [sp_addmergepublication &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_helpmergepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
