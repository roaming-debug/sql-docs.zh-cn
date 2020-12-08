---
title: 设置 SQL Server 数据库警报 (Windows) | Microsoft Docs
description: 了解如何创建在系统监视器计数器达到阈值时引发的警报。 作为响应，系统监视器可以启动应用程序。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- alerts [SQL Server], creating
ms.assetid: 65d2c5c1-921f-4eff-9ef7-149170ab61e8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 60e50e5680aebd9db384a8f4ace56ee5687d640a
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504982"
---
# <a name="set-up-a-sql-server-database-alert-windows"></a>设置 SQL Server 数据库警报 (Windows)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  可以使用系统监视器来创建在系统监视器计数器达到阈值时引发的警报。 作为对警报的响应，系统监视器会启动一个应用程序，例如为处理警报情况而编写的自定义应用程序。 例如，您可以创建在死锁数超过特定值时将会引发的警报。 
  
 此外，还可以使用 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理来定义警报。 有关详细信息，请参阅 [“警报”](../../ssms/agent/alerts.md)。  
  
## <a name="set-up-a-sql-server-database-alert"></a>设置 SQL Server 数据库警报  
  
1. 在“性能”窗口的导航树中，展开“性能日志和警报” 。  
  
2. 右键单击“警报”，再选择“新建警报设置” 。
  
3. 在“新建警报设置”对话框中，键入新警报的名称，再选择“确定” 。  
  
4. 在新建警报对话框的“常规”选项卡上，添加一个“注释” 。 选择“添加”，向警报添加计数器。  
  
     所有警报必须至少有一个计数器。  
  
5. 在“添加计数器”对话框中，选择“性能对象”列表中的 SQL Server 对象 。 从“从列表中选择计数器”选择一个计数器。  
  
6. 要为警报添加计数器，请选择“添加”。 可以继续添加计数器，也可以选择“关闭”返回到新建警报对话框。  
  
7. 在新建警报对话框的“将触发警报，如果值是”列表中，选择“超过”或者“低于”  。 然后在“限制”中输入一个阈值。  
  
     当计数器的值大于或小于阈值时（取决于选择的是“超过”还是“低于”），可生成警报。  
  
8. 在 **“数据采样间隔”** 框中，设置采样频率。  
  
9. 在 **“操作”** 选项卡上，设置每次触发警报时要执行的操作。  
  
10. 在 **“计划”** 选项卡上，设置警报扫描的开始和停止计划。  
  
## <a name="see-also"></a>另请参阅  
 [创建 SQL Server 数据库警报](../../relational-databases/performance-monitor/create-a-sql-server-database-alert.md)  
  
  
