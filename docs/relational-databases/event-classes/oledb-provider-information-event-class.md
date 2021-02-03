---
description: OLEDB Provider Information 事件类
title: OLEDB Provider Information 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- OLEDB Provider Information event class
ms.assetid: a0316c4e-4b8c-4754-8a35-222f3c0907d1
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9b93a45664a05c7e64d09ef3c04932026f49804e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190877"
---
# <a name="oledb-provider-information-event-class"></a>OLEDB Provider Information 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  当某个分布式查询运行和收集与访问接口连接对应的信息时，会发生 **OLEDB Provider Information** 事件类。  
  
 此事件类包含使用各种属性集从远程访问接口收集的所有属性，包括：  
  
-   DBPROPSET_DATASOURCEINFO  
  
-   SQLPROPSET_OPTHINTS  
  
-   DBPROPSET_SQLSERVERDATASOURCEINFO（仅限[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ）  
  
-   DBPROPSET_SQLSERVERDBINIT（仅限[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ）  
  
-   DBPROPSET_ROWSET  
  
-   IDBInfo 接口  
  
 查询优化器将使用这些属性以及可用的元数据选择最佳查询执行计划。 此信息对于跟踪执行以及分析 OLE DB 调用和分布式查询事件探查器跟踪中的事件十分有用。  
  
## <a name="oledb-provider-information-event-class-data-columns"></a>OLEDB Provider Information 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|**ClientProcessID**|**int**|主机为运行该客户端应用程序的进程分配的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|**DatabaseID**|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据列而且服务器可用，则 **ServerName** 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|**DatabaseName**|**nvarchar**|正在其中运行用户语句的数据库的名称。|35|是|  
|**EventClass**|**int**|事件类型 = 194。|27|否|  
|**EventSequence**|**int**|给定事件在请求中的顺序。|51|否|  
|**GroupID**|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|**HostName**|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|**LinkedServerName**|**nvarchar**|链接服务器的名称。|45|是|  
|**LoginName**|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 Windows 登录凭据，格式为“域\用户名”）。|11|是|  
|**LoginSid**|**图像**|已登录的用户的安全标识符 (SID)。 你可以在 **sys.server_principals** 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|**NTDomainName**|**nvarchar**|用户所属的 Windows 域。|7|是|  
|**NTUserName**|**nvarchar**|Windows 用户名。|6|是|  
|**ProviderName**|**nvarchar**|OLE DB 访问接口的名称。|46|是|  
|**RequestID**|**int**|包含该语句的请求的 ID。|49|是|  
|**SessionLoginName**|**nvarchar**|发起会话的用户的登录名。 例如，如果你使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 **SessionLoginName** 将显示 Login1，而 **LoginName** 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|**SPID**|**int**|发生该事件的会话的 ID。|12|是|  
|**StartTime**|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|**TextData**|**ntext**|依赖于跟踪中捕获的事件类的文本值。|1|是|  
|**TransactionID**|**bigint**|系统分配的事务 ID。|4|是|  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Transact-SQL 中的 OLE 自动化对象](../../relational-databases/stored-procedures/ole-automation-objects-in-transact-sql.md)  
  
  
