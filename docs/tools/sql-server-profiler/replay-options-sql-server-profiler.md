---
title: 重播选项
titleSuffix: SQL Server Profiler
description: 浏览 SQL Server Profiler 重播捕获的跟踪时使用的设置。 了解如何使用“重播配置”对话框来调整设置。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 58761a25-a84f-4a90-9c61-97700bc5ad9c
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: af342941ca797d9054492e59a5004096f901bf69
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353377"
---
# <a name="replay-options-sql-server-profiler"></a>重播选项 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]重播捕获的跟踪之前，请在 **“重播配置”** 对话框中指定重播选项。 若要启动此对话框，请在 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]中打开重播跟踪文件或表，然后在 **“重播”** 菜单上单击 **“开始”** 。 有关重播跟踪需要哪些权限的信息，请参阅 [Permissions Required to Run SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)。  
  
 本主题介绍使用 **“重播配置”** 对话框指定的选项。  
  
> [!NOTE]  
>  建议使用分布式重播实用工具重播密集型 OLTP 应用程序（具有大量活动并发连接或高吞吐量）。 分布式重播实用工具可以从多台计算机重播跟踪数据，并更好地模拟任务关键型工作负荷。 有关详细信息，请参阅 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)。  
  
## <a name="basic-replay-options"></a>基本重播选项  
 **重播服务器**  
 此服务器是要对其重播跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。 此服务器必须遵循 [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md)中说明的重播要求。  
  
 **保存到文件**  
 用于写入重播跟踪的结果以供将来查看的输出文件。 默认情况下， [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 只在屏幕上显示重播跟踪的结果。  
  
 **保存到表**  
 用于写入重播跟踪的结果以供将来查看的数据库表。  
  
 **重播线程数**  
 指定要并发使用的重播线程数。 此数值越高，重播过程中占用的资源越多，但重播速度也越快。 使用多个线程时，不能完全保持事件的排序顺序。  
  
 **按跟踪事件的顺序重播事件**  
 使您可以使用调试方法，如逐步重播每个跟踪。 如果未选中此选项，重播将不能保证重播事件的顺序与原先捕获事件的顺序一致。  
  
 **使用多个线程重播事件**  
 优化性能并禁用调试。 将按照对某一特定的服务器进程 ID (SPID) 记录事件的顺序来重播事件，但是不能保证 SPID 的排序顺序。  
  
 **显示重播结果**  
 显示重播的结果。 这是默认选项。 如果正在重播的跟踪非常大，可能需要禁用此选项以节省磁盘空间。  
  
> [!NOTE]  
>  为了获得最佳的重播性能，建议选择使用多线程重播事件，不要选择显示重播结果。  
  
## <a name="advanced-replay-options"></a>高级重播选项  
 **重播系统 SPID**  
 重播所有 SPID。 这是默认选项。  
  
 **仅重播一个 SPID**  
 重播从列表中选择的 SPID 编号。  
  
 **按日期和时间限制重播**  
 重播指定的“开始时间”和“结束时间”内的跟踪。  
  
 **Health Monitor 等待间隔**  
 设置允许进程运行的时间，经过此时间段后 Health Monitor 将终止该进程。  
  
 **Health Monitor 轮询间隔**  
 设置 Health Monitor 轮询终止候选项的频率。  
  
 **启用 SQL Server 阻塞的进程监视器**  
 设置阻塞进程监视器搜索已阻塞的进程或正在阻塞的进程的频率。  
  
## <a name="about-the-health-monitor"></a>关于 Health Monitor  
 Health Monitor 是一个应用程序线程，用于监视重播跟踪过程中涉及的模拟进程，并结束在重播过程中阻塞的那些进程。 在“重播配置”对话框的“高级重播选项”选项卡中，可以指定 Health Monitor 在结束阻塞进程之前应等待的秒数（**Health Monitor 等待间隔**）。 如果将此间隔设置为 0，则在重播跟踪过程中，Health Monitor 永远不会结束模拟阻塞进程。  
  
## <a name="see-also"></a>另请参阅  
 [重播跟踪](../../tools/sql-server-profiler/replay-traces.md)   
 [重播要求](../../tools/sql-server-profiler/replay-requirements.md)   
 [重播跟踪的注意事项 (SQL Server Profiler)](../../tools/sql-server-profiler/considerations-for-replaying-traces-sql-server-profiler.md)  
  
  
