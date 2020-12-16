---
description: Lock:Cancel 事件类
title: Lock:Cancel 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Cancel event class
ms.assetid: d9203e58-40ba-4712-a918-2c34a5d396d7
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7ac6f7773736f09368825e89a5b0b6707ad0d176
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485409"
---
# <a name="lockcancel-event-class"></a>Lock:Cancel 事件类
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  **Lock:Cancel** 事件类指示已取消获取某个资源的锁；例如，由于查询被取消。  
  
## <a name="lockcancel-event-class-data-columns"></a>Lock:Cancel 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|**BinaryData**|**图像**|锁资源标识符。|2|是|  
|**ClientProcessID**|**int**|主机为运行该客户端应用程序的进程分配的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|**DatabaseID**|**int**|获取锁所在的数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据列而且服务器可用，则 **ServerName** 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|**DatabaseName**|**nvarchar**|尝试获取锁的数据库的名称。|35|是|  
|**Duration**|**bigint**|发出锁请求到取消锁之间的时间量（微秒）。|13|是|  
|**EndTime**|**datetime**|事件结束的时间。|15|是|  
|**EventClass**|**int**|事件类型 = 26。|27|否|  
|**EventSequence**|**int**|给定事件在请求中的顺序。|51|否|  
|**GroupID**|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|**HostName**|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|**IntegerData2**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|55|是|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|**LoginName**|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 登录凭据，格式为“DOMAIN\username”）。|11|是|  
|**LoginSid**|**图像**|登录用户的安全标识号 (SID)。 你可以在 **sys.server_principals** 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|**模式**|**int**|取消锁后的结果模式。<br /><br /> 0=NULL - 与所有其他锁定模式兼容 (LCK_M_NL)<br /><br /> 1 = 架构稳定性锁 (LCK_M_SCH_S)<br /><br /> 2 = 架构修改锁 (LCK_M_SCH_M)<br /><br /> 3 = 共享锁 (LCK_M_S)<br /><br /> 4 = 更新锁 (LCK_M_U)<br /><br /> 5 = 排他锁 (LCK_M_X)<br /><br /> 6 = 意向共享锁 (LCK_M_IS)<br /><br /> 7 = 意向更新锁 (LCK_M_IU)<br /><br /> 8 = 意向排他锁 (LCK_M_IX)<br /><br /> 9 = 意向更新共享 (LCK_M_SIU)<br /><br /> 10 = 意向排他共享 (LCK_M_SIX)<br /><br /> 11 = 意向排他更新 (LCK_M_UIX)<br /><br /> 12 = 大容量更新锁 (LCK_M_BU)<br /><br /> 13 = 键范围共享/共享 (LCK_M_RS_S)<br /><br /> 14 = 键范围共享/更新 (LCK_M_RS_U)<br /><br /> 15 = 键范围插入 NULL (LCK_M_RI_NL)<br /><br /> 16 = 键范围插入共享 (LCK_M_RI_S)<br /><br /> 17 = 键范围插入更新 (LCK_M_RI_U)<br /><br /> 18 = 键范围插入排他 (LCK_M_RI_X)<br /><br /> 19 = 键范围排他共享 (LCK_M_RX_S)<br /><br /> 20 = 键范围排他更新 (LCK_M_RX_U)<br /><br /> 21 = 键范围排他排他 (LCK_M_RX_X)|32|是|  
|**NTDomainName**|**nvarchar**|用户所属的 Windows 域。|7|是|  
|**NTUserName**|**nvarchar**|Windows 用户名。|6|是|  
|**Exchange Spill**|**int**|已取消锁的对象 ID（如果可用且适用）。|22|是|  
|**ObjectID2**|**bigint**|相关对象或实体的 ID（如果可用且适用）。|56|是|  
|**OwnerID**|**int**|1 = TRANSACTION<br /><br /> 2 = CURSOR<br /><br /> 3 = SESSION<br /><br /> 4 = SHARED_TRANSACTION_WORKSPACE<br /><br /> 5 = EXCLUSIVE_TRANSACTION_WORKSPACE|58|是|  
|**RequestID**|**int**|包含该语句的请求的 ID。|49|是|  
|**ServerName**|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|**SessionLoginName**|**nvarchar**|发起会话的用户的登录名。 例如，如果你使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 **SessionLoginName** 将显示 Login1，而 **LoginName** 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|**SPID**|**int**|发生该事件的会话的 ID。|12|是|  
|**StartTime**|**datetime**|事件开始的时间（如果可用）。|14|是|  
|**TextData**|**ntext**|文本值取决于获取的锁类型。 该值与 **sys.dm_tran_locks** 中 **resource_description** 列的值相同。|1|是|  
|**TransactionID**|**bigint**|系统分配的事务 ID。|4|是|  
|**类型**|**int**|1 = NULL_RESOURCE<br /><br /> 2 = DATABASE<br /><br /> 3 = FILE<br /><br /> 5 = OBJECT<br /><br /> 6 = PAGE<br /><br /> 7 = KEY<br /><br /> 8 = EXTENT<br /><br /> 9 = RID<br /><br /> 10 = APPLICATION<br /><br /> 11 = METADATA<br /><br /> 12 = AUTONAMEDB<br /><br /> 13 = HOBT<br /><br /> 14 = ALLOCATION_UNIT|57|是|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sys.dm_tran_locks (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)  
  
  
