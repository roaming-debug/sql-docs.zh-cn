---
description: PreConnect:Starting 事件类
title: PreConnect:Starting 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- PreConnect:Starting Event Class
ms.assetid: d43ed0ad-3dbd-42e0-9cef-8320b8d87497
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b5f452391bb263054018bbff5f615e35866b7fd6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181376"
---
# <a name="preconnectstarting-event-class"></a>PreConnect:Starting 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  PreConnect:Starting事件类指示 LOGON 触发器或资源调控器分类器函数开始执行的时间。  
  
## <a name="preconnectstarting-event-class-data-columns"></a>PreConnect:Starting 事件类数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|EventClass|**int**|215|27|否|  
|SPID|**int**|激发此事件的服务器进程的 ID。|12|是|  
|EventSubClass|**int**|1 表示用户定义的分类器函数。|21|是|  
|StartTime|**datetime**|用户定义的分类器函数开始时间。|14|是|  
|ObjectID|**int**|用户定义的分类器对象的 ID。|22|是|  
|ObjectName|**nvarchar(256)**|用户定义的分类器函数的两部分名称。 例如，dbo.classifier。|34|是|  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)   
 [PreConnect:Completed 事件类](../../relational-databases/event-classes/preconnect-completed-event-class.md)   
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)  
  
  
