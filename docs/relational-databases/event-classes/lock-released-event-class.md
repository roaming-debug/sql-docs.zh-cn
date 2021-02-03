---
description: Lock:Released 事件类
title: Lock:Released 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Released event class
ms.assetid: a150c300-72fa-4231-8f41-f1abd550a429
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 11628392ac0f87d506eeebb584b6011ce093c731
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183441"
---
# <a name="lockreleased-event-class"></a>Lock:Released 事件类
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Lock:Released 事件类指示已释放某个资源（例如页）的锁。  
  
 Lock:Acquired 和 Lock:Released 事件类可以用于监视锁定对象的时间、使用的锁类型以及锁保留的时间。 保留较长时间的锁可能导致争用问题，应进行调查。 例如，应用程序可以为表中的行获取锁，然后等待用户输入。 由于用户输入可能需要较长时间，所以锁可能会阻塞其他用户。 在这种情况下，应重新设计应用程序，以便只在需要时才请求锁，并且在获取锁后不需要用户输入。  
  
## <a name="lock-released-event-class-data-columns"></a>Lock: Released 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|BinaryData|**图像**|锁资源标识符。|2|是|  
|ClientProcessID|**int**|主机为运行该客户端应用程序的进程分配的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|DatabaseID|**int**|释放锁所在的数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 如果在跟踪中捕获 ServerName 数据列而且服务器可用，则将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|EventClass|**int**|事件类型 = 23。|27|否|  
|EventSequence|**int**|给定事件在请求中的顺序。|51|否|  
|GroupID|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|HostName|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|IntegerData2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|55|是|  
|IsSystem|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|LoginName|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 Windows 登录凭据，格式为“域\用户名”）。|11|是|  
|LoginSid|**图像**|登录用户的安全标识号 (SID)。 您可以在 sys.server_principals 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|“模式”|**int**|释放锁后所得到的模式。<br /><br /> 0=NULL - 与所有其他锁定模式兼容 (LCK_M_NL)<br /><br /> 1 = 架构稳定性锁 (LCK_M_SCH_S)<br /><br /> 2 = 架构修改锁 (LCK_M_SCH_M)<br /><br /> 3 = 共享锁 (LCK_M_S)<br /><br /> 4 = 更新锁 (LCK_M_U)<br /><br /> 5 = 排他锁 (LCK_M_X)<br /><br /> 6 = 意向共享锁 (LCK_M_IS)<br /><br /> 7 = 意向更新锁 (LCK_M_IU)<br /><br /> 8 = 意向排他锁 (LCK_M_IX)<br /><br /> 9 = 意向更新共享 (LCK_M_SIU)<br /><br /> 10 = 意向排他共享 (LCK_M_SIX)<br /><br /> 11 = 意向排他更新 (LCK_M_UIX)<br /><br /> 12 = 大容量更新锁 (LCK_M_BU)<br /><br /> 13 = 键范围共享/共享 (LCK_M_RS_S)<br /><br /> 14 = 键范围共享/更新 (LCK_M_RS_U)<br /><br /> 15 = 键范围插入 NULL (LCK_M_RI_NL)<br /><br /> 16 = 键范围插入共享 (LCK_M_RI_S)<br /><br /> 17 = 键范围插入更新 (LCK_M_RI_U)<br /><br /> 18 = 键范围插入排他 (LCK_M_RI_X)<br /><br /> 19 = 键范围排他共享 (LCK_M_RX_S)<br /><br /> 20 = 键范围排他更新 (LCK_M_RX_U)<br /><br /> 21 = 键范围排他排他 (LCK_M_RX_X)|32|是|  
|NTDomainName|**nvarchar**|用户所属的 Windows 域。|7|是|  
|NTUserName|**nvarchar**|Windows 用户名。|6|是|  
|ObjectID|**int**|系统为已释放的对象分配的 ID（如果可用且适用）。|22|是|  
|ObjectID2|**bigint**|相关对象或实体的 ID（如果可用且适用）。|56|是|  
|OwnerID|**int**|1 = TRANSACTION<br /><br /> 2 = CURSOR<br /><br /> 3 = SESSION<br /><br /> 4 = SHARED_TRANSACTION_WORKSPACE<br /><br /> 5 = EXCLUSIVE_TRANSACTION_WORKSPACE|58|是|  
|RequestID|**int**|包含该语句的请求的 ID。|49|是|  
|ServerName|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果您使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 SessionLoginName 将显示 Login1，而 LoginName 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**ntext**|依赖于跟踪中捕获的事件类的文本值。|1|是|  
|TransactionID|**bigint**|系统分配的事务 ID。|4|是|  
|类型|**int**|1 = NULL_RESOURCE<br /><br /> 2 = DATABASE<br /><br /> 3 = FILE<br /><br /> 5 = OBJECT<br /><br /> 6 = PAGE<br /><br /> 7 = KEY<br /><br /> 8 = EXTENT<br /><br /> 9 = RID<br /><br /> 10 = APPLICATION<br /><br /> 11 = METADATA<br /><br /> 12 = AUTONAMEDB<br /><br /> 13 = HOBT<br /><br /> 14 = ALLOCATION_UNIT|57|是|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Lock:Acquired 事件类](../../relational-databases/event-classes/lock-acquired-event-class.md)   
 [sys.dm_tran_locks (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)  
  
  
