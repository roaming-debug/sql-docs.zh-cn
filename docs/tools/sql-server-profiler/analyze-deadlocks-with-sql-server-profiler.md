---
title: 分析死锁
titleSuffix: SQL Server Profiler
description: 通过在 SQL Server Profiler 中重播和显示要分析的死锁事件，以及通过生成等待图形，确定死锁的原因。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 72d6718f-501b-4ea6-b344-c0e653f19561
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/03/2017
ms.openlocfilehash: 373bcce8731dad1375efd9a4c2196ab184b38954
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349460"
---
# <a name="analyze-deadlocks-with-sql-server-profiler"></a>使用 SQL Server Profiler 分析死锁

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 确定死锁的原因。 当 SQL Server 中某组资源的两个或多个线程或进程之间存在循环的依赖关系时，将会发生死锁。 使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]，可以创建记录、重播和显示死锁事件的跟踪以进行分析。  
  
 若要跟踪死锁事件，请将 **Deadlock graph** 事件类添加到跟踪。 此事件类会在跟踪中的 **TextData** 数据列中填充有关死锁中涉及的进程和对象的 XML 数据。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 可将 XML 文档提取到死锁 XML (.xdl) 文件，你稍后可在 SQL Server Management Studio 中查看该文件。 您可以配置 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ，将 **Deadlock graph** 事件提取到一个包含了所有 **Deadlock graph** 事件的文件中，或提取到多个单独的文件中。 可以通过下列任一方法进行提取：  
  
-   在配置跟踪时，使用 **“事件提取设置”** 选项卡。请注意，只有在 **“事件选择”** 选项卡上选择了 **Deadlock graph** 事件，才会出现此选项卡。  
  
-   使用 **“文件”** 菜单上的 **“提取 SQL Server 事件”** 选项。  
  
-   通过右键单击特定事件并选择“提取事件数据”，也可以提取并保存各个事件。  
  
## <a name="deadlock-graphs"></a>死锁图形  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 和 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 使用死锁等待图形描述死锁。 此死锁等待图形中包含进程节点、资源节点以及表示进程和资源之间关系的边。 等待图形的组件的定义如下表所示：  
  
 进程节点  
 执行任务的线程。例如，INSERT、UPDATE 或 DELETE。  
  
 资源节点  
 数据库对象。例如，表、索引或行。  
  
 Microsoft Edge  
 进程和资源之间的关系。 当进程等待资源时，将出现 **request** 边。 当资源等待进程时，将出现 **owner** 边。 边说明中包括了锁模式。 例如，模式：X。  
  
## <a name="deadlock-process-node"></a>死锁进程节点  
 在等待图形中，进程节点包含有关进程的信息。 下表介绍了进程的组件。  
  
|组件|定义|  
|---------------|----------------|  
|服务器进程 ID|服务器进程标识符 (SPID)，即服务器给拥有锁的进程分配的标识符。|  
|服务器批 ID|服务器批标识符 (SBID)。|  
|执行上下文 ID|执行上下文标识符 (ECID)。 与指定 SPID 相关联的给定线程的执行上下文 ID。<br /><br /> ECID = {0, 1, 2, 3, *...n*}，其中 0 始终表示主或父线程，并且 {1, 2, 3, *...n*} 表示子线程。|  
|死锁优先级|进程的死锁优先级 有关可能值的详细信息，请参阅 [SET DEADLOCK_PRIORITY (Transact-SQL)](../../t-sql/statements/set-deadlock-priority-transact-sql.md)。|  
|已用日志|进程所使用的日志空间量。|  
|所有者 ID|正在使用事务并且当前正在等待锁的进程的事务 ID。|  
|事务描述符|指向描述事务状态的事务描述符的指针。|  
|输入缓冲区|当前进程的输入缓冲区。定义了事件的类型和正在执行的语句。 可能的值包括：<br /><br /> **语言**<br /><br /> **RPC**<br /><br /> 无|  
|语句|语句类型。 可能的值包括：<br /><br /> **NOP**<br /><br /> **SELECT**<br /><br /> **UPDATE**<br /><br /> **INSERT**<br /><br /> **DELETE**<br /><br /> **Unknown**|  
  
## <a name="deadlock-resource-node"></a>死锁资源节点  
 在死锁中，两个进程都在等待对方占用的资源。 在死锁图形中，资源显示为资源节点。  
  
  
