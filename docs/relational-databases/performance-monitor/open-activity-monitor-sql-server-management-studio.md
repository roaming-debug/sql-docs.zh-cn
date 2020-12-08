---
title: 打开活动监视器 (SSMS)
description: 了解如何打开 SQL Server Management Studio 中的活动监视器。 活动监视器查询受监视的实例以获取要显示的信息。
ms.custom: seo-dt-2019
ms.date: 08/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Activity Monitor [SQL Server], setting the refresh interval
- refresh interval for Activity Monitor
- Activity Monitor [SQL Server], opening
- opening Activity Monitor
ms.assetid: 0a6eeb16-f02b-479d-9a60-543e40ebf46b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5569286a31b9897b8fcca0eafea6830593d05183
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505987"
---
# <a name="open-activity-monitor-in-sql-server-management-studio-ssms"></a>打开 SQL Server Management Studio (SSMS) 中的活动监视器
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   
 活动监视器将在被监视的实例上运行查询以获取有关活动监视器显示窗格的信息。 当刷新间隔设置为小于 10 秒时，运行这些查询所用的时间可能会对服务器性能产生影响。  
  
  
##  <a name="check-your-permissions"></a><a name="Permissions"></a> 检查你的权限！  
 若要查看实际的活动，必须拥有 VIEW SERVER STATE 权限。 若要查看活动监视器的“数据文件 I/O”部分，除了 VIEW SERVER STATE 之外，您还必须具有 CREATE DATABASE、ALTER ANY DATABASE 或 VIEW ANY DEFINITION 权限。  
  
 若要终止进程，用户必须是 sysadmin 或 processadmin 固定服务器角色的成员。  
  
  
## <a name="open-activity-monitor"></a>打开活动监视器  

### <a name="keyboard-shortcut"></a>键盘快捷键  
 - 键入“Ctrl+Alt+A”可随时打开活动监视器。

 >**提示！** 将鼠标悬停在 SSMS 中的任何图标上方可了解该图标是什么，以及哪个键盘快捷键可激活它！

### <a name="toolbar"></a>工具栏

在标准工具栏上，单击“活动监视器”图标。 该图标位于中间，就在“撤消/重做”按钮的右边。
![Activity_Monitor_icon](../../relational-databases/performance-monitor/media/activity-monitor-icon.png)  
  
如果尚未连接到想要监视的 SQL Server 的实例，请完成“连接到服务器”对话框。
  
## <a name="launch-activity-monitor-and-object-explorer-on-startup"></a>在启动时启动活动监视器和对象资源管理器
  
1.  从 **“工具”** 菜单中，单击 **“选项”** 。  
  
2.  在“选项”对话框中，展开“环境”，再选择“启动”。    
  
3.  在“启动时”下拉列表中，选择“打开对象资源管理器和活动监视器”。   

4.  单击“确定”。

![open_object_explorer](../../relational-databases/performance-monitor/media/open-object-explorer.png)
  
  
## <a name="set-the-activity-monitor-refresh-interval"></a>设置活动监视器刷新间隔  
  
1.   打开活动监视器。  
  
2.   右键单击“概述”，选择“刷新间隔”，然后选择活动监视器获取新实例信息所用的间隔。  
  
  
