---
description: sp_help_jobschedule (Transact-SQL)
title: sp_help_jobschedule (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobschedule
- sp_help_jobschedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobschedule
ms.assetid: 2cded902-9272-4667-ac4b-a4f95a9f008e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9765feaa9e3237f1ccd8d3a6dcc203c88758aefa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200126"
---
# <a name="sp_help_jobschedule-transact-sql"></a>sp_help_jobschedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 用来执行自动活动的计划作业的信息。  
 
 
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_jobschedule { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }  
     [ , [ @schedule_name = ] 'schedule_name' ]  
     [ , [ @schedule_id = ] schedule_id ]  
     [ , [ @include_description = ] include_description ]  
```  
  
## <a name="arguments"></a>参数  
`[ @job_id = ] job_id` 作业标识号。 *job_id* 的值为 **uniqueidentifier**，默认值为 NULL。  
  
`[ @job_name = ] 'job_name'` 作业的名称。 *job_name* 的默认值为 **sysname**，默认值为 NULL。  
  
> [!NOTE]
> 必须指定 *job_id* 或 *job_name* ，但不能同时指定两者。

`[ @schedule_name = ] 'schedule_name'` 作业的计划项的名称。 *schedule_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @schedule_id = ] schedule_id` 作业的计划项的标识号。 *schedule_id* 的值为 **int**，默认值为 NULL。  
  
`[ @include_description = ] include_description` 指定是否在结果集中包含计划的说明。 *include_description* 为 **bit**，默认值为 **0**。 当 *include_description* 为 **0** 时，将不在结果集中包含计划的说明。 当 *include_description* 为 **1** 时，将在结果集中包含计划的说明。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|计划标识号。|  
|**schedule_name**|**sysname**|计划名称。|  
|**enabled**|**int**| (**1**) 还是未启用计划 (**0**) 。|  
|**freq_type**|**int**|指示何时执行作业的值。<br /><br /> **1** = 一次<br /><br /> **4** = 每天<br /><br /> **8** = 每周<br /><br /> **16** = 每月<br /><br /> **32** = 每月，相对于 **freq_interval**<br /><br /> **64** = 当 **SQLServerAgent** 服务启动时运行。|  
|**freq_interval**|**int**|执行作业的天数。 此值取决于 **freq_type** 的值。 有关详细信息，请参阅 [&#40;transact-sql&#41;sp_add_schedule ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)。|  
|**freq_subday_type**|**int**|**Freq_subday_interval** 的单位。 有关详细信息，请参阅 [&#40;transact-sql&#41;sp_add_schedule ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)。|  
|**freq_subday_interval**|**int**|每次执行作业之间要发生的 **freq_subday_type** 周期数。 有关详细信息，请参阅 [&#40;transact-sql&#41;sp_add_schedule ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)。|  
|**freq_relative_interval**|**int**|计划作业每月的 **freq_interval** 。 有关详细信息，请参阅 [&#40;transact-sql&#41;sp_add_schedule ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)。|  
|**freq_recurrence_factor**|**int**|作业的已计划执行日期之间的间隔月数。|  
|**active_start_date**|**int**|激活计划的日期。|  
|**active_end_date**|**int**|计划的结束日期。|  
|**active_start_time**|**int**|计划开始的时间。|  
|**active_end_time**|**int**|计划结束的时间。|  
|**date_created**|**datetime**|创建计划的日期。|  
|**schedule_description**|**nvarchar(4000)**|计划的英语说明，派生自 **msdb.dbo.sys计划** 中的值。 当 *include_description* 为 **0** 时，此列包含文本，指出未请求说明。|  
|**next_run_date**|**int**|计划下一次引发作业运行的日期。|  
|**next_run_time**|**int**|计划下一次引发作业运行的时间。|  
|**schedule_uid**|**uniqueidentifier**|计划的标识符。|  
|**job_count**|**int**|返回的作业数。|  
  
> **注意： sp_help_jobschedule** 从 **dbo.sysjobschedules** 返回值，并在 **msdb** 中 **dbo.sys计划** 系统表。 **sysjobschedules** 每20分钟更新一次。 这可能会影响此存储过程返回的值。  
  
## <a name="remarks"></a>备注  
 **Sp_help_jobschedule** 的参数只能在某些组合中使用。 如果指定 *schedule_id* ，则 *job_id* 和 *job_name* 都无法指定。 否则， *job_id* 或 *job_name* 参数可与 *schedule_name* 一起使用。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 **SQLAgentUserRole** 的成员只能查看其所拥有的作业计划的属性。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-the-job-schedule-for-a-specific-job"></a>A. 返回特定作业的作业计划  
 以下示例返回名为 `BackupDatabase` 的作业的计划信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobschedule  
    @job_name = N'BackupDatabase' ;  
GO  
```  
  
### <a name="b-returning-the-job-schedule-for-a-specific-schedule"></a>B. 返回特定计划的作业计划  
 下面的示例返回名为 `NightlyJobs` 的计划和名为 `RunReports` 的作业的信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobschedule   
    @job_name = N'RunReports',  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
### <a name="c-returning-the-job-schedule-and-schedule-description-for-a-specific-schedule"></a>C. 返回特定计划的作业计划和计划说明  
 下面的示例返回名为 `NightlyJobs` 的计划和名为 `RunReports` 的作业的信息。 返回的结果集包括对计划的说明。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobschedule  
    @job_name = N'RunReports',  
    @schedule_name = N'NightlyJobs',  
    @include_description = 1 ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_add_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_update_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
