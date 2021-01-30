---
description: sp_help_jobhistory (Transact-SQL)
title: sp_help_jobhistory (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobhistory_TSQL
- sp_help_jobhistory
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobhistory
ms.assetid: a944d44e-411b-4735-8ce4-73888d4262d7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2cfef1ba5f28b498ab360daf67cc1bf3c79f0d2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200133"
---
# <a name="sp_help_jobhistory-transact-sql"></a>sp_help_jobhistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为多服务器管理域中的服务器提供有关作业的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_jobhistory [ [ @job_id = ] job_id ]   
     [ , [ @job_name = ] 'job_name' ]   
     [ , [ @step_id = ] step_id ]   
     [ , [ @sql_message_id = ] sql_message_id ]   
     [ , [ @sql_severity = ] sql_severity ]   
     [ , [ @start_run_date = ] start_run_date ]   
     [ , [ @end_run_date = ] end_run_date ]   
     [ , [ @start_run_time = ] start_run_time ]   
     [ , [ @end_run_time = ] end_run_time ]   
     [ , [ @minimum_run_duration = ] minimum_run_duration ]   
     [ , [ @run_status = ] run_status ]   
     [ , [ @minimum_retries = ] minimum_retries ]   
     [ , [ @oldest_first = ] oldest_first ]   
     [ , [ @server = ] 'server' ]   
     [ , [ @mode = ] 'mode' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @job_id = ] job_id` 作业标识号。 *job_id* 的值为 **uniqueidentifier**，默认值为 NULL。  
  
`[ @job_name = ] 'job_name'` 作业的名称。 *job_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @step_id = ] step_id` 步骤标识号。 *step_id* 的值为 **int**，默认值为 NULL。  
  
`[ @sql_message_id = ] sql_message_id` 执行作业时 Microsoft SQL Server 返回的错误消息的标识号。 *sql_message_id* 的值为 **int**，默认值为 NULL。  
  
`[ @sql_severity = ] sql_severity` 执行作业时 SQL Server 返回的错误消息的严重级别。 *sql_severity* 的值为 **int**，默认值为 NULL。  
  
`[ @start_run_date = ] start_run_date` 作业的开始日期。 *start_run_date* 的值为 **int**，默认值为 NULL。 必须以 YYYYMMDD 形式输入 *start_run_date* ，其中 YYYY 是四个字符的年份，MM 是一个两个字符的月份名称，而 DD 是两个字符的日期。  
  
`[ @end_run_date = ] end_run_date` 作业的完成日期。 *end_run_date* 的值为 **int**，默认值为 NULL。 必须以 YYYYMMDD 形式输入 *end_run_date*，其中 YYYY 是四位数年份，MM 表示两个字符的月份，DD 表示两个字符的日期。  
  
`[ @start_run_time = ] start_run_time` 作业启动的时间。 *start_run_time* 的值为 **int**，默认值为 NULL。 必须以 HHMMSS 格式输入 *start_run_time*，其中，HH 是一天中的两个字符，MM 是一天中两个字符的分钟，SS 是一天中的两个字符。  
  
`[ @end_run_time = ] end_run_time` 作业完成其执行的时间。 *end_run_time* 的值为 **int**，默认值为 NULL。 必须以 HHMMSS 格式输入 *end_run_time*，其中，HH 是一天中的两个字符，MM 是一天中两个字符的分钟，SS 是一天中的两个字符。  
  
`[ @minimum_run_duration = ] minimum_run_duration` 完成作业所用的最小时间长度。 *minimum_run_duration* 的值为 **int**，默认值为 NULL。 必须以 HHMMSS 格式输入 *minimum_run_duration*，其中，HH 是一天中的两个字符，MM 是一天中两个字符的分钟，SS 是一天中的两个字符。  
  
`[ @run_status = ] run_status` 作业的执行状态。 *run_status* 的数据值为 **int**，默认值为 NULL，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|已失败|  
|**1**|成功|  
|**2**|重试（只针对步骤）|  
|**3**|已取消|  
|**4**|进行中的消息|  
|**5**|Unknown|  
  
`[ @minimum_retries = ] minimum_retries` 作业应重试运行的最少次数。 *minimum_retries* 的值为 **int**，默认值为 NULL。  
  
`[ @oldest_first = ] oldest_first` 指示是否要先显示最早的作业的输出。 *oldest_first* 的值为 **int**，默认值为 **0**，表示首先显示最新的作业。 **1** 表示首先显示最早的作业。  
  
`[ @server = ] 'server'` 在其上执行作业的服务器的名称。 *服务器* **(30) 为 nvarchar**，默认值为 NULL。  
  
`[ @mode = ] 'mode'` SQL Server 是否将结果集中的所有列打印 (**完整**) 或列的摘要。 *模式* 为 **varchar (7)**，默认值为 **SUMMARY**。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 实际列列表取决于 *模式* 的值。 最全面的一组列如下所示，并且当 *mode* 为 FULL 时返回。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|历史记录条目标识号。|  
|**job_id**|**uniqueidentifier**|作业标识号。|  
|**job_name**|**sysname**|作业名称。|  
|**step_id**|**int**|对于作业历史记录) ，步骤标识号 (将为 **0** 。|  
|**step_name**|**sysname**|步骤名称（对于作业历史记录将为 NULL）。|  
|**sql_message_id**|**int**|对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 步骤，为运行命令时遇到的最近的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 错误号。|  
|**sql_severity**|**int**|对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 步骤，为运行命令时遇到的最高级别的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 错误严重性。|  
|**message**|**nvarchar(1024)**|作业或步骤历史记录消息。|  
|**run_status**|**int**|作业或步骤的结果。|  
|**run_date**|**int**|作业或步骤开始执行的日期。|  
|**run_time**|**int**|作业或步骤开始执行的时间。|  
|**run_duration**|**int**|执行作业或步骤所花费的时间，以 HHMMSS 格式表示。|  
|**operator_emailed**|**nvarchar (20)**|接收有关该作业的电子邮件的操作员（对于步骤历史记录为 NULL）。|  
|**operator_netsent**|**nvarchar (20)**|接收有关该作业的网络消息的操作员（对于步骤历史记录为 NULL）。|  
|**operator_paged**|**nvarchar (20)**|接收有关该作业的寻呼的操作员（对于步骤历史记录为 NULL）。|  
|**retries_attempted**|**int**|步骤的重试次数（对于作业历史记录始终为 0）。|  
|服务器|**nvarchar(30)**|执行步骤或作业的服务器。 始终 (**本地**) 。|  
  
## <a name="remarks"></a>备注  
 **sp_help_jobhistory** 返回一个报表，其中包含指定计划作业的历史记录。 如果没有指定参数，则该报表包含所有预定作业的历史记录。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 **SQLAgentUserRole** 数据库角色的成员只能查看其所拥有的作业的历史记录。  
  
## <a name="examples"></a>示例  
  
### <a name="a-listing-all-job-information-for-a-job"></a>A. 列出指定作业的所有作业信息  
 以下示例列出了 `NightlyBackups` 作业的所有作业信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobhistory   
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-listing-information-for-jobs-that-match-certain-conditions"></a>B. 列出符合某些条件的作业的信息  
 以下示例针对遇到错误号为 `50100`（用户定义错误消息）、严重级别为 `20` 的错误的任何失败作业和失败作业步骤，打印所有列和所有作业的信息。  
  
```  
USE msdb  
GO  
  
EXEC dbo.sp_help_jobhistory  
    @sql_message_id = 50100,  
    @sql_severity = 20,  
    @run_status = 0,  
    @mode = N'FULL' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_purge_jobhistory (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-purge-jobhistory-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
