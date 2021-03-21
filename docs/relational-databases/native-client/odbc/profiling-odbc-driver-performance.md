---
description: ODBC 驱动程序性能事件探查
title: 分析 ODBC 驱动程序性能 |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- profiling ODBC driver performance data [SQL Server Native Client]
- performance [ODBC]
- application statistics [ODBC]
- time statistics [ODBC]
- ODBC, performance data
- SQL Server Native Client ODBC driver, profiling performance data
- SQLPERF data structure
- statistical information [ODBC]
ms.assetid: 8f44e194-d556-4119-a759-4c9dec7ecead
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1e2771a14544aed31304779c66fc089769b5b7a2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740717"
---
# <a name="profiling-odbc-driver-performance"></a>ODBC 驱动程序性能事件探查
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序可以对两种类型的性能数据进行事件探查：  
  
-   长时间运行的查询。  
  
     驱动程序可以将在指定时间内没有从服务器获得响应的任何查询写入日志文件。 然后，应用程序编程人员或者数据库管理员可以研究每个记录在日志中的 SQL 语句，确定如何提高它的性能。  
  
-   驱动程序性能数据。  
  
     驱动程序可以记录性能统计信息，并将它们写入文件或者通过名为 SQLPERF 且特定于驱动程序的数据结构让应用程序能够使用这些信息。 包含性能统计信息的文件是以制表符分隔的文件，可以使用支持以制表符分隔的文件的任何电子表格软件（比如 Microsoft Excel）对该文件进行分析。  
  
 可以通过以下方式打开任一类型的事件探查：  
  
-   连接到指定了日志记录的数据源。  
  
-   调用 [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) 以设置控制分析的特定于驱动程序的属性。  
  
 每个应用程序进程都获取它自己的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序副本，而且事件探查工作是将驱动程序副本和应用程序进程综合在一起进行考虑的全局性工作。 当应用程序中的任意操作打开了事件探查时，事件探查将记录从该应用程序建立的、在驱动程序中仍处于活动状态的所有连接的信息。 其中甚至包括了那些不是专为进行事件探查而建立的连接。  
  
 在驱动程序打开事件探查日志（性能数据日志或长时间运行查询日志）之后，它会始终打开日志，直到驱动程序被 ODBC 驱动程序管理器卸载（当应用程序释放它在驱动程序中打开的所有环境句柄的时候）。 如果应用程序打开了新的环境句柄，则会加载驱动程序的新副本。 然后，如果应用程序所连接的数据源指定了相同日志文件或者设置了特定于驱动程序的属性以在相同文件中记录日志，驱动程序将覆盖旧的日志文件。  
  
 如果一个应用程序启动了事件探查并将信息写入某个日志文件，然后第二个应用程序试图启动事件探查并将信息写入同一个日志文件，第二个应用程序将无法在日志中记录任何事件探查数据。 如果第二个应用程序在第一个应用程序已卸载了其驱动程序之后启动事件探查，则第二个应用程序将覆盖第一个应用程序写入的日志文件。  
  
 如果应用程序连接到已启用分析的数据源，则驱动程序将返回 SQL_ERROR （如果应用程序调用 **SQLSetConnectOption** 启动日志记录）。 然后调用 **SQLGetDiagRec** 返回以下内容：  
  
```  
SQLState: 01000, pfNative = 0  
ErrorMsg: [Microsoft][SQL Server Native Client]  
   An error has occurred during the attempt to access  
   the log file, logging disabled.  
```  
  
 当环境句柄关闭时，驱动程序将停止收集性能数据。 如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 应用程序具有多条连接，每个都有自己的环境句柄，那么，当任何关联的环境句柄关闭之后，驱动程序都将停止收集性能数据。  
  
 驱动程序的性能数据既可以存储在 SQLPERF 数据结构中，也可以记录在以制表符分隔的文件中。 包括以下几个类别的统计信息数据：  
  
-   应用程序概况  
  
-   连接  
  
-   网络  
  
-   时间  
  
 下表对 SQLPERF 数据结构中的字段的说明也适用于性能日志文件中记录的统计信息。  
  
### <a name="application-profile-statistics"></a>应用程序配置文件统计信息  
  
