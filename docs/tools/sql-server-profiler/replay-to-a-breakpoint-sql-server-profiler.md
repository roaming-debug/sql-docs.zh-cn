---
title: 重播到断点
titleSuffix: SQL Server Profiler
description: 通过设置断点使调试更加容易，以便在特定事件处暂停播放。 使用 SQL Server Profiler 在跟踪文件或表中设置断点。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 3caf751e-df3b-40c7-b5e8-4490ae178e0c
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: cf65d528ca2f141e9af161a602ff6f9e56aacd68
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353345"
---
# <a name="replay-to-a-breakpoint-sql-server-profiler"></a>重播到断点 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题说明如何在您要通过使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]进行重播的跟踪文件或表中设置断点。 在重播跟踪之前，在跟踪文件或表中设置断点可以使您在发生特定事件时暂停重播跟踪。 在重播跟踪时使用断点能够支持调试，因为您可以将长跟踪脚本的重播分解为较短的段以便进行增量分析。  
  
### <a name="to-replay-to-a-breakpoint"></a>重播到断点  
  
1.  打开要重播的跟踪文件或跟踪表。 有关详细信息，请参阅 [打开跟踪文件 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md) 或 [打开跟踪表 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)一起提供的预定义优化模板。  
  
     请确保打开的跟踪文件或跟踪表包含重播所需的事件类。 有关详细信息，请参阅 [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md)。  
  
2.  在跟踪窗口中，单击要用作断点的事件。 可以使用以下三种方法来设置断点：  
  
    -   按 F9。  
  
    -   在 **“重播”** 菜单上，单击 **“切换断点”** 。  
  
    -   右键单击“事件”，再单击“切换断点”。  
  
     选定跟踪事件的旁边将显示一个红色项目符号，表示其为跟踪断点。  
  
     重复此步骤可以设置多个断点。  
  
3.  在 **“重播”** 菜单上，单击 **“启动”** ，然后连接到要重播跟踪的服务器。  
  
4.  在 **“重播配置”** 对话框中，验证设置，再单击 **“确定”** 。  
  
     将启动重播，并在到达断点时暂停。  
  
5.  按 F5 可恢复重播并继续运行直到下一个断点。  
  
6.  重复步骤 5 到跟踪的末尾。  
  
## <a name="see-also"></a>另请参阅  
 [重播至光标处 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-to-a-cursor-sql-server-profiler.md)   
 [重播跟踪](../../tools/sql-server-profiler/replay-traces.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
