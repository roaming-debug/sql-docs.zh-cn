---
description: sp_reinitsubscription (Transact-SQL)
title: sp_reinitsubscription (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_reinitsubscription
- sp_reinitsubscription_TSQL
helpviewer_keywords:
- sp_reinitsubscription
ms.assetid: d56ae218-6128-4ff9-b06c-749914505c7b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0d8c12b3ca1b27f7560aeff466e51b7f43a9a42e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185663"
---
# <a name="sp_reinitsubscription-transact-sql"></a>sp_reinitsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  将订阅标记为要重新初始化。 此存储过程在发布服务器的推送订阅中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_reinitsubscription [ [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
        , [ @subscriber = ] 'subscriber'  
    [ , [ @destination_db = ] 'destination_db']  
    [ , [ @for_schema_change = ] 'for_schema_change']  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @ignore_distributor_failure = ] ignore_distributor_failure ]   
    [ , [ @invalidate_snapshot = ] invalidate_snapshot ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，默认值为 all。  
  
`[ @article = ] 'article'` 项目的名称。 *项目* 的值为 **sysname**，默认值为 all。 对于即时更新发布， *项目* 必须 **全部** 为;否则，存储过程将跳过发布，并报告错误。  
  
`[ @subscriber = ] 'subscriber'` 订阅服务器的名称。 *订阅服务器* 的 **sysname**，无默认值。  
  
`[ @destination_db = ] 'destination_db'` 目标数据库的名称。 *destination_db* 的值为 **sysname**，默认值为 all。  
  
`[ @for_schema_change = ] 'for_schema_change'` 指示是否由于发布数据库中的架构更改而发生重新初始化。 *for_schema_change* 为 **bit**，默认值为0。 如果为 **0**，则只要整个发布（而不仅仅是部分项目）重新初始化，就会重新激活允许即时更新的发布的活动订阅。 这意味着，架构更改导致启动重新初始化。 如果为 **1**，则在快照代理运行之前，不会重新激活活动订阅。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  *发布服务器* 不应用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。  
  
`[ @ignore_distributor_failure = ] ignore_distributor_failure` 即使分发服务器不存在或脱机，也可以重新初始化。 *ignore_distributor_failure* 为 **bit**，默认值为0。 如果为 **0**，则在分发服务器不存在或处于脱机状态时重新初始化失败。  
  
`[ @invalidate_snapshot = ] invalidate_snapshot` 使现有的发布快照失效。 *invalidate_snapshot* 为 **bit**，默认值为0。 如果为 **1**，则为发布生成新的快照。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_reinitsubscription** 用于事务复制。  
  
 对等事务复制不支持 **sp_reinitsubscription** 。  
  
 对于自动应用初始快照和发布不允许可更新订阅的订阅，执行此存储过程之后必须运行快照代理，以便准备架构和大容量复制程序文件，并使分发代理随后能够重新同步订阅。  
  
 对于自动应用初始快照和发布允许可更新订阅的订阅，分发代理使用先前由快照代理创建的最新架构和大容量复制程序文件重新同步订阅。 如果分发代理不忙，则分发代理在用户执行 **sp_reinitsubscription** 后立即重新同步订阅;否则，可能会在 (分发代理命令提示符参数指定的消息间隔后发生同步) ： **MessageInterval** 。  
  
 **sp_reinitsubscription** 对手动应用初始快照的订阅不起作用。  
  
 若要重新同步对发布的匿名订阅，请将 **所有** 或 NULL 作为 *订阅服务器* 传递。  
  
 事务复制支持项目级的订阅重新初始化。 当项目被标记为要重新初始化后，在下一个同步过程中将在订阅服务器上重新应用项目快照。 但是，如果有相关的项目也由同一个订阅服务器订阅，那么在项目上重新应用快照可能会失败，除非发布中的相关项目在某些情况下也自动重新初始化：  
  
-   如果项目上的预创建命令是“删除”，则项目基对象上的架构绑定视图和架构绑定存储过程也将被标记为要重新初始化。  
  
-   如果项目上的架构选项包括在主键上创建声明引用完整性脚本，则基表同重新初始化项目的基表有外键关系的项目也将被标记为要重新初始化。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_reinittranpushsub](../../relational-databases/replication/codesnippet/tsql/sp-reinitsubscription-tr_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员、 **db_owner** 固定数据库角色的成员或订阅的创建者才能执行 **sp_reinitsubscription**。  
  
## <a name="see-also"></a>另请参阅  
 [重新初始化订阅](../../relational-databases/replication/reinitialize-a-subscription.md)   
 [重新初始化订阅](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
