---
title: 安装：PowerShell 所需状态配置
description: 使用 PowerShell DSC 安装 SQL Server，并了解 Windows Server 2016 上 SQL Server 2017 独立实例的初始设置。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.devlang: PowerShell
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
author: randomnote1
ms.author: dareist
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: d0c3828e26e5bde24d678cfac8959264adafcb13
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091591"
---
# <a name="install-sql-server-with-powershell-desired-state-configuration"></a>使用 PowerShell Desired State Configuration 安装 SQL Server

你是否曾经历过下述情况：使用 SQL Server 安装界面时，只是选择相同的按钮，输入相同的信息，而没有多加考虑？ 虽然安装已完成，但却忘了在 sysadmin 角色中指定 DBA 组  。 然后，必须执行以下操作：
* 进入单用户模式。
* 添加适当的用户或组。
* 在多用户模式下重新启动 SQL Server。
* 测试。 

更糟糕的是，现在你会对整个安装过程产生怀疑。 “我还忘记了什么？” 你可能会问自己。

请阅读 [PowerShell Desired State Configuration (DSC)](/powershell/scripting/dsc/overview/overview)。 通过使用 DSC，可以构建一个能在成百上千个服务器上重复使用的配置模板。 根据构建，可能需要调整几个安装参数。 但这并不是最重要的问题，因为你可以保持所有标准设置不变。 这消除了忘记输入重要参数的可能性。

本文将使用 SqlServerDsc DSC 资源探索 Windows Server 2016 上 SQL Server 2017 独立实例的初始设置  。 预先了解一些有关 DSC 的知识大有好处，因为我们不会探索 DSC 的工作原理。

本演练需要以下项：

- 运行 Windows Server 2016 的计算机。
- SQL Server 2017 安装介质。
- SqlServerDsc DSC 资源  。

## <a name="prerequisites"></a>先决条件

在大多数情况下，DSC 用于处理先决条件要求。 但是，出于本演示的目的，我们将手动处理先决条件。

## <a name="install-the-sqlserverdsc-dsc-resource"></a>安装 SqlServerDsc DSC 资源

