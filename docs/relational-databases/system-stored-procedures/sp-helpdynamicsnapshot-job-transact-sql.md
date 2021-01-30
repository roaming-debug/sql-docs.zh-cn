---
description: sp_helpdynamicsnapshot_job (Transact-SQL)
title: sp_helpdynamicsnapshot_job (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdynamicsnapshot_TSQL
- sp_helpdynamicsnapshot_job_TSQL
- job_TSQL
- helpdynamicsnapshot
- job
- sp_helpdynamicsnapshot
- sp_helpdynamicsnapshot_job
- helpdynamicsnapshot_TSQL
helpviewer_keywords:
- sp_helpdynamicsnapshot_job
ms.assetid: d6dfdf26-f874-495f-a8a6-8780699646d7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: faf8fcfe5636dc588b111701c5b2cf0668feed78
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204437"
---
# <a name="sp_helpdynamicsnapshot_job-transact-sql"></a>sp_helpdynamicsnapshot_job (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  返回有关生成筛选数据快照的代理作业的信息。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpdynamicsnapshot_job [ [ @publication = ] 'publication' ]   
    [ , [ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname' ]  
    [ , [ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 的数据类型为 **sysname**，默认值为 **%** ，它返回与所有发布的指定 *dynamic_snapshot_jobid* 和 *dynamic_snapshot_jobname* 匹配的所有已筛选的数据快照作业的信息。  
  
`[ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname'` 筛选的数据快照作业的名称。 *dynamic_snapshot_jobname* 为 **sysname**，默认值为 **%** ""，它将返回具有指定 *dynamic_snapshot_jobid* 的发布的所有动态作业。 如果创建作业时未显式指定作业名称，则作业名称具有以下格式：  
  
```  
'dyn_' + <name of the standard snapshot job> + <GUID>  
```  
  
`[ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid'` 筛选的数据快照作业的标识符。 *dynamic_snapshot_jobid* 为 **uniqueidentifier**，默认值为 NULL，表示返回与指定 *dynamic_snapshot_jobname* 匹配的所有快照作业。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**id**|**int**|标识已筛选的数据快照作业。|  
|**job_name**|**sysname**|已筛选数据快照作业的名称。|  
|**job_id**|**uniqueidentifier**|标识 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分发服务器上的代理作业。|  
|**dynamic_filter_login**|**sysname**|用于计算为发布定义的参数化行筛选器中的 [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) 函数的值。|  
|**dynamic_filter_hostname**|**sysname**|用于计算为发布定义的参数化行筛选器中的 [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) 函数的值。|  
|**dynamic_snapshot_location**|**nvarchar(255)**|在使用参数化行筛选器时，从中读取快照文件的文件夹的路径。|  
|**frequency_type**|**int**|代理计划运行的频率，可以为下列值之一：<br /><br /> **1** = 一次<br /><br /> **2** = 按需<br /><br /> **4** = 每天<br /><br /> **8** = 每周<br /><br /> **16** = 每月<br /><br /> **32** = 每月相对<br /><br /> **64** = 自动启动<br /><br /> **128** = 定期|  
|**frequency_interval**|**int**|代理运行的日期，可以为下列值之一：<br /><br /> **1** = 星期日<br /><br /> **2** = 星期一<br /><br /> **3** = 星期二<br /><br /> **4** = 星期三<br /><br /> **5** = 星期四<br /><br /> **6** = 星期五<br /><br /> **7** = 星期六<br /><br /> **8** = 日<br /><br /> **9** = 工作日<br /><br /> **10** = 周末|  
|**frequency_subday_type**|**int**|定义代理在 *frequency_type* 为每) 日 (为 **4** 时运行的频率的类型，可以是下列值之一。<br /><br /> **1** = 在指定时间<br /><br /> **2** = 秒<br /><br /> **4** = 分钟<br /><br /> **8** = 小时|  
|**frequency_subday_interval**|**int**|在计划的代理执行之间发生的 *frequency_subday_type* 间隔数。|  
|**frequency_relative_interval**|**int**|当 *frequency_type* 为 **32** (每月相对) ，并且可以是下列值之一时，代理在指定月份内运行的周。<br /><br /> **1** = 第一<br /><br /> **2** = 秒<br /><br /> **4** = 第三<br /><br /> **8** = 第四<br /><br /> **16** = 最后|  
|**frequency_recurrence_factor**|**int**|在计划的代理执行之间间隔的周数或月数。|  
|**active_start_date**|**int**|计划第一次运行代理的日期，格式为 YYYYMMDD。|  
|**active_end_date**|**int**|计划最后一次运行代理的日期，格式为 YYYYMMDD。|  
|**active_start_time**|**int**|计划第一次运行代理的时间，格式为 HHMMSS。|  
|**active_end_time**|**int**|计划最后一次运行代理的时间，格式为 HHMMSS。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpdynamicsnapshot_job** 用于合并复制。  
  
 如果使用所有默认参数值，则将返回用于整个发布数据库的所有已分区数据快照作业的信息。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员、 **db_owner** 固定数据库角色的成员和发布的发布访问列表才能执行 **sp_helpdynamicsnapshot_job**。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
