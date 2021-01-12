---
title: 使用 PowerShell Core 管理 Linux 上的 SQL Server
description: 通过演示将 SQL Server PowerShell 用于 macOS 和 Linux 上的 PowerShell Core (PS Core) 的几个示例，来了解 SQL Server PowerShell。
ms.date: 04/22/2019
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
author: SQLvariant
ms.author: aanelson
ms.reviewer: vanto
ms.openlocfilehash: a65ad9ad35e4feb2a55ffbdc259601a7b16106f9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094552"
---
# <a name="manage-sql-server-on-linux-with-powershell-core"></a>使用 PowerShell Core 管理 Linux 上的 SQL Server

本文介绍 [SQL Server PowerShell](../powershell/sql-server-powershell.md)，并演示将其用于 macOS 和 Linux 上的 PowerShell Core (PS Core) 的几个示例。 PowerShell Core 现在是 [GitHub](https://github.com/powershell/powershell) 上的开源项目。

## <a name="cross-platform-editor-options"></a>跨平台编辑器选项

针对 PowerShell Core 的以下所有步骤都将在常规终端中运行，你也可以在 VS Code 或 Azure Data Studio 的终端中运行它们。  VS Code 和 Azure Data Studio 都可以在 macOS 和 Linux 上使用。  有关 Azure Data Studio 的详细信息，请参阅[此快速入门](../azure-data-studio/quickstart-sql-server.md)。  你还可以考虑对它使用 [PowerShell 扩展](../azure-data-studio/extensions/powershell-extension.md)。

## <a name="installing-powershell-core"></a>安装 PowerShell Core

有关在各种受支持的平台和实验平台上安装 PowerShell Core 的详细信息，请参阅以下文章：

- [在 Windows 上安装 PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)
- [在 Linux 上安装 PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-linux)
- [在 macOS 上安装 PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-macos)
- [在 ARM 上安装 PowerShell Core](/powershell/scripting/install/powershell-core-on-arm)

## <a name="install-the-sqlserver-module"></a>安装 SqlServer 模块

`SqlServer` 模块在 [PowerShell 库](https://www.powershellgallery.com/packages/SqlServer/)中维护。 使用 SQL Server 时，应始终使用最新版本的 SqlServer PowerShell 模块。

若要安装 SqlServer 模块，请打开 PowerShell Core 会话并运行以下代码：

```powerhsell
Install-Module -Name SqlServer
```

有关如何从 PowerShell 库安装 SqlServer 模块的详细信息，请参阅此[页面](../powershell/download-sql-server-ps-module.md)。

## <a name="using-the-sqlserver-module"></a>使用 SqlServer 模块

首先启动 PowerShell Core。  如果使用的是 macOS 或 Linux，请在计算机上打开 *终端会话*，然后键入“pwsh”以启动新的 PowerShell Core 会话  。  在 Windows 上，使用 <kbd>Win</kbd>+<kbd>R</kbd>，然后键入 `pwsh` 以启动新的 PowerShell Core 会话。

```
pwsh
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

以下步骤使用 PowerShell Core 连接到 Linux 上的 SQL Server 实例，并显示几个服务器属性。

在 PowerShell 提示符下复制并粘贴以下命令。 运行这些命令时，PowerShell 将：
- 显示提示输入实例的主机名或 IP 地址的对话框
- 显示“PowerShell 凭据请求”对话框，提示你输入凭据  。 可以使用 *SQL 用户名* 和 *SQL 密码* 连接到 Linux 上的 SQL Server 实例
- 使用 Get-SqlInstance cmdlet 连接到服务器，并显示一些属性  

也可选择仅将 `$serverInstance` 变量替换为 SQL Server 实例的 IP 地址或主机名。

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Connect to the Server and return a few properties
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

还可以使用 SQL Server PowerShell 提供程序进行 SQL 身份验证。 为此，请使用 `New-PSDrive` cmdlet 创建新的 PSDrive，并提供适当的凭据进行连接。

在下面的示例中，你将看到一个有关如何使用 SQL 身份验证新建 PSDrive 的示例。

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

如果需要查看实例上的所有数据库，可以使用 `Get-SqlDatabase` cmdlet。

## <a name="get-databases"></a>获取数据库

要了解的一个重要 cmdlet 是 `Get-SqlDatabase`。  对于涉及数据库或数据库中对象的许多操作，都可以使用 `Get-SqlDatabase` cmdlet。  如果为 `-ServerInstance` 和 `-Database` 参数都提供值，则仅检索一个数据库对象。  但如果仅指定 `-ServerInstance` 参数，则将返回该实例上所有数据库的完整列表。

```powershell
# NOTE: We are reusing the values saved in the $credential variable from the above example.

# Connect to the Instance and retrieve all databases
Get-SqlDatabase -ServerInstance ServerB -Credential $credential
```

下面是上述 Get-SqlDatabase 命令可能返回的内容的示例：

```powershell
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2016   Normal      209.63 MB    1.31 MB Simple       130 sa
AdventureWorksDW2012 Normal      167.00 MB   32.47 MB Simple       110 sa
AdventureWorksDW2014 Normal      188.00 MB   78.10 MB Simple       120 sa
AdventureWorksDW2016 Normal      172.00 MB   74.88 MB Simple       130 sa
AdventureWorksDW2017 Normal      208.00 MB   40.63 MB Simple       140 sa
master               Normal        6.00 MB  600.00 KB Simple       140 sa
model                Normal       16.00 MB    5.70 MB Full         140 sa
msdb                 Normal       15.50 MB    1.14 MB Simple       140 sa
tempdb               Normal       16.00 MB    5.49 MB Simple       140 sa

```

## <a name="examine-sql-server-error-logs"></a>检查 SQL Server 错误日志

以下步骤使用 PowerShell Core 检查连接到 Linux 上 SQL Server 实例的错误日志。

在 PowerShell 提示符下复制并粘贴以下命令。 它们可能会运行几分钟。 这些命令执行以下步骤：
- 显示提示输入实例的主机名或 IP 地址的对话框
- 显示“PowerShell 凭据请求”对话框，提示你输入凭据  。 可以使用 *SQL 用户名* 和 *SQL 密码* 连接到 Linux 上的 SQL Server 实例
- 使用 **Get-SqlErrorLog** cmdlet 连接到 Linux 上的 SQL Server 实例，并检索自 **昨天** 起的错误日志

也可以选择将 `$serverInstance` 变量替换为 SQL Server 实例的 IP 地址或主机名。

```powershell
# Prompt for instance & credentials to login into SQL Server
$serverInstance = Read-Host "Enter the name of your instance"
$credential = Get-Credential

# Retrieve error logs since yesterday
Get-SqlErrorLog -ServerInstance $serverInstance -Credential $credential -Since Yesterday
# done
```

## <a name="explore-cmdlets-currently-available-in-ps-core"></a>探索 PS Core 中当前可用的 cmdlet
虽然 SqlServer 模块当前在 Windows PowerShell 中有 109 个 cmdlet 可用，但其中只有 62 个可用于 PSCore。 下面提供了当前可用的 62 个 cmdlet 的完整列表。  有关 SqlServer 模块中所有 cmdlet 的深入文档，请参阅 SqlServer [cmdlet 参考](/powershell/module/sqlserver/)。

以下命令将显示正在使用的 PowerShell 版本上可用的所有 cmdlet。

```powershell
Get-Command -Module SqlServer -CommandType Cmdlet |
Sort-Object -Property Noun |
SELECT Name
```

- ConvertFrom-EncodedSqlName
- ConvertTo-EncodedSqlName
- Get-SqlAgent
- Get-SqlAgentJob
- Get-SqlAgentJobHistory
- Get-SqlAgentJobSchedule
- Get-SqlAgentJobStep
- Get-SqlAgentSchedule
- Invoke-SqlAssessment
- Get-SqlAssessmentItem
- Remove-SqlAvailabilityDatabase
- Resume-SqlAvailabilityDatabase
- Add-SqlAvailabilityDatabase
- Suspend-SqlAvailabilityDatabase
- New-SqlAvailabilityGroup
- Set-SqlAvailabilityGroup
- Remove-SqlAvailabilityGroup
- Switch-SqlAvailabilityGroup
- Join-SqlAvailabilityGroup
- Revoke-SqlAvailabilityGroupCreateAnyDatabase
- Grant-SqlAvailabilityGroupCreateAnyDatabase
- New-SqlAvailabilityGroupListener
- Set-SqlAvailabilityGroupListener
- Add-SqlAvailabilityGroupListenerStaticIp
- Set-SqlAvailabilityReplica
- Remove-SqlAvailabilityReplica
- New-SqlAvailabilityReplica
- Set-SqlAvailabilityReplicaRoleToSecondary
- New-SqlBackupEncryptionOption
- Get-SqlBackupHistory
- Invoke-Sqlcmd
- New-SqlCngColumnMasterKeySettings
- Remove-SqlColumnEncryptionKey
- Get-SqlColumnEncryptionKey
- Remove-SqlColumnEncryptionKeyValue
- Add-SqlColumnEncryptionKeyValue
- Get-SqlColumnMasterKey
- Remove-SqlColumnMasterKey
- New-SqlColumnMasterKey
- Get-SqlCredential
- Set-SqlCredential
- New-SqlCredential
- Remove-SqlCredential
- New-SqlCspColumnMasterKeySettings
- Get-SqlDatabase
- Restore-SqlDatabase
- Backup-SqlDatabase
- Set-SqlErrorLog
- Get-SqlErrorLog
- New-SqlHADREndpoint
- Set-SqlHADREndpoint
- Get-SqlInstance
- Add-SqlLogin
- Remove-SqlLogin
- Get-SqlLogin
- Set-SqlSmartAdmin
- Get-SqlSmartAdmin
- Read-SqlTableData
- Write-SqlTableData
- Read-SqlViewData
- Read-SqlXEvent
- Convert-UrnToPath

## <a name="see-also"></a>另请参阅
- [SQL Server PowerShell](../powershell/sql-server-powershell.md)