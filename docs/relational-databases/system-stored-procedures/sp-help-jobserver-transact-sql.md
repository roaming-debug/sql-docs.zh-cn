---
description: sp_help_jobserver (Transact-SQL)
title: sp_help_jobserver (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobserver
- sp_help_jobserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobserver
ms.assetid: 57971787-f9f5-4199-9f64-c2b61a308906
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4c5e5f8a4c7ec027c72452ea725ec6def715309a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200120"
---
# <a name="sp_help_jobserver-transact-sql"></a>sp_help_jobserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为给定的作业返回有关服务器的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_jobserver  
     { [ @job_id = ] job_id   
     | [ @job_name = ] 'job_name' }  
     [ , [ @show_last_run_details = ] show_last_run_details ]  
```  
  
## <a name="arguments"></a>参数  
`[ @job_id = ] job_id` 要为其返回信息的作业标识号。 *job_id* 的值为 **uniqueidentifier**，默认值为 NULL。  
  
`[ @job_name = ] 'job_name'` 要为其返回信息的作业的名称。 *job_name* 的默认值为 **sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  必须指定 *job_id* 或 *job_name* ，但不能同时指定两者。  
  
`[ @show_last_run_details = ] show_last_run_details` 指示上次运行的执行信息是否是结果集的一部分。 *show_last_run_details* 为 **tinyint**，默认值为 **0**。 **0** 不包含上一次运行的信息， **1** 则为。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|目标服务器的标识号。|  
|server_name|**nvarchar(30)**|目标服务器的计算机名称。|  
|**enlist_date**|**datetime**|将目标服务器登记到主服务器的日期。|  
|**last_poll_date**|**datetime**|目标服务器上一次轮询主服务器的日期。|  
  
 如果在 *show_last_run_details* 设置为 **1** 的情况下执行 **sp_help_jobserver** ，则结果集将包含这些其他列。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**last_run_date**|**int**|作业上一次在此目标服务器上开始执行的日期。|  
|**last_run_time**|**int**|作业上一次在此服务器上开始执行的时间。|  
|**last_run_duration**|**int**|作业上一次在此目标服务器上运行所持续的时间（以秒为单位）。|  
|**last_outcome_message**|**nvarchar(1024)**|说明作业上一次运行的结果。|  
|**last_run_outcome**|**int**|作业上一次在此服务器上运行的结果：<br /><br /> **0** = 失败<br /><br /> **1** = 成功<br /><br /> **3** = 已取消<br /><br /> **5** = 未知|  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 **SQLAgentUserRole** 的成员只能查看其所拥有的作业的信息。  
  
## <a name="examples"></a>示例  
 以下示例返回有关 `NightlyBackups` 作业的信息，其中包括上一次运行的信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobserver  
    @job_name = N'NightlyBackups',  
    @show_last_run_details = 1 ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_add_jobserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_delete_jobserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
