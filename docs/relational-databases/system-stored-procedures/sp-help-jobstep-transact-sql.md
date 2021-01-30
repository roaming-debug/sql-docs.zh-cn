---
description: sp_help_jobstep (Transact-SQL)
title: sp_help_jobstep (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobstep_TSQL
- sp_help_jobstep
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobstep
ms.assetid: 4a13b804-45f2-4f82-987f-42d9a57dd6db
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c6a4a59a69612c37a6b6d9208e121b11ea448a35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200111"
---
# <a name="sp_help_jobstep-transact-sql"></a>sp_help_jobstep (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务在执行自动活动时使用的作业中的步骤信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_jobstep { [ @job_id = ] 'job_id' | [ @job_name = ] 'job_name' }  
     [ , [ @step_id = ] step_id ]   
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @suffix = ] suffix ]   
```  
  
## <a name="arguments"></a>参数  
`[ @job_id = ] 'job_id'` 要为其返回作业信息的作业标识号。 *job_id* 的值为 **uniqueidentifier**，默认值为 NULL。  
  
`[ @job_name = ] 'job_name'` 作业的名称。 *job_name* 的值为 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  必须指定 *job_id* 或 *job_name* ，但不能同时指定两者。  
  
`[ @step_id = ] step_id` 作业中步骤的标识号。 如果尚未包括，则包括作业中的所有步骤。 *step_id* 的值为 **int**，默认值为 NULL。  
  
`[ @step_name = ] 'step_name'` 作业中步骤的名称。 *step_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @suffix = ] suffix` 指示是否在输出的 **flags** 列中追加文本说明的标志。 *后缀* 为 **bit**，默认值为 **0**。 如果 *后缀* 为 **1**，则追加说明。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**step_id**|**int**|步骤的唯一标识符。|  
|**step_name**|**sysname**|作业中步骤的名称。|  
|**适用**|**nvarchar(40)**|执行步骤命令的子系统。|  
|**command**|**nvarchar(max)**|在步骤中执行的命令。|  
|**flag**|**int**|控制步骤行为的值的位掩码。|  
|**cmdexec_success_code**|**int**|对于 **CmdExec** 步骤，这是成功命令的进程退出代码。|  
|**on_success_action**|**tinyint**|如果步骤成功，则采取下列某个后续操作：<br /><br /> **1** = 退出报告成功的作业。<br /><br /> **2** = 退出报告失败的作业。<br /><br /> **3** = 中转到下一步。<br /><br /> **4** = 跳到步骤。|  
|**on_success_step_id**|**int**|如果 **on_success_action** 为4，则指示要执行的下一步。|  
|**on_fail_action**|**tinyint**|如果步骤失败了，应采取什么后续操作。 值与 **on_success_action** 相同。|  
|**on_fail_step_id**|**int**|如果 **on_fail_action** 为4，则指示要执行的下一步。|  
|服务器|**sysname**|保留。|  
|**database_name**|**sysname**|对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 步骤，这是命令执行时所在的数据库。|  
|**database_user_name**|**sysname**|对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 步骤，这是命令执行时所在的数据库用户上下文。|  
|**retry_attempts**|**int**|应该对命令进行重试的最大次数（如果命令没有成功）。|  
|**retry_interval**|**int**|重试尝试的间隔（以分钟为单位）。|  
|**os_run_priority**|**int**|保留。|  
|**output_file_name**|**nvarchar(200)**|命令输出应写入到的文件 ([!INCLUDE[tsql](../../includes/tsql-md.md)] 、 **CmdExec** 和 **PowerShell** 步骤仅) 。|  
|**last_run_outcome**|**int**|步骤上一次运行的结果：<br /><br /> **0** = 失败<br /><br /> **1** = 成功<br /><br /> **2** = 重试<br /><br /> **3** = 已取消<br /><br /> **5** = 未知|  
|**last_run_duration**|**int**|该步骤上次运行时的持续时间 (hhmmss)。|  
|**last_run_retries**|**int**|步骤上一次运行时，重试命令的次数。|  
|**last_run_date**|**int**|步骤上一次开始执行的日期。|  
|**last_run_time**|**int**|步骤上一次开始执行的时间。|  
|**proxy_id**|**int**|作业步骤的代理。|  
  
## <a name="remarks"></a>备注  
 **sp_help_jobstep** 在 **msdb** 数据库中。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 **SQLAgentUserRole** 的成员只能查看其所拥有作业的作业步骤。  
  
## <a name="examples"></a>示例  
  
### <a name="a-return-information-for-all-steps-in-a-specific-job"></a>A. 返回特定作业中所有步骤的信息  
 以下示例返回名为 `Weekly Sales Data Backup` 的作业的所有作业步骤。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobstep  
    @job_name = N'Weekly Sales Data Backup' ;  
GO  
```  
  
### <a name="b-return-information-about-a-specific-job-step"></a>B. 返回特定作业步骤的信息  
 以下示例返回名为 `Weekly Sales Data Backup` 的作业的第一个作业步骤的信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1 ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_add_jobstep &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_delete_jobstep &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_update_jobstep &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
