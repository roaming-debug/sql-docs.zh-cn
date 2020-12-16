---
description: Lock:Deadlock Chain 事件类
title: Lock:Deadlock Chain 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Deadlock Chain event class
ms.assetid: 9883127b-aa34-4235-88cc-c161cd2112cc
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 91a44b8d2db79a7c22ac99b2125540f9f332667a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97420423"
---
# <a name="lockdeadlock-chain-event-class"></a>Lock:Deadlock Chain 事件类
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  对于死锁中的每个参与者都会发生 Lock:Deadlock Chain 事件类。  
  
 使用 Lock:Deadlock Chain 事件类可以监视何时出现死锁情况。 此信息有助于确定死锁是否会对应用程序的性能造成重大影响，以及会涉及哪些对象。 可以检查用于修改这些对象的应用程序代码，以便确定是否可以做出更改以便将死锁的情况减到最少。  
  
## <a name="lockdeadlock-chain-event-class-data-columns"></a>Lock:Deadlock Chain 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BinaryData|**图像**|锁资源标识符。|2|是|  
|DatabaseID|**int**|此资源所属的数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 如果在跟踪中捕获 ServerName 数据列而且服务器可用，则将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|DatabaseName|**nvarchar**|资源所属的数据库的名称。|35|是|  
|EventClass|**int**|事件类型 = 59。|27|否|  
|EventSequence|**int**|给定事件在请求中的顺序。|51|否|  
|EventSubClass|**int**|事件子类的类型。<br /><br /> 101 = 资源类型锁<br /><br /> 102 = 资源类型交换|21|是|  
|IntegerData|**int**|死锁编号。 从服务器启动时起，从 0 开始按递增顺序为每个死锁指派编号。|25|是|  
|IntegerData2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|55|是|  
|IsSystem|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|LoginSid|**图像**|登录用户的安全标识号 (SID)。 您可以在 sys.server_principals 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|“模式”|**int**|0=NULL - 与所有其他锁定模式兼容 (LCK_M_NL)<br /><br /> 1 = 架构稳定性锁 (LCK_M_SCH_S)<br /><br /> 2 = 架构修改锁 (LCK_M_SCH_M)<br /><br /> 3 = 共享锁 (LCK_M_S)<br /><br /> 4 = 更新锁 (LCK_M_U)<br /><br /> 5 = 排他锁 (LCK_M_X)<br /><br /> 6 = 意向共享锁 (LCK_M_IS)<br /><br /> 7 = 意向更新锁 (LCK_M_IU)<br /><br /> 8 = 意向排他锁 (LCK_M_IX)<br /><br /> 9 = 意向更新共享 (LCK_M_SIU)<br /><br /> 10 = 意向排他共享 (LCK_M_SIX)<br /><br /> 11 = 意向排他更新 (LCK_M_UIX)<br /><br /> 12 = 大容量更新锁 (LCK_M_BU)<br /><br /> 13 = 键范围共享/共享 (LCK_M_RS_S)<br /><br /> 14 = 键范围共享/更新 (LCK_M_RS_U)<br /><br /> 15 = 键范围插入 NULL (LCK_M_RI_NL)<br /><br /> 16 = 键范围插入共享 (LCK_M_RI_S)<br /><br /> 17 = 键范围插入更新 (LCK_M_RI_U)<br /><br /> 18 = 键范围插入排他 (LCK_M_RI_X)<br /><br /> 19 = 键范围排他共享 (LCK_M_RX_S)<br /><br /> 20 = 键范围排他更新 (LCK_M_RX_U)<br /><br /> 21 = 键范围排他排他 (LCK_M_RX_X)|32|是|  
|ObjectID|**int**|锁定对象的 ID（如果可用且适用）。|22|是|  
|ObjectID2|**bigint**|相关对象或实体的 ID（如果可用且适用）。|56|是|  
|OwnerID|**int**|1 = TRANSACTION<br /><br /> 2 = CURSOR<br /><br /> 3 = SESSION<br /><br /> 4 = SHARED_TRANSACTION_WORKSPACE<br /><br /> 5 = EXCLUSIVE_TRANSACTION_WORKSPACE|58|是|  
|RequestID|**int**|包含该语句的请求的 ID。|49|是|  
|ServerName|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果您使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 SessionLoginName 将显示 Login1，而 LoginName 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 登录名。|64|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**ntext**|依赖于资源类型的文本值。|1|是|  
|TransactionID|**bigint**|系统分配的事务 ID。|4|是|  
|类型|**int**|1 = NULL_RESOURCE<br /><br /> 2 = DATABASE<br /><br /> 3 = FILE<br /><br /> 5 = OBJECT<br /><br /> 6 = PAGE<br /><br /> 7 = KEY<br /><br /> 8 = EXTENT<br /><br /> 9 = RID<br /><br /> 10 = APPLICATION<br /><br /> 11 = METADATA<br /><br /> 12 = AUTONAMEDB<br /><br /> 13 = HOBT<br /><br /> 14 = ALLOCATION_UNIT|57|是|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sys.dm_tran_locks (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)  
  
  
