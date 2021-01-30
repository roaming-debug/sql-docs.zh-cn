---
description: sp_help_schedule (Transact-SQL)
title: sp_help_schedule (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_schedule
- sp_help_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_schedule
ms.assetid: b2fc4ce1-0a8e-44d2-b206-7dc7b258d8c9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 24a0d594e02fad844d651933bab8d89bc9d35bb2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193525"
---
# <a name="sp_help_schedule-transact-sql"></a>sp_help_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  列出有关计划的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_schedule   
     [ @schedule_id = ] id ,  
     [ @schedule_name = ] 'schedule_name'   
     [ , [ @attached_schedules_only = ] attached_schedules_only ]  
     [ , [ @include_description = ] include_description ]  
```  
  
## <a name="arguments"></a>参数  
`[ @schedule_id = ] id` 要列出的计划的标识符。 *schedule_name* 为 **int**，没有默认值。 可以指定 *schedule_id* 或 *schedule_name* 。  
  
`[ @schedule_name = ] 'schedule_name'` 要列出的计划的名称。 *schedule_name* **sysname**，无默认值。 可以指定 *schedule_id* 或 *schedule_name* 。  
  
`[ @attached_schedules_only = ] attached_schedules_only ]` 指定是否仅显示作业附加到的计划。 *attached_schedules_only* 为 **bit**，默认值为 **0**。 当 *attached_schedules_only* 为 **0** 时，将显示所有计划。 如果 *attached_schedules_only* 为 **1**，则结果集仅包含附加到作业的计划。  
  
`[ @include_description = ] include_description` 指定是否在结果集中包含说明。 *include_description* 为 **bit**，默认值为 **0**。 当 *include_description* 为 **0** 时，结果集的 *schedule_description* 列包含占位符。 当 *include_description* 为 **1** 时，将在结果集中包含计划的说明。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 此过程返回以下结果集：  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|计划标识号。|  
|**schedule_uid**|**uniqueidentifier**|计划的标识符。|  
|**schedule_name**|**sysname**|计划名称。|  
|**enabled**|**int**| (**1**) 还是未启用计划 (**0**) 。|  
|**freq_type**|**int**|指示何时执行作业的值。<br /><br /> **1** = 一次<br /><br /> **4** = 每天<br /><br /> **8** = 每周<br /><br /> **16** = 每月<br /><br /> **32** = 每月，相对于 **freq_interval**<br /><br /> **64** = 当 SQLServerAgent 服务启动时运行。|  
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
|**schedule_description**|**nvarchar(4000)**|对计划的英语说明（如果需要的话）。|  
|**job_count**|**int**|返回引用此计划的作业数。|  
  
## <a name="remarks"></a>备注  
 如果未提供任何参数， **sp_help_schedule** 会列出实例中所有计划的信息。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 **SQLAgentUserRole** 的成员只能查看其所拥有的计划。  
  
## <a name="examples"></a>示例  
  
### <a name="a-listing-information-for-all-schedules-in-the-instance"></a>A. 列出实例中所有计划的信息  
 以下示例列出了实例中所有计划的信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_schedule ;  
GO  
```  
  
### <a name="b-listing-information-for-a-specific-schedule"></a>B. 列出特定计划的信息  
 以下示例列出了名为 `NightlyJobs` 的计划的信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_schedule  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_add_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_detach_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-detach-schedule-transact-sql.md)  
  
  
