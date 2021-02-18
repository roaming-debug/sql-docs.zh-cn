---
title: Replay Requirements
titleSuffix: SQL Server Profiler
description: 了解要在跟踪中捕获的事件类和数据列，以便可以使用 SQL Server Profiler 或分布式重播实用工具重播跟踪数据。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 0e01dfc7-84b9-47f6-8bf7-b0656df4fa7d
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 9105e4d7c28b003c191d5a9794717496d07eb0f5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353362"
---
# <a name="replay-requirements"></a>重播要求

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

若要使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 或分布式重播实用工具重播跟踪数据，则必须在跟踪中捕获一组特定的事件类和列。 如果使用 **TSQL_Replay** 跟踪模板配置稍后用于重播的跟踪，则会默认启用这些设置。 本主题介绍这些设置以及其他重播要求。  
  
> [!NOTE]  
>  建议使用分布式重播实用工具重播密集型 OLTP 应用程序（具有大量活动并发连接或高吞吐量）。 分布式重播实用工具可以从多台计算机重播跟踪数据，并更好地模拟任务关键型工作负荷。 有关详细信息，请参阅 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)。  
  
## <a name="event-classes-required-for-replay"></a>重播所需的事件类  
 若要通过 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]进行重播，除了要监视的任何其他事件类之外，还必须在跟踪中捕获以下一组事件类：  
  
-   **CursorClose**（仅在重播服务器端游标时需要）  
  
-   **CursorExecute** （仅在重播服务器端游标时需要）  
  
-   **CursorOpen** （仅在重播服务器端游标时需要）  
  
-   **CursorPrepare** （仅在重播服务器端游标时需要）  
  
-   **CursorUnprepare** （仅在重播服务器端游标时需要）  
  
-   **审核登录**  
  
-   **审核注销**  
  
-   **ExistingConnection**  
  
-   **RPC Output Parameter**  
  
-   **RPC:Completed**  
  
-   **RPC:Starting**  
  
-   **Exec Prepared SQL** （仅在重播服务器端准备的 SQL 语句时需要）  
  
-   **Prepare SQL** （仅在重播服务器端准备的 SQL 语句时需要）  
  
-   **SQL:BatchCompleted**  
  
-   **SQL:BatchStarting**  
  
## <a name="data-columns-required-for-replay"></a>重播所需的数据列  
 除任何其他要捕获的数据列外，还必须在跟踪内捕获下列数据列才能重播跟踪：  
  
-   **Event Class**  
  
-   **EventSequence**  
  
-   **TextData**  
  
-   **应用程序名称**  
  
-   **LoginName**  
  
-   **DatabaseName**  
  
-   **数据库 ID**  
  
-   **ClientProcessID**  
  
-   **HostName**  
  
-   **ServerName**  
  
-   **Binary Data**  
  
-   **SPID**  
  
-   **Start Time**  
  
-   **EndTime**  
  
-   **IsSystem**  
  
-   **NTDomainName**  
  
-   **NTUserName**  
  
-   **错误**  
  
> [!NOTE]  
>  将跟踪模板 **TSQL_Replay** 用于捕获重播数据的跟踪。  
  
## <a name="other-replay-requirements"></a>其他重播要求  
 在 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，重播将检查是否存在所需的事件和列。 此更改有助于改进重播的准确性，从而在缺少所需数据的情况下对重播进行故障排除时无需进行任何猜测。 当跟踪中缺少所需的数据时，重播将会返回一个错误，并停止重播文件。  
  
 若要重播对运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的服务器（目标）而非最初跟踪的服务器（源）的跟踪，请确保已执行下列操作：  
  
-   必须在目标上而且是在与源相同的数据库中创建跟踪包含的所有登录名和用户。  
  
-   目标中的所有登录名和用户具有的权限必须与源中的相同。  
  
-   所有登录密码必须是执行重播的用户的密码。  
  
-   目标上的数据库 ID 最好与源上的数据库 ID 相同。 但是，如果它们不相同，当跟踪中出现 **DatabaseName** 时，将根据它进行匹配。  
  
-   必须将跟踪内包含的每个登录名的默认数据库在目标上设置成登录名各自的目标数据库。 例如，在源上，要重播的跟踪包含数据库 **Fred_Db** 内登录名 **Fred** 的活动。 因此在目标上，必须将登录名 **Fred** 的默认数据库设置成与 **Fred_Db** 相匹配的数据库（即使数据库名称不同）。 若要设置登录名的默认数据库，请使用 **sp_defaultdb** 系统存储过程。  
  
 重播与不存在的或不正确的登录名相关的事件会导致重播错误，但重播操作会继续。  
  
 有关重播跟踪需要哪些权限的信息，请参阅 [Permissions Required to Run SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)。  
  
## <a name="see-also"></a>另请参阅  
 [重播跟踪表 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-trace-table-sql-server-profiler.md)   
 [重播跟踪文件 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-trace-file-sql-server-profiler.md)   
 [SQL Server 事件类参考](../../relational-databases/event-classes/sql-server-event-class-reference.md)   
 [sp_defaultdb (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-defaultdb-transact-sql.md)   
 [SQL Server 分布式重播](../../tools/distributed-replay/sql-server-distributed-replay.md)  
  
  