可使用 [Install-Module](/powershell/module/powershellget/Install-Module) cmdlet 从 [PowerShell 库](https://www.powershellgallery.com/)下载 [SqlServerDsc](https://www.powershellgallery.com/packages/SqlServerDsc) DSC 资源。 

> [!NOTE]
> 安装模块时，需确保“以管理员身份”运行 PowerShell  。

```PowerShell
Install-Module -Name SqlServerDsc
```

### <a name="get-the-sql-server-2017-installation-media"></a>获取 SQL Server 2017 安装介质
将 SQL Server 2017 安装介质下载到服务器。 我们已通过 Visual Studio 订阅下载 SQL Server 2017 Enterprise，并将 ISO 复制到 `C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso`。

现在必须将 ISO 提取到目录：

```PowerShell
New-Item -Path C:\SQL2017 -ItemType Directory
$mountResult = Mount-DiskImage -ImagePath 'C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso' -PassThru
$volumeInfo = $mountResult | Get-Volume
$driveInfo = Get-PSDrive -Name $volumeInfo.DriveLetter
Copy-Item -Path ( Join-Path -Path $driveInfo.Root -ChildPath '*' ) -Destination C:\SQL2017\ -Recurse
Dismount-DiskImage -ImagePath 'C:\en_sql_server_2017_enterprise_x64_dvd_11293666.iso'
```

## <a name="create-the-configuration"></a>创建配置

### <a name="configuration"></a>配置

创建配置函数，将调用该函数生成[托管对象格式 (MOF)](/windows/desktop/WmiSdk/managed-object-format--mof-) 文档：

```PowerShell
Configuration SQLInstall
{...}
```

### <a name="modules"></a>模块

将模块导入当前会话。 这些模块告知配置文档如何构建 MOF 文档。 此外，它们还告知 DSC 引擎如何将 MOF 文档应用于服务器：

```PowerShell
Import-DscResource -ModuleName SqlServerDsc
```

### <a name="resources"></a>资源

#### <a name="net-framework"></a>.NET Framework

SQL Server 依赖于 .NET Framework。 因此我们需要确保在安装 SQL Server 前安装它。 WindowsFeature 资源用于安装 Net-Framework-45-Core Windows 功能   ：

```PowerShell
WindowsFeature 'NetFramework45'
{
     Name = 'Net-Framework-45-Core'
     Ensure = 'Present'
}
```

#### <a name="sqlsetup"></a>SqlSetup

SqlSetup 资源用于告知 DSC 如何安装 SQL Server  。 基本安装所需的参数如下所示：

- InstanceName  。 实例的名称。 使用 MSSQLSERVER 作为默认实例  。
- **Features**。 要安装的功能。 在此示例中，我们仅安装 SQLEngine 功能  。
- **SourcePath**。 SQL 安装介质的路径。 在此示例中，我们将 SQL 安装介质存储在 `C:\SQL2017` 中。 网络共享可最大程度减少服务器上使用的空间。
- **SQLSysAdminAccounts**。 要成为 sysadmin 角色成员的用户或组  。 在此示例中，我们授予本地管理员组 sysadmin 访问权限  。 

> [!NOTE]
> 不建议在高安全性环境中使用此配置。

有关 SqlSetup 上可用参数的完整列表和说明，请参阅 [SqlServerDsc GitHub 存储库](https://github.com/PowerShell/SqlServerDsc/tree/master#sqlsetup)  。

SqlSetup 资源仅安装 SQL Server，但不维护所应用的设置   。 例如在安装时指定 SQLSysAdminAccounts  的情况。 管理员可以在 sysadmin 角色中添加或删除登录  。 但 SqlSetup 资源不会受到影响  。 如果希望 DSC 强制执行 sysadmin 角色的成员身份，请使用 [SqlServerRole](https://github.com/PowerShell/SqlServerDsc/tree/master#sqlserverrole) 资源  。

#### <a name="finish-configuration"></a>完成配置

```PowerShell
Configuration SQLInstall
{
     Import-DscResource -ModuleName SqlServerDsc

     node localhost
     {
          WindowsFeature 'NetFramework45'
          {
               Name   = 'NET-Framework-45-Core'
               Ensure = 'Present'
          }

          SqlSetup 'InstallDefaultInstance'
          {
               InstanceName        = 'MSSQLSERVER'
               Features            = 'SQLENGINE'
               SourcePath          = 'C:\SQL2017'
               SQLSysAdminAccounts = @('Administrators')
               DependsOn           = '[WindowsFeature]NetFramework45'
          }
     }
}
```

## <a name="build-and-deploy"></a>生成并部署

### <a name="compile-the-configuration"></a>编译配置

在配置脚本中添加圆点：

```PowerShell
. .\SQLInstallConfiguration.ps1
```

运行配置函数：

```PowerShell
SQLInstall
```

在工作目录中创建一个名为 SQLInstall 的目录  。 该目录包含一个名为 localhost.mof 的文件  。 检查显示已编译的 DSC 配置的 MOF 的内容。

### <a name="deploy-the-configuration"></a>部署配置

若要启动 SQL Server 的 DSC 部署，请调用 Start-DscConfiguration cmdlet  。 为 cmdlet 提供了以下参数：

- **Path**。 包含要部署的 MOF 文档的文件夹的路径。 示例为 `C:\SQLInstall`。
- **Wait**。 等待配置作业完成。
- **Force**。 替代任何现有 DSC 配置。
- **Verbose**。 显示详细输出。 首次推送配置时非常有用，可帮助排除故障。

```PowerShell
Start-DscConfiguration -Path C:\SQLInstall -Wait -Force -Verbose
```

随着配置的应用，详细输出将显示所发生的情况。 只要没有引发任何错误（红色文本），那么当屏幕上出现“操作‘调用 CimMethod’完成”时，应安装 SQL Server  。

## <a name="validate-installation"></a>验证安装

### <a name="dsc"></a>DSC

[Test-DscConfiguration](/powershell/module/psdesiredstateconfiguration/test-dscconfiguration) cmdlet 可以确定服务器的当前状态是否符合所需状态。 在此情况下，状态为 SQL Server 安装。 Test-DscConfiguration 的结果应为“True”   ：

```PowerShell
PS C:\> Test-DscConfiguration
True
```

### <a name="services"></a>服务

现在服务列表返回 SQL Server 服务：

```PowerShell
PS C:\> Get-Service -Name *SQL*
Status  Name           DisplayName
------  ----           -----------
Running MSSQLSERVER    SQL Server (MSSQLSERVER)
Stopped SQLBrowser     SQL Server Browser
Running SQLSERVERAGENT SQL Server Agent (MSSQLSERVER)
Running SQLTELEMETRY   SQL Server CEIP service (MSSQLSERVER)
Running SQLWriter      SQL Server VSS Writer
```

### <a name="sql-server"></a>SQL Server

```PowerShell
PS C:\> & sqlcmd -S $env:COMPUTERNAME
1> SELECT @@SERVERNAME
2> GO
1> quit
```

## <a name="see-also"></a>另请参阅

[Windows PowerShell Desired State Configuration 概述](/powershell/scripting/dsc/overview/overview)

[从命令提示符安装 SQL Server](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)

[使用配置文件安装 SQL Server](../../database-engine/install-windows/install-sql-server-using-a-configuration-file.md)
