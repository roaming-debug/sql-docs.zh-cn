---
description: sp_addmergesubscription (Transact-SQL)
title: sp_addmergesubscription (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergesubscription_TSQL
- sp_addmergesubscription
helpviewer_keywords:
- sp_addmergesubscription
ms.assetid: a191d817-0132-49ff-93ca-76f13e609b38
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d6689fed03126a8e5bc65d8d9e7e556a070ec483
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209392"
---
# <a name="sp_addmergesubscription-transact-sql"></a>sp_addmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  创建推送合并订阅或请求合并订阅。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addmergesubscription [ @publication= ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db= ] 'subscriber_db' ]  
    [ , [ @subscription_type= ] 'subscription_type' ]  
    [ , [ @subscriber_type= ] 'subscriber_type' ]  
    [ , [ @subscription_priority= ] subscription_priority ]  
    [ , [ @sync_type= ] 'sync_type' ]  
    [ , [ @frequency_type= ] frequency_type ]  
    [ , [ @frequency_interval= ] frequency_interval ]  
    [ , [ @frequency_relative_interval= ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor= ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday= ] frequency_subday ]  
    [ , [ @frequency_subday_interval= ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day= ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day= ] active_end_time_of_day ]  
    [ , [ @active_start_date= ] active_start_date ]  
    [ , [ @active_end_date= ] active_end_date ]  
    [ , [ @optional_command_line= ] 'optional_command_line' ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @enabled_for_syncmgr= ] 'enabled_for_syncmgr' ]  
    [ , [ @offloadagent= ] remote_agent_activation]  
    [ , [ @offloadserver= ] 'remote_agent_server_name' ]  
    [ , [ @use_interactive_resolver= ] 'use_interactive_resolver' ]  
    [ , [ @merge_job_name= ] 'merge_job_name' ]  
    [ , [ @hostname = ] 'hostname'  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。 该发布必须已存在。  
  
`[ @subscriber = ] 'subscriber'` 订阅服务器的名称。 *订阅服务器* 的值为 **sysname**，默认值为 NULL。  
  
`[ @subscriber_db = ] 'subscriber_db'` 订阅数据库的名称。 *subscriber_db* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @subscription_type = ] 'subscription_type'` 订阅的类型。 *subscription_type* 为 **nvarchar (15)**，默认值为 PUSH。 如果 **推送**，则添加推送订阅，并在分发服务器上添加合并代理。 如果是 **pull**，则添加请求订阅，而不在分发服务器上添加合并代理。  
  
> [!NOTE]  
>  匿名订阅无需使用此存储过程。  
  
`[ @subscriber_type = ] 'subscriber_type'` 订阅服务器的类型。 *subscriber_type* 为 **nvarchar (15)**，可以为以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**本地** (默认值) |订阅服务器仅对发布服务器是已知的。|  
|**global**|订阅服务器对所有服务器都是已知的。|  
  
 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本中，本地订阅称为客户端订阅，全局订阅称为服务器订阅  
  
`[ @subscription_priority = ] subscription_priority` 表示订阅的优先级的数字。 *subscription_priority* 是 **真实** 的，默认值为 NULL。 对于本地订阅和匿名订阅，优先级为 0.0。 对于全局订阅，优先级必须小于 100.0。  
  
`[ @sync_type = ] 'sync_type'` 订阅同步类型。 *sync_type* 为 **nvarchar (15)**，默认值为 " **自动**"。 可以是 **自动** 的，也可以是 **none**。 如果为 " **自动**"，则已发布表的架构和初始数据将首先传输到订阅服务器。 如果 **没有**，则假定订阅服务器已拥有已发布表的架构和初始数据。 始终会传输系统表和数据。  
  
> [!NOTE]  
>  建议不要将值指定为 **none**。  
  
`[ @frequency_type = ] frequency_type` 指示何时运行合并代理的值。 *frequency_type* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次|  
|**4**|每天|  
|**8**|每周|  
|**10**|每月一次|  
|**20**|每月，相对于频率间隔|  
|**40**|当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理启动时|  
|NULL（默认值）||  
  
`[ @frequency_interval = ] frequency_interval` 合并代理运行的日期。 *frequency_interval* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|星期日|  
|**2**|星期一|  
|**3**|星期二|  
|**4**|星期三|  
|**5**|星期四|  
|**6**|星期五|  
|**7**|星期六|  
|**8**|日期|  
|**9**|工作日|  
|**10**|周末|  
|NULL（默认值）||  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` 每月计划合并频率间隔。 *frequency_relative_interval* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|First|  
|**2**|秒|  
|**4**|第三个|  
|**8**|第四个|  
|**16**|上一个|  
|NULL（默认值）||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor`*Frequency_type* 使用的重复因子。 *frequency_recurrence_factor* 的值为 **int**，默认值为 NULL。  
  
`[ @frequency_subday = ] frequency_subday`*Frequency_subday_interval* 的单位。 *frequency_subday* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次|  
|**2**|秒|  
|**4**|Minute|  
|**8**|小时|  
|NULL（默认值）||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` 每次合并之间 *frequency_subday* 发生的频率。 *frequency_subday_interval* 的值为 **int**，默认值为 NULL。  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` 第一次计划合并代理的时间，格式为 HHMMSS。 *active_start_time_of_day* 的值为 **int**，默认值为 NULL。  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` 停止计划合并代理的时间，格式为 HHMMSS。 *active_end_time_of_day* 的值为 **int**，默认值为 NULL。  
  
