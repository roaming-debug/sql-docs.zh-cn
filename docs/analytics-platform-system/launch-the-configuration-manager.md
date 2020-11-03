---
title: 启动 Configuration Manager
description: 有关为分析平台系统设备启动 Configuration Manager 工具的说明。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: be69331ec9f9daa091035d6ef21142c4f40d6a84
ms.sourcegitcommit: 442fbe1655d629ecef273b02fae1beb2455a762e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235164"
---
# <a name="launch-the-configuration-manager-in-analytics-platform-system"></a>在分析平台系统中启动 Configuration Manager
本主题提供有关为分析平台系统设备启动 **Configuration Manager** 的说明。  
  
## <a name="before-you-begin"></a>开始之前  
  
### <a name="prerequisites"></a>先决条件  
分析平台系统 **Configuration Manager** 只能由设备域管理员运行。 若要运行此工具，需要设备管理员的密码。 若要创建其他的 AP 管理员，请参阅 [创建 Ap 域管理员 &#40;ap&#41;](create-an-aps-domain-administrator-aps.md)。  
  
## <a name="launch-the-configuration-manager-tool"></a><a name="Accessing"></a>启动 Configuration Manager 工具  
若要运行 Configuration Manager，请使用远程桌面连接到 PDW 控件节点 ( **_PDW_region_ -CTL01** ) 节点，并以 _appliance_domain_**\Administrator** 身份登录。 启动 **Configuration Manager** 程序时，请使用 "以 **管理员身份运行** " 选项，以确保使用您的管理员凭据。  
  
#### <a name="to-launch-from-a-browser-window"></a>从浏览器窗口启动  
  
1.  打开浏览器并导航到该目录 `C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100` 。  
  
2.  右键单击 `dwconfig.exe` ，然后单击 "以 **管理员身份运行** "。  
  
#### <a name="to-launch-from-a-command-prompt"></a>在命令提示符下启动  
  
1.  在桌面上，打开 " **开始** " 菜单，单击 " **程序** "，单击 " **附件** "，右键单击 " **命令提示符** "，然后单击 "以 **管理员身份运行** "。  
  
2.  在命令提示符下，输入以下命令以更改目录： `cd /d "C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100"` 。  
  
3.  在命令提示符处，输入 `dwconfig.exe`。  
  
开始 **Configuration Manager** 后，你将看到左窗格中列出的所有可用功能。 本部分的其余部分讨论如何执行工具中的每个可用操作。  
  
若要关闭并退出 **Configuration Manager** ，请单击任何屏幕右下角的 " **退出** "。  
  
![显示设备拓扑的 Microsoft Analytics Platform System Configuration Manager "对话框的屏幕截图。](./media/launch-the-configuration-manager/SQL_Server_PDW_DWConfig_ApplTop.png "SQL_Server_PDW_DWConfig_ApplTop")  
  
## <a name="see-also"></a>另请参阅  
[使用管理控制台 &#40;分析平台系统来监视设备&#41;](monitor-the-appliance-by-using-the-admin-console.md)  
  
