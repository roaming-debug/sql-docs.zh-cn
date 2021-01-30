---
description: sp_remove_job_from_targets (Transact-SQL)
title: sp_remove_job_from_targets (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_remove_job_from_targets_TSQL
- sp_remove_job_from_targets
dev_langs:
- TSQL
helpviewer_keywords:
- sp_remove_job_from_targets
ms.assetid: b8171fb1-c11d-4244-8618-a12e28a150ce
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b55575c5e57a72cb5a1d0b3260550cf9e14bc21a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185632"
---
# <a name="sp_remove_job_from_targets-transact-sql"></a>sp_remove_job_from_targets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从指定的目标服务器或目标服务器组中删除指定的作业。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_remove_job_from_targets [ @job_id = ] job_id   
     | [ @job_name = ] 'job_name'   
     [ , [ @target_server_groups = ] 'target_server_groups' ]   
     [ , [ @target_servers = ] 'target_servers' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @job_id = ] job_id` 要从中删除指定目标服务器或目标服务器组的作业的标识号。 必须指定 *job_id* 或 *job_name* ，但不能同时指定两者。 *job_id* 的值为 **uniqueidentifier**，默认值为 NULL。  
  
`[ @job_name = ] 'job_name'` 要从中删除指定目标服务器或目标服务器组的作业的名称。 必须指定 *job_id* 或 *job_name* ，但不能同时指定两者。 *job_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @target_server_groups = ] 'target_server_groups'` 要从指定作业中删除的目标服务器组的逗号分隔列表。 *target_server_groups* 为 **nvarchar (1024)**，默认值为 NULL。  
  
`[ @target_servers = ] 'target_servers'` 要从指定作业中删除的目标服务器的逗号分隔列表。 *target_servers* 为 **nvarchar (1024)**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="permissions"></a>权限  
 默认情况下授予 **sysadmin** 固定服务器角色的成员执行此过程的权限。  
  
## <a name="examples"></a>示例  
 以下示例从 `Weekly Sales Backups` 目标服务器组以及 `Servers Processing Customer Orders` 和 `SEATTLE1` 服务器中删除以前创建的 `SEATTLE2` 作业。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_remove_job_from_targets  
    @job_name = N'Weekly Sales Backups',  
    @target_server_groups = N'Servers Processing Customer Orders',   
    @target_servers = N'SEATTLE2,SEATTLE1' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_apply_job_to_targets &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-apply-job-to-targets-transact-sql.md)   
 [sp_delete_jobserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
