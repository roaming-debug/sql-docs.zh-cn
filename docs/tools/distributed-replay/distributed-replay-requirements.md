---
description: Distributed Replay 要求
title: Distributed Replay 要求
titleSuffix: SQL Server Distributed Replay
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/18/2018
ms.openlocfilehash: 237a2ca3b7488688162b38ea3e7f916e2a4d3c6d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836882"
---
# <a name="distributed-replay-requirements"></a>Distributed Replay 要求

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 功能之前，请考虑本主题中列出的产品要求。  

## <a name="input-trace-requirements"></a>输入跟踪要求  

若要成功地重播跟踪数据，它必须满足版本和格式的要求，并包含所需的事件和列。  

### <a name="input-trace-versions"></a>输入跟踪版本  

Distributed Replay 支持在以下版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中所收集的输入跟踪数据：  

- [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] 累积更新 1 及更高版本。 请参阅 [SQL Server 2017 累积更新](https://aka.ms/sql2017cu)。
- [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]   
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
- [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
- [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]    
- [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]  
  
### <a name="input-trace-formats"></a>输入跟踪格式

输入跟踪数据可以采用以下任意一种格式：  

- 具有 `.trc` 扩展名的单个跟踪文件。  

- 一组遵循文件滚动更新命名约定（例如：`<TraceFile>.trc`、`<TraceFile>_1.trc`、`<TraceFile>_2.trc`、`<TraceFile>_3.trc`…`<TraceFile>_n.trc`）的滚动更新跟踪文件。

### <a name="input-trace-events-and-columns"></a>输入跟踪事件和列

输入跟踪数据必须包含特定的事件和列才能由 Distributed Replay 进行重播。 **中的** TSQL_Replay [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 模板包含所有必需的事件和列以及其他信息。 有关该模板的详细信息，请参阅 [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md)。  
  
> [!WARNING]
> 如果你没有使用 **TSQL_Replay** 模板来捕获输入跟踪数据，或者未满足输入跟踪要求，则可能会收到意外的重播结果。  

您也可以创建一个自定义跟踪模板，并使用该模板通过 Distributed Replay 来重播事件，前提条件是该模板包含以下事件：
  
- 审核登录  
  
- 审核注销  
  
- ExistingConnection  
  
- RPC Output Parameter  
  
- RPC:Completed  
  
- RPC:Starting  
  
- SQL:BatchCompleted  
  
- SQL:BatchStarting  
  
 如果您要重播服务器端游标，则还需要以下事件：  
  
- CursorClose  
  
- CursorExecute  
  
- CursorOpen  
  
- CursorPrepare  
  
- CursorUnprepare  
  
 如果您要重播服务器端准备的 SQL 语句，则还需要以下事件：  
  
- Exec Prepared SQL  
  
- Prepare SQL  
  
 所有输入跟踪数据都必须包含以下列：  
  
- Event Class  
  
- EventSequence  
  
- TextData  
  
- 应用程序名称  
  
- LoginName  
  
- DatabaseName  
  
- 数据库 ID  
  
- HostName  
  
- Binary Data  
  
- SPID  
  
- 开始时间  
  
- EndTime  
  
- IsSystem  
  
## <a name="supported-input-trace-and-target-server-combinations"></a>支持的输入跟踪和目标服务器组合  

下表列出了支持的跟踪数据版本，以及对于每种版本，可对其重播数据的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持版本。  
  
|输入跟踪数据的版本|目标服务器实例支持的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本|  
|---------------------------------|------------------------------------------------------------------------------------|  
|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)],[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
  
## <a name="operating-system-requirements"></a>操作系统要求

支持运行管理工具、控制器和客户端服务的操作系统与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例所要求的操作系统相同。 有关对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例受支持的操作平台的详细信息，请参阅 [安装 SQL Server 2016 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。  

Distributed Replay 功能在基于 x86 和基于 x64 的操作系统上均受支持。 对于基于 x64 的操作系统，仅支持 Windows on Windows (WOW) 模式。  

## <a name="installation-limitations"></a>安装限制

任何一台计算机只能安装每个 Distributed Replay 功能的一个实例。 下表列出了在单个 Distributed Replay 环境中每种功能所允许的安装数。  
  
|Distributed Replay 功能|每个重播环境的最大安装数|  
|--------------------------------|--------------------------------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 控制器服务|1|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 客户端服务|16（物理或虚拟计算机）|  
|管理工具|无限制|  
  
> [!NOTE]  
>  虽然一台计算机上只能安装一个管理工具实例，但是您可以启动管理工具的多个实例。 多个管理工具发出的命令按照接收命令的顺序进行解析。  
  
## <a name="data-access-provider"></a>数据访问提供程序

 Distributed Replay 仅支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 数据访问接口。  

## <a name="target-server-preparation-requirements"></a>目标服务器准备要求  

建议将目标服务器置于测试环境中。 若要针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的其他实例而非最初记录跟踪数据的实例重播跟踪数据，请确保目标服务器满足以下条件：  

- 跟踪数据中包含的所有登录名和用户必须出现在目标服务器的相同数据库中。  

- 目标服务器上的所有登录名和用户拥有的权限必须与他们在原始服务器上的权限相同。  

- 目标上的数据库 ID 最好与源上的数据库 ID 相同。 但是，如果它们不相同，当跟踪中出现 **DatabaseName** 时，将根据它进行匹配。

- 跟踪数据中包含的每个登录名的默认数据库必须设置（在目标服务器上）为该登录名相应的目标数据库。 例如，要重播的跟踪数据包含登录名 **Fred** 在原始 **实例上** Fred_Db [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]数据库中的活动。 因此，在目标服务器上，必须将登录名 **Fred** 的默认数据库设置为与 **Fred_Db** 匹配的数据库（即使数据库名称不同）。 若要设置登录名的默认数据库，请使用 `sp_defaultdb` 系统存储过程。  
  
 重播与不存在的或不正确的登录名相关的事件会导致重播错误，但重播操作会继续。  
  
## <a name="see-also"></a>另请参阅

- [SQL Server 分布式重播](../../tools/distributed-replay/sql-server-distributed-replay.md)
- [Distributed Replay 安全性](../../tools/distributed-replay/distributed-replay-security.md)
- [安装 Distributed Replay - 概述](../../tools/distributed-replay/install-distributed-replay-overview.md)