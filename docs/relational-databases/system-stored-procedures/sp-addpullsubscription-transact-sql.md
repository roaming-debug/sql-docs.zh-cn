---
description: sp_addpullsubscription (Transact-SQL)
title: sp_addpullsubscription (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpullsubscription
- sp_addpullsubscription_TSQL
helpviewer_keywords:
- sp_addpullsubscription
ms.assetid: 0f4bbedc-0c1c-414a-b82a-6fd47f0a6a7f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c119173ba184ef8fe3a03c59e90965650bb9c35c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206677"
---
# <a name="sp_addpullsubscription-transact-sql"></a>sp_addpullsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  将请求订阅添加到快照或事务发布中。 此存储过程在订阅服务器上要创建请求订阅的数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addpullsubscription [ @publisher= ] 'publisher'  
    [ , [ @publisher_db= ] 'publisher_db' ]  
        , [ @publication= ] 'publication'  
    [ , [ @independent_agent= ] 'independent_agent' ]  
    [ , [ @subscription_type= ] 'subscription_type' ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @update_mode= ] 'update_mode' ]  
    [ , [ @immediate_sync = ] immediate_sync ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  

> [!NOTE]
> 服务器名称可指定为 `<Hostname>,<PortNumber>` 。 如果在 Linux 或 Windows 上使用自定义端口部署 SQL Server，并且禁用了 browser 服务，则可能需要指定连接的端口号。
  
`[ @publisher_db = ] 'publisher_db'` 发布服务器数据库的名称。 *publisher_db* 的默认值为 **sysname**，默认值为 NULL。 Oracle 发布服务器将忽略 *publisher_db* 。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @independent_agent = ] 'independent_agent'` 指定该发布是否有独立的分发代理。 *independent_agent* 为 **nvarchar (5)**，默认值为 TRUE。 如果为 **true**，则此发布有独立的分发代理。 如果为 **false**，则每个发布服务器数据库/订阅服务器数据库对都有一个分发代理。 *independent_agent* 是发布的属性，并且在此处的值必须与发布服务器上的值相同。  
  
`[ @subscription_type = ] 'subscription_type'` 订阅的类型。 *subscription_type* 为 **nvarchar (9)**，默认值为 **anonymous**。 除非要创建订阅，而不在发布服务器上注册订阅，否则必须为 *subscription_type* 指定 **拉取** 值。 在这种情况下，必须将值指定为 **anonymous**。 如果在订阅配置期间无法建立与发布服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接，则匿名订阅是必需的。  
  
`[ @description = ] 'description'` 发布的说明。 *描述* 为 **nvarchar (100)**，默认值为 NULL。  
  
`[ @update_mode = ] 'update_mode'` 更新的类型。 *update_mode* 为 **nvarchar (30)**，可以为以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**只读** (默认值) |该订阅是只读的。 在订阅服务器上所做的任何更改不会发送回发布服务器。 应在订阅服务器不进行更新时使用。|  
|**同步事务**|支持立即更新订阅。|  
|**queued tran**|支持订阅进行排队更新。 可以在订阅服务器上进行数据修改，将其存储在队列中，然后传播到发布服务器。|  
|**故障转移**|将排队更新作为故障转移的情况下启用用于即时更新的订阅。 可以在订阅服务器上进行数据修改并立即传播到发布服务器。 如果发布服务器与订阅服务器未连接在一起，则可以将在订阅服务器上所做的数据修改存储在队列中，直到订阅服务器与发布服务器重新连接在一起。|  
|**queued failover**|支持将订阅作为排队更新订阅，并允许更改为立即更新模式。 在订阅服务器和发布服务器之间建立连接之前，可以在订阅服务器上修改数据，并将数据修改存储在队列中。 建立起持续连接后，即可将更新模式更改为立即更新。 *对于 Oracle 发布服务器不支持*。|  
  
`[ @immediate_sync = ] immediate_sync` 快照代理每次运行时是否创建或重新创建同步文件。 *immediate_sync* 为 **bit** ，默认值为1，并且必须设置为与 **sp_addpublication** 中 *immediate_sync* 相同的值。*immediate_sync* 是发布的属性，并且在此处的值必须与发布服务器上的值相同。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_addpullsubscription** 用于快照复制和事务复制。  
  
> [!IMPORTANT]  
>  对于在队列中排队的更新订阅，请将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 验证用于与订阅服务器的连接，并为每个订阅服务器连接指定一个不同的帐户。 创建支持排队更新的请求订阅时，复制始终将连接设置为使用 Windows 身份验证（对于请求订阅，复制无法访问使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证所需的订阅服务器中的元数据）。 在这种情况下，应在配置订阅后执行 [sp_changesubscription](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md) 来更改连接以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。  
  
 如果订阅服务器上不存在 [MSreplication_subscriptions &#40;transact-sql&#41;](../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md) 表， **sp_addpullsubscription** 会创建它。 它还向 [MSreplication_subscriptions &#40;transact-sql&#41;](../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md) 表中添加了一行。 对于请求订阅，应首先在发布服务器上调用 [sp_addsubscription &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) 。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addpullsubscription-t_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addpullsubscription**。  
  
## <a name="see-also"></a>另请参阅  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [创建对](../../relational-databases/replication/publish/create-an-updatable-subscription-to-a-transactional-publication.md)[发布](../../relational-databases/replication/subscribe-to-publications.md)的发布订阅的可更新订阅   
 [sp_addpullsubscription_agent &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)   
 [sp_change_subscription_properties &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md)   
 [sp_droppullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
