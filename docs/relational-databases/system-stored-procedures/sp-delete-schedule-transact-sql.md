---
description: sp_delete_schedule (Transact-SQL)
title: sp_delete_schedule (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_schedule
- sp_delete_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_schedule
ms.assetid: 18b2c985-47b8-49c8-82d1-8a4af3d7d33a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b152260dca3ab9791124b9f7f1ac13e311f6e514
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199153"
---
# <a name="sp_delete_schedule-transact-sql"></a>sp_delete_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除计划。  
 
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_delete_schedule { [ @schedule_id = ] schedule_id | [ @schedule_name = ] 'schedule_name' } ,  
     [ @force_delete = ] force_delete  
```  
  
## <a name="arguments"></a>参数  
`[ @schedule_id = ] schedule_id` 要删除的计划的标识号。 *schedule_id* 的值为 **int**，默认值为 NULL。  
  
> **注意：** 必须指定 *schedule_id* 或 *schedule_name* ，但不能同时指定两者。  
  
`[ @schedule_name = ] 'schedule_name'` 要删除的计划的名称。 *schedule_name* 的默认值为 **sysname**，默认值为 NULL。  
  
> **注意：** 必须指定 *schedule_id* 或 *schedule_name* ，但不能同时指定两者。  
  
`[ @force_delete = ] force_delete` 指定在将计划附加到作业时，该过程是否应失败。 *Force_delete* 为 bit，默认值为 **0**。 当 *force_delete* 为 **0** 时，如果将计划附加到作业，则存储过程将失败。 当 *force_delete* 为 **1** 时，将删除计划，而不考虑是否将计划附加到作业。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 默认情况下，如果将计划附加到作业，则无法删除计划。 若要删除附加到作业的计划，请将 *force_delete* 的值指定为 **1** 。 删除计划不会停止当前正在运行的作业。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 请注意，作业所有者可以将作业附加到计划以及从计划中分离作业，而不必要求也是计划所有者。 但是，如果分离会使其不包含任何作业，则不能删除计划，除非调用方是计划所有者。  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 只有 **sysadmin** 角色的成员才能删除其他用户拥有的作业计划。  
  
## <a name="examples"></a>示例  
  
### <a name="a-deleting-a-schedule"></a>A. 删除计划  
 以下示例删除计划 `NightlyJobs`。 如果该计划已附加到任何作业，则此示例不会删除该计划。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_schedule  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
### <a name="b-deleting-a-schedule-attached-to-a-job"></a>B. 删除附加到作业的计划  
 以下示例删除计划 `RunOnce`，无论该计划是否已附加到作业。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_schedule  
    @schedule_name = 'RunOnce',  
    @force_delete = 1;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [执行作业](../../ssms/agent/implement-jobs.md)   
 [sp_add_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)  
  
  
