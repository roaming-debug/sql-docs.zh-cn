---
description: Audit Fulltext 事件类
title: Audit Fulltext 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
ms.assetid: 95e4c5fd-e16f-446e-b42b-105495a8f39a
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 51cc9e52574085e6d751098bac6f545e88cac467
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126815"
---
# <a name="audit-fulltext-event-class"></a>Audit Fulltext 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接到全文筛选器后台程序进程并与之通信时，会出现 Audit Fulltext  事件类。  
  
## <a name="audit-fulltext-event-class-data-columns"></a>审核全文事件类 - 数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**错误**|**int**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误号（如果此事件报告了错误）。|31|是|  
|**EventSequence**|**int**|特定事件在请求中的顺序。|51|否|  
|**EventSubClass**|**int**|登录使用的连接类型。 1 = 非共用，2 = 共用。|21|是|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|**SessionLoginName**|**nvarchar**|发起会话的用户的登录名。 例如，如果你使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 **SessionLoginName** 将显示 Login1，而 **LoginName** 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|**SPID**|**int**|发生该事件的会话的 ID。|12|是|  
|**StartTime**|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|**Success**|**int**|1 = 成功。 0 = 失败。 例如，值为 1 时表示权限检查成功；值为 0 时表示权限检查失败。|23|是|  
|**TargetLoginName**|**int**|如果是针对登录的操作（例如，添加新的登录），则为所针对登录的名称。|42|是|  
|**TargetLoginSid**|**int**|如果是针对登录的操作（例如，添加新的登录），则为所针对登录的安全标识号 (SID)。|43|是|  
|**TextData**|**ntext**|有关全文事件的文本信息。 通常，此字段提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程和全文筛选器后台程序进程之间的连接的信息。|1|是|  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
