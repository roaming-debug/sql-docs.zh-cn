---
title: 设置全局跟踪选项
titleSuffix: SQL Server Profiler
description: 了解如何设置全局跟踪选项以及 SQL Server Profiler 的特定实例可应用于所有跟踪的选项。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 2854608a-c3c7-4eb8-b567-034bfec4b1a9
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 0394038ecd114f30c1a32005e126a4aa20b548a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345817"
---
# <a name="set-global-trace-options-sql-server-profiler"></a>设置全局跟踪选项 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题介绍了如何设置应用于随特定的 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]实例创建的所有跟踪的选项。  
  
### <a name="to-set-global-trace-options"></a>设置全局跟踪选项  
  
1.  在“工具”  菜单上，单击“选项” 。  
  
2.  在“常规选项”对话框中，单击“选择字体”修改显示选项，再单击“确定”。  
  
3.  根据需要选择 **“建立连接后立即开始跟踪”** 。  
  
4.  根据需要选择 **“当提供程序版本发生更改时更新跟踪定义”** 。 建议选择该选项，且默认情况下选择该选项。 选择该选项后，跟踪定义会自动更新为执行跟踪的服务器的当前版本。  
  
5.  根据需要指定服务器管理滚动更新文件的方式：  
  
    -   选择 **“不作提示，依次加载所有滚动更新文件”** 在重播期间自动加载滚动更新文件。  
  
    -   选择 **“加载滚动更新文件之前进行提示”** 在重播期间控制滚动更新文件。  
  
    -   选择 **“从不加载后续滚动更新文件”** 每次只重播一个文件。  
  
6.  根据需要设置重播选项：  
  
    -   **默认重播线程数** ：控制重播期间使用的处理器线程数。 线程数越多，重播越快，但这会导致重播期间服务器的性能降低。 建议将该项设置为 **4**。 下表列出了可用选项：  
  
        |值|描述|  
        |-----------|-----------------|  
        |**2**|最小值。 使用两个线程重播。|  
        |**4**|默认值。|  
        |**255**|最大值。 设置为最大值会影响其他进程的性能。|  
  
    -   “默认 Health Monitor 等待间隔(秒)”设置重播线程可以阻塞其他进程的最长时间（以秒为单位）。 下表说明了这些值。  
  
        |值|描述|  
        |-----------|-----------------|  
        |**0**|最小值。 设置为 **0** 表示 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 永远不会停止阻塞进程。|  
        |**3600**|默认值。 允许不超过 **3600** 秒（1 小时）的阻塞进程。|  
        |**86400**|最大值。 允许不超过 **86400** 秒（一天）的阻塞进程。|  
  
    -   “默认 Health Monitor 轮询间隔(秒)”设置阻塞进程的轮询重播线程的频率。 下表说明了这些值。  
  
        |值|说明|  
        |-----------|-----------------|  
        |**1**|最小值。 设置为 **1** 表示 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 每秒针对阻塞进程轮询一次。|  
        |**60**|默认值。 每分钟针对阻塞进程轮询一次。|  
        |**86400**|最大值。 每 **86400** 秒（一天）针对阻塞进程轮询一次。|  
  
## <a name="see-also"></a>另请参阅  
 [设置跟踪显示默认值 (SQL Server Profiler)](../../tools/sql-server-profiler/set-trace-display-defaults-sql-server-profiler.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
