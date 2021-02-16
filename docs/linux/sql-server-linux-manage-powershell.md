---
title: 使用 PowerShell 管理 Linux 上的 SQL Server
description: 详细了解 SQL Server PowerShell，并查看将 Windows 与 Linux 上的 SQL Server 结合使用的几个示例。
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: a3492ce1-5d55-4505-983c-d6da8d1a94ad
ms.openlocfilehash: d10b498569bac6a166ded3eaf9bd03f23ea6b718
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345629"
---
# <a name="use-powershell-on-windows-to-manage-sql-server-on-linux"></a>使用 Windows 上的 PowerShell 管理 Linux 上的 SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本主题介绍 [SQL Server PowerShell](../powershell/sql-server-powershell.md)，并演示将其用于 Linux 上的 SQL Server 的几个示例。 Windows、MacOS 和 Linux 上当前提供对 SQL Server 的 PowerShell 支持。 本文将指导你使用 Windows 计算机连接到 Linux 上的远程 SQL Server 实例。

## <a name="install-the-newest-version-of-sql-powershell-on-windows"></a>安装最新版本的 Windows 上的 SQL PowerShell

在 PowerShell 库中维护 Windows 上的 [SQL PowerShell](../powershell/download-sql-server-ps-module.md)。 使用 SQL Server 时，应始终使用最新版本的 SqlServer PowerShell 模块。

## <a name="before-you-begin"></a>开始之前

请阅读 Linux 上的 SQL Server 的[已知问题](sql-server-linux-release-notes.md)。

## <a name="launch-powershell-and-import-the-sqlserver-module"></a>启动 PowerShell 并导入 sqlserver 模块 

首先启动 Windows 上的 PowerShell。 在 Windows 计算机上使用 <kbd>Win</kbd>+<kbd>R</kbd>，并键入“PowerShell”以启动新的 Windows PowerShell 会话  。

```
PowerShell
```

SQL Server 提供名为“SqlServer”的 PowerShell 模块  。 可以使用 SqlServer 模块将 SQL Server 组件（SQL Server 提供程序和 cmdlet）导入到 PowerShell 环境或脚本  。

在 PowerShell 提示符处复制并粘贴以下命令，将 SqlServer 模块导入当前的 PowerShell 会话  ：

```powershell
Import-Module SqlServer
```

在 PowerShell 提示符处键入以下命令，验证是否已正确导入 SqlServer 模块  ：

```powershell
Get-Module -Name SqlServer
```

PowerShell 应显示类似于以下输出的信息：

```
ModuleType Version    Name          ExportedCommands
---------- -------    ----          ----------------
Script     21.1.18102 SqlServer     {Add-SqlAvailabilityDatabase, Add-SqlAvailabilityGroupList...
```

## <a name="connect-to-sql-server-and-get-server-information"></a>连接到 SQL Server 并获取服务器信息

使用 Windows 上的 PowerShell 连接到 Linux 上的 SQL Server 实例，并显示几个服务器属性。

在 PowerShell 提示符下复制并粘贴以下命令。 运行这些命令时，PowerShell 将：
- 显示提示输入实例的主机名或 IP 地址的对话框
- 显示提示输入凭据的 Windows PowerShell 凭据请求对话框  。 可以使用 *SQL 用户名* 和 *SQL 密码* 连接到 Linux 上的 SQL Server 实例
- 使用 Get-SqlInstance cmdlet 连接到服务器，并显示一些属性  

