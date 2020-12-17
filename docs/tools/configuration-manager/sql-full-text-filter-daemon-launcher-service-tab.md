---
title: SQL 全文筛选器后台程序启动器（“服务”选项卡）
description: 了解有关 SQL Server 全文搜索使用的 SQL 全文筛选器后台程序启动器的信息。 了解“属性”对话框的“服务”选项卡。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 6aad7ebe-c4be-4d37-8536-61502f51faa2
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c682e1e3598750cbbcc2e0e508b427ed88dc1e1f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463768"
---
# <a name="sql-full-text-filter-daemon-launcher-service-tab"></a>SQL 全文筛选器后台程序启动器（“服务”选项卡）
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]开始， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 全文将使用 SQL 全文筛选器后台程序启动器 (FDHOST Launcher) 服务。 如果使用全文搜索，则必须运行此服务。 有关筛选器后台程序主机进程的信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的“全文搜索体系结构”。  
  
 使用“SQL 全文筛选器后台程序启动器属性”的“属性”对话框上的“服务”选项卡，可以查看或指定以下选项。  
  
## <a name="options"></a>选项  
 **二进制路径**  
 列出此服务所使用的程序文件的位置。  
  
 **错误控制**  
 1 指示 `SERVICE_ERROR_NORMAL`。 如果在计算机启动过程中无法启动该服务，则启动程序会记录该错误并显示一个弹出消息框，但仍会继续执行启动操作。 此值不能更改。  
  
 **退出代码**  
 当发生错误时，错误号显示在此信息框中。 您可以通过在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 知识库中搜索该号码获取相关信息来排除故障，或将该错误号提供给技术支持人员以获得帮助。  
  
 **Host Name**  
 显示运行 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的计算机或群集的名称。  
  
 **名称**  
 指示该服务的显示名称。  
  
 **进程 ID**  
 显示 Windows 进程 ID。  
  
 **SQL 服务类型**  
 显示用于调用进程的服务的类型。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装有多种服务。  
  
 **启动模式**  
 对此服务设置以下选项：  
  
-   手动：计算机启动时，此服务不自动启动。 您必须使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器或其他工具来启动该服务。  
  
-   自动：计算机启动时，此服务将尝试启动。  
  
-   禁用：此服务无法启动。  
  
 **State**  
 指示此服务是正在运行、已停止还是已禁用。 “...”指明状态更改是挂起的。  
  
  
