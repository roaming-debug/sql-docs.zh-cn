---
title: 安装客户端工具：故障转移群集
description: 了解如何在 SQL Server 故障转移实例上安装客户端工具，例如 SQL Server Management Studio。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: high-availability
ms.topic: how-to
ms.assetid: 3c82d510-9798-46be-bebb-cac9bef56936
author: cawrites
ms.author: chadam
ms.openlocfilehash: 467014114a17d6cafacdaf44f1f7a2c60042721a
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127665"
---
# <a name="install-client-tools-on-a-sql-server-failover-cluster"></a>在 SQL Server 故障转移群集上安装客户端工具
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  诸如 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 的客户端工具是在同一台计算机上的所有实例间公用的共享功能。 它们与支持的、可并行安装的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本兼容。 在某一时刻，节点上只能存在客户端工具的一个版本。  
  
 如果在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 群集的第一个节点上进行安装时安装了 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 客户端工具，它们将自动添加到稍后可能使用“添加节点”功能添加到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例的任何节点。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 联机丛书不会自动添加到使用“添加节点”功能添加到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 群集的其他节点。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 联机丛书手动安装到您希望具有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 联机丛书的本地副本的节点上。  
  
 如果在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 群集的初始安装过程中没有安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 客户端工具，稍后可按下面的过程中所述进行安装。  
  
## <a name="installation-procedures"></a>安装过程  
  
#### <a name="installing-ssnoversion-client-tools-using-the-setup-user-interface"></a>使用安装程序用户界面安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 客户端工具  
  
1.  插入 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装介质， 然后双击根安装文件夹中的 Setup.exe。 若要从网络共享进行安装，请找到共享中的根文件夹，然后双击 Setup.exe。  
  
2.  在“安装”页上，单击“全新” **[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]独立安装或向现有安装添加功能**。 请勿单击“新建” **[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]故障转移群集安装**。  
  
3.  系统配置检查器将验证计算机的系统状态，然后安装程序继续运行。  
  
4.  在“安装类型”页上，单击“执行全新安装” **[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]** 。  
  
5.  在 **“功能选择”** 页上，选择需要安装的工具，然后按照安装过程的剩余步骤操作。  
  
#### <a name="installing-ssnoversion-client-tools-at-the-command-prompt"></a>在命令提示符处安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 客户端工具  
  
1.  若要安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 客户端工具和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 联机丛书，请运行以下命令：Setup.exe/q/Action=Install /Features=Tools  
  
2.  若要只安装基本 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 管理工具，请运行以下命令：Setup.exe/q/Action=Install Features=SSMS。 这将为 [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)]、[!INCLUDE[ssExpress](../../../includes/ssexpress-md.md)]、sqlcmd 实用工具以及 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell 提供程序安装 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] 支持。  
  
3.  若要安装完整的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 管理工具，请运行以下命令：Setup.exe/q/Action=Install /Features=ADV_SSMS。 有关功能的参数值的详细信息，请参阅 [从命令提示符安装 SQL Server 2016](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)。  
  
### <a name="uninstalling-ssnoversion-client-tools"></a>卸载 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 客户端工具  
 它们在“控制面板”的“添加或删除程序”中显示为 **[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]** ，可从此处将其删除。 当使用“删除节点”从故障转移群集中卸载 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例时，并不同时卸载这些客户端组件。  
  
## <a name="see-also"></a>另请参阅  
 [查看和读取 SQL Server 安装程序日志文件](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
