---
description: PreConnect:Completed 事件类
title: PreConnect:Completed 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- PreConnect:Completed Event Class
ms.assetid: 7ed2f620-6511-4985-9961-d2927c2b1759
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b432f36c72b2118c6fd6a04e7f56a9a1d40ff24b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180846"
---
# <a name="preconnectcompleted-event-class"></a>PreConnect:Completed 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  PreConnect:Completed 事件类指示何时 LOGON 触发器或资源调控器分类器函数结束执行。  
  
## <a name="preconnectcompleted-event-class-data-columns"></a>PreConnect:Completed 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|EventClass|**int**|216|27|否|  
|SPID|**int**|激发此事件的服务器进程的 ID。|12|是|  
|EventSubClass|**int**|1 表示用户定义的分类器函数。|21|是|  
|StartTime|**datetime**|用户定义的分类器函数开始时间。|14|是|  
|EndTime|**datetime**|用户定义的分类器函数开始时间。|15|是|  
|Duration|**bigint**|分类器函数所用的时间，以微秒为单位。|13|是|  
|ObjectID|**int**|用户定义的分类器对象的 ID。|22|是|  
|CPU|**int**|CPU 使用率，以毫秒为单位。|18|是|  
|读取|**int**|逻辑读取数。|16|是|  
|写入|**int**|逻辑写入数。|17|是|  
|GroupID|**int**|分类工作负荷组的 ID。|66|是|  
|错误|**int**|如果用户定义的分类器函数无法执行，则为最后一个错误号。|31|是|  
|状态|**int**|最后一个错误的状态。|30|是|  
|TargetUserName|**sysname**|如果系统无法找到相应的活动组，则为用户定义的分类器函数的返回值（工作负荷组名）。 否则，此列设为 NULL.|39|是|  
|ObjectName|**nvarchar(256)**|用户定义的分类器函数的两部分名称。 例如，dbo.classifier。|34|是|  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)   
 [PreConnect:Starting 事件类](../../relational-databases/event-classes/preconnect-starting-event-class.md)   
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)  
  
  
