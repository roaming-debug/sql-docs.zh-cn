---
title: 每次重播一个事件
titleSuffix: (SQL Server Profiler
description: 了解如何通过单步执行重播跟踪文件或表，在 SQL Server Profiler 中每次重播一个跟踪事件。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 220fb192-9636-41a2-b15c-62af6cab8fff
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: b9000da1493b61383f301308d5269fe4349c2e76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353379"
---
# <a name="replay-a-single-event-at-a-time-sql-server-profiler"></a>每次重播一个事件 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题介绍如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]每次重播跟踪文件或表中的一个事件。  
  
### <a name="to-replay-a-single-event-at-a-time"></a>每次重播一个事件  
  
1.  打开要重播的跟踪文件或跟踪表。 有关详细信息，请参阅 [打开跟踪文件 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md) 或 [打开跟踪表 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)一起提供的预定义优化模板。  
  
     请确保打开的跟踪文件或跟踪表包含重播所需的事件类。 有关详细信息，请参阅 [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md)。  
  
2.  在 **“重播”** 菜单上，单击 **“步骤”** ，然后连接到要在其上重播跟踪的服务器实例。  
  
3.  在 **“重播配置”** 对话框中，验证设置，再单击 **“确定”** 。 有关指定“重播配置”对话框中的设置的详细信息，请参阅 [重播跟踪文件 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-trace-file-sql-server-profiler.md) 或 [重播跟踪表 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-trace-table-sql-server-profiler.md)。  
  
4.  若要重播第一个事件，请单击 **“重播配置”** 对话框中的 **“确定”** 。  
  
5.  若要重播后续事件，请在 **“重播”** 菜单上，单击 **“步骤”** ，或按 F10。 对每个事件重复单击 **“步骤”** 或按 F10。  
  
## <a name="see-also"></a>另请参阅  
 [重播跟踪](../../tools/sql-server-profiler/replay-traces.md)   
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
