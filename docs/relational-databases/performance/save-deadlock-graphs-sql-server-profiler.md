---
title: 保存死锁图形 (SQL Server Profiler) | Microsoft Docs
description: 了解如何使用 SQL Server Profiler 保存死锁图形。 Deadlock Graph 事件以 XML 文件形式保存。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- deadlocks [SQL Server], saving deadlock graphs
- graphs [SQL Server]
- saving deadlock graphs
ms.assetid: bf1fc906-abd6-4a89-842e-da0d66b2defe
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 15fd61803ab64355672c792c628aa8d9b3e7bea6
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505032"
---
# <a name="save-deadlock-graphs-sql-server-profiler"></a>保存死锁图形 (SQL Server Profiler)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍了如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]保存 Deadlock Graph 事件。 Deadlock Graph 事件以 XML 文件形式保存。  
  
## <a name="save-deadlock-graph-events-separately"></a>单独保存死锁图形事件  
  
1. 在“文件”菜单上，选择“新建跟踪”，再连接到 SQL Server 实例 。  
  
     此时，将显示 **“跟踪属性”** 对话框。  
  
    > [!NOTE]  
    >  如果选择“建立连接后立即开始跟踪”，则“跟踪属性”对话框不会出现，而是开始跟踪 。 要关闭此设置，请在“工具”菜单上选择“选项”，再清除“建立连接后立即开始跟踪”复选框  。  
  
2. 在 **“跟踪属性”** 对话框内的 **“跟踪名称”** 框中键入跟踪的名称。  
  
3. 在“使用模板”列表中，选择一个跟踪所基于的跟踪模板。 如果不想使用模板，请选择“无”。  
  
4. 执行下列操作之一：  
  
    -   要将跟踪内容捕获到文件，请选中“保存到文件”复选框。 指定 **“设置最大文件大小”** 的值。  
  
         也可以选中 **“启用文件滚动更新”** 复选框和 **“服务器处理跟踪数据”** 复选框。 
  
    -   要将跟踪内容捕获到数据库表，请选中“保存到表”复选框。  
  
         也可以选择“设置最大行数”，指定一个值。  
  
5. 根据需要，可以选中 **“启用跟踪停止时间”** 复选框，再指定停止日期和时间。 
  
6. 选择“事件选择”选项卡。  
  
7. 在“事件”数据列中，展开“锁定”事件类别，然后选中“死锁图形”复选框  。 如果未显示“锁定”事件类别，请选中“显示所有事件”复选框以显示该类别 。  
  
     “事件提取设置”选项卡将添加到“跟踪属性”对话框中 。  
  
8. 在“事件提取设置”选项卡上，选择“单独保存死锁 XML 事件” 。  
  
9. 在“另存为”对话框中，输入想用于存储死锁图形事件的文件的名称。  
  
10. 选择“单个文件中的所有死锁 XML 批”，将所有死锁图形事件保存在一个 XML 文件中。 或者选择“不同文件中的每个死锁 XML 批”，为每个死锁图形创建一个新的 XML 文件。  
  
 保存死锁文件后，可在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中打开该文件。 有关详细信息，请参阅[打开、查看和打印死锁文件 (SQL Server Management Studio)](../../relational-databases/performance/open-view-and-print-a-deadlock-file-sql-server-management-studio.md)。  
  
## <a name="see-also"></a>另请参阅  
 [使用 SQL Server Profiler 分析死锁](../../tools/sql-server-profiler/analyze-deadlocks-with-sql-server-profiler.md)  
  
  
