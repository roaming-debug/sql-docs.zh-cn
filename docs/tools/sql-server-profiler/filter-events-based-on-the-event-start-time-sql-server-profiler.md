---
title: 基于事件开始时间筛选事件
titleSuffix: SQL Server Profiler
description: 在跟踪期间按开始时间筛选事件。 了解如何在 SQL Server Profiler 中设置基于事件开始时间的筛选器。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: e965579e-d006-41a3-89ec-cfd5398c67d2
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: bb6e0be223667f8758f93fad8eaa59c67362ae4e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345796"
---
# <a name="filter-events-based-on-the-event-start-time-sql-server-profiler"></a>基于事件开始时间筛选事件 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题介绍了如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]来基于事件开始时间筛选跟踪事件。  
  
### <a name="to-filter-an-event-based-on-the-event-start-time"></a>基于事件开始时间筛选事件  
  
1.  在 **“文件”** 菜单上，单击 **“新建跟踪”** ，再连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例。  
  
     将出现“跟踪属性”对话框。 **“跟踪属性”** 对话框。  
  
    > [!NOTE]  
    >  如果选择“建立连接后立即开始跟踪”，则不会显示“跟踪属性”对话框，而是开始跟踪。 若要关闭此设置，请在“工具”菜单上，单击“选项”，然后清除“建立连接后立即开始跟踪”复选框。  
  
2.  在 **“跟踪名称”** 框中，键入跟踪的名称。  
  
3.  在“使用模板”名称列表中，选择跟踪模板。  
  
4.  为跟踪结果指定目标（可选）。  
  
5.  在“事件选择”选项卡上，单击“StartTime”列标题。 也可以右键单击列标题，然后单击“编辑列筛选器”打开“编辑筛选器”对话框。  
  
6.  展开 **“大于”** 或 **“小于”** ，然后在比较运算符下方的字段中输入 **datetime** 值。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
