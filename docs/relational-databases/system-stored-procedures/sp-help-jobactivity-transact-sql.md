---
description: sp_help_jobactivity (Transact-SQL)
title: sp_help_jobactivity (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobactivity_TSQL
- sp_help_jobactivity
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobactivity
ms.assetid: d344864f-b4d3-46b1-8933-b81dec71f511
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b399b08120b8153af01d0faab42633f5a8325142
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199646"
---
# <a name="sp_help_jobactivity-transact-sql"></a>sp_help_jobactivity (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  列出有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业的运行时状态的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_jobactivity { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }  
     [ , [ @session_id = ] session_id ]  
```  
  
## <a name="arguments"></a>参数  
`[ @job_id = ] job_id` 作业标识号。 *job_id* 的值为 **uniqueidentifier**，默认值为 NULL。  
  
`[ @job_name = ] 'job_name'` 作业的名称。 *job_name* 的默认值为 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  必须指定 *job_id* 或 *job_name* ，但不能同时指定两者。  
  
`[ @session_id = ] session_id` 要报告其相关信息的会话 id。 *session_id* 的值为 **int**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 返回以下结果集：  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|代理会话标识号。|  
|**job_id**|**uniqueidentifier**|作业标识符。|  
|**job_name**|**sysname**|作业的名称。|  
|**run_requested_date**|**datetime**|请求运行作业的时间。|  
|**run_requested_source**|**sysname**|请求运行作业的请求源。 下列其中一项：<br /><br /> **1** = 按计划运行<br /><br /> **2** = 为响应警报而运行<br /><br /> **3** = 启动时运行<br /><br /> **4** = 按用户运行<br /><br /> **6** = 按 CPU 空闲计划运行|  
|**queued_date**|**datetime**|将请求加入队列的时间。 如果直接运行作业，则该值为 NULL。|  
|**start_execution_date**|**datetime**|将作业分配给可运行线程的时间。|  
|**last_executed_step_id**|**int**|最近运行的作业步骤的 ID。|  
|**last_exectued_step_date**|**datetime**|最近运行的作业步骤开始运行的时间。|  
|**stop_execution_date**|**datetime**|作业停止运行的时间。|  
|**next_scheduled_run_date**|**datetime**|计划下一次作业运行的时间。|  
|**job_history_id**|**int**|作业历史记录表中作业历史记录的标识符。|  
|**message**|**nvarchar(1024)**|上次运行作业期间产生的消息。|  
|**run_status**|**int**|作业上次运行时返回的状态：<br /><br /> **0** = 错误失败<br /><br /> **1** = 成功<br /><br /> **3** = 已取消<br /><br /> **5** = 状态未知|  
|**operator_id_emailed**|**int**|作业完成时通过电子邮件通知的操作员的 ID 号。|  
|**operator_id_netsent**|**int**|作业完成时通过 **net send** 通知的操作员的 ID 号。|  
|**operator_id_paged**|**int**|作业完成时通过寻呼通知的操作员的 ID 号。|  
  
## <a name="remarks"></a>备注  
 此过程可提供作业的当前状态快照。 返回的结果表示处理请求时的有关信息。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理在每次启动代理服务时都会创建一个会话 ID。 会话 id 存储在 **msdb.dbo.sys会话** 的表中。  
  
 如果未提供 *session_id* ，则列出有关最近会话的信息。  
  
 如果未提供 *job_name* 或 *job_id* ，则列出所有作业的信息。  
  
## <a name="permissions"></a>权限  
 默认情况下， **sysadmin** 固定服务器角色的成员可以运行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 只有 **sysadmin** 的成员才可以查看其他用户拥有的作业的活动。  
  
## <a name="examples"></a>示例  
 以下示例列出了当前用户有权查看的全部作业的活动。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobactivity ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 代理存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)  
  
  
