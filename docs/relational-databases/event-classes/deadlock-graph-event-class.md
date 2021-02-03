---
description: Deadlock Graph 事件类
title: Deadlock Graph 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Deadlock Graph event class
ms.assetid: 20f92233-c912-4382-8993-8e2e23d03fbe
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a104043847b5600ac3701919d3a545291b7118bf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182952"
---
# <a name="deadlock-graph-event-class"></a>Deadlock Graph 事件类
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  **Deadlock Graph** 事件类提供了有关死锁的 XML 描述。 该类与 **Lock:Deadlock** 事件类同时发生。  
  
## <a name="deadlock-graph-event-class-data-columns"></a>Deadlock Graph 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**EventClass**|**int**|事件类型 = 148。|27|否|  
|**EventSequence**|**int**|给定事件在请求中的顺序。|51|否|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。 对于此事件，该值始终为 1。|60|是|  
|**LoginName**|**nvarchar**|用户的登录名（ [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 登录凭据，格式为“DOMAIN\username”）。 对于此事件，此值始终为系统用户。|11|是|  
|**LoginSid**|**图像**|登录用户的安全标识号 (SID)。 您可以在 sys.server_principals 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。 对于此事件，此值始终为系统用户的 SID。|41|是|  
|**ServerName**|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|**SessionLoginName**|**nvarchar**|发起会话的用户的登录名。 例如，如果你使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 **SessionLoginName** 将显示 Login1，而 **LoginName** 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|**SPID**|**int**|发生该事件的会话的 ID。|12|是|  
|**StartTime**|**datetime**|检测到死锁的时间。|14|是|  
|**TextData**|**ntext**|死锁的 XML 描述。|1|是|  
|**TransactionID**|**bigint**|未使用。|4|是|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Lock:Deadlock 事件类](../../relational-databases/event-classes/lock-deadlock-event-class.md)  
  
  
