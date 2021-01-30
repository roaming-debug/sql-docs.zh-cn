---
description: dbo.sysschedules (Transact-SQL)
title: " (Transact-sql) dbo.sys计划 |Microsoft Docs"
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysschedules_TSQL
- sysschedules
- sysschedules_TSQL
- dbo.sysschedules
dev_langs:
- TSQL
helpviewer_keywords:
- sysschedules system table
ms.assetid: 4cac9237-7a69-4035-bb3e-928b76aad698
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5d72d6b837b867c242a1755648d7a90aee2558fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165211"
---
# <a name="dbosysschedules-transact-sql"></a>dbo.sysschedules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  包含有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业计划的信息。 该表存储在 **msdb** 数据库中。  
  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业计划 ID。|  
|**schedule_uid**|**uniqueidentifier**|作业计划的唯一标识符。 此值用于标识分布式作业的计划。|  
|**originating_server_id**|**int**|作为作业计划来源的主服务器 ID。|  
|name |**sysname (nvarchar (128) # B3**|作业计划的用户定义名称。 该名称在作业中必须唯一。|  
|**owner_sid**|**varbinary(85)**|拥有作业计划的用户或组的 Microsoft Windows *security_identifier* 。|  
|**enabled**|**int**|作业计划的状态：<br /><br /> **0** = 未启用。<br /><br /> **1** = 已启用。<br /><br /> 如果未启用计划，则不会运行该计划中的任何作业。|  
|**freq_type**|**int**|此计划中作业运行的频率。<br /><br /> **1** = 仅限一次<br /><br /> **4** = 每天<br /><br /> **8** = 每周<br /><br /> **16** = 每月<br /><br /> **32** = 每月，相对于 **freq_interval**<br /><br /> **64** = 在 SQL Server 代理服务启动时运行<br /><br /> **128** = 在计算机空闲时运行|  
|**freq_interval**|**int**|执行作业的间隔天数。 取决于 **freq_type** 的值。 默认值为 **0**，指示不使用 **freq_interval** 。 请参阅下表，了解可能的值及其影响。|  
|**freq_subday_type**|**int**|**Freq_subday_interval** 的单位。 下面是可能的值及其说明。<br /><br /> <br /><br /> **1** ：在指定时间<br /><br /> **2** ：秒<br /><br /> **4** ：分钟<br /><br /> **8** ：小时|  
|**freq_subday_interval**|**int**|每次执行作业之间要发生的 **freq_subday_type** 周期数。|  
|**freq_relative_interval**|**int**|如果每个月出现 **freq_interval** ， **freq_type** 为 **32** (月相对) 。 可以是以下其中一个值：<br /><br /> **0**  = **freq_relative_interval** 未使用<br /><br /> **1** = 第一<br /><br /> **2** = 秒<br /><br /> **4** = 第三<br /><br /> **8** = 第四<br /><br /> **16** = 最后|  
|**freq_recurrence_**<br /><br /> **一元**|**int**|在计划的作业执行之间间隔的周数或月数。 仅当 **freq_type** 为 **8**、 **16** 或 **32** 时才使用 **freq_recurrence_factor** 。 如果此列包含 **0**，则不使用 **freq_recurrence_factor** 。|  
|**active_start_date**|**int**|可以开始执行作业的日期。 日期的格式为 YYYYMMDD。 NULL 表示当天的日期。|  
|**active_end_date**|**int**|可以停止执行作业的日期。 日期格式为 YYYYMMDD。|  
|**active_start_time**|**int**|**Active_start_date** 和作业开始执行 **active_end_date** 之间的任何日期。 时间格式为 HHMMSS，采用 24 小时制。|  
|**active_end_time**|**int**|**Active_start_date** 和作业停止执行 **active_end_date** 之间的任何日期。 时间格式为 HHMMSS，采用 24 小时制。|  
|**date_created**|**datetime**|创建计划的日期和时间。|  
|**date_modified**|**datetime**|上次修改计划的日期和时间。|  
|**version_number**|**int**|计划的当前版本号。 例如，如果某个计划已修改10次，则 **version_number** 为10。|  
  
|freq_type 的值|对 freq_interval 的影响|  
|-------------------------|------------------------------|  
|**1** (一次) |**freq_interval** 未使用 (**0**) |  
|**4** (每日) |每 **freq_interval** 天|  
|**8** (每周) |**freq_interval** 是下列一项或多项操作：<br /><br /> **1** = 星期日<br /><br /> **2** = 星期一<br /><br /> **4** = 星期二<br /><br /> **8** = 星期三<br /><br /> **16** = 星期四<br /><br /> **32** = 星期五<br /><br /> **64** = 星期六|  
|每月 **16** () |在月份的 **freq_interval** 日期|  
|**32** (每月，相对) |**freq_interval** 是以下项之一：<br /><br /> **1** = 星期日<br /><br /> **2** = 星期一<br /><br /> **3** = 星期二<br /><br /> **4** = 星期三<br /><br /> **5** = 星期四<br /><br /> **6** = 星期五<br /><br /> **7** = 星期六<br /><br /> **8** = 日<br /><br /> **9** = 工作日<br /><br /> **10** = 周末|  
|**64** 启动 SQL Server 代理服务时， (启动) |**freq_interval** 未使用 (**0**) |  
|当计算机处于空闲状态时， **128** (运行) |**freq_interval** 未使用 (**0**) |  
  
## <a name="see-also"></a>请参阅  
 [dbo.sysjobschedules &#40;Transact-sql&#41;](../../relational-databases/system-tables/dbo-sysjobschedules-transact-sql.md)  
  
  
