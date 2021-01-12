---
description: dbo.cdc_jobs (Transact-SQL)
title: dbo.cdc_jobs (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- cdc_jobs
- cdc_jobs_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dbo.cdc_jobs
ms.assetid: 85e2d580-1c54-4b81-b7e6-2e12997199fd
author: cawrites
ms.author: chadam
ms.openlocfilehash: c100dfdaad26065946ccba76907ceb0ac6069d2c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094882"
---
# <a name="dbocdc_jobs-transact-sql"></a>dbo.cdc_jobs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  存储用于捕获和清除作业的变更数据捕获配置参数。 此表存储在 **msdb** 中。  
  
 
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**int**|特定数据库的 ID，作业在此数据库中运行。|  
|**job_type**|**nvarchar (20)**|作业的类型，为“capture”或“cleanup”。|  
|**job_id**|**uniqueidentifier**|与作业关联的唯一 ID。|  
|**maxtrans**|**int**|在每个扫描循环中要处理的最大事务数。<br /><br /> **maxtrans** 仅对捕获作业有效。|  
|**maxscans**|**int**|为了从日志中提取所有行而要执行的扫描周期的最大数目。<br /><br /> **maxscans** 仅对捕获作业有效。|  
|**持续**|**bit**|指示捕获作业是连续运行 (1) 还是以一次性模式运行 (0) 的标志。 有关详细信息，请参阅 [&#40;transact-sql&#41;sys.sp_cdc_add_job ](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)。<br /><br /> **连续** 仅对捕获作业有效。|  
|**pollinginterval**|**bigint**|日志扫描循环之间间隔的秒数。<br /><br /> **pollinginterval** 仅对捕获作业有效。|  
|**保留**|**bigint**|更改行要在更改表中保留的分钟数。<br /><br /> **保留** 仅对清除作业有效。|  
|**threshold**|**bigint**|清除时可以使用一条语句删除的删除条目的最大数量。|  
  
## <a name="see-also"></a>另请参阅  
 [sys.sp_cdc_add_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)   
 [sys.sp_cdc_change_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-change-job-transact-sql.md)   
 [sys.sp_cdc_help_jobs &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md)   
 [sys.sp_cdc_drop_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-drop-job-transact-sql.md)  
  
  
