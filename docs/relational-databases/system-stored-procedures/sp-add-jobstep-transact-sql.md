---
description: sp_add_jobstep (Transact-SQL)
title: sp_add_jobstep (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_jobstep_TSQL
- sp_add_jobstep
helpviewer_keywords:
- sp_add_jobstep
ms.assetid: 97900032-523d-49d6-9865-2734fba1c755
author: markingmyname
ms.author: maghan
ms.custom: ''
ms.date: 03/15/2017
ms.openlocfilehash: 93d4818a1cffc14452bdf25bab8be6e1fc9a1a50
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206761"
---
# <a name="sp_add_jobstep-transact-sql"></a>sp_add_jobstep (Transact-SQL)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

将步骤 (操作) 添加到 SQL 代理作业。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

> [!IMPORTANT]
> 在 [AZURE SQL 托管实例](/azure/sql-database/sql-database-managed-instance)上，大多数（但不是所有） SQL Server 代理作业类型都受支持。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

## <a name="syntax"></a>语法

```sql
sp_add_jobstep [ @job_id = ] job_id | [ @job_name = ] 'job_name'
     [ , [ @step_id = ] step_id ]
     { , [ @step_name = ] 'step_name' }
     [ , [ @subsystem = ] 'subsystem' ]
     [ , [ @command = ] 'command' ]
     [ , [ @additional_parameters = ] 'parameters' ]
          [ , [ @cmdexec_success_code = ] code ]
     [ , [ @on_success_action = ] success_action ]
          [ , [ @on_success_step_id = ] success_step_id ]
          [ , [ @on_fail_action = ] fail_action ]
          [ , [ @on_fail_step_id = ] fail_step_id ]
     [ , [ @server = ] 'server' ]
     [ , [ @database_name = ] 'database' ]
     [ , [ @database_user_name = ] 'user' ]
     [ , [ @retry_attempts = ] retry_attempts ]
     [ , [ @retry_interval = ] retry_interval ]
     [ , [ @os_run_priority = ] run_priority ]
     [ , [ @output_file_name = ] 'file_name' ]
     [ , [ @flags = ] flags ]
     [ , { [ @proxy_id = ] proxy_id
         | [ @proxy_name = ] 'proxy_name' } ]
```  
  
## <a name="arguments"></a>参数

`[ @job_id = ] job_id` 要向其中添加步骤的作业的标识号。 *job_id* 的值为 **uniqueidentifier**，默认值为 NULL。

`[ @job_name = ] 'job_name'` 要向其中添加步骤的作业的名称。 *job_name* 的默认值为 **sysname**，默认值为 NULL。

> [!NOTE]
> 必须指定 *job_id* 或 *job_name* ，但不能同时指定两者。

`[ @step_id = ] step_id` 作业步骤的序列标识号。 步骤标识号从 **1** 开始，递增无间隔。 如果在现有序列中插入一个步骤，则将自动调整序列号。 如果未指定 *step_id* ，则会提供一个值。 *step_id* 的值为 **int**，默认值为 NULL。

`[ @step_name = ] 'step_name'` 步骤的名称。 *step_name* **sysname**，无默认值。

`[ @subsystem = ] 'subsystem'`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]代理服务用来执行 *命令* 的子系统。 *子系统* 为 **nvarchar (40)**，可以是以下值之一。

