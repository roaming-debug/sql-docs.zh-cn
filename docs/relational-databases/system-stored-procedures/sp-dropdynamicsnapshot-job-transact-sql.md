---
description: sp_dropdynamicsnapshot_job (Transact-SQL)
title: sp_dropdynamicsnapshot_job (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropdynamicsnapshot_job_TSQL
- sp_dropdynamicsnapshot_job
helpviewer_keywords:
- sp_dropdynamicsnapshot_job
ms.assetid: 128e428a-01b3-4062-8c6e-d22d5fa268a9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 481ef137f8aa2eca0438cee08e571c3e21bddfe7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210041"
---
# <a name="sp_dropdynamicsnapshot_job-transact-sql"></a>sp_dropdynamicsnapshot_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为具有参数化行筛选器的发布删除筛选的数据快照作业。 此存储过程在发布服务器上对发布数据库执行。 删除该作业后，将从 [MSdynamicsnapshotjobs](../../relational-databases/system-tables/msdynamicsnapshotjobs-transact-sql.md) 系统表中删除所有相关数据。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dropdynamicsnapshot_job [ @publication = ] 'publication'   
    [ , [ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname' ]   
    [ , [ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid' ]   
    [ , [ @ignore_distributor =] ignore_distributor ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 要从中删除筛选数据快照作业的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname'` 要删除的筛选数据快照作业的名称。 *dynamic_snapshot_jobname* 为 sysname，如果未提供它，则默认为与 *dynamic_snapshot_jobid* 关联的任何作业名称。  
  
`[ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid'` 是要删除的筛选数据快照作业的标识符。 *dynamic_snapshot_jobid* 为 **uniqueidentifier**，默认值为 NULL。  
  
> [!IMPORTANT]  
>  只能指定 *dynamic_snapshot_jobid* 或 *dynamic_snapshot_jobname* 。 如果没有为 *dynamic_snapshot_jobid* 或 *dynamic_snapshot_jobname* 提供值，则会删除发布的所有动态快照作业。  
  
`[ @ignore_distributor = ] ignore_distributor`*ignore_distributor* 为 **bit**，默认值为 **0**。 该参数可用于删除动态快照作业但不清除分发服务器上的任务。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_dropdynamicsnapshot** 用于合并复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_dropdynamicsnapshot**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_adddynamicsnapshot_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-adddynamicsnapshot-job-transact-sql.md)  
  
  
