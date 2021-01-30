---
description: sp_post_msx_operation (Transact-SQL)
title: sp_post_msx_operation (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_post_msx_operation
- sp_post_msx_operation_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_post_msx_operation
ms.assetid: 085deef8-2709-4da9-bb97-9ab32effdacf
author: markingmyname
ms.author: maghan
ms.openlocfilehash: be9ef622f686044800f634837819b4cba92821a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196041"
---
# <a name="sp_post_msx_operation-transact-sql"></a>sp_post_msx_operation (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  向 **sysdownloadlist** 系统表中 (行) 插入操作，以便下载和执行目标服务器。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_post_msx_operation  
     [ @operation = ] 'operation'  
     [ , [ @object_type = ] 'object' ]   
     { , [ @job_id = ] job_id }   
     [ , [ @specific_target_server = ] 'target_server' ]   
     [ , [ @value = ] value ]  
     [ , [ @schedule_uid = ] schedule_uid ]  
```  
  
## <a name="arguments"></a>参数  
`[ @operation = ] 'operation'` 已发布操作的操作类型。 *操作* 是 **varchar (64)**，无默认值。 有效的操作取决于 *object_type*。  
  
|对象类型|Operation|  
|-----------------|---------------|  
|**任务**|INSERT<br /><br /> UPDATE<br /><br /> DELETE<br /><br /> START<br /><br /> STOP|  
|**服务**|RE-ENLIST<br /><br /> DEFECT<br /><br /> SYNC-TIME<br /><br /> SET-POLL|  
|**编制**|INSERT<br /><br /> UPDATE<br /><br /> DELETE|  
  
`[ @object_type = ] 'object'` 要为其发布操作的对象的类型。 有效类型为 " **作业**"、" **服务器**" 和 " **计划**"。 *对象* 是 **varchar (64)**，默认值 **为。**  
  
`[ @job_id = ] job_id` 应用操作的作业的标识号。 *job_id* 是 **uniqueidentifier**，无默认值。 **0x00** 指示所有作业。 如果 *对象* 为 **SERVER**，则不需要 *job_id*。  
  
`[ @specific_target_server = ] 'target_server'` 为其应用指定操作的目标服务器的名称。 如果指定了 *job_id* ，但未指定 *target_server* ，则将为作业的所有作业服务器发布操作。 *target_server* 为 **nvarchar (30)**，默认值为 NULL。  
  
`[ @value = ] value` 轮询间隔（以秒为单位）。 *value* 的数据类型为 **int**，默认值为 NULL。 仅在 **设置** 了 "轮询"*时才指定* 此参数。  
  
`[ @schedule_uid = ] schedule_uid` 应用操作的计划的唯一标识符。 *schedule_uid* 是 **uniqueidentifier**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 必须从 **msdb** 数据库运行 **sp_post_msx_operation** 。  
  
 始终可以安全地调用 **sp_post_msx_operation** ，因为它首先确定当前服务器是否为多服务器 Microsoft SQL Server 代理，如果是，则 *对象* 是否为多服务器作业。  
  
 操作发布后，它将显示在 **sysdownloadlist** 表中。 创建并发布作业后，对该作业的后续更改也必须通知目标服务器 (TSX)。 这也是用下载列表完成的。  
  
 我们强烈建议使用 SQL Server Management Studio 来管理下载列表。 有关详细信息，请参阅 [查看或修改作业](../../ssms/agent/view-or-modify-jobs.md)。  
  
## <a name="permissions"></a>权限  
 若要运行此存储过程，用户必须被授予 **sysadmin** 固定服务器角色。  
  
## <a name="see-also"></a>另请参阅  
 [sp_add_jobserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_delete_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_delete_jobserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [sp_delete_targetserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-targetserver-transact-sql.md)   
 [sp_resync_targetserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-resync-targetserver-transact-sql.md)   
 [sp_start_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)   
 [sp_stop_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-stop-job-transact-sql.md)   
 [sp_update_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [sp_update_operator &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-update-operator-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
