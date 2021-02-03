---
description: QN:Subscription 事件类
title: QN:Subscription 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- event classes [SQL Server], QN:Subscription
ms.assetid: 4916167e-8541-43b4-900e-ec8e6adcbc34
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1efb259b1cd895c3c7b7978bde78121d0e042d54
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212145"
---
# <a name="qnsubscription-event-class"></a>QN:Subscription 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  QN:Subscription 事件报告有关通知订阅的信息。  
  
## <a name="qnsubscription-event-class-data-columns"></a>QN:Subscription 事件类的数据列  
  
|数据列|类型|说明|列号|可筛选|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|ClientProcessID|**int**|由主机分配给正在运行客户端应用程序的进程的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|DatabaseID|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|DatabaseName|**nvarchar**|正在运行用户语句的数据库的名称。|35|是|  
|EventClass|**int**|事件类型 = 199。|27|否|  
|EventSequence|**int**|此事件的序列号。|51|否|  
|EventSubClass|**nvarchar**|事件子类的类型，提供有关每个事件类的进一步信息。 此列可能包含下列值：<br /><br /> **Subscription registered**：指示查询通知订阅在数据库中成功注册的时间。<br /><br /> **Subscription rewound**：指示 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 收到完全符合现有订阅的订阅请求的时间。 在这种情况下， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将现有订阅的超时值设置为新订阅请求中指定的超时值。<br /><br /> **Subscription fired**：指示通知订阅生成通知消息的时间。<br /><br /> **Firing failed with broker error**：指示通知消息因 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 错误而失败的时间。<br /><br /> **Firing failed without broker error**：指示通知消息因非 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 错误而失败的时间。<br /><br /> **Broker error intercepted**：指示 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 已在查询通知使用的会话中传递错误。<br /><br /> **Subscription deletion attempt**：指示 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 已尝试删除过期的订阅以释放资源。<br /><br /> **Subscription deletion failed**：指示尝试删除过期订阅失败。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将自动重新计划删除订阅以释放资源。<br /><br /> **Subscription destroyed**：指示 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 已成功删除过期的订阅。|21|是|  
|GroupID|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|HostName|**nvarchar**|正在运行客户端程序的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|IsSystem|**int**|指示事件是发生在系统进程中还是发生在用户进程中。<br /><br /> 0 = 用户<br /><br /> 1 = 系统|60|否|  
|LoginName|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录或 Windows 登录凭据，格式为域\用户名）。|11|否|  
|LoginSID|**图像**|已登录用户的安全标识号 (SID)。 您可以在 sys.server_principals 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|NTDomainName|**nvarchar**|用户所属的 Windows 域。|7|是|  
|NTUserName|**nvarchar**|拥有生成此事件的连接的用户的名称。|6|是|  
|RequestID|**int**|包含该语句的请求的标识符。|49|是|  
|ServerName|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果应用程序使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，并以 Login2 的身份执行语句，则 SessionLoginName 将显示“Login1”，而 LoginName 将显示“Login2”。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**ntext**|返回包含特定于此事件的信息的 XML 文档。 此文档符合 [SQL Server Query Notification Profiler Event Schema](https://go.microsoft.com/fwlink/?LinkId=63331) 页上提供的 XML 架构。|1|是|  
  
  
