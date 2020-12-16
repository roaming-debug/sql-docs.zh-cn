---
title: 使用 system_health 会话
description: system_health 扩展事件会话包含在 SQL Server 中。 此会话收集系统数据以解决数据库引擎的性能问题。
ms.date: 11/27/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- extended events [SQL Server], system health session
- extended events [SQL Server], system_health session
- system_health session [SQL Server extended events]
- system health session [SQL Server extended events]
ms.assetid: 1e1fad43-d747-4775-ac0d-c50648e56d78
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f157320744b80593264225be72d42d6f08ba398f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481318"
---
# <a name="use-the-system_health-session"></a>使用 system_health 会话

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

system_health 会话是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]默认包含的扩展事件会话。 该会话在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 启动时自动启动，并且运行时不会对性能造成任何明显影响。 该会话收集的系统数据可用于帮助对 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的性能问题进行故障排除。 

> [!IMPORTANT]
> 建议不要停止、更改或删除 system_health 会话。 将来的产品更新可能会覆盖对 system_health 会话设置所做的任何更改。
  
该会话收集的信息包括：  
  
-   发生严重性 >=20 的错误的任何会话的 sql_text 和 session_id   。  
  
-   发生内存相关错误的任何会话的 sql_text 和 session_id   。 这些错误包括 17803、701、802、8645、8651、8657 和 8902。  
  
-   任何无法完成的计划程序问题的记录。 这些问题在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中显示为错误 17883。  
  
-   检测到的任何死锁，包括死锁图形。  
  
-   等待闩锁（或其他相关资源）时间 > 15 秒的任何会话的 callstack、sql_text 和 session_id    。  
  
-   等待锁（或其他相关资源）时间 > 30 秒的任何会话的 callstack、sql_text 和 session_id    。  
  
-   为获得“抢先等待”（或其他相关资源）而等待时间很长的任何会话的 callstack、sql_text 和 session_id    。 持续时间因等待类型而异。 在抢先等待中， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 等待的是外部 API 调用。  
  
-   CLR 分配失败和虚拟分配失败的调用堆栈和 session_id   。  
  
-   有关内存代理、计划程序监视、内存节点 OOM、安全性和连接的环形缓冲区事件。  
  
-   来自 `sp_server_diagnostics` 的系统组件结果。  
  
-   scheduler_monitor_system_health_ring_buffer_recorded 收集的实例运行状况  。  
  
-   CLR 分配失败。  
  
-   使用 connectivity_ring_buffer_recorded 时的连接错误  。  
  
-   使用 security_error_ring_buffer_recorded 时的安全错误  。  

> [!NOTE]
> 有关死锁的详细信息，请参阅[事务锁定和行版本控制指南中的死锁](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md#deadlocks)。   
> 有关 SQL 错误消息的详细信息，请参阅[数据库引擎错误](../../relational-databases/errors-events/database-engine-events-and-errors.md)。

## <a name="viewing-the-session-data"></a>查看会话数据  
会话使用环形缓冲区目标和事件文件目标存储数据。 事件文件目标配置为最大大小为 5 MB，文件保留策略为 4 个文件。 

若要使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中提供的扩展事件用户界面查看环形缓冲区目标中的会话数据，请参阅 [SQL Server 中扩展事件的目标数据的高级查看功能 - 查看实时数据](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md#b3-watch-live-data)。

若要使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查看环形缓冲区中的会话数据，请使用以下查询：  
  
```sql  
SELECT CAST(xet.target_data as xml) 
FROM sys.dm_xe_session_targets xet  
JOIN sys.dm_xe_sessions xe ON (xe.address = xet.event_session_address)  
WHERE xe.name = 'system_health'  
```  
  
若要查看事件文件中的会话数据，请使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中提供的扩展事件用户界面。 有关详细信息，请参阅 [SQL Server 中扩展事件的目标数据的高级查看功能](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md)。
  
## <a name="restoring-the-system_health-session"></a>还原 system_health 会话  
如果删除 system_health 会话，则可以通过在查询编辑器中执行 **u_tables.sql** 文件来还原该会话。 该文件位于下面的文件夹中，其中 C: 表示安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 程序文件的驱动器，MSSQL1x 表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的主要版本   ：  
  
 `C:\Program Files\Microsoft SQL Server\MSSQL1x.\<*instanceid*>\MSSQL\Install`  
  
请注意，在还原该会话后，必须使用 `ALTER EVENT SESSION` 语句或使用对象资源管理器中的“扩展事件”节点启动会话  。 否则，该会话会在您下次重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务时自动启动。  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)    
 [扩展事件工具](../../relational-databases/extended-events/extended-events-tools.md)    
 [数据库引擎错误](../../relational-databases/errors-events/database-engine-events-and-errors.md)    
 [（错误）消息目录视图 - sys.messages](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md) 
