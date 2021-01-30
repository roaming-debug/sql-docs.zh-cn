---
description: dbo.sysjobsteps (Transact-SQL)
title: dbo.sysjobsteps (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysjobsteps
- dbo.sysjobsteps_TSQL
- sysjobsteps
- sysjobsteps_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobsteps system table
ms.assetid: 978b8205-535b-461c-91f3-af9b08eca467
author: cawrites
ms.author: chadam
ms.openlocfilehash: d4e06b76f069086a6223ba02969c15e5cc1c7908
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202315"
---
# <a name="dbosysjobsteps-transact-sql"></a>dbo.sysjobsteps (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理要执行的作业中的各个步骤的信息。 该表存储在 **msdb** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|作业的 ID。|  
|**step_id**|**int**|作业中步骤的 ID。|  
|**step_name**|**sysname**|作业步骤的名称。|  
|**适用**|**nvarchar(40)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理用于执行作业步骤的子系统的名称。|  
|**command**|**nvarchar(max)**|要由 **子系统** 执行的命令。|  
|**flag**|**int**|保留。|  
|**additional_parameters**|**ntext**|保留。|  
|**cmdexec_success_code**|**int**|**CmdExec** 子系统步骤返回的错误级别值指示成功。|  
|**on_success_action**|**tinyint**|成功执行了某个步骤时将要执行的操作。<br /><br /> **1** = (默认值) 成功退出<br /><br /> **2** = 失败时退出<br /><br /> **3** = 中转到下一步<br /><br /> **4** = 中转到步骤 _on_success_step_id_|
|**on_success_step_id**|**int**|成功执行了某个步骤时将要执行的下一个步骤的 ID。|  
|**on_fail_action**|**tinyint**|未成功执行某个步骤时将要执行的操作。<br /><br /> **1** = 成功退出<br /><br /> **2** = (默认) 退出但失败<br /><br /> **3** = 中转到下一步<br /><br /> **4** = 中转到步骤 _on_fail_step_id_|
|**on_fail_step_id**|**int**|未成功执行某个步骤时将要执行的下一个步骤的 ID。|  
|服务器|**sysname**|保留。|  
|**database_name**|**sysname**|如果 **子系统** 为 TSQL，则在其中执行 **命令** 的数据库的名称。|  
|**database_user_name**|**sysname**|执行该步骤时使用的帐户所属的数据库用户的名称。|  
|**retry_attempts**|**int**|步骤失败时的重试次数。|  
|**retry_interval**|**int**|每次重试间的等待时间。|  
|**os_run_priority**|**int**|保留。|  
|**output_file_name**|**nvarchar(200)**|**子系统** 为 TSQL、PowerShell 或 **CmdExec** 时，在其中保存步骤输出的文件的名称 _。_|  
|**last_run_outcome**|**int**|前一次执行作业步骤的结果。<br /><br /> **0** = 失败<br /><br /> **1** = 成功<br /><br /> **2** = 重试<br /><br /> **3** = 已取消<br /><br /> **5** = 未知|  
|**last_run_duration**|**int**|该步骤上次运行时的持续时间 (hhmmss)。|  
|**last_run_retries**|**int**|上一次执行作业步骤时的重试次数。|  
|**last_run_date**|**int**|上次开始执行该步骤的日期 (yyyymmdd)。|  
|**last_run_time**|**int**|上次开始执行该步骤的时间 (hhmmss)。|  
|**proxy_id**|**int**|作业步骤的代理。|  
|**step_uid**|**uniqueidentifier**|作业步骤的标识符。|  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 SQL Server 代理表 ](../../relational-databases/system-tables/sql-server-agent-tables-transact-sql.md)  
  
  
