---
description: CLR 事件类别
title: CLR 事件类别 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- event classes [SQL Server], CLR event category
- SQL Server event classes, CLR event category
- CLR event category [SQL Server]
ms.assetid: a7c0cd60-3bec-42be-ad5e-473bd26a06d9
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2b4d0d49cda095b57b4625f3d1308978d7ded3f2
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364693"
---
# <a name="clr-event-category"></a>CLR 事件类别
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  CLR 事件类别包括由于在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内执行 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 公共语言运行时 (CLR) 对象而生成的事件类。  
 
 ## <a name="assembly-load-event-class"></a>Assembly Load 事件类 
  执行加载程序集的请求时，会发生 **Assembly Load** 事件类。  
  
 **Assembly Load** 事件类包含在要监视的程序集加载的跟踪中。 当排除使用公共语言运行时 (CLR) 的查询故障时，当排除运行 CLR 查询的低速服务器故障时，或当监视服务器以收集用户、数据库、成功或有关程序集加载的其他信息时，这很有用。  
  
## <a name="assembly-load-event-class-data-columns"></a>Assembly Load 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|请求加载的应用程序的名称。|10|是|  
|**ClientProcessID**|**int**|主机为运行该客户端应用程序的进程分配的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|**DatabaseID**|**int**|由 USE 数据库语句指定的数据库的 ID；如果未对给定实例发出 USE 数据库语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据列而且服务器可用，则 **ServerName** 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|**DatabaseName**|**nvarchar**|正在其中运行用户语句的数据库的名称。|35|是|  
|**EventSequence**|**int**|给定事件在请求中的顺序。|51|否|  
|**GroupID**|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|**HostName**|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|**LoginName**|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 登录凭据，格式为“DOMAIN\username”）。|11|是|  
|**LoginSID**|**图像**|已登录的用户的安全标识符 (SID)。 你可以在 **sys.server_principals** 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|**NTDomainName**|**nvarchar**|用户所属的 Windows 域。|7|是|  
|**NTUserName**|**nvarchar**|Windows 用户名。|6|是|  
|**Exchange Spill**|**int**|程序集 ID。|22|是|  
|**ObjectName**|**nvarchar**|程序集的完全限定名称。|34|是|  
|**RequestID**|**int**|包含该语句的请求的 ID。|49|是|  
|**ServerName**|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|**SessionLoginName**|**nvarchar**|发起会话的用户的登录名。 例如，如果你使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 **SessionLoginName** 将显示 Login1，而 **LoginName** 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|**SPID**|**int**|发生该事件的会话的 ID。|12|是|  
|**StartTime**|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|**Success**|**int**|指示程序集加载是成功 (1) 还是失败 (0)。|23|是|  
|**TextData**|**ntext**|如果加载成功，将显示“程序集加载成功”；否则，将显示“程序集加载失败”。|1|是|  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)   
 [程序集（数据库引擎）](../../relational-databases/clr-integration/assemblies-database-engine.md)  

