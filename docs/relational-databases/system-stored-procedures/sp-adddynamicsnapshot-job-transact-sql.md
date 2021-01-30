---
description: sp_adddynamicsnapshot_job (Transact-SQL)
title: sp_adddynamicsnapshot_job (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddynamicsnapshot_job
- sp_adddynamicsnapshot_job_TSQL
helpviewer_keywords:
- sp_adddynamicsnapshot_job
ms.assetid: ef50ccf6-e360-4e4b-91b9-6706b8fabefa
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 9798613901be96552813ebe6f419f2e2c24229fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182793"
---
# <a name="sp_adddynamicsnapshot_job-transact-sql"></a>sp_adddynamicsnapshot_job (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  创建一个代理作业，该代理作业可为具有参数化行筛选器的发布生成筛选数据快照。 此存储过程在发布服务器上对发布数据库执行。 管理员使用此存储过程可手动为订阅服务器创建筛选数据快照作业。  
  
> [!NOTE]  
>  为了创建筛选数据快照作业，必须存在该发布的标准快照作业。  
  
 有关详细信息，请参阅 [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_adddynamicsnapshot_job [ @publication = ] 'publication'   
    [ , [ @suser_sname = ] 'suser_sname' ]   
    [ , [ @host_name = ] 'host_name' ]   
    [ , [ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname' OUTPUT ]   
    [ , [ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid' OUTPUT ]   
    [ , [ @frequency_type= ] frequency_type ]  
    [ , [ @frequency_interval= ] frequency_interval ]  
    [ , [ @frequency_subday= ] frequency_subday ]  
    [ , [ @frequency_subday_interval= ] frequency_subday_interval ]  
    [ , [ @frequency_relative_interval= ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor= ] frequency_recurrence_factor ]  
    [ , [ @active_start_date= ] active_start_date ]  
    [ , [ @active_end_date= ] active_end_date ]  
    [ , [ @active_start_time_of_day= ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day= ] active_end_time_of_day ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 要向其中添加筛选数据快照作业的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @suser_sname = ] 'suser_sname'` 为订阅创建筛选数据快照时使用的值，该订阅按订阅服务器上 [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) 函数的值进行筛选。 *suser_sname* **sysname**，无默认值。 如果不使用此函数对发布进行动态筛选， *suser_sname* 应为 NULL。  
  
`[ @host_name = ] 'host_name'` 为订阅创建筛选数据快照时使用的值，该订阅按订阅服务器上 [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) 函数的值进行筛选。 *host_name* **sysname**，无默认值。 如果不使用此函数对发布进行动态筛选， *host_name* 应为 NULL。  
  
`[ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname'` 创建的筛选数据快照作业的名称。 *dynamic_snapshot_jobname* 的默认值为 **sysname**，默认值为 NULL，是一个可选的 OUTPUT 参数。 如果已指定，则 *dynamic_snapshot_jobname* 必须在分发服务器上解析为唯一作业。 如果不指定，则将自动生成并在结果集中返回一个作业名，该名称的创建方式如下：  
  
```  
'dyn_' + <name of the standard snapshot job> + <GUID>  
```  
  
> [!NOTE]  
>  生成动态快照作业的名称时，您可以截断标准快照作业的名称。  
  
`[ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid'` 是已创建的筛选数据快照作业的标识符。 *dynamic_snapshot_jobid* 为 **uniqueidentifier**，默认值为 NULL，是一个可选的 OUTPUT 参数。  
  
`[ @frequency_type = ] frequency_type` 用于计划筛选的数据快照作业的频率。 *frequency_type* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次性|  
|**2**|按需|  
|**4** (默认值) |每天|  
|**8**|每周|  
|**16**|每月一次|  
|**32**|与“每月”选项相关|  
|**64**|自动启动|  
|**128**|重复执行|  
  
`[ @frequency_interval = ] frequency_interval` 执行筛选的数据快照作业)  (时间（以天计）。 *frequency_interval* 为 **int**，默认值为1，并且取决于 *frequency_type* 的值。  
  
|*Frequency_type* 的值|对 *frequency_interval* 的影响|  
|--------------------------------|-------------------------------------|  
|**1**|*frequency_interval* 未使用。|  
|**4** (默认值) |每 *frequency_interval* 天，默认为每天。|  
|**8**|*frequency_interval* 是以下 (中的一个或多个与 [&#124; &#40;按位或&#41; &#40;transact-sql&#41;](../../t-sql/language-elements/bitwise-or-transact-sql.md) 逻辑运算符组合在一起的：<br /><br /> **1** = 星期日 &#124; **2** = 星期一 &#124; **4** = 星期二 &#124; **8** = 星期三 &#124; **16** = 星期四 &#124; **32** = 星期五 &#124; **64** = 星期六|  
|**16**|*Frequency_interval* 月中的第几天。|  
|**32**|*frequency_interval* 是以下项之一：<br /><br /> **1** = 星期日 &#124; **2** = 星期一 &#124; **3** = 星期二 &#124; **4** = 星期三 &#124; **5** = 星期四 &#124; **6** = 星期五 &#124; **7** = 星期六 &#124; **8** = 日 &#124; **9** = 工作日 &#124; **10** = 周末|  
|**64**|*frequency_interval* 未使用。|  
|**128**|*frequency_interval* 未使用。|  
  
`[ @frequency_subday = ] frequency_subday` 指定 *frequency_subday_interval* 的单位。 *frequency_subday* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|一次|  
|**2**|秒|  
|**4** (默认值) |Minute|  
|**8**|小时|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` 作业的每次执行之间出现的 *frequency_subday* 周期数。 *frequency_subday_interval* 的值为 **int**，默认值为5。  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` 每月筛选的数据快照作业的发生次数。 如果 *frequency_type* 设置为 **32** (每月相对) ，则使用此参数。 *frequency_relative_interval* 为 **int**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1** （默认值）|First|  
|**2**|秒|  
|**4**|第三个|  
|**8**|第四个|  
|**16**|上一个|  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor`*Frequency_type* 使用的重复因子。 *frequency_recurrence_factor* 的值为 **int**，默认值为0。  
  
`[ @active_start_date = ] active_start_date` 第一次安排筛选数据快照作业的日期，格式为 YYYYMMDD。 *active_start_date* 的值为 **int**，默认值为 NULL。  
  
`[ @active_end_date = ] active_end_date` 停止安排筛选数据快照作业的日期，格式为 YYYYMMDD。 *active_end_date* 的值为 **int**，默认值为 NULL。  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` 第一次安排筛选数据快照作业的时间，格式为 HHMMSS。 *active_start_time_of_day* 的值为 **int**，默认值为 NULL。  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` 停止安排筛选数据快照作业的时间，格式为 HHMMSS。 *active_end_time_of_day* 的值为 **int**，默认值为 NULL。  
  
## <a name="result-set"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**id**|**int**|标识 [MSdynamicsnapshotjobs](../../relational-databases/system-tables/msdynamicsnapshotjobs-transact-sql.md) 系统表中的筛选数据快照作业。|  
|**dynamic_snapshot_jobname**|**sysname**|已筛选数据快照作业的名称。|  
|**dynamic_snapshot_jobid**|**uniqueidentifier**|唯一标识 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分发服务器上的代理作业。|  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_adddynamicsnapshot_job** 用于使用参数化筛选器的发布的合并复制。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_MergeDynamicPubPlusPartition](../../relational-databases/replication/codesnippet/tsql/sp-adddynamicsnapshot-jo_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_adddynamicsnapshot_job**。  
  
## <a name="see-also"></a>另请参阅  
 [为包含参数化筛选器的合并发布创建快照](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)   
 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [sp_dropdynamicsnapshot_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropdynamicsnapshot-job-transact-sql.md)   
 [sp_helpdynamicsnapshot_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpdynamicsnapshot-job-transact-sql.md)  
  
  
