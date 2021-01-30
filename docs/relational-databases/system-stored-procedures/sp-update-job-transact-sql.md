---
description: sp_update_job (Transact-SQL)
title: sp_update_job (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_job
- sp_update_job_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_job
ms.assetid: cbdfea38-9e42-47f3-8fc8-5978b82e2623
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e559c50589681f7d00b1bd6048b1e23579213bee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184702"
---
# <a name="sp_update_job-transact-sql"></a>sp_update_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改作业的属性。  
  

  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_update_job [ @job_id =] job_id | [@job_name =] 'job_name'  
     [, [@new_name =] 'new_name' ]   
     [, [@enabled =] enabled ]  
     [, [@description =] 'description' ]   
     [, [@start_step_id =] step_id ]  
     [, [@category_name =] 'category' ]   
     [, [@owner_login_name =] 'login' ]  
     [, [@notify_level_eventlog =] eventlog_level ]  
     [, [@notify_level_email =] email_level ]  
     [, [@notify_level_netsend =] netsend_level ]  
     [, [@notify_level_page =] page_level ]  
     [, [@notify_email_operator_name =] 'operator_name' ]  
     [, [@notify_netsend_operator_name =] 'netsend_operator' ]  
     [, [@notify_page_operator_name =] 'page_operator' ]  
     [, [@delete_level =] delete_level ]   
     [, [@automatic_post =] automatic_post ]  
```  
  
## <a name="arguments"></a>参数  
`[ @job_id = ] job_id` 要更新的作业的标识号。 *job_id* 是 **uniqueidentifier**。  
  
`[ @job_name = ] 'job_name'` 作业的名称。 *job_name* **(128) 为 nvarchar**。  
  
> **注意：** 必须指定 *job_id* 或 *job_name* ，但不能同时指定两者。  
  
`[ @new_name = ] 'new_name'` 作业的新名称。 *new_name* **(128) 为 nvarchar**。  
  
`[ @enabled = ] enabled` 指定是否 (**1**) 或未启用 (**0**) 启用作业。 *enabled* 为 **tinyint**。  
  
`[ @description = ] 'description'` 作业的说明。 *描述* 为 **nvarchar (512)**。  
  
`[ @start_step_id = ] step_id` 作业要执行的第一个步骤的标识号。 *step_id* 是 **int**。  
  
`[ @category_name = ] 'category'` 作业的类别。 *类别* 为 **nvarchar (128)**。  
  
`[ @owner_login_name = ] 'login'` 拥有作业的登录名。 *登录名* 为 **nvarchar (128)** 只有 **sysadmin** 固定服务器角色的成员才可以更改作业所有权。  
  
`[ @notify_level_eventlog = ] eventlog_level` 指定何时将条目放入此作业的 Microsoft Windows 应用程序日志中。 *eventlog_level* 为 **int**，可以是下列值之一。  
  
|值|说明（操作）|  
|-----------|----------------------------|  
|**0**|从不|  
|**1**|成功时|  
|**2**|失败时|  
|**3**|始终|  
  
`[ @notify_level_email = ] email_level` 指定在完成该作业后何时发送电子邮件。 *email_level* 是 **int**。 *email_level* 使用与 *eventlog_level* 相同的值。  
  
`[ @notify_level_netsend = ] netsend_level` 指定在此作业完成后发送网络消息的时间。 *netsend_level* 是 **int**。 *netsend_level* 使用与 *eventlog_level* 相同的值。  
  
`[ @notify_level_page = ] page_level` 指定在此作业完成后发送页面的时间。 *page_level* 是 **int**。 *page_level* 使用与 *eventlog_level* 相同的值。  
  
`[ @notify_email_operator_name = ] 'operator_name'`*Email_level* 达到时向其发送电子邮件的操作员的名称。 *email_name* **(128) 为 nvarchar**。  
  
`[ @notify_netsend_operator_name = ] 'netsend_operator'` 向其发送网络消息的操作员的名称。 *netsend_operator* **(128) 为 nvarchar**。  
  
`[ @notify_page_operator_name = ] 'page_operator'` 向其发送页的操作员的名称。 *page_operator* **(128) 为 nvarchar**。  
  
`[ @delete_level = ] delete_level` 指定删除作业的时间。 *delete_value* 是 **int**。 *delete_level* 使用与 *eventlog_level* 相同的值。  
  
`[ @automatic_post = ] automatic_post` 保护.  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 必须从 **msdb** 数据库运行 **sp_update_job** 。  
  
 **sp_update_job** 仅更改为其提供参数值的设置。 如果省略某一参数，则保留其当前设置。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 只有 **sysadmin** 的成员才可以使用此存储过程来编辑其他用户拥有的作业的属性。  
  
## <a name="examples"></a>示例  
 下面的示例更改作业的名称、说明和已启用状态 `NightlyBackups` 。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_job  
    @job_name = N'NightlyBackups',  
    @new_name = N'NightlyBackups -- Disabled',  
    @description = N'Nightly backups disabled during server migration.',  
    @enabled = 0 ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)   
 [sp_delete_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_help_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
