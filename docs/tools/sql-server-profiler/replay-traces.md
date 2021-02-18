---
title: 重播跟踪
titleSuffix: SQL Server Profiler
description: 了解如何从一台计算机上重播 SQL Server Profiler 数据，以及如何在重播时使用断点和模拟用户连接来解决问题。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: da958d3c-7f3e-44c9-aecc-8a9493bea7c0
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 52da7b97159ed6c7ca85d63afab6b6b3dbb92934
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335870"
---
# <a name="replay-traces"></a>重播跟踪

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

重播是重现在跟踪内捕获的活动的功能。 在创建或编辑跟踪时，可以将跟踪保存到文件中并在以后重播。 可以使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 从一台计算机重播跟踪活动。 对于大型工作负荷，可使用分布式重播实用工具从多台计算机重播跟踪数据。  
  
 本节描述如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]的重播功能。 有关分布式重播实用工具的详细信息，请参阅 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)。  
  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 具有多线程播放引擎，能模拟用户连接和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 重播对于解决应用程序或进程问题是很有用的。 当标识出问题并进行了纠正后，请对纠正后的应用程序或进程运行找出潜在问题的跟踪。 然后，重播原始跟踪并比较结果。  
  
 跟踪重播支持通过使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]“重播”菜单上的“切换断点”和“运行至光标处”选项来进行调试。 由于这些选项可以将跟踪重播打断为较短的段以便进行增量分析，因此尤其改善了对长脚本的分析。  
  
 有关重播跟踪所需权限的信息，请参阅 [运行 SQL Server Profiler 所需的权限](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)。  
  
## <a name="in-this-section"></a>本节内容  
  
|主题|说明|  
|-----------|-----------------|  
|[重播要求](../../tools/sql-server-profiler/replay-requirements.md)|说明跟踪定义必须包含的事件，以便它能使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]进行重播。|  
|[重播选项 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-options-sql-server-profiler.md)|说明在 **的** “重播配置” [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]对话框中可以设置的选项。|  
|[重播跟踪的注意事项 (SQL Server Profiler)](../../tools/sql-server-profiler/considerations-for-replaying-traces-sql-server-profiler.md)|说明不能使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 重播的跟踪事件以及重播跟踪对服务器性能的影响。|  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 分布式重播](../../tools/distributed-replay/sql-server-distributed-replay.md)  
  
  