也可选择仅将 `$serverInstance` 变量替换为 SQL Server 实例的 IP 地址或主机名。

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Connect to the Server and get a few properties
Get-SqlInstance -ServerInstance $serverInstance -Credential $credential
# done
```

PowerShell 应显示类似于以下输出的信息：

```
Instance Name                   Version    ProductLevel UpdateLevel  HostPlatform HostDistribution                
-------------                   -------    ------------ -----------  ------------ ----------------                
your_server_instance            14.0.3048  RTM          CU13         Linux        Ubuntu 
```
> [!NOTE]
> 如果没有显示这些值的内容，与目标 SQL Server 实例的连接可能已失败。 请确保可以使用相同的连接信息从 SQL Server Management Studio 进行连接。 然后查看[连接故障排除建议](sql-server-linux-troubleshooting-guide.md#connection)。

## <a name="using-the-sql-server-powershell-provider"></a>使用 SQL Server PowerShell 提供程序

连接到 SQL Server 实例的另一种方法是使用 [SQL Server PowerShell 提供程序](../powershell/sql-server-powershell-provider.md)。  使用此提供程序可以导航 SQL Server 实例，就像在对象资源管理器中（但在命令行中）导航树结构一样。  此提供程序默认显示为名为 `SQLSERVER:\` 的 PSDrive，可用于连接和导航域帐户有权访问的 SQL Server 实例。  有关如何为 Linux 上的 SQL Server 设置 Active Directory 身份验证的信息，请参阅[配置步骤](./sql-server-linux-active-directory-auth-overview.md#configuration-steps)。

还可以使用 SQL Server PowerShell 提供程序进行 SQL 身份验证。 为此，请使用 `New-PSDrive` cmdlet 创建新的 PSDrive，并提供适当的凭据以进行连接。

在下面的示例中，你将看到一个有关如何使用 SQL 身份验证创建新 PSDrive 的示例。

```powershell
# NOTE: We are reusing the values saved in the $credential variable from the above example.
New-PSDrive -Name SQLonDocker -PSProvider SqlServer -Root 'SQLSERVER:\SQL\localhost,10002\Default\' -Credential $credential
```

可以运行 `Get-PSDrive` cmdlet 来确认是否已创建驱动器。

```powershell
Get-PSDrive
```

创建新的 PSDrive 后即可开始进行导航。

```powershell
dir SQLonDocker:\Databases
```

输出可能如下所示。  你可能会注意到，此输出类似于 SSMS 在数据库节点中显示的内容。  它显示用户数据库，而不显示系统数据库。

```powershell
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2016   Normal      209.63 MB    1.31 MB Simple       130 sa
AdventureWorksDW2012 Normal      167.00 MB   32.47 MB Simple       110 sa
AdventureWorksDW2014 Normal      188.00 MB   78.10 MB Simple       120 sa
AdventureWorksDW2016 Normal      172.00 MB   74.76 MB Simple       130 sa
AdventureWorksDW2017 Normal      208.00 MB   40.57 MB Simple       140 sa
```

如果需要查看实例上的所有数据库，可以使用 [Get-SqlDatabase](/powershell/module/sqlserver/Get-SqlDatabase) cmdlet。

## <a name="examine-sql-server-error-logs"></a>检查 SQL Server 错误日志

以下步骤使用 Windows 上的 PowerShell 检查连接到 Linux 上 SQL Server 实例的错误日志。 我们还将使用 Out-GridView cmdlet 以网格视图样式显示错误日志中的信息  。

在 PowerShell 提示符下复制并粘贴以下命令。 它们可能会运行几分钟。 这些命令执行以下操作：
- 显示提示输入实例的主机名或 IP 地址的对话框
- 显示提示输入凭据的 Windows PowerShell 凭据请求对话框  。 可以使用 *SQL 用户名* 和 *SQL 密码* 连接到 Linux 上的 SQL Server 实例
- 使用 **Get-SqlErrorLog** cmdlet 连接到 Linux 上的 SQL Server 实例，并检索自 **昨天** 起的错误日志
- 将输出传送到 Out-GridView cmdlet 

也可以选择将 `$serverInstance` 变量替换为 SQL Server 实例的 IP 地址或主机名。

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Retrieve error logs since yesterday
Get-SqlErrorLog -ServerInstance $serverInstance -Credential $credential -Since Yesterday | Out-GridView
# done
```
## <a name="see-also"></a>另请参阅
- [SQL Server PowerShell](../powershell/sql-server-powershell.md)
- [SqlServer cmdlet](/powershell/module/sqlserver)