`[ @active_start_date = ] active_start_date` 第一次计划合并代理的日期，格式为 YYYYMMDD。 *active_start_date* 的值为 **int**，默认值为 NULL。  
  
`[ @active_end_date = ] active_end_date` 停止计划合并代理的日期，格式为 YYYYMMDD。 *active_end_date* 的值为 **int**，默认值为 NULL。  
  
`[ @optional_command_line = ] 'optional_command_line'` 要执行的可选命令提示符。 *optional_command_line* 为 **nvarchar (4000)**，默认值为 NULL。 此参数用于添加捕获输出并将输出保存到文件的命令，或者用于指定配置文件或属性。  
  
`[ @description = ] 'description'` 是此合并订阅的简短说明。 *描述* 为 **nvarchar (255)**，默认值为 NULL。 此值由复制监视器在 " **友好名称** " 列中显示，可用于对被监视的发布的订阅进行排序。  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` 指定是否可以通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 同步管理器同步订阅。 *enabled_for_syncmgr* 为 **nvarchar (5)**，默认值为 FALSE。 如果 **为 false**，则不向同步管理器注册订阅。 如果 **为 true**，则会向同步管理器注册订阅，并在不启动的情况下同步订阅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。  
  
`[ @offloadagent = ] remote_agent_activation` 指定可以远程激活代理。 *remote_agent_activation* 为 **bit** ，默认值为 **0**。  
  
> [!NOTE]  
>  不推荐使用此参数，保留它只是为了让脚本能够向后兼容。  
  
`[ @offloadserver = ] 'remote_agent_server_name'` 指定用于远程代理激活的服务器的网络名称。 *remote_agent_server_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @use_interactive_resolver = ] 'use_interactive_resolver'` 允许交互式解决允许交互式解决的所有项目的冲突。 *use_interactive_resolver* 为 **nvarchar (5)**，默认值为 FALSE。  
  
`[ @merge_job_name = ] 'merge_job_name'`*\@ Merge_job_name* 参数已弃用，无法设置。 *merge_job_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @hostname = ] 'hostname'` 当在参数化筛选器的 WHERE 子句中使用此函数时，将重写 [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) 返回的值。 *Hostname* 的值为 **sysname**，默认值为 NULL。  
  
> [!IMPORTANT]  
>  出于性能方面的考虑，我们建议您不要将这些函数应用于参数化行筛选器子句（如 `LEFT([MyColumn]) = SUSER_SNAME()`）中的列名。 如果在筛选子句中使用 [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) 并重写 HOST_NAME 值，则可能需要使用 [convert](../../t-sql/functions/cast-and-convert-transact-sql.md)转换数据类型。 有关此情况的最佳实践的详细信息，请参阅主题 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)中的“覆盖 HOST_NAME() 值”一节。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_addmergesubscription** 用于合并复制。  
  
 当由 **sysadmin** 固定服务器角色的成员执行 **sp_addmergesubscription** 以创建推送订阅时，合并代理作业将被隐式创建并在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户下运行。 建议你执行 [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md) ，并为 **\@ job_login** 和 **\@ Job_password** 指定其他特定于代理的 Windows 帐户的凭据。 有关详细信息，请参阅 [复制代理安全模式](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_addmergepushsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergesubscription-_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addmergesubscription**。  
  
## <a name="see-also"></a>另请参阅  
 [ssSDSFull](../../relational-databases/replication/create-a-push-subscription.md)   
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [交互式冲突解决方法](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_changemergesubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [sp_helpmergesubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md)  
  
  
