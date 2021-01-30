---
description: MSsubscriber_schedule (Transact-SQL)
title: MSsubscriber_schedule (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsubscriber_schedule
- MSsubscriber_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscriber_schedule system table
ms.assetid: ff428306-0ef4-49a3-b536-07ccdf6e2196
author: cawrites
ms.author: chadam
ms.openlocfilehash: 13acf08fb86206c6127e62db465217920d22a927
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211610"
---
# <a name="mssubscriber_schedule-transact-sql"></a>MSsubscriber_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSsubscriber_schedule** 表包含每个发布服务器/订阅服务器对的默认合并和事务同步计划。 此表存储在分发数据库中。  
  
> [!NOTE]
>  已不推荐使用此系统表，正在维护此表以支持早期版本的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|发布服务器的名称。|  
|**订阅服务器**|**sysname**|订阅服务器的名称。|  
|**agent_type**|**smallint**|代理的类型：<br /><br /> 0 = 分发代理。<br /><br /> 1 = 合并代理。|  
|**frequency_type**|**int**|调度分发代理的频率：<br /><br /> **1** = 一次。<br /><br /> **2** = 按需。<br /><br /> **4** = 每天。<br /><br /> **8** = 每周。<br /><br /> **16** = 每月。<br /><br /> **32** = 每月相对的。<br /><br /> **64** = Autostart。<br /><br /> **128** = 重复。|  
|**frequency_interval**|**int**|要应用于 **frequency_type** 设置的频率的值。|  
|**frequency_relative_interval**|**int**|分发代理的日期：<br /><br /> **1** = 第一个。<br /><br /> **2** = 秒。<br /><br /> **4** = 第三个。<br /><br /> **8** = 第四个。<br /><br /> **16** = 最后。|  
|**frequency_recurrence_factor**|**int**|**Frequency_type** 使用的重复因子。|  
|**frequency_subday**|**int**|在定义的周期内的重新调度频率：<br /><br /> **1** = 一次。<br /><br /> **2** = 秒。<br /><br /> **4** = 分钟。<br /><br /> **8** = 小时。|  
|**frequency_subday_interval**|**int**|**Frequency_subday** 的间隔。|  
|**active_start_time_of_day**|**int**|首次调度分发代理的时间，格式为 HHMMSS。|  
|**active_end_time_of_day**|**int**|停止调度分发代理的时间，格式为 HHMMSS。|  
|**active_start_date**|**int**|首次调度分发代理的日期，格式为 YYYYMMDD。|  
|**active_end_date**|**int**|停止计划分发代理的日期，格式为 YYYYMMDD。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
