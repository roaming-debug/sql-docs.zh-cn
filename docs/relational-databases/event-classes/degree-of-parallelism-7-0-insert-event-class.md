---
description: Degree of Parallelism (7.0 Insert) 事件类
title: Degree of Parallelism (7.0 Insert) 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Degree of Parallelism event class
ms.assetid: 6753ef30-890f-47a3-b0b6-8abb184e1d83
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 97800bdd8137d20fabec298bce235bd2a4232b79
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122743"
---
# <a name="degree-of-parallelism-70-insert-event-class"></a>Degree of Parallelism (7.0 Insert) 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  每次 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 执行 SELECT、INSERT、UPDATE 或 DELETE 语句时都会发生 Degree of Parallelism (7.0 Insert)  事件类。  
  
 当该事件包含在跟踪中时，如果经常发生这些事件则所引起的开销可能会明显影响性能。 若要最大限度地降低引起的开销，请仅将此事件类用于在短时间内监视特定问题的跟踪操作。  
  
## <a name="degree-of-parallelism-70-insert-event-class-data-columns"></a>Degree of Parallelism (7.0 Insert) 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|**BinaryData**|**图像**|用于根据以下值完成进程的 CPU 数量：<br /><br /> 0x00000000，指示以串行方式运行的串行计划。<br /><br /> 0x01000000，指示以串行方式运行的并行计划。<br /><br /> >= 0x02000000 指示以并行方式运行的并行计划。|2|否|  
|**ClientProcessID**|**int**|主机为运行该客户端应用程序的进程分配的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|**DatabaseID**|**int**|由 USE 数据库语句指定的数据库的 ID；如果未对给定实例发出 USE 数据库语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据列而且服务器可用，则 **ServerName** 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|**DatabaseName**|**nvarchar**|正在其中运行用户语句的数据库的名称。|35|是|  
|**Event Class**|**int**|事件类型 = 28。|27|否|  
|**EventSequence**|**int**|给定事件在请求中的顺序。|51|否|  
|**EventSubClass**|**int**|指示根据以下值执行的语句：<br /><br /> 1 = 选择<br /><br /> 2 = 插入<br /><br /> 3 = 更新<br /><br /> 4 = 删除|21|否|  
|**GroupID**|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|**HostName**|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|**Integer Data**|**int**|授予查询用来执行操作的“工作区内存”总量 (KB)，这些操作涉及到哈希、排序或创建索引操作。 在执行期间将根据需要获取内存。|25|是|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|**LoginName**|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 Windows 登录凭据，格式为 “DOMAIN\\*username*”）。|11|是|  
|**LoginSid**|**图像**|登录用户的安全标识号 (SID)。 您可以在 sys.server_principals 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|**NTDomainName**|**nvarchar**|用户所属的 Windows 域。|7|是|  
|**NTUserName**|**nvarchar**|Windows 用户名。|6|是|  
|**RequestID**|**int**|启动全文查询的请求标识。|49|是|  
|**ServerName**|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|**SessionLoginName**|**nvarchar**|发起会话的用户的登录名。 例如，如果你使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 **SessionLoginName** 将显示 Login1，而 **LoginName** 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|**SPID**|**int**|发生该事件的会话的 ID。|12|是|  
|**StartTime**|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|**TransactionID**|**bigint**|系统分配的事务 ID。|4|是|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)  
  
  
