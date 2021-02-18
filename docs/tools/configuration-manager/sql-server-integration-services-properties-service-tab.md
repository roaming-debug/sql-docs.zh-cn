---
title: SQL Server Integration Services 属性（“服务”选项卡）
description: 了解“集成服务属性”对话框中“服务”选项卡上的选项，例如二进制文件路径、主机名以及启动模式。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 37f0acd9-c96f-48fd-9b53-2ca0097af242
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 4a24a62880a263dc6babd9708b9f4d7e0c3a8a05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345139"
---
# <a name="sql-server-integration-services-properties-service-tab"></a>SQL Server Integration Services 属性（“服务”选项卡）
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]“属性”**对话框中的“服务”选项卡可以查看或指定下列选项** 。  
  
## <a name="options"></a>选项  
 **二进制路径**  
 显示此服务所使用的程序文件的位置。  
  
 **错误控制**  
 1 指示 `SERVICE_ERROR_NORMAL`。 如果在计算机启动过程中无法启动该服务，则启动程序会记录该错误并显示一个弹出消息框，但仍会继续执行启动操作。 此值不能更改。  
  
 **退出代码**  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 错误代码，定义了在启动或停止服务时遇到的任何问题。 如果错误是此类表示的服务所特有的，并且 **ServiceSpecificExitCode** 属性中包含该错误的相关信息，则此属性将设置为 **ERROR_SERVICE_SPECIFIC_ERROR** (1066)。 当服务运行时，会将此值设置为 NO_ERROR (0)；而当服务正常终止时，会再次对它进行设置。  
  
 **Host Name**  
 显示运行 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服务的计算机或群集的名称。  
  
 **名称**  
 指示该服务的显示名称。  
  
 **进程 ID**  
 显示 Windows 进程 ID。  
  
 **SQL 服务类型**  
 显示用于调用进程的服务的类型。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装有多种服务。  
  
 **启动模式**  
 对此服务设置以下选项：  
  
-   手动：计算机启动时，此服务不自动启动。 您必须使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器或其他工具来启动该服务。  
  
-   自动：计算机启动时，此服务将尝试启动。  
  
-   禁用：此服务无法启动。  
  
 **State**  
 指示此服务是正在运行、已停止还是已禁用。 “...”指明状态更改是挂起的。  
  
  
