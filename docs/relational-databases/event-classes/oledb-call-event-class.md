---
description: OLEDB Call 事件类
title: OLEDB Call 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- OLEDB Call event class
ms.assetid: e1be1e90-98cc-47a3-addd-59d4aeca6547
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a4722cfc417b3c0b269d50b1c6673846e86d70fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190215"
---
# <a name="oledb-call-event-class"></a>OLEDB Call 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  **为分布式查询和远程存储过程调用 OLE DB 访问接口时，会发生** OLEDB Call [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件类。  
  
 将 **OLEDB Call** 事件类包括在跟踪中可以只监视那些不请求数据的调用或未对 **QueryInterface** 方法进行的调用。 跟踪中包括 **OLEDB Call** 事件类时，引发的开销量取决于跟踪过程中针对数据库调用 OLE DB 的频率。 如果调用频繁发生，则跟踪可能会显著地降低性能。  
  
## <a name="oledb-call-event-class-data-columns"></a>OLEDB Call 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|ClientProcessID|**Int**|主机为运行该客户端应用程序的进程分配的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|DatabaseID|**Int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据列而且服务器可用，则 **ServerName** 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|DatabaseName|**nvarchar**|正在其中运行用户语句的数据库的名称。|35|是|  
|Duration|**Bigint**|完成 OLE DB 调用事件所需的时间。|13|否|  
|EndTime|**日期时间**|事件的结束时间。|15|是|  
|错误|**int**|给定事件的错误号。 通常是 sys.messages 目录视图中存储的错误号。|31|是|  
|EventClass|**Int**|事件类型 = 119。|27|否|  
|EventSequence|**Int**|OLE DB 事件类在批处理中的顺序。|51|否|  
|EventSubClass|**Int**|0 = 正在启动<br /><br /> 1 = 已完成|21|否|  
|GroupID|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|HostName|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|IsSystem|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|LinkedServerName|**nvarchar**|链接服务器的名称。|45|是|  
|LoginName|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 登录凭据，格式为“DOMAIN\username”）。|11|是|  
|LoginSid|**图像**|已登录的用户的安全标识符 (SID)。 您可以在 sys.server_principals 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|MethodName|**nvarchar**|OLE DB 方法的名称。|47|是|  
|NTDomainName|**nvarchar**|用户所属的 Windows 域。|7|是|  
|NTUserName|**nvarchar**|Windows 用户名。|6|是|  
|ProviderName|**nvarchar**|OLE DB 访问接口的名称。|46|是|  
|RequestID|**Int**|包含该语句的请求的 ID。|49|是|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果你使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 **SessionLoginName** 将显示 Login1，而 **LoginName** 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|SPID|**Int**|发生该事件的会话的 ID。|12|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**nvarchar**|在 OLE DB 调用中发送和接收的参数。|1|否|  
|TransactionID|**bigint**|系统分配的事务 ID。|4|是|  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Transact-SQL 中的 OLE 自动化对象](../../relational-databases/stored-procedures/ole-automation-objects-in-transact-sql.md)  
  
  
