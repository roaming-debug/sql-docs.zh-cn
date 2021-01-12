---
description: dbo.sysjobservers (Transact-SQL)
title: dbo.sysjobservers (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/26/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysjobservers
- sysjobservers_TSQL
- dbo.sysjobservers
- dbo.sysjobservers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysjobservers system table
ms.assetid: 9abcc20f-a421-4591-affb-62674d04575e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7ac1d8facbabb42be31a9d45e091e1106c25e0c9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097408"
---
# <a name="dbosysjobservers-transact-sql"></a>dbo.sysjobservers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

存储特定作业与一个或多个目标服务器的关联或关系。 该表存储在 msdb 数据库中。
  
|列名称|数据类型|描述|  
|-----------------|---------------|-----------------|  
|job_id|**uniqueidentifier**|作业标识号。|  
|server_id|**int**|服务器标识号。|  
|last_run_outcome|**tinyint**|作业上次运行的结果：<br /><br /> **0** = 失败<br /><br /> **1** = 成功<br /><br /> **2** = 重试<br /><br /> **3** = 取消<br /><br /> **4** = 正在进行<br /><br /> **5** = 未知 (请参阅下面的 "备注" 部分)  |  
|last_outcome_message|**nvarchar(1024)**|与 last_run_outcome 列关联的消息（如果有）。|  
|last_run_date|**int**|上次运行作业的日期。|  
|last_run_time|**int**|上次运行作业的时间。|  
|last_run_duration|**int**|作业运行的持续时间，以小时、分钟和秒为单位。 使用公式计算： (*小时* \* 10000) + (*分钟* \* 100) +*秒*。|  


## <a name="remarks"></a>备注

大于 *4* 的值表示 SQL 代理不知道该作业的状态。 创建作业时， *last_run_outcome* 最初设置为 *5* 。


## <a name="see-also"></a>另请参阅

[&#40;Transact-sql&#41;的 SQL Server 代理表 ](../../relational-databases/system-tables/sql-server-agent-tables-transact-sql.md)  
