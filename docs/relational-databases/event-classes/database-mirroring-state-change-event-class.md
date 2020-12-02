---
description: Database Mirroring State Change 事件类
title: Database Mirroring State Change 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- event notifications [SQL Server], database mirroring
- database mirroring [SQL Server], event notifications
- Database Mirroring State Change event class
ms.assetid: f936a99e-2a81-4768-8177-5c969bbe2e04
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7c63c19ac6a6d80d7c61f8b2cdccc1b46871c145
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126639"
---
# <a name="database-mirroring-state-change-event-class"></a>Database Mirroring State Change 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Database Mirroring State Change 事件类指明镜像数据库的状态改变时间。 此事件类包括在监视镜像数据库条件的跟踪中。  
  
 当 **Database Mirroring State Change** 事件类包括在跟踪中后，相关开销会较低。 如果镜像数据库的状态增多，开销可能会上升。  
  
## <a name="data-database-mirroring-state-change-event-class-data-columns"></a>Data Database Mirroring State Change 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**DatabaseID**|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据列而且服务器可用，则 **ServerName** 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|**DatabaseName**|**nvarchar**|镜像数据库的名称。|35|是|  
|**EventClass**|**int**|事件类型 = 167。|27|否|  
|**EventSequence**|**int**|事件类在批处理中的顺序。|51|否|  
|**IntegerData**|**int**|前一个状态 ID。|25|是|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|**LoginSid**|**图像**|登录用户的安全标识号 (SID)。 你可以在 **sys.server_principals** 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|**RequestID**|**int**|包含该语句的请求的 ID。|49|是|  
|**ServerName**|**nvarchar**|所跟踪的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|**SessionLoginName**|**nvarchar**|发起会话的用户的登录名。 例如，如果你使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 **SessionLoginName** 将显示 Login1，而 **LoginName** 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|**SPID**|**int**|发生该事件的会话的 ID。|12|是|  
|**StartTime**|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|**State**|**int**|新的镜像状态 ID：<br /><br /> 0 = 空值通知<br /><br /> 1 = 已通过见证服务器同步了主体服务器<br /><br /> 2 = 未通过见证服务器同步了主体服务器<br /><br /> 3 = 已通过见证服务器同步了镜像服务器<br /><br /> 4 = 未通过见证服务器同步了镜像服务器<br /><br /> 5 = 丢失了与主体服务器的连接<br /><br /> 6 = 丢失了与镜像服务器的连接<br /><br /> 7 = 手动故障转移<br /><br /> 8 = 自动故障转移<br /><br /> 9 = 镜像已挂起<br /><br /> 10 = 无仲裁<br /><br /> 11 = 正在同步镜像服务器<br /><br /> 12 = 主体服务器运行已公开|30|是|  
|**TextData**|**ntext**|状态改变说明。|1|是|  
|**TransactionID**|**bigint**|系统分配的事务 ID。|4|是|  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
