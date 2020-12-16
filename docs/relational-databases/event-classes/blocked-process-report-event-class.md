---
title: Blocked Process Report 事件类 | Microsoft Docs
description: Blocked Process Report 事件类指明某个任务已被阻塞，导致超过 SQL Server 中指定的时间。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Blocked Process Report event class
ms.assetid: e8acb408-938d-4b36-81dd-04f087410cc5
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5bd315448e0c00f9fc628ee7d7c3d56a977f4423
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468078"
---
# <a name="blocked-process-report-event-class"></a>Blocked Process Report 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  **Blocked Process Report** 事件类指明某个任务已被阻塞，导致超过指定的时间。 此事件类不包括系统任务和正在等待未发现死锁的资源的任务。  
  
 若要配置阈值和报告生成频率，请使用 **sp_configure** 命令配置“阻塞的进程阈值”选项（以秒为单位进行设置）。 默认情况下，不生成阻塞的进程报告。 有关设置“阻塞的进程阈值”选项的详细信息，请参阅[服务器配置选项](../../database-engine/configure-windows/blocked-process-threshold-server-configuration-option.md)。  
  
 有关筛选 **阻塞的进程报告** 事件类返回的数据的信息，请参阅 [在跟踪中筛选事件 (SQL Server Profiler)](../../tools/sql-server-profiler/filter-events-in-a-trace-sql-server-profiler.md)、[设置跟踪筛选器 (Transact-SQL)](../../relational-databases/sql-trace/set-a-trace-filter-transact-sql.md) 或 [sp_trace_setfilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)。  
  
## <a name="blocked-process-report-event-class-data-columns"></a>Blocked Process Report 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**DatabaseID**|**int**|获取锁所在的数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据列而且服务器可用，则 **ServerName** 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|**Duration**|**bigint**|进程受阻止的时长（微秒）。|13|是|  
|**EndTime**|**datetime**|事件结束的时间。 启动事件类（如 **SQL:BatchStarting** 或 **SP:Starting**）不填充此列。|15|是|  
|**EventClass**|**int**|事件类型 = 137。|27|否|  
|**EventSequence**|**int**|特定事件在请求中的顺序。|51|否|  
|**IndexID**|**int**|受事件影响的对象的索引的 ID。 若要确定对象的索引的 ID，请使用 **sysindexes** 系统表的 **indid** 列。|24|是|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|**LoginSid**|**图像**|已登录的用户的安全标识符 (SID)。 系统线程中始终报告此事件。 IsSystem = 1；SID = sa。|41|是|  
|**模式**|**int**|事件已接收的或要请求的状态。<br /><br /> 0 = NULL<br /><br /> 1 = Sch-S<br /><br /> 2 = Sch-M<br /><br /> 3 = S<br /><br /> 4 = U<br /><br /> 5 = X<br /><br /> 6 = IS<br /><br /> 7 = IU<br /><br /> 8 = IX<br /><br /> 9 = SIU<br /><br /> 10 = SIX<br /><br /> 11 = UIX<br /><br /> 12 = BU<br /><br /> 13 = RangeS-S<br /><br /> 14 = RangeS-U<br /><br /> 15 = RangeI-N<br /><br /> 16 = RangeI-S<br /><br /> 17 = RangeI-U<br /><br /> 18 = RangeI-X<br /><br /> 19 = RangeX-S<br /><br /> 20 = RangeX-U<br /><br /> 21 = RangeX-X|32|是|  
|**Exchange Spill**|**int**|获取其锁的对象的系统分配的 ID（如果可用并适用）。|22|是|  
|**ServerName**|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26||  
|**SessionLoginName**|**nvarchar**|发起该会话的用户的登录名。 例如，如果您使用 Login1 连接到 SQL Server 并以 Login2 身份执行语句，则 **SessionLoginName** 将显示 Login1，而 **LoginName** 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|**TextData**|**ntext**|依赖于跟踪中捕获的事件类的文本值。|1|是|  
|**TransactionID**|**bigint**|系统分配的事务 ID。|4|是|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
