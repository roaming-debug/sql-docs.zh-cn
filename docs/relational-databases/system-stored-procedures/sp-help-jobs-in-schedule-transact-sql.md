---
description: sp_help_jobs_in_schedule (Transact-SQL)
title: sp_help_jobs_in_schedule (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobs_in_schedule_TSQL
- sp_help_jobs_in_schedule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobs_in_schedule
ms.assetid: 1168aa2c-136b-4ba3-b18e-9070d95a26fa
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1890fc4b8aa3757b55dfc5ad7e33070126300897
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200148"
---
# <a name="sp_help_jobs_in_schedule-transact-sql"></a>sp_help_jobs_in_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关附加了特定计划的作业的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_jobs_in_schedule   
     [ @schedule_name = ] 'schedule_name' ,  
     [ @schedule_id = ] schedule_id   
```  
  
## <a name="arguments"></a>参数  
`[ @schedule_id = ] schedule_id` 要列出其信息的计划的标识符。 *schedule_id* 为 **int**，没有默认值。 可以指定 *schedule_id* 或 *schedule_name* 。  
  
`[ @schedule_name = ] 'schedule_name'` 要列出其信息的计划的名称。 *schedule_name* **sysname**，无默认值。 可以指定 *schedule_id* 或 *schedule_name* 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 返回以下结果集：  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|作业的唯一 ID。|  
|**originating_server**|**nvarchar(30)**|作业来自的服务器的名称。|  
|name|**sysname**|作业的名称。|  
|**enabled**|**tinyint**|指示是否启用要执行的作业。|  
|description|**nvarchar(512)**|对作业的说明。|  
|**start_step_id**|**int**|执行作业的起始步骤的 ID。|  
|**category**|**sysname**|作业类别。|  
|**owner**|**sysname**|作业所有者。|  
|**notify_level_eventlog**|**int**|位掩码，它表示在何种情况下通知事件应记录到 Microsoft Windows 应用程序日志中。 可以是下列值之一：<br /><br /> **0** = 从不<br /><br /> **1** = 作业成功时<br /><br /> **2** = 作业失败时<br /><br /> **3** = 无论作业完成与否 (，无论作业结果如何) |  
|**notify_level_email**|**int**|位掩码，它指示当作业完成时，在什么情况下应该发送一个通知电子邮件。 可能的值与 **notify_level_eventlog** 的值相同。|  
|**notify_level_netsend**|**int**|位掩码，它表示当作业完成时，在什么情况下应该发送一个网络消息。 可能的值与 **notify_level_eventlog** 的值相同。|  
|**notify_level_page**|**int**|位掩码，它表示当作业完成时，在什么情况下应该发送一个呼叫。 可能的值与 **notify_level_eventlog** 的值相同。|  
|**notify_email_operator**|**sysname**|被通知的操作员的电子邮件名称。|  
|**notify_netsend_operator**|**sysname**|在发送网络消息时所使用的计算机或用户的名称。|  
|**notify_page_operator**|**sysname**|在发送寻呼时所使用的计算机或用户的名称。|  
|**delete_level**|**int**|位掩码，它表示当作业完成时，在什么情况下应该删除作业。 可能的值与 **notify_level_eventlog** 的值相同。|  
|**date_created**|**datetime**|作业的创建日期。|  
|**date_modified**|**datetime**|上次修改作业的日期。|  
|**version_number**|**int**|作业的版本（每次修改作业时都自动对其进行更新）。|  
|**last_run_date**|**int**|作业上一次开始执行的日期。|  
|**last_run_time**|**int**|作业上一次开始执行的时间。|  
|**last_run_outcome**|**int**|作业上一次运行时所得到的结果：<br /><br /> **0** = 失败<br /><br /> **1** = 成功<br /><br /> **3** = 已取消<br /><br /> **5** = 未知|  
|**next_run_date**|**int**|计划作业下一次运行的日期。|  
|**next_run_time**|**int**|计划作业下一次运行的时间。|  
|**next_run_schedule_id**|**int**|下一个运行的计划的标识号。|  
|**current_execution_status**|**int**|当前执行状态。|  
|**current_execution_step**|**sysname**|作业中当前的执行步骤。|  
|**current_retry_attempt**|**int**|如果作业正在运行，并且已经重试过该步骤，那么这就是当前的重试尝试。|  
|**has_step**|**int**|作业具有的作业步骤数。|  
|**has_schedule**|**int**|作业具有的作业计划数。|  
|**has_target**|**int**|作业具有的目标服务器数。|  
|type |**int**|作业类型：<br /><br /> **1** = 本地作业。<br /><br /> **2** = 多服务器作业。<br /><br /> **0** = 作业没有目标服务器。|  
  
## <a name="remarks"></a>备注  
 此过程列出有关附加到指定计划的作业的信息。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 **SQLAgentUserRole** 的成员只能查看其所拥有的作业的状态。  
  
## <a name="examples"></a>示例  
 以下示例列出附加到 `NightlyJobs` 计划的作业。  
  
```  
USE msdb ;  
GO  
  
EXEC sp_help_jobs_in_schedule  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 代理存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_detach_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-detach-schedule-transact-sql.md)  
  
  
