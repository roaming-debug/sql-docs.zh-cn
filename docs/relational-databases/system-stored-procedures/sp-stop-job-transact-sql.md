---
description: sp_stop_job (Transact-SQL)
title: sp_stop_job (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_stop_job_TSQL
- sp_stop_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_stop_job
ms.assetid: 64b4cc75-99a0-421e-b418-94e37595bbb0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b7f4c7010ae6b00f083e54bcd3b6ec152d5d66ab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159785"
---
# <a name="sp_stop_job-transact-sql"></a>sp_stop_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理停止执行作业。  

  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_stop_job   
      [@job_name =] 'job_name'  
    | [@job_id =] job_id   
    | [@originating_server =] 'master_server'  
    | [@server_name =] 'target_server'  
```  
  
## <a name="arguments"></a>参数  
`[ @job_name = ] 'job_name'` 要停止的作业的名称。 *job_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @job_id = ] job_id` 要停止的作业的标识号。 *job_id* 的值为 **uniqueidentifier**，默认值为 NULL。  
  
`[ @originating_server = ] 'master_server'` 主服务器的名称。 如果指定主服务器的名称，则将停止所有多服务器作业。 *master_server* 为 **nvarchar (128)**，默认值为 NULL。 仅当在目标服务器上调用 **sp_stop_job** 时才指定此参数。  
  
> [!NOTE]  
>  只能指定前三个参数之一。  
  
`[ @server_name = ] 'target_server'` 要在其上停止多服务器作业的特定目标服务器的名称。 *target_server* 为 **nvarchar (128)**，默认值为 NULL。 仅当在多服务器作业的主服务器上调用 **sp_stop_job** 时才指定此参数。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 **sp_stop_job** 将停止信号发送到数据库。 某些进程可以立即停止，某些进程必须到达稳定点 (或代码路径的入口点) 才能停止。 某些长时间运行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句（例如 BACKUP、RESTORE）和某些 DBCC 命令可能要花较长的时间才能完成。 如果这些操作正在运行，则可能需要一段时间才能取消作业。 停止作业导致在作业历史记录中记录“作业已取消”项。  
  
 如果作业当前正在执行 **CmdExec** 或 **PowerShell** 类型的步骤，则运行的进程 (例如，MyProgram.exe) 强制提前结束。 提前结束可能导致不可预知的行为，如进程正在使用的文件保持为打开状态。 因此，如果作业包含 **CmdExec** 或 **PowerShell** 类型的步骤，则只能在极极端情况下使用 **sp_stop_job** 。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 **SQLAgentUserRole** 和 **SQLAgentReaderRole** 的成员只能停止其拥有的作业。 **SQLAgentOperatorRole** 的成员可以停止所有本地作业，包括其他用户所拥有的作业。 **Sysadmin** 的成员可以停止所有本地作业和多服务器作业。  
  
## <a name="examples"></a>示例  
 以下示例将停止一个名为 `Weekly Sales Data Backup` 的作业。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_stop_job  
    N'Weekly Sales Data Backup' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_delete_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_help_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_start_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)   
 [sp_update_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
