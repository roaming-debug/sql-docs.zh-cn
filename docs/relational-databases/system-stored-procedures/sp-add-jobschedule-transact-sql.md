---
description: sp_add_jobschedule (Transact-SQL)
title: sp_add_jobschedule (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 07/28/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_jobschedule
- sp_add_jobschedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_jobschedule
ms.assetid: ffce19d9-d1d6-45b4-89fd-ad0f60822ba0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6c44f473d71f67718e9444ab2994fcfc17258411
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206468"
---
# <a name="sp_add_jobschedule-transact-sql"></a>sp_add_jobschedule (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  创建 SQL 代理作业的计划。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
  > [!IMPORTANT]  
  > [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

## <a name="syntax"></a>语法  
  
```  
  
sp_add_jobschedule [ @job_id = ] job_id, | [ @job_name = ] 'job_name', [ @name = ] 'name'  
     [ , [ @enabled = ] enabled_flag ]  
     [ , [ @freq_type = ] frequency_type ]  
     [ , [ @freq_interval = ] frequency_interval ]  
     [ , [ @freq_subday_type = ] frequency_subday_type ]  
     [ , [ @freq_subday_interval = ] frequency_subday_interval ]  
     [ , [ @freq_relative_interval = ] frequency_relative_interval ]  
     [ , [ @freq_recurrence_factor = ] frequency_recurrence_factor ]  
     [ , [ @active_start_date = ] active_start_date ]  
     [ , [ @active_end_date = ] active_end_date ]  
     [ , [ @active_start_time = ] active_start_time ]  
     [ , [ @active_end_time = ] active_end_time ]  
     [ , [ @schedule_id = ] schedule_id OUTPUT ]
     [ , [ @schedule_uid = ] _schedule_uid OUTPUT ]
```  
  
## <a name="arguments"></a>参数  
`[ @job_id = ] job_id` 向其中添加计划的作业的作业标识号。 *job_id* 是 **uniqueidentifier**，无默认值。  
  
`[ @job_name = ] 'job_name'` 向其中添加计划的作业的名称。 *job_name* 为 **nvarchar (128)**，无默认值。  
  
> [!NOTE]  
>  必须指定 *job_id* 或 *job_name* ，但不能同时指定两者。  
  
`[ @name = ] 'name'` 计划的名称。 *name* 为 **nvarchar (128)**，无默认值。  
  
`[ @enabled = ] enabled_flag` 指示计划的当前状态。 *enabled_flag* 为 **tinyint**，默认值为 **1** (启用) 。 如果为 **0**，则不启用计划。 禁用该计划时，将不运行作业。  
  
`[ @freq_type = ] frequency_type` 指示何时执行作业的值。 *frequency_type* 的数据值为 **int**，默认值为 **0**，可以是下列值之一：  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次|  
|**4**|每天|  
|**8**|每周|  
|**16**|每月一次|  
|**32**|每月，相对于 *frequency_interval。*|  
|**64**|在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务启动时运行。|  
|**128**|在计算机空闲时运行。|  
  
`[ @freq_interval = ] frequency_interval` 执行作业的日期。 *frequency_interval* 的值为 **int**，默认值为0，并且取决于 *frequency_type* 的值，如下表中所示：  
  
|值|效果|  
|-----------|------------|  
|**1** (一次) |*frequency_interval* 未使用。|  
|**4** (每日) |每 *frequency_interval* 天。|  
|**8** (每周) |*frequency_interval* 是 (与或逻辑运算符组合在一起的一个或多个) ：<br /><br /> 1 = 星期日<br /><br /> 2 = 星期一<br /><br /> 4 = 星期二<br /><br /> 8 = 星期三<br /><br /> 16 = 星期四<br /><br /> 32 = 星期五<br /><br /> 64 = 星期六|  
|每月 **16** () |*Frequency_interval* 月中的第几天。|  
|**32** (月相对) |*frequency_interval* 是以下项之一：<br /><br /> 1 = 星期日<br /><br /> 2 = 星期一<br /><br /> 3 = 星期二<br /><br /> 4 = 星期三<br /><br /> 5 = 星期四<br /><br /> 6 = 星期五<br /><br /> 7 = 星期六<br /><br /> 8 = 天<br /><br /> 9 = 工作日<br /><br /> 10 = 休息日|  
|**64** ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务启动时) |*frequency_interval* 未使用。|  
|**128**|*frequency_interval* 未使用。|  
  
`[ @freq_subday_type = ] frequency_subday_type` 指定 *frequency_subday_interval* 的单位。 *frequency_subday_type* 是 **int**，没有默认值，可以是下列值之一：  
  
|值|说明（单位）|  
|-----------|--------------------------|  
|**0x1**|在指定的时间|  
|**0x2**|秒|  
|**0x4**|分钟数|  
|**0x8**|小时|  
  
`[ @freq_subday_interval = ] frequency_subday_interval` 每次执行作业之间要发生的 *frequency_subday_type* 周期数。 *frequency_subday_interval* 的值为 **int**，默认值为0。  
  
`[ @freq_relative_interval = ] frequency_relative_interval`当 *frequency_type* 设置为 **32** (每月相对) 时，进一步定义 *frequency_interval* 。  
  
 *frequency_relative_interval* 是 **int**，没有默认值，可以是下列值之一：  
  
|值|说明（单位）|  
|-----------|--------------------------|  
|**1**|First|  
|**2**|秒|  
|**4**|第三个|  
|**8**|第四个|  
|**16**|上一个|  
  
 *frequency_relative_interval* 指示间隔的发生次数。 例如，如果 *frequency_relative_interval* 设置为 **2**， *frequency_type* 设置为 **32**， *frequency_interval* 设置为 **3**，则计划作业将在每月的第二个星期二发生。  
  
`[ @freq_recurrence_factor = ] frequency_recurrence_factor` 作业的计划执行之间的周数或月数。 仅当 *frequency_type* 设置为 **8**、 **16** 或 **32** 时才使用 *frequency_recurrence_factor* 。 *frequency_recurrence_factor* 的值为 **int**，默认值为0。  
  
`[ @active_start_date = ] active_start_date` 可以开始执行作业的日期。 *active_start_date* 为 **int**，没有默认值。 日期的格式为 YYYYMMDD。 如果设置 *active_start_date* ，则日期必须大于或等于19900101。  
  
 创建了计划后，请检查其开始日期，确认该日期是否正确。 有关详细信息，请参阅 [创建计划并将计划附加到作业](../../ssms/agent/create-and-attach-schedules-to-jobs.md)中的 "计划开始日期" 部分。  
  
`[ @active_end_date = ] active_end_date` 可以停止作业执行的日期。 *active_end_date* 为 **int**，没有默认值。 日期的格式为 YYYYMMDD。  
  
`[ @active_start_time = ] active_start_time`*Active_start_date* 和 *active_end_date* 之间的任意一天的时间，用于开始执行作业。 *active_start_time* 为 **int**，没有默认值。 此时间的格式为24小时制的 HHMMSS。  
  
`[ @active_end_time = active_end_time_`*Active_start_date* 和 *active_end_date* 之间的任意一天的时间结束作业执行。 *active_end_time* 为 **int**，没有默认值。 此时间的格式为24小时制的 HHMMSS。  
  
`[ @schedule_id = schedule_idOUTPUT` 已成功创建计划时分配给计划的计划标识号。 *schedule_id* 是 **int** 类型的输出变量，无默认值。  
  
`[ @schedule_uid = ] _schedule_uidOUTPUT` 计划的唯一标识符。 *schedule_uid* 是 **uniqueidentifier** 类型的变量。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 作业计划现在可以独立于作业进行管理。 若要将计划添加到作业，请使用 **sp_add_schedule** 创建计划，并 **sp_attach_schedule** 将计划附加到作业。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
 
 ## <a name="example"></a>示例
 下面的示例将作业计划分配给 `SaturdayReports` 每个星期六的凌晨2:00 执行。
```sql  
EXEC msdb.dbo.sp_add_jobschedule 
        @job_name = N'SaturdayReports', -- Job name
        @name = N'Weekly_Sat_2AM',  -- Schedule name
        @freq_type = 8, -- Weekly
        @freq_interval = 64, -- Saturday
        @freq_recurrence_factor = 1, -- every week
        @active_start_time = 20000 -- 2:00 AM
```
  
## <a name="see-also"></a>另请参阅  
 [创建计划并将计划附加到作业](../../ssms/agent/create-and-attach-schedules-to-jobs.md)   
 [计划作业](../../ssms/agent/schedule-a-job.md)   
 [创建计划](../../ssms/agent/create-a-schedule.md)   
 [SQL Server 代理存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_update_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_help_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-schedule-transact-sql.md)   
 [sp_attach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