|值|说明|
|-----------|-----------------|
|'**ActiveScripting**'|活动脚本<br /><br /> **\*\* 重要说明 \*\*** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]|
|'**CmdExec**'|操作系统命令或可执行程序|
|"**分发**"|复制分发代理作业|
|"**Snapshot**"|复制快照代理作业|
|"**日志读取** 器"|复制日志读取器代理作业|
|"**合并**"|复制合并代理作业|
|'**QueueReader**'|复制队列读取器代理作业|
|'**ANALYSISQUERY**'|Analysis Services 查询 (MDX、DMX)。|
|'**ANALYSISCOMMAND**'|Analysis Services 命令 (XMLA)。|
|"**SSIS**"|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包执行|  
|"**PowerShell**"|PowerShell 脚本|  
|"**TSQL**" (默认值) |[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句|

`[ @command = ] 'command'` 由 **SQLServerAgent** 服务通过 *子系统* 执行的命令。 *命令* 为 **nvarchar (max)**，默认值为 NULL。 SQL Server 代理提供标记替换功能；在编写软件程序时，它可提供与变量相同的灵活性。

> [!IMPORTANT]
> 作业步骤中使用的所有标记现在必须附带转义宏，否则，这些作业步骤将会失败。 此外，您现在还必须用括号将标记名称括起来，并在标记语法开头加上美元符号 (`$`)。 例如：
>
> `$(ESCAPE_` *宏名* `(DATE))`  

有关这些令牌并更新作业步骤以使用新令牌语法的详细信息，请参阅 [在作业步骤中使用令牌](../../ssms/agent/use-tokens-in-job-steps.md)。

> [!IMPORTANT]
> 对 Windows 事件日志拥有写入权限的任何 Windows 用户都可以访问由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理警报或 WMI 警报激活的作业步骤。 为了防范此安全隐患，默认情况下，可以在由警报激活的作业中使用的特定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理标记已被禁用。 这些标记包括：A-DBN、A-SVR、A-ERR、A-SEV、A-MSG 和 WMI（属性）     。 请注意，在此版本中，对标记的使用扩展至所有警报。
>
> 如果您需要使用这些标记，请首先确保只有可信任的 Windows 安全组（如 Administrators 组）成员才对安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机的事件日志拥有写入权限。 然后在对象资源管理器中右键单击“SQL Server 代理”，选择“属性”，并在“警报系统”页上选择“为警报的所有作业响应替换标记”以启用这些标记。

`[ @additional_parameters = ] 'parameters'`[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]*参数* 的值为 **ntext**，默认值为 NULL。

`[ @cmdexec_success_code = ] code`**CmdExec** 子系统命令返回的值，指示 *命令* 已成功执行。 *代码* 为 **int**，默认值为 **0**。

`[ @on_success_action = ] success_action` 步骤成功时要执行的操作。 *success_action* 为 **tinyint**，可以是下列值之一。
  
|值|说明（操作）|  
|-----------|----------------------------|  
|**1** （默认值）|成功后退出|  
|**2**|失败后退出|  
|**3**|转到下一步|  
|**4**|中转到步骤 *on_success_step_id*|  

`[ @on_success_step_id = ] success_step_id` 如果步骤成功并且 *success_action* 为 **4**，则该作业中要执行的步骤的 ID。 *success_step_id* 的值为 **int**，默认值为 **0**。

`[ @on_fail_action = ] fail_action` 步骤失败时要执行的操作。 *fail_action* 为 **tinyint**，可以是下列值之一。

|值|说明（操作）|  
|-----------|----------------------------|  
|**1**|成功后退出|  
|**2** （默认值）|失败后退出|  
|**3**|转到下一步|  
|**4**|中转到步骤 *on_fail_step_id*|  

`[ @on_fail_step_id = ] fail_step_id` 如果步骤失败并且 *fail_action* 为 **4**，则该作业中要执行的步骤的 ID。 *fail_step_id* 的值为 **int**，默认值为 **0**。  

`[ @server = ] 'server'`[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]*服务器* **(30) 为 nvarchar**，默认值为 NULL。  

`[ @database_name = ] 'database'` 要在其中执行步骤的数据库的名称 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 *database 的数据* 值为 **sysname**，默认值为 NULL，在这种情况下，将使用 **master** 数据库。 不允许用方括号 ([ ]) 将名称括起来。 对于 ActiveX 作业步骤， *数据库* 是该步骤使用的脚本语言的名称。  

`[ @database_user_name = ] 'user'` 执行步骤时要使用的用户帐户的名称 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 *user* 的值为 **sysname**，默认值为 NULL。 如果 *用户* 为 NULL，则该步骤会在 *数据库* 的作业所有者的用户上下文中运行。  只有在作业所有者是 SQL Server sysadmin 时，SQL Server 代理才包括此参数。 如果是这样，则给定的 Transact-SQL 步骤将在给定的 SQL Server 用户名的上下文中执行。 如果作业所有者不是 SQL Server sysadmin，则 Transact-sql 步骤将始终在拥有此作业的登录名的上下文中执行，并且该 @database_user_name 参数将被忽略。  

`[ @retry_attempts = ] retry_attempts` 此步骤失败时的重试尝试次数。 *retry_attempts* 的值为 **int**，默认值为 **0**，指示不重试。  

`[ @retry_interval = ] retry_interval` 重试尝试之间的时间量（以分钟为单位）。 *retry_interval* 的值为 **int**，默认值为 **0**，表示时间间隔为 **0** 分钟。  

`[ @os_run_priority = ] run_priority` 保护.

`[ @output_file_name = ] 'file_name'` 此步骤的输出保存到的文件的名称。 *file_name* 为 **nvarchar (200)**，默认值为 NULL。 *file_name* 可以包含 " *command*" 下列出的一个或多个令牌。 此参数仅对在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 、 **CmdExec**、 **PowerShell**、 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 或子系统上运行的命令有效 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 。  

`[ @flags = ] flags` 控制行为的选项。 *flags* 为 **int**，可以是下列值之一。  

|值|说明|  
|-----------|-----------------|  
|**0** （默认值）|覆盖输出文件|  
|**2**|追加到输出文件|  
|**4**|将 [!INCLUDE[tsql](../../includes/tsql-md.md)] 作业步骤输出写入步骤历史记录|  
|**8**|将日志写入表（覆盖现有的历史记录）|  
|**16**|将日志写入表（追加到现有的历史记录）|  
|**32**|将所有输出写入作业历史记录|  
|**64**|创建一个 Windows 事件以便用作 Cmd jobstep 要中止的信号|  

`[ @proxy_id = ] proxy_id` 作业步骤运行时所用代理的 id 号。 *proxy_id* 的类型为 **int**，默认值为 NULL。 如果未指定 *proxy_id* 、未指定 *proxy_name* 并且未指定 *user_name* ，则作业步骤将作为代理的服务帐户运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  

`[ @proxy_name = ] 'proxy_name'` 作业步骤运行时所用代理的名称。 *proxy_name* 的类型为 **sysname**，默认值为 NULL。 如果未指定 *proxy_id* 、未指定 *proxy_name* 并且未指定 *user_name* ，则作业步骤将作为代理的服务帐户运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  

## <a name="return-code-values"></a>返回代码值

**0** (成功) 或 **1** (失败) 

## <a name="result-sets"></a>结果集

无

## <a name="remarks"></a>备注

必须从 **msdb** 数据库运行 **sp_add_jobstep** 。  

SQL Server Management Studio 为管理作业提供了一种图形化的简便方法，建议使用此方法来创建和管理作业基础结构。  

默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 除非指定了另一个代理，否则作业步骤将作为代理的服务帐户运行。 此帐户的要求是 **sysadmin** 固定安全角色的成员。

代理可以通过 *proxy_name* 或 *proxy_id* 来识别。

## <a name="permissions"></a>权限

 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：

- **SQLAgentUserRole**

- **SQLAgentReaderRole**

- **SQLAgentOperatorRole**

有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。

作业步骤的创建者必须有权访问作业步骤的代理。 **Sysadmin** 固定服务器角色的成员有权访问所有代理。 其他用户必须经过显式授予才能访问代理。

## <a name="examples"></a>示例

以下示例可创建一个作业步骤，将销售数据库的访问权限更改为只读。 此外，此示例还指定了 5 次重试，每次重试之间的间隔为 5 分钟。

> [!NOTE]
> 此示例假定 `Weekly Sales Data Backup` 作业已存在。  

```sql
USE msdb;
GO
EXEC sp_add_jobstep
    @job_name = N'Weekly Sales Data Backup',
    @step_name = N'Set database to read only',
    @subsystem = N'TSQL',
    @command = N'ALTER DATABASE SALES SET READ_ONLY',
    @retry_attempts = 5,
    @retry_interval = 5 ;
GO
```

## <a name="next-steps"></a>后续步骤

- [查看或修改作业](../../ssms/agent/view-or-modify-jobs.md)
- [sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)
- [sp_add_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)
- [sp_delete_jobstep &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)
- [sp_help_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)
- [sp_help_jobstep &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)
- [sp_update_jobstep &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)
- [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)