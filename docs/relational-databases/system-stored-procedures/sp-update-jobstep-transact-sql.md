---
description: sp_update_jobstep (Transact-SQL)
title: sp_update_jobstep (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_jobstep
- sp_update_jobstep_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_jobstep
ms.assetid: e158802c-c347-4a5d-bf75-c03e5ae56e6b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c102cee625bc81611bd67209306d6bc5f9398660
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183569"
---
# <a name="sp_update_jobstep-transact-sql"></a>sp_update_jobstep (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改执行自动活动的作业中某一步骤的设置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_update_jobstep   
     {   [@job_id =] job_id   
       | [@job_name =] 'job_name' } ,  
     [@step_id =] step_id  
     [ , [@step_name =] 'step_name' ]  
     [ , [@subsystem =] 'subsystem' ]   
     [ , [@command =] 'command' ]  
     [ , [@additional_parameters =] 'parameters' ]  
     [ , [@cmdexec_success_code =] success_code ]  
     [ , [@on_success_action =] success_action ]   
     [ , [@on_success_step_id =] success_step_id ]  
     [ , [@on_fail_action =] fail_action ]   
     [ , [@on_fail_step_id =] fail_step_id ]  
     [ , [@server =] 'server' ]   
     [ , [@database_name =] 'database' ]  
     [ , [@database_user_name =] 'user' ]   
     [ , [@retry_attempts =] retry_attempts ]  
     [ , [@retry_interval =] retry_interval ]   
     [ , [@os_run_priority =] run_priority ]  
     [ , [@output_file_name =] 'file_name' ]   
     [ , [@flags =] flags ]  
     [ ,  {   [ @proxy_id = ] proxy_id   
            | [ @proxy_name = ] 'proxy_name' }   
```  
  
## <a name="arguments"></a>参数  
`[ @job_id = ] job_id` 步骤所属的作业的标识号。 *job_id* 的值为 **uniqueidentifier**，默认值为 NULL。 必须指定 *job_id* 或 *job_name* ，但不能同时指定两者。  
  
`[ @job_name = ] 'job_name'` 步骤所属的作业的名称。 *job_name* 的默认值为 **sysname**，默认值为 NULL。 必须指定 *job_id* 或 *job_name* ，但不能同时指定两者。  
  
`[ @step_id = ] step_id` 要修改的作业步骤的标识号。 不能更改该标识号。 *step_id* 为 **int**，没有默认值。  
  
`[ @step_name = ] 'step_name'` 步骤的新名称。 *step_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @subsystem = ] 'subsystem'` Microsoft SQL Server 代理执行 *命令* 所使用的子系统。 *子系统* **(40) 为 nvarchar**，默认值为 NULL。  
  
`[ @command = ] 'command'` 要通过 *子系统* 执行)  (的命令。 *命令* 为 **nvarchar (max)**，默认值为 NULL。  
  
`[ @additional_parameters = ] 'parameters'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @cmdexec_success_code = ] success_code`**CmdExec** 子系统命令返回的值，指示 *命令* 已成功执行。 *success_code* 的值为 **int**，默认值为 NULL。  
  
`[ @on_success_action = ] success_action` 步骤成功时要执行的操作。*success_action* 为 **tinyint**，默认值为 NULL，可以是下列值之一。  
  
|值|说明（操作）|  
|-----------|----------------------------|  
|**1**|成功后退出。|  
|**2**|失败后退出。|  
|**3**|继续下一步。|  
|**4**|请参阅步骤 *success_step_id。*|  
  
`[ @on_success_step_id = ] success_step_id` 步骤成功并且 *success_action* 为 **4** 时，该作业中要执行的步骤的标识号。 *success_step_id* 的值为 **int**，默认值为 NULL。  
  
`[ @on_fail_action = ] fail_action` 步骤失败时要执行的操作。 *fail_action* 为 **tinyint**，默认值为 NULL，可以为以下值之一。  
  
|值|说明（操作）|  
|-----------|----------------------------|  
|**1**|成功后退出。|  
|**2**|失败后退出。|  
|**3**|继续下一步。|  
|**4**|请参阅步骤 *fail_step_id * *。*|  
  
`[ @on_fail_step_id = ] fail_step_id` 如果步骤失败并且 *fail_action* 为 **4**，则该作业中要执行的步骤的标识号。 *fail_step_id* 的值为 **int**，默认值为 NULL。  
  
`[ @server = ] 'server'`[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]*服务器* **(128) 为 nvarchar**，默认值为 NULL。  
  
`[ @database_name = ] 'database'` 要在其中执行步骤的数据库的名称 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 *数据库* 为 **sysname**。 不允许用方括号 ([ ]) 将名称括起来。 默认值为 NULL。  
  
`[ @database_user_name = ] 'user'` 执行步骤时要使用的用户帐户的名称 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 *user* 的值为 **sysname**，默认值为 NULL。  
  
`[ @retry_attempts = ] retry_attempts` 此步骤失败时的重试尝试次数。 *retry_attempts* 的值为 **int**，默认值为 NULL。  
  
`[ @retry_interval = ] retry_interval` 重试尝试之间的时间量（以分钟为单位）。 *retry_interval* 的值为 **int**，默认值为 NULL。  
  
`[ @os_run_priority = ] run_priority` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @output_file_name = ] 'file_name'` 此步骤的输出保存到的文件的名称。 *file_name* 为 **nvarchar (200)**，默认值为 NULL。 此参数只对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 或 CmdExec 子系统中运行的命令有效。  
  
 若要将 output_file_name 设置回 NULL，必须将 *output_file_name* 设置为空字符串 ( "" ) 或空白字符的字符串，但不能使用 **CHAR (32)** 函数。 例如，按如下所示将此参数设置为空字符串：  
  
 **@output_file_name = ' '**  
  
`[ @flags = ] flags` 控制行为的选项。 *flags* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0** （默认值）|覆盖输出文件。|  
|**2**|追加到输出文件|  
|**4**|将 Transact-SQL 作业步骤输出写入步骤历史记录|  
|**8**|将日志写入表（覆盖现有的历史记录）|  
|**16**|将日志写入表（追加到现有的历史记录）|  
  
`[ @proxy_id = ] proxy_id` 作业步骤运行时所用代理的 ID 号。 *proxy_id* 的类型为 **int**，默认值为 NULL。 如果未指定 *proxy_id* 、未指定 *proxy_name* 并且未指定 *user_name* ，则作业步骤将作为代理的服务帐户运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
`[ @proxy_name = ] 'proxy_name'` 作业步骤运行时所用代理的名称。 *proxy_name* 的类型为 **sysname**，默认值为 NULL。 如果未指定 *proxy_id* 、未指定 *proxy_name* 并且未指定 *user_name* ，则作业步骤将作为代理的服务帐户运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 必须从 **msdb** 数据库运行 **sp_update_jobstep** 。  
  
 更新作业步骤将增加作业的版本号。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 只有 **sysadmin** 的成员才能更新其他用户所拥有的作业步骤。  
  
 如果作业步骤需要访问代理，则该作业步骤的创建者必须拥有对该作业步骤的代理的访问权限。 除 Transact-SQL 之外的所有子系统都需要一个代理帐户。 **Sysadmin** 的成员可以访问所有代理，并且可以使用代理的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户。  
  
## <a name="examples"></a>示例  
 下面的示例更改 `Weekly Sales Data Backup` 作业的第一步的重试次数。 在运行此示例后，重试次数将为 `10`。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1,  
    @retry_attempts = 10 ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [查看或修改作业](../../ssms/agent/view-or-modify-jobs.md)   
 [sp_delete_jobstep &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_jobstep &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
