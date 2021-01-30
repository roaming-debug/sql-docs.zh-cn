---
description: sp_addmergepullsubscription (Transact-SQL)
title: sp_addmergepullsubscription (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepullsubscription_TSQL
- sp_addmergepullsubscription
helpviewer_keywords:
- sp_addmergepullsubscription
ms.assetid: d63909a0-8ea7-4734-9ce8-8204d936a3e4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 54975d84e497f02dd28191c59041553994dac24d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209410"
---
# <a name="sp_addmergepullsubscription-transact-sql"></a>sp_addmergepullsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  添加对合并发布的请求订阅。 此存储过程在订阅服务器的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addmergepullsubscription [ @publication= ] 'publication'   
    [ , [ @publisher= ] 'publisher' ]   
    [ , [ @publisher_db = ] 'publisher_db' ]   
    [ , [ @subscriber_type= ] 'subscriber_type' ]   
    [ , [ @subscription_priority= ] subscription_priority ]   
    [ , [ @sync_type= ] 'sync_type' ]   
    [ , [ @description= ] 'description' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 为 **sysname**，默认值为本地服务器名称。 发布服务器必须为有效服务器。  
  
`[ @publisher_db = ] 'publisher_db'` 发布服务器数据库的名称。 *publisher_db* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @subscriber_type = ] 'subscriber_type'` 订阅服务器的类型。 *subscriber_type* 为 **nvarchar (15)**，可以是 **global**、 **local** 或 **anonymous**。 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本中，本地订阅称为客户端订阅，全局订阅称为服务器订阅。  
  
`[ @subscription_priority = ] subscription_priority` 订阅优先级。 *subscription_priority* 是 **真实** 的，默认值为 NULL。 对于本地订阅和匿名订阅，优先级为 **0.0**。 在检测到冲突时，默认冲突解决程序将使用该优先级来选取入选方。 全局订阅服务器的订阅优先级必须低于 100，因为 100 是发布服务器的优先级。  
  
`[ @sync_type = ] 'sync_type'` 订阅同步类型。 *sync_type* 为 **nvarchar (15)**，默认值为 " **自动**"。 可以是 **自动** 的，也可以是 **none**。 如果为 " **自动**"，则已发布表的架构和初始数据将首先传输到订阅服务器。 如果 **没有**，则假定订阅服务器已拥有已发布表的架构和初始数据。 始终会传输系统表和数据。  
  
> [!NOTE]  
>  建议不要将值指定为 **none**。  
  
`[ @description = ] 'description'` 是此请求订阅的简短说明。 *描述* 为 **nvarchar (255)**，默认值为 NULL。 此值由复制监视器在 " **友好名称** " 列中显示，可用于对被监视的发布的订阅进行排序。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_addmergepullsubscription** 用于合并复制。  
  
 如果使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理同步订阅，则必须在订阅服务器上运行 [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) 存储过程，以创建与发布同步的代理和作业。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_addmergepullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergepullsubscript_0_1.sql)]  
  
 [!code-sql[HowTo#sp_addmergepullsub_websync_anon](../../relational-databases/replication/codesnippet/tsql/sp-addmergepullsubscript_0_2.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addmergepullsubscription**。  
  
## <a name="see-also"></a>另请参阅  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addmergepullsubscription_agent &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)   
 [sp_changemergepullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [sp_helpmergepullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