|SQLPERF 字段|说明|  
|-------------------|-----------------|  
|TimerResolution|服务器时钟时间的最小解析度（以毫秒为单位）。 通常，此值报告为 0（零）并且只有在报告的数值很大时才应予以考虑。 如果服务器时钟的最小解析度大于某些基于计时器的统计信息可能使用的间隔时间，这些统计信息可能会急剧增加。|  
|SQLidu|SQL_PERF_START 之后 INSERT、DELETE 或 UPDATE 语句的数量。|  
|SQLiduRows|SQL_PERF_START 之后 INSERT、DELETE 或 UPDATE 语句的数量。|  
|SQLSelects|在 SQL_PERF_START 之后处理的 SELECT 语句的数量。|  
|SQLSelectRows|在 SQL_PERF_START 之后选择的行数。|  
|事务|SQL_PERF_START 之后用户事务的数量（包括回滚的数量）。 如果使用 SQL_AUTOCOMMIT_ON 运行 ODBC 应用程序，则每个命令都视为一个事务。|  
|SQLPrepares|SQL_PERF_START 后的 [SQLPrepare 函数](../../../odbc/reference/syntax/sqlprepare-function.md) 调用数。|  
|ExecDirects|SQL_PERF_START 后的 **SQLExecDirect** 调用数。|  
|SQLExecutes|SQL_PERF_START 后的 **SQLExecute** 调用数。|  
|CursorOpens|SQL_PERF_START 之后驱动程序打开服务器游标的次数。|  
|CursorSize|SQL_PERF_START 之后游标打开的结果集中行的数量。|  
|CursorUsed|SQL_PERF_START 之后通过驱动程序从游标实际检索的行的数量。|  
|PercentCursorUsed|等于 CursorUsed/CursorSize。 例如，如果应用程序导致驱动程序打开一个服务器游标来执行“SELECT COUNT(*) FROM Authors”，SELECT 语句的结果集中将包含 23 行。 如果应用程序随后仅提取了这些行中的三行，CursorUsed/CursorSize 将等于 3/23，因此 PercentCursorUsed 等于 13.043478。|  
|AvgFetchTime|等于 SQLFetchTime/SQLFetchCount。|  
|AvgCursorSize|等于 CursorSize/CursorOpens。|  
|AvgCursorUsed|等于 CursorUsed/CursorOpens。|  
|SQLFetchTime|针对服务器游标执行提取所需的累积时间量。|  
|SQLFetchCount|SQL_PERF_START 之后针对服务器游标完成的提取操作的数量。|  
|CurrentStmtCount|当前在驱动程序中处于打开状态的所有连接上打开的语句句柄的数量。|  
|MaxOpenStmt|SQL_PERF_START 之后并发打开的语句句柄的最大数量。|  
|SumOpenStmt|SQL_PERF_START 之后打开的语句句柄的数量。|  
|**连接统计信息：**||  
|CurrentConnectionCount|应用程序对服务器打开的活动连接句柄的当前数量。|  
|MaxConnectionsOpened|SQL_PERF_START 之后打开的并发连接句柄的最大数量。|  
|SumConnectionsOpened|SQL_PERF_START 之后打开的连接句柄的总数。|  
|SumConnectionTime|SQL_PERF_START 之后打开的所有连接的时间总和。 例如，如果应用程序打开了 10 个连接并且每个连接持续 5 秒钟，则 SumConnectionTime 将为 50 秒。|  
|AvgTimeOpened|等于 SumConnectionsOpened/ SumConnectionTime。|  
|**网络统计信息：**||  
|ServerRndTrips|驱动程序将命令发送到服务器并且获得了回复的次数。|  
|BuffersSent|SQL_PERF_START 之后由驱动程序发送到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的表格格式数据流 (TDS) 数据包的数量。 大型命令可能会占用多个缓冲区，因此如果向服务器发送了一个大型命令并且它填充了 6 个数据包，则 ServerRndTrips 将增加 1，而 BuffersSent 会增加 6。|  
|BuffersRec|使用驱动程序启动应用程序之后，驱动程序从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 接收的 TDS 数据包的数量。|  
|BytesSent|使用驱动程序启动应用程序之后，放在 TDS 数据包中发送到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的数据的字节数。|  
|BytesRec|使用驱动程序启动应用程序之后，驱动程序从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 接收的 TDS 数据包中数据的字节数。|  
  
### <a name="time-statistics"></a>时间统计信息  
  
|SQLPERF 字段|说明|  
|-------------------|-----------------|  
|msExecutionTime|SQL_PERF_START 之后驱动程序花费在处理工作上的累积时间量，其中包括等待服务器的回复所花费的时间。|  
|msNetworkServerTime|驱动程序等待服务器回复所花费的累积时间量。|  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client &#40;ODBC&#41;](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [分析 ODBC 驱动程序性能操作指南主题 &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/profiling-odbc-driver-performance-odbc.md)  
  
