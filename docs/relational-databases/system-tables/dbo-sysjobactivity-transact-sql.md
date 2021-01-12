---
description: dbo.sysjobactivity (Transact-SQL)
title: dbo.sysjobactivity (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysjobactivity_TSQL
- dbo.sysjobactivity
- sysjobactivity
- sysjobactivity_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobactivity system table
ms.assetid: fd17cac9-5d1f-4b44-b2dc-ee9346d8bf1e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 091f3052a568fa5c1c0e7f521333c67947716cdf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098352"
---
# <a name="dbosysjobactivity-transact-sql"></a>dbo.sysjobactivity (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  记录当前的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业活动和状态。  该表存储在 **msdb** 数据库中。
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|存储在 **msdb** 数据库的 **syssessions** 表中的会话的 ID。|  
|**job_id**|**uniqueidentifier**|作业的 ID。|  
|**run_requested_date**|**datetime**|请求运行作业的日期和时间。|  
|**run_requested_source**|**sysname(nvarchar(128))**|请求运行作业的请求者。<br /><br /> **1** = SOURCE_SCHEDULER<br /><br /> **2** = SOURCE_ALERTER<br /><br /> **3** = SOURCE_BOOT<br /><br /> **4** = SOURCE_USER<br /><br /> **6** = SOURCE_ON_IDLE_SCHEDULE|  
|**queued_date**|**datetime**|该作业排队的日期和时间。 如果直接运行该作业，则此列为 NULL。|  
|**start_execution_date**|**datetime**|计划运行作业的日期和时间。|  
|**last_executed_step_id**|**int**|上一个运行的作业步骤的 ID。|  
|**last_executed_step_**<br /><br /> **date**|**datetime**|上一个作业步骤开始运行的日期和时间。|  
|**stop_execution_date**|**datetime**|作业完成运行的日期和时间。|  
|**job_history_id**|**int**|用于标识 **sysjobhistory** 表中的行。|  
|**next_scheduled_run_date**|**datetime**|计划运行作业的下一日期和时间。|  

## <a name="example"></a>示例
此示例将返回所有 SQL Server 代理作业的运行时状态。  在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中执行以下 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。
```sql
SELECT sj.Name, 
    CASE
        WHEN sja.start_execution_date IS NULL THEN 'Not running'
        WHEN sja.start_execution_date IS NOT NULL AND sja.stop_execution_date IS NULL THEN 'Running'
        WHEN sja.start_execution_date IS NOT NULL AND sja.stop_execution_date IS NOT NULL THEN 'Not running'
    END AS 'RunStatus'
FROM msdb.dbo.sysjobs sj
JOIN msdb.dbo.sysjobactivity sja
ON sj.job_id = sja.job_id
WHERE session_id = (
    SELECT MAX(session_id) FROM msdb.dbo.sysjobactivity); 
```
  
## <a name="see-also"></a>另请参阅  
 [dbo.sysjobhistory &#40;Transact-sql&#41;](../../relational-databases/system-tables/dbo-sysjobhistory-transact-sql.md)  
  
  
