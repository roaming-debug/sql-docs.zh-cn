---
description: sp_changesubstatus (Transact-SQL)
title: sp_changesubstatus (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changesubstatus
- sp_changesubstatus_TSQL
helpviewer_keywords:
- sp_changesubstatus
ms.assetid: 9370e47a-d128-4f15-9224-1c3642770c39
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 89de8f8a4c5f1abb3a07551bcca6eac53e0b2e2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158963"
---
# <a name="sp_changesubstatus-transact-sql"></a>sp_changesubstatus (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  更改现有订阅服务器的状态。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_changesubstatus [ [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
        , [ @status = ] 'status'  
    [ , [ @previous_status = ] 'previous_status' ]  
    [ , [ @destination_db = ] 'destination_db' ]  
    [ , [ @frequency_type = ] frequency_type ]  
    [ , [ @frequency_interval = ] frequency_interval ]  
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday = ] frequency_subday ]  
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]  
    [ , [ @active_start_date = ] active_start_date ]  
    [ , [ @active_end_date = ] active_end_date ]  
    [ , [ @optional_command_line = ] 'optional_command_line' ]  
    [ , [ @distribution_jobid = ] distribution_jobid ]  
    [ , [ @from_auto_sync = ] from_auto_sync ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @offloadagent= ] remote_agent_activation ]  
    [ , [ @offloadserver= ] 'remote_agent_server_name' ]  
    [ , [ @dts_package_name= ] 'dts_package_name' ]  
    [ , [ @dts_package_password= ] 'dts_package_password' ]  
    [ , [ @dts_package_location= ] dts_package_location ]  
    [ , [ @skipobjectactivation = ] skipobjectactivation  
  [ , [ @distribution_job_name= ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，默认值为 **%** 。 如果未指定 *发布* ，将影响所有发布。  
  
`[ @article = ] 'article'` 项目的名称。 该名称对发布必须是唯一的。 *项目* 的默认值为 **sysname**，默认值为 **%** 。 如果未指定 *项目* ，将影响所有项目。  
  
`[ @subscriber = ] 'subscriber'` 要更改其状态的订阅服务器的名称。 *订阅服务器* 的默认值为 **sysname**，默认值为 **%** 。 如果未指定 *订阅服务器* ，则会更改指定项目的所有订阅服务器的状态。  
  
`[ @status = ] 'status'`**Syssubscriptions** 表中的订阅状态。 *状态* 为 **sysname**，无默认值，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**active**|订阅服务器已被同步并且正在接收数据。|  
|**inactive**|存在订阅服务器项，但没有订阅。|  
|**subscribed**|订阅服务器正在请求数据，但尚未同步。|  
  
`[ @previous_status = ] 'previous_status'` 订阅的前一状态。 *previous_status* 的默认值为 **sysname**，默认值为 NULL。 此参数允许你更改当前具有该状态的所有订阅，从而允许组函数在一组特定订阅上 (例如，将所有活动订阅设置回 **订阅** 的) 。  
  
`[ @destination_db = ] 'destination_db'` 目标数据库的名称。 *destination_db* 的默认值为 **sysname**，默认值为 **%** 。  
  
`[ @frequency_type = ] frequency_type` 用于计划分发任务的频率。 *frequency_type* 的值为 **int**，默认值为 NULL。  
  
`[ @frequency_interval = ] frequency_interval` 要应用于 *frequency_type* 设置的频率的值。 *frequency_interval* 的值为 **int**，默认值为 NULL。  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` 分发任务的日期。 如果 *frequency_type* 设置为 32 (每月相对) ，则使用此参数。 *frequency_relative_interval* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|First|  
|**2**|秒|  
|**4**|第三个|  
|**8**|第四个|  
|**16**|上一个|  
|NULL（默认值）||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor`*Frequency_type* 使用的重复因子。 *frequency_recurrence_factor* 的值为 **int**，默认值为 NULL。  
  
`[ @frequency_subday = ] frequency_subday` 在定义的时间段内重新计划的频率（分钟）。 *frequency_subday* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次|  
|**2**|秒|  
|**4**|Minute|  
|**8**|小时|  
|NULL（默认值）||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval`*Frequency_subday* 的间隔。 *frequency_subday_interval* 的值为 **int**，默认值为 NULL。  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` 第一次安排分发任务的时间，格式为 HHMMSS。 *active_start_time_of_day* 的值为 **int**，默认值为 NULL。  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` 停止安排分发任务的时间，格式为 HHMMSS。 *active_end_time_of_day* 的值为 **int**，默认值为 NULL。  
  
`[ @active_start_date = ] active_start_date` 第一次安排分发任务的日期，格式为 YYYYMMDD。 *active_start_date* 的值为 **int**，默认值为 NULL。  
  
`[ @active_end_date = ] active_end_date` 停止安排分发任务的日期，格式为 YYYYMMDD。 *active_end_date* 的值为 **int**，默认值为 NULL。  
  
`[ @optional_command_line = ] 'optional_command_line'` 可选的命令提示符。 *optional_command_line* 为 **nvarchar (4000)**，默认值为 NULL。  
  
`[ @distribution_jobid = ] distribution_jobid` 将订阅状态从非活动更改为活动时，订阅服务器上的分发代理的作业 ID。 在其他情况下，不定义该参数。 如果对此存储过程的单个调用中涉及多个分发代理，则不定义结果。 *distribution_jobid* 为 **二进制 (16)**，默认值为 NULL。  
  
`[ @from_auto_sync = ] from_auto_sync` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @ignore_distributor = ] ignore_distributor` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @offloadagent = ] remote_agent_activation`
 > [!NOTE]  
>  已不推荐使用远程代理激活，也不再支持该功能。 支持此参数只是为了让脚本能够向后兼容。 如果将 *remote_agent_activation* 设置为 **0** 以外的值，将生成错误。  
  
`[ @offloadserver = ] 'remote_agent_server_name'`
 > [!NOTE]  
>  已不推荐使用远程代理激活，也不再支持该功能。 支持此参数只是为了让脚本能够向后兼容。 将 *remote_agent_server_name* 设置为任何非 NULL 值将生成错误。  
  
`[ @dts_package_name = ] 'dts_package_name'` 指定数据转换服务 (DTS) 包的名称。 *dts_package_name* 是 **sysname**，默认值为 NULL。 例如，对于名为 **DTSPub_Package** 的包，你将指定 `@dts_package_name = N'DTSPub_Package'` 。  
  
`[ @dts_package_password = ] 'dts_package_password'` 指定包的密码。 *dts_package_password* 的默认 **值为 NULL** ，默认值为 NULL，指定密码属性保持不变。  
  
> [!NOTE]  
>  DTS 包必须具有密码。  
  
`[ @dts_package_location = ] dts_package_location` 指定包位置。 *dts_package_location* 为 **int**，默认值为 **0**。 如果为 **0**，则包位置在分发服务器上。 如果为 **1**，则包位置在订阅服务器上。 包的位置可以是 **分发服务器** 或 **订阅服务器**。  
  
`[ @skipobjectactivation = ] skipobjectactivation` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @distribution_job_name = ] 'distribution_job_name'` 分发作业的名称。 *distribution_job_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  更改发布服务器上的项目属性时，不应使用 *publisher* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_changesubstatus** 用于快照复制和事务复制。  
  
 **sp_changesubstatus** 在 **syssubscriptions** 表中将订阅服务器的状态更改为 "已更改" 状态。 如果需要，它会更新 **sysarticles** 表中的项目状态，以指示活动或非活动。 如果需要，它会在 **sysobjects** 表中为复制的表设置复制标志。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员、 **db_owner** 固定数据库角色的成员或订阅的创建者才能执行 **sp_changesubstatus**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_addsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpdistributor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
