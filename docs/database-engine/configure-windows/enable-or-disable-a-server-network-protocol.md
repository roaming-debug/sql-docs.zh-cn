---
title: 启用或禁用服务器网络协议 | Microsoft Docs
description: 了解如何启用或禁用服务器网络协议。 了解如何使用 SQL Server 配置管理器或 PowerShell 执行此任务。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- network protocols [SQL Server], disabling
- remote connections [SQL Server], enabling using Configuration Manager
- protocols [SQL Server], enabling using Configuration Manager
- protocols [SQL Server], disabling using Configuration Manager
- disabling network protocols, Configuration Manager
- network protocols [SQL Server], enabling
- enabling network protocols, Configuration Manager
- surface area configuration [SQL Server], connection protocols
- connections [SQL Server], enabling remote using Configuration Manager
ms.assetid: ec5ccb69-61c9-4576-8843-014b976fd46e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3b5f15490744ccfe8bfd47a69892f7033ddae361
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170999"
---
# <a name="enable-or-disable-a-server-network-protocol"></a>启用或禁用服务器网络协议
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所有网络协议都是由  安装程序安装的，可以启用也可以禁用这些网络协议。 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 本主题介绍如何通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器或 PowerShell，在  中启用或禁用服务器网络协议。 必须停止并重新启动 [!INCLUDE[ssDE](../../includes/ssde-md.md)] ，更改才能生效。  
  
> [!IMPORTANT]  
>  在安装 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 过程中，为 BUILTIN\Users 组添加一个登录名。 这可以使计算机的所有通过身份验证的用户作为 public 角色成员访问 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 实例。 可以安全地删除 BUILTIN\Users 登录名，以限制 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 对具有单独登录名或为使用此登录名的其他 Windows 组成员的计算机用户的访问。  
  
> [!WARNING]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 至 [!INCLUDE[sssql14](../../includes/sssql14-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 数据提供商在默认情况下仅支持 TLS 1.0 和 SSL 3.0。 如果通过更改操作系统 SChannel 层强制使用其他协议（如 TLS 1.1 或 TLS 1.2），与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的连接可能会失败，除非已安装相应的更新，将 TLS 1.1 和 1.2 的支持添加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，<a href="https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server">此处</a>列出了这些更新。 从 [!INCLUDE[sssql15](../../includes/sssql16-md.md)] 开始，SQL Server 的所有发布版本均包括 TLS 1.2 支持，无需安全其他更新。
  
 **本主题内容**  
  
-   **使用以下工具启用或禁用服务器网络协议：**  
  
     [SQL Server 配置管理器](#SSMSProcedure)  
  
     [PowerShell](#PowerShellProcedure)  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> 使用 SQL Server 配置管理器  
  
#### <a name="to-enable-a-server-network-protocol"></a>启用服务器网络协议  
  
1.  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 配置管理器的控制台窗格中，展开“SQL Server 网络配置”。  
  
2.  在控制台窗格中，单击“\<instance name> 的协议”。  
  
3.  在细节窗格中，右键单击要更改的协议，再单击“启用”  或“禁用” 。  
  
4.  在控制台窗格中，单击“SQL Server 服务”。  
  
5.  在详细信息窗格中，右键单击“SQL Server (\<instance name>)”，然后单击“重新启动”以停止并重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务 。  
  
##  <a name="using-sql-server-powershell"></a><a name="PowerShellProcedure"></a> 使用 SQL Server PowerShell  
  
#### <a name="to-enable-a-server-network-protocol-using-powershell"></a>使用 PowerShell 启用服务器网络协议  
  
1.  使用管理员权限打开一个命令提示符。  
  
2.  可以从任务栏启动 Windows PowerShell，也可以通过依次单击“开始”、“所有程序”、“附件”、“Windows PowerShell”、“Windows PowerShell”来启动。  
  
3.  通过输入“sqlps”导入“Import-Module “sqlps””模块 。  
  
4.  执行以下语句以启用 TCP 和 Named Pipes 协议。 `<computer_name>` 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]替换为运行  的计算机的名称。 `MSSQLSERVER` 如果您在配置命名实例，请将  替换为该实例的名称。  
  
     `IsEnabled` 若要禁用协议，请将 `$false`属性设置为 。  
  
    ```  
    $smo = 'Microsoft.SqlServer.Management.Smo.'  
    $wmi = new-object ($smo + 'Wmi.ManagedComputer').  
  
    # List the object properties, including the instance names.  
    $Wmi  
  
    # Enable the TCP protocol on the default instance.  
    $uri = "ManagedComputer[@Name='<computer_name>']/ ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
    $Tcp = $wmi.GetSmoObject($uri)  
    $Tcp.IsEnabled = $true  
    $Tcp.Alter()  
    $Tcp  
  
    # Enable the named pipes protocol for the default instance.  
    $uri = "ManagedComputer[@Name='<computer_name>']/ ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Np']"  
    $Np = $wmi.GetSmoObject($uri)  
    $Np.IsEnabled = $true  
    $Np.Alter()  
    $Np  
    ```  
  
#### <a name="to-configure-the-protocols-for-the-local-computer"></a>为本地计算机配置协议  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 当脚本在本地运行并配置本地计算机时， PowerShell 可以通过动态确定本地计算机的名称使脚本更为灵活。 `$uri` 若要检索本地计算机的名称，请将设置  变量的行替换为以下行。  
  
    ```  
    $uri = "ManagedComputer[@Name='" + (get-item env:\computername).Value + "']/ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
    ```  
  
#### <a name="to-restart-the-database-engine-by-using-sql-server-powershell"></a>使用 SQL Server PowerShell 重新启动数据库引擎  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] 启用或禁用了协议后，必须停止并重新启动才能使更改生效。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 执行以下语句，通过使用  PowerShell 来停止和启动默认实例。 `'MSSQLSERVER'` 若要停止和启动命名实例，请将 `'MSSQL$<instance_name>'`替换为 。  
  
    ```  
    # Get a reference to the ManagedComputer class.  
    CD SQLSERVER:\SQL\<computer_name>  
    $Wmi = (get-item .).ManagedComputer  
    # Get a reference to the default instance of the Database Engine.  
    $DfltInstance = $Wmi.Services['MSSQLSERVER']  
    # Display the state of the service.  
    $DfltInstance  
    # Stop the service.  
    $DfltInstance.Stop();  
    # Wait until the service has time to stop.  
    # Refresh the cache.  
    $DfltInstance.Refresh();   
    # Display the state of the service.  
    $DfltInstance  
    # Start the service again.  
    $DfltInstance.Start();  
    # Wait until the service has time to start.  
    # Refresh the cache and display the state of the service.  
    $DfltInstance.Refresh(); $DfltInstance  
    ```  
  
  
