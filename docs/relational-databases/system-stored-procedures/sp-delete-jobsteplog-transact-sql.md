---
description: sp_delete_jobsteplog (Transact-SQL)
title: sp_delete_jobsteplog (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_jobsteplog
- sp_delete_jobsteplog_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_jobsteplog
ms.assetid: e9ef4c99-abde-4038-b6a3-a25dcbaf0958
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 17cdcb1ba86e214ce707ff084a0d921b3817cbe0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211150"
---
# <a name="sp_delete_jobsteplog-transact-sql"></a>sp_delete_jobsteplog (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除用参数指定的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业步骤日志。 使用此存储过程来维护 **msdb** 数据库中的 **sysjobstepslogs** 表。  
  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_delete_jobsteplog { [ @job_id = ] 'job_id' | [ @job_name = ] 'job_name' }  
       [ , [ @step_id = ] step_id | [ @step_name = ] 'step_name' ]  
       [ , [ @older_than = ] 'date' ]  
       [ , [ @larger_than = ] 'size_in_bytes' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @job_id = ] 'job_id'` 包含要删除的作业步骤日志的作业的标识号。 *job_id* 的值为 **int**，默认值为 NULL。  
  
`[ @job_name = ] 'job_name'` 作业的名称。 *job_name* 的默认值为 **sysname**，默认值为 NULL。  
  
> **注意：** 必须指定 *job_id* 或 *job_name* ，但不能同时指定两者。  
  
`[ @step_id = ] step_id` 作业步骤日志要删除的作业中步骤的标识号。 如果未包括，则删除作业中的所有作业步骤日志，除非指定 **\@ older_than** 或 **\@ larger_than** 。 *step_id* 的值为 **int**，默认值为 NULL。  
  
`[ @step_name = ] 'step_name'` 作业步骤日志要删除的作业中步骤的名称。 *step_name* 的默认值为 **sysname**，默认值为 NULL。  
  
> **注意：** 可以指定 *step_id* 或 *step_name* ，但不能同时指定两者。  
  
`[ @older_than = ] 'date'` 您要保留的最早作业步骤日志的日期和时间。 将删除早于该日期和时间的所有作业步骤日志。 *date* 为 **datetime**，默认值为 NULL。 可以同时指定 **\@ older_than** 和 **\@ larger_than** 。  
  
`[ @larger_than = ] 'size_in_bytes'` 您要保留的最大作业步骤日志的大小（以字节为单位）。 大于此大小的所有作业步骤日志都会被删除。 可以同时指定 **\@ larger_than** 和 **\@ older_than** 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 **sp_delete_jobsteplog** 在 **msdb** 数据库中。  
  
 如果未指定除 **\@ job_id** 或 **\@ job_name** 以外的参数，则将删除指定作业的所有作业步骤日志。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 只有 **sysadmin** 的成员才能删除其他用户拥有的作业步骤日志。  
  
## <a name="examples"></a>示例  
  
### <a name="a-removing-all-job-step-logs-from-a-job"></a>A. 删除作业中的所有作业步骤日志  
 以下示例删除作业 `Weekly Sales Data Backup` 的所有作业步骤日志。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup';  
GO  
```  
  
### <a name="b-removing-the-job-step-log-for-a-particular-job-step"></a>B. 删除特定作业步骤的作业步骤日志  
 以下示例删除作业 `Weekly Sales Data Backup` 中步骤 2 的作业步骤日志。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 2;  
GO  
```  
  
### <a name="c-removing-all-job-step-logs-based-on-age-and-size"></a>C. 基于存在时间和大小删除所有作业步骤日志  
 以下示例从作业 `Weekly Sales Data Backup` 中删除时间早于 2005 年 10 月 25 日中午、大小大于 100 兆字节 (MB) 的所有作业步骤日志。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @older_than = '10/25/2005 12:00:00',  
    @larger_than = 104857600;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_help_jobsteplog &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-jobsteplog-transact-sql.md)   
 [SQL Server 代理存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)  
  
  
