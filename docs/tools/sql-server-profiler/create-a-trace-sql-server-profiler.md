---
title: 创建跟踪
titleSuffix: SQL Server Profiler
description: 了解如何通过创建跟踪在 SQL Server Profiler 中捕获事件数据。 了解可为跟踪指定的各种选项。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 0302fa6d-d2b5-43fe-ad70-7a337575b112
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 08/01/2016
ms.openlocfilehash: b4da55a4a4af843074eef50ce50f12c8b891d829
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349401"
---
# <a name="create-a-trace-sql-server-profiler"></a>创建跟踪 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本主题介绍如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 来创建跟踪。  
  
### <a name="to-create-a-trace"></a>创建跟踪  
  
1.  在 **“文件”** 菜单上，单击 **“新建跟踪”** ，并连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。  
  
     此时，将显示 **“跟踪属性”** 对话框。  
  
    > **注意：** 如果选中 “建立连接后立即开始跟踪” ，就不会显示 “跟踪属性” 对话框，而是直接开始跟踪。 要关闭此设置，请在“工具”菜单上，单击“选项”，再清除“建立连接后立即开始跟踪”复选框。  
  
2.  在 **“跟踪名称”** 框中，键入跟踪的名称。  
  
3.  在 **“使用模板”** 列表中，为此跟踪选择一个跟踪模板；如果不想使用模板，请选择 **“空白”** 。  
  
4.  若要保存跟踪结果，请执行下列操作之一：  
  
    -   单击“保存到文件”，将跟踪内容捕获到文件中。 指定 **“设置最大文件大小”** 的值。 默认值为 5 MB。  
  
         或者，选择 **“启用文件滚动更新”** ，以便当文件大小达到最大值时自动创建新文件。 也可以选择 **“服务器处理跟踪数据”**，由正在运行跟踪的服务而不是客户端应用程序来处理跟踪数据。 在服务器处理跟踪数据时，即使是在压力较大的情况下也不会跳过事件，但是服务器性能可能会受到影响。  
  
    -   单击 **“保存到表”** 将跟踪捕获到数据库表中。  
  
         根据需要，可以单击 **“设置最大行数”** ，并指定值。  
  
    > **警告！！** 如果不将跟踪结果保存到文件或表中，则当 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 打开时可以查看跟踪。 但是，在停止跟踪并关闭 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]之后会丢失跟踪结果。 为了避免这种丢失跟踪结果的情况，可以在关闭 **之前单击** “文件” **菜单上的** “保存” [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]来保存结果。  
  
5.  根据需要，可以选中 **“启用跟踪停止时间”** 复选框，再指定停止日期和时间。  
  
6.  若要添加或删除事件、数据列或筛选器，请单击“事件选择”选项卡。有关详细信息，请参阅：[指定跟踪文件的事件和数据列 (SQL Server Profiler)](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md)  
  
7.  单击 **“运行”** 以启动跟踪。  
  
## <a name="see-also"></a>另请参阅  
 [运行 SQL Server Profiler 所需的权限](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)   
 [SQL Server Profiler 模板和权限](../../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md)   
 [SQL Server 事件探查器](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [将跟踪与 Windows 性能日志数据关联 (SQL Server Profiler)](./correlate-a-trace-with-windows-performance-log-data.md)  
  
