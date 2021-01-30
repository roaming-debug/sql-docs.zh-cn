---
description: sp_add_job (Transact-SQL)
title: sp_add_job (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_job_TSQL
- sp_add_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_job
ms.assetid: 6ca8fe2c-7b1c-4b59-b4c7-e3b7485df274
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39f083d083a38e4a581c507354efaf6548471e7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206799"
---
# <a name="sp_add_job-transact-sql"></a>sp_add_job (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  添加 SQL 代理服务执行的新作业。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 
 > [!IMPORTANT]  
 > [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。
 
## <a name="syntax"></a>语法  
  
```  
  
sp_add_job [ @job_name = ] 'job_name'  
     [ , [ @enabled = ] enabled ]   
     [ , [ @description = ] 'description' ]   
     [ , [ @start_step_id = ] step_id ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @category_id = ] category_id ]   
     [ , [ @owner_login_name = ] 'login' ]   
     [ , [ @notify_level_eventlog = ] eventlog_level ]   
     [ , [ @notify_level_email = ] email_level ]   
     [ , [ @notify_level_netsend = ] netsend_level ]   
     [ , [ @notify_level_page = ] page_level ]   
     [ , [ @notify_email_operator_name = ] 'email_name' ]   
          [ , [ @notify_netsend_operator_name = ] 'netsend_name' ]   
     [ , [ @notify_page_operator_name = ] 'page_name' ]   
     [ , [ @delete_level = ] delete_level ]   
     [ , [ @job_id = ] job_id OUTPUT ]   
```  
  
## <a name="arguments"></a>参数  
`[ @job_name = ] 'job_name'` 作业的名称。 该名称必须唯一，并且不能包含 (**%**) 字符的百分比。 *job_name* 为 **nvarchar (128)**，无默认值。  
  
`[ @enabled = ] enabled` 指示添加的作业的状态。 *enabled* 为 **tinyint**，默认值为 1 (启用) 。 如果为 **0**，则不启用作业，并且该作业不会根据其计划运行。但是，可以手动运行它。  
  
`[ @description = ] 'description'` 作业的说明。 *描述* 为 **nvarchar (512)**，默认值为 NULL。 如果省略 *说明* ，则使用 "无可用说明"。  
  
`[ @start_step_id = ] step_id` 作业要执行的第一个步骤的标识号。 *step_id* 的值为 **int**，默认值为1。  
  
`[ @category_name = ] 'category'` 作业的类别。 *category 的类型* 为 **sysname**，默认值为 NULL。  
  
`[ @category_id = ] category_id` 一种与语言无关的机制，用于指定作业类别。 *category_id* 的值为 **int**，默认值为 NULL。  
  
`[ @owner_login_name = ] 'login'` 拥有作业的登录名。 *login* 的值为 **sysname**，默认值为 NULL，它被解释为当前登录名。 只有 **sysadmin** 固定服务器角色的成员才能设置或更改 **\@ owner_login_name** 的值。 如果不是 **sysadmin** 角色的成员的用户设置或更改 **\@ owner_login_name** 的值，则此存储过程的执行将失败并返回错误。  
  
`[ @notify_level_eventlog = ] eventlog_level` 一个值，该值指示何时将条目放入此作业的 Microsoft Windows 应用程序日志中。 *eventlog_level* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|从不|  
|**1**|成功时|  
|**2** （默认值）|失败时|  
|**3**|始终|  
  
`[ @notify_level_email = ] email_level` 一个值，该值指示在完成该作业后何时发送电子邮件。 *email_level* 的值为 **int**，默认值为 **0**，表示从不。 *email_level* 使用与 *eventlog_level* 相同的值。  
  
`[ @notify_level_netsend = ] netsend_level` 一个值，该值指示在完成该作业后何时发送网络消息。 *netsend_level* 的值为 **int**，默认值为 **0**，表示从不。 *netsend_level* 使用与 *eventlog_level* 相同的值。  
  
`[ @notify_level_page = ] page_level` 一个值，该值指示在完成该作业后何时发送页面。 *page_level* 的值为 **int**，默认值为 **0**，表示从不。 *page_level* 使用与 *eventlog_level* 相同的值。  
  
`[ @notify_email_operator_name = ] 'email_name'` 达到 *email_level* 时要向其发送电子邮件的人员的电子邮件名称。 *email_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @notify_netsend_operator_name = ] 'netsend_name'` 完成此作业后发送网络消息的操作员的名称。 *netsend_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @notify_page_operator_name = ] 'page_name'` 完成此作业时要分页的人员的姓名。 *page_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @delete_level = ] delete_level` 指示何时删除作业的值。 *delete_value* 的值为 **int**，默认值为0，表示从不。 *delete_level* 使用与 *eventlog_level* 相同的值。  
  
> [!NOTE]  
>  当 *delete_level* 为 **3** 时，仅执行一次作业，而不考虑为作业定义的任何计划。 而且，如果作业将自身删除，则将同时删除该作业的历史记录。  
  
`[ @job_id = ] _job_idOUTPUT` 如果成功创建，则分配给作业的作业标识号。 *job_id* 是 **uniqueidentifier** 类型的输出变量，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 **\@ originating_server** 存在于 **sp_add_job 中，** 但未在 "参数" 下列出。 **\@ originating_server** 保留供内部使用。  
  
 执行 **sp_add_job** 以添加作业后，可以使用 **sp_add_jobstep** 添加执行作业的活动的步骤。 **sp_add_jobschedule** 可用于创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务用于执行作业的计划。 使用 **sp_add_jobserver** 设置作业的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 执行实例，并 **sp_delete_jobserver** 从实例中删除作业 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 如果作业将在多服务器环境中的一个或多个目标服务器上执行，请使用 **sp_apply_job_to_targets** 来设置作业的目标服务器或目标服务器组。 若要从目标服务器或目标服务器组中删除作业，请使用 **sp_remove_job_from_targets**。  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 为管理作业提供了一种图形化的简便方法，建议使用此方法来创建和管理作业基础结构。  
  
## <a name="permissions"></a>权限  
 若要运行此存储过程，用户必须是 **sysadmin** 固定服务器角色的成员，或者被授予以下 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理固定数据库角色之一，这些角色驻留在 **msdb** 数据库中：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关与这些固定数据库角色关联的特定权限的信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 只有 **sysadmin** 固定服务器角色的成员才能设置或更改 **\@ owner_login_name** 的值。 如果不是 **sysadmin** 角色的成员的用户设置或更改 **\@ owner_login_name** 的值，则此存储过程的执行将失败并返回错误。  
  
## <a name="examples"></a>示例  
  
### <a name="a-adding-a-job"></a>A. 添加作业  
 此示例将添加一个名为 `NightlyBackups` 的新作业。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_job  
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-adding-a-job-with-pager-e-mail-and-net-send-information"></a>B. 添加一个具有寻呼、电子邮件和网络发送信息的作业  
 该示例将创建一个名为 `Ad hoc Sales Data Backup` 的作业。如果该作业失败，则会通知 `François Ajenstat`（通过寻呼、电子邮件或网络弹出消息）；如果作业成功，则删除该作业。  
  
> [!NOTE]  
>  本例假定已经存在一个名为 `François Ajenstat` 的操作员和名为 `françoisa` 的登录名。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_job  
    @job_name = N'Ad hoc Sales Data Backup',   
    @enabled = 1,  
    @description = N'Ad hoc backup of sales data',  
    @owner_login_name = N'françoisa',  
    @notify_level_eventlog = 2,  
    @notify_level_email = 2,  
    @notify_level_netsend = 2,  
    @notify_level_page = 2,  
    @notify_email_operator_name = N'François Ajenstat',  
    @notify_netsend_operator_name = N'François Ajenstat',   
    @notify_page_operator_name = N'François Ajenstat',  
    @delete_level = 1 ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_add_schedule &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_add_jobstep &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_add_jobserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_apply_job_to_targets &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-apply-job-to-targets-transact-sql.md)   
 [sp_delete_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_delete_jobserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [sp_remove_job_from_targets &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-remove-job-from-targets-transact-sql.md)   
 [sp_help_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_help_jobstep &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [sp_update_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
