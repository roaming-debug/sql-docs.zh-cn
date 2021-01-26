---
title: 连接到其他计算机（SQL Server 配置管理器）| Microsoft Docs
description: 了解如何管理远程计算机的服务。 了解如何使用 SQL Server 配置管理器或 SQL Server Management Studio 执行此任务。
ms.custom: ''
ms.date: 11/19/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- connections [SQL Server], other computers
ms.assetid: c4c1e94f-4f5f-431e-8b5b-d5ff97baf723
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b178e38f6e59499ddf74e9a8aaec037bbaa4d442
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783427"
---
# <a name="scm-services---connect-to-another-computer"></a>SCM 服务 - 连接到其他计算机

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本文介绍如何连接到 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中的另一台计算机。 按照第一个过程执行操作，打开 Windows 的“计算机管理” [!INCLUDE[msCoName](../../includes/msconame-md.md)] 管理控制台 (mmc)，连接到该计算机，然后展开“服务和应用程序”树。 遵循第二个过程，创建一个包含到远程计算机中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器的链接的文件。

> [!NOTE]
> 远程连接时，某些程序无法由配置管理器执行。

若要在其他计算机上启动、停止、暂停或恢复服务，也可以连接到安装有 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]的服务器，右键单击此服务器或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理，然后单击需要的操作。

## <a name="SSMSProcedure"></a>

### <a name="to-connect-to-another-computer-with-windows-computer-management"></a>使用 Windows 的“计算机管理”连接到另一台计算机

1. 右键单击“开始”菜单按钮，然后单击“计算机管理 (本地)” 。
2. 在“操作”菜单上，单击“连接到另一台计算机” 。
3. 在 **“选择计算机”** 对话框中的 **“另一台计算机”** 文本框中，键入想要管理的计算机名，然后单击 **“确定”** 。

   “计算机管理”将显示运行在远程计算机上的服务。 顶级节点更改为“计算机管理 \<*remotecomputer*>”。

4. 在控制台树中，依次展开 **“服务和应用程序”** 、 **“SQL Server 配置管理器”** 来管理远程计算机的服务。

### <a name="to-save-a-link-to-sql-server-configuration-manager-for-another-computer"></a>将链接保存到其他计算机的 SQL Server 配置管理器中

1. 在 **“开始”** 菜单上，单击 **“运行”** 。

2. 在“打开”框中，键入 mmc -a（在 64 位计算机上键入 mmc /32 -a），以作者模式打开 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 管理控制台  。
3. 在“文件”  菜单上，单击“添加/删除管理单元” 。
4. 在“添加/删除管理单元”  窗口中，单击“添加” 。
5. 在“添加独立管理单元”  窗口中，单击“计算机管理”  ，再单击“添加” 。
6. 在 **“计算机管理”** 窗口中，单击 **“其他计算机”** ，键入要管理的远程计算机的名称，再单击 **“完成”** 。
7. 在“添加单独管理单元”  窗口中，单击“关闭” 。
8. 在“添加/删除管理单元”  窗口中，单击“确定” 。
9. 依次展开“计算机管理(\<computer name>)”和“服务和应用程序” 。
10. 右键单击“SQL Server 配置管理器” ，再单击“从此处新建窗口” 。
11. 在“窗口”菜单上，单击“控制台根节点”，切换回第一个窗口，并删除该窗口 。
12. 在“文件”  菜单上，单击“另存为” ，使用文件扩展名为 **.msc** 的相应文件名将文件保存到目标文件夹。 关闭 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 管理控制台。
13. 若要打开目标计算机中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器，请双击此文件。 如果需要，则在桌面上或 **“开始”** 菜单中保存到此文件的链接。

> [!CAUTION]
> 当使用远程计算机中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器时，计算机名称不是显而易见的，因此可能会错误地停止或配置不正确的计算机。 在 **“服务”** 选项卡上，选中 **“主机名”** 复选框以在修改服务前确认计算机名称。

## <a name="see-also"></a>另请参阅

- [在 SQL Server 工具中将 WMI 配置为显示服务器状态](../../ssms/configure-wmi-to-show-server-status-in-sql-server-tools.md)
