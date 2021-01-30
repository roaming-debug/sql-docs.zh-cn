---
description: sp_update_schedule (Transact-SQL)
title: sp_update_schedule (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_schedule
- sp_update_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_schedule
ms.assetid: 97b3119b-e43e-447a-bbfb-0b5499e2fefe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 13359a3d3588e238ef0b78ed5bfe5148fb447dae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189537"
---
# <a name="sp_update_schedule-transact-sql"></a>sp_update_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理计划设置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_update_schedule   
    {   [ @schedule_id = ] schedule_id   
      | [ @name = ] 'schedule_name' }  
    [ , [ @new_name = ] new_name ]  
    [ , [ @enabled = ] enabled ]  
    [ , [ @freq_type = ] freq_type ]  
    [ , [ @freq_interval = ] freq_interval ]   
    [ , [ @freq_subday_type = ] freq_subday_type ]   
    [ , [ @freq_subday_interval = ] freq_subday_interval ]   
    [ , [ @freq_relative_interval = ] freq_relative_interval ]   
    [ , [ @freq_recurrence_factor = ] freq_recurrence_factor ]   
    [ , [ @active_start_date = ] active_start_date ]   
    [ , [ @active_end_date = ] active_end_date ]   
    [ , [ @active_start_time = ] active_start_time ]   
    [ , [ @active_end_time = ] active_end_time ]   
    [ , [ @owner_login_name = ] 'owner_login_name' ]  
    [ , [ @automatic_post =] automatic_post ]  
```  
  
## <a name="arguments"></a>参数  
`[ @schedule_id = ] schedule_id` 要修改的计划的标识符。 *schedule_id* 为 **int**，没有默认值。 必须指定 *schedule_id* 或 *schedule_name* 。  
  
`[ @name = ] 'schedule_name'` 要修改的计划的名称。 *schedule_name* **sysname**，无默认值。 必须指定 *schedule_id* 或 *schedule_name* 。  
  
`[ @new_name = ] new_name` 计划的新名称。 *new_name* 的默认值为 **sysname**，默认值为 NULL。 当 *new_name* 为 NULL 时，计划的名称将保持不变。  
  
`[ @enabled = ] enabled` 指示计划的当前状态。 *enabled* 为 **tinyint**，默认值为 **1** (启用) 。 如果为 **0**，则不启用计划。 如果不启用计划，则作业不会按此计划运行。  
  
`[ @freq_type = ] freq_type` 指示何时执行作业的值。 *freq_type* 的数据值为 **int**，默认值为 **0**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次|  
|**4**|每天|  
|**8**|每周|  
|**16**|每月一次|  
|**32**|每月，相对于 *频率间隔*|  
|**64**|SQLServerAgent 服务启动时运行|  
|**128**|计算机空闲时运行|  
  
`[ @freq_interval = ] freq_interval` 作业的执行日期。 *freq_interval* 的值为 **int**，默认值为 **0**，它取决于 *freq_type* 的值。  
  
|*Freq_type* 的值|对 *freq_interval* 的影响|  
|---------------------------|--------------------------------|  
|**1** (一次) |*freq_interval* 未使用。|  
|**4** (每日) |每 *freq_interval* 天。|  
|**8** (每周) |*freq_interval* 是 (与 **或** 逻辑运算符组合在一起的一个或多个) ：<br /><br /> **1** = 星期日<br /><br /> **2** = 星期一<br /><br /> **4** = 星期二<br /><br /> **8** = 星期三<br /><br /> **16** = 星期四<br /><br /> **32** = 星期五<br /><br /> **64** = 星期六|  
|每月 **16** () |*Freq_interval* 月中的第几天。|  
|**32** (月相对) |*freq_interval* 是以下项之一：<br /><br /> **1** = 星期日<br /><br /> **2** = 星期一<br /><br /> **3** = 星期二<br /><br /> **4** = 星期三<br /><br /> **5** = 星期四<br /><br /> **6** = 星期五<br /><br /> **7** = 星期六<br /><br /> **8** = 日<br /><br /> **9** = 工作日<br /><br /> **10** = 周末|  
|**64** 启动 SQLServerAgent 服务时 () |*freq_interval* 未使用。|  
|**128**|*freq_interval* 未使用。|  
  
`[ @freq_subday_type = ] freq_subday_type` 指定 freq_subday_interval 的单位 ** *。* *freq_subday_type* 的数据值为 **int**，默认值为 **0**，可以是下列值之一。  
  
|值|说明（单位）|  
|-----------|--------------------------|  
|**0x1**|在指定的时间|  
|**0x2**|秒|  
|**0x4**|分钟数|  
|**0x8**|小时|  
  
`[ @freq_subday_interval = ] freq_subday_interval` 每次执行作业之间要发生的 *freq_subday_type* 周期数。 *freq_subday_interval* 的值为 **int**，默认值为 **0**。  
  
`[ @freq_relative_interval = ] freq_relative_interval`如果 *freq_interval* 为 **32** (月相对) ，则每个月 *freq_interval* 的作业出现。 *freq_relative_interval* 的数据值为 **int**，默认值为 **0**，可以是下列值之一。  
  
|值|说明（单位）|  
|-----------|--------------------------|  
|**1**|First|  
|**2**|秒|  
|**4**|第三个|  
|**8**|第四个|  
|**16**|上一个|  
  
`[ @freq_recurrence_factor = ] freq_recurrence_factor` 作业的计划执行之间的周数或月数。 仅当 *freq_type* 为 **8**、 **16** 或 **32** 时才使用 *freq_recurrence_factor* 。 *freq_recurrence_factor* 的值为 **int**，默认值为 **0**。  
  
`[ @active_start_date = ] active_start_date` 可以开始执行作业的日期。 *active_start_date* 的数据值为 **int**，默认值为 NULL，表示当天的日期。 日期的格式为 YYYYMMDD。 如果 *active_start_date* 不为 NULL，则日期必须大于或等于19900101。  
  
 创建了计划后，请检查其开始日期，确认该日期是否正确。 有关详细信息，请参阅 [创建计划并将计划附加到作业](../../ssms/agent/create-and-attach-schedules-to-jobs.md)中的 "计划开始日期" 部分。  
  
`[ @active_end_date = ] active_end_date` 作业的执行停止日期。 *active_end_date* 的值为 **int**，默认值为 **99991231**，表示9999年12月31日。 其格式为 YYYYMMDD。  
  
`[ @active_start_time = ] active_start_time`*Active_start_date* 和 *active_end_date* 之间的任何一天的时间开始执行作业。 *active_start_time* 的值为 **int**，默认值为000000，表示凌晨12:00:00。 并且必须使用 HHMMSS 格式输入。  
  
`[ @active_end_time = ] active_end_time`*Active_start_date* 和 *active_end_date* 之间的任何一天的时间结束执行作业。 *active_end_time* 的值为 **int**，默认值为 **235959**，表示 11:59:59 P.M.。 并且必须使用 HHMMSS 格式输入。  
  
`[ @owner_login_name = ] 'owner_login_name']` 拥有该计划的服务器主体的名称。 *owner_login_name* 的默认值为 **sysname**，默认值为 NULL，指示计划由创建者拥有。  
  
`[ @automatic_post = ] automatic_post` 保护.  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 所有使用该计划的作业将立即使用新设置。 但是，更改计划不会停止当前正在运行的作业。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 只有 **sysadmin** 的成员才能修改其他用户所拥有的计划。  
  
## <a name="examples"></a>示例  
 下面的示例将 `NightlyJobs` 计划的启用状态更改为 `0`，并将所有者设置为 `terrid`。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_schedule  
    @name = 'NightlyJobs',  
    @enabled = 0,  
    @owner_login_name = 'terrid' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [创建计划并将计划附加到作业](../../ssms/agent/create-and-attach-schedules-to-jobs.md)   
 [计划作业](../../ssms/agent/schedule-a-job.md)   
 [创建计划](../../ssms/agent/create-a-schedule.md)   
 [SQL Server 代理存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_add_jobschedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md)   
 [sp_delete_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_help_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-schedule-transact-sql.md)   
 [sp_attach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
  
