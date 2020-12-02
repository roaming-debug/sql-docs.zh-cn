---
description: Auto Stats 事件类
title: Auto Stats 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Auto Stats event class
ms.assetid: cd613fce-01e1-4d8f-86cc-7ffbf0759f9e
author: stevestein
ms.author: sstein
ms.openlocfilehash: f9e20179799dbc61dae0a1e02247fe5eb0df2628
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126719"
---
# <a name="auto-stats-event-class"></a>Auto Stats 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Auto Stats 事件类指示索引和列统计信息自动更新事件的发生。  优化器加载使用统计数据时，也会触发 Auto Stats。
  
## <a name="auto-stats-event-class-data-columns"></a>Auto Stats 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|**ClientProcessID**|**int**|主机为运行该客户端应用程序的进程分配的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|**DatabaseID**|**int**|由 USE *database* 语句指定的数据库的标识符；如果未对给定实例发出 USE *database* 语句，则为默认数据库的标识符。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据列而且服务器可用，则 **ServerName** 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|**DatabaseName**|**nvarchar**|正在其中运行用户语句的数据库的名称。|35|是|  
|**Duration**|**bigint**|事件占用的时间（微秒）。|13|是|  
|**EndTime**|**datetime**|事件结束的时间。|15|是|  
|**错误**|**int**|给定事件的错误号。 通常是 **sys.messages** 目录视图中存储的错误号。|31|是|  
|**EventClass**|**int**|事件类型 = 58。|27|否|  
|**EventSequence**|**int**|给定事件在请求中的顺序。|51|否|  
|**EventSubClass**|**int**|事件子类的类型：<br /><br /> 1: 同步创建/更新的统计信息； **TextData** 列指示创建或更新的统计信息，以及哪些统计信息是创建的，哪些是更新的<br /><br /> 2: 异步统计信息更新；作业已排队。<br /><br /> 3: 异步统计信息更新；作业已开始。<br /><br /> 4: 异步统计信息更新；作业已完成。|21|是|  
|**GroupID**|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|**HostName**|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|**IndexID**|**int**|受事件影响的对象上的索引/统计信息项 ID。 若要确定对象的索引 ID，请使用 **sys.indexes** 目录视图的 **index_id** 列。|24|是|  
|**IntegerData**|**int**|成功更新的统计信息集合数。|25|是|  
|**IntegerData2**|**int**|作业序列号。|55|是|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|**LoginName**|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 Windows 登录凭据，格式为“域\用户名”）。|11|是|  
|**LoginSid**|**图像**|登录用户的安全标识号 (SID)。 你可以在 **sys.server_principals** 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|**NTDomainName**|**nvarchar**|用户所属的 Windows 域。|7|是|  
|**NTUserName**|**nvarchar**|Windows 用户名。|6|是|  
|**Exchange Spill**|**int**|系统分配的对象 ID。|22|是|  
|**RequestID**|**int**|包含该语句的请求的 ID。|49|是|  
|**ServerName**|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|**SessionLoginName**|**nvarchar**|发起会话的用户的登录名。 例如，如果你使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 **SessionLoginName** 将显示 Login1，而 **LoginName** 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|**SPID**|**int**|发生该事件的会话的 ID。|12|是|  
|**StartTime**|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|**Success**|**int**|0 = 错误。<br /><br /> 1 = 成功。<br /><br /> 2 = 因服务器中止而跳过 (MSDE)。|23|是|  
|**TextData**|**ntext**|此列的内容取决于统计信息是同步更新的 (**EventSubClass** 1) 还是异步更新的（**EventSubClass** 2、3 或 4）：<br /><br /> 1: 列出更新/创建的统计信息<br /><br /> 2、3 或 4：NULL；用已更新的统计信息的索引/统计信息 ID 填充 **IndexID** 列。|1|是|  
|**TransactionID**|**bigint**|系统分配的事务 ID。|4|是|  
|**类型**|**int**|作业类型。|57|是|  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
