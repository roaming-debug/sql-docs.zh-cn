---
title: Lock:Timeout 事件类 | Microsoft Docs
description: Lock:Timeout 事件类指示由于 SQL Server 中的资源存在阻塞锁而使对资源的锁的请求超时。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Timeout event class
ms.assetid: 8492f4be-4ea9-4059-80e0-9e7b71597da9
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ec230556d518a6f84b7e0ad6266a68c383921a5f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211995"
---
# <a name="locktimeout-event-class"></a>Lock:Timeout 事件类
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Lock:Timeout 事件类指示由于其他事务持有所需资源的阻塞锁而使对资源（例如页）锁的请求超时。 超时由 @@LOCK_TIMEOUT 系统函数确定，并可用 SET LOCK_TIMEOUT 语句设置。  
  
 超时情况出现时，使用 Lock:Timeout 事件类进行监视。 此信息有助于确定超时是否对应用程序的性能造成重大影响，以及涉及哪些对象。 您可以检查修改这些对象的应用程序代码，以确定是否可以进行更改以将超时减到最小。  
  
 持续时间为 0 的 Lock:Timeout 事件通常是内部锁探测的结果，并不表示存在问题。 Lock:Timeout (timeout > 0) 事件可用来忽略持续时间为 0 的超时。  
  
## <a name="locktimeout-event-class-data-columns"></a>Lock:Timeout 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|BinaryData|**图像**|锁资源标识符。|2|是|  
|ClientProcessID|**int**|主机为运行该客户端应用程序的进程分配的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|DatabaseID|**int**|发生锁超时的数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 如果在跟踪中捕获 ServerName 数据列而且服务器可用，则将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|DatabaseName|**nvarchar**|发生超时的数据库的名称。|35|是|  
|Duration|**bigint**|从发出锁请求到锁超时之间的时间长度（微秒）。|13|是|  
|EndTime|**datetime**|事件结束的时间。|15|是|  
|EventClass|**int**|事件类型 = 27。|27|否|  
|EventSequence|**int**|特定事件在请求中的顺序。|51|否|  
|GroupID|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|HostName|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|IntegerData2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|55|是|  
|IsSystem|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|LoginName|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 登录凭据，格式为“DOMAIN\username”）。|11|是|  
|LoginSid|**图像**|登录用户的安全标识号 (SID)。 您可以在 sys.server_principals 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|“模式”|**int**|超时后导致的模式。<br /><br /> 0=NULL - 与所有其他锁定模式兼容 (LCK_M_NL)<br /><br /> 1 = 架构稳定性锁 (LCK_M_SCH_S)<br /><br /> 2 = 架构修改锁 (LCK_M_SCH_M)<br /><br /> 3 = 共享锁 (LCK_M_S)<br /><br /> 4 = 更新锁 (LCK_M_U)<br /><br /> 5 = 排他锁 (LCK_M_X)<br /><br /> 6 = 意向共享锁 (LCK_M_IS)<br /><br /> 7 = 意向更新锁 (LCK_M_IU)<br /><br /> 8 = 意向排他锁 (LCK_M_IX)<br /><br /> 9 = 意向更新共享 (LCK_M_SIU)<br /><br /> 10 = 意向排他共享 (LCK_M_SIX)<br /><br /> 11 = 意向排他更新 (LCK_M_UIX)<br /><br /> 12 = 大容量更新锁 (LCK_M_BU)<br /><br /> 13 = 键范围共享/共享 (LCK_M_RS_S)<br /><br /> 14 = 键范围共享/更新 (LCK_M_RS_U)<br /><br /> 15 = 键范围插入 NULL (LCK_M_RI_NL)<br /><br /> 16 = 键范围插入共享 (LCK_M_RI_S)<br /><br /> 17 = 键范围插入更新 (LCK_M_RI_U)<br /><br /> 18 = 键范围插入排他 (LCK_M_RI_X)<br /><br /> 19 = 键范围排他共享 (LCK_M_RX_S)<br /><br /> 20 = 键范围排他更新 (LCK_M_RX_U)<br /><br /> 21 = 键范围排他排他 (LCK_M_RX_X)|32|是|  
|NTDomainName|**nvarchar**|用户所属的 Windows 域。|7|是|  
|NTUserName|**nvarchar**|Windows 用户名。|6|是|  
|ObjectID|**int**|超时对象的 ID（如果可用且适用）。|22|是|  
|ObjectID2|**bigint**|相关对象或实体的 ID（如果可用且适用）。|56|是|  
|OwnerID|**int**|1 = TRANSACTION<br /><br /> 2 = CURSOR<br /><br /> 3 = SESSION<br /><br /> 4 = SHARED_TRANSACTION_WORKSPACE<br /><br /> 5 = EXCLUSIVE_TRANSACTION_WORKSPACE|58|是|  
|RequestID|**int**|包含该语句的请求的 ID。|49|是|  
|ServerName|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果您使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 SessionLoginName 将显示 Login1，而 LoginName 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**ntext**|取决于超时发生时获取的锁类型的文本值。|1|是|  
|TransactionID|**bigint**|系统分配的事务 ID。|4|是|  
|类型|**int**|1 = NULL_RESOURCE<br /><br /> 2 = DATABASE<br /><br /> 3 = FILE<br /><br /> 5 = OBJECT<br /><br /> 6 = PAGE<br /><br /> 7 = KEY<br /><br /> 8 = EXTENT<br /><br /> 9 = RID<br /><br /> 10 = APPLICATION<br /><br /> 11 = METADATA<br /><br /> 12 = AUTONAMEDB<br /><br /> 13 = HOBT<br /><br /> 14 = ALLOCATION_UNIT|57|是|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Lock:Timeout (timeout > 0) 事件类](../../relational-databases/event-classes/lock-timeout-timeout-0-event-class.md)   
 [sys.dm_tran_locks (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)  
  
  
