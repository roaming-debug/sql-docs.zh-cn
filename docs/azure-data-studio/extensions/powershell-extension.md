---
title: PowerShell 扩展
description: 了解如何安装和使用 Azure Data Studio PowerShell 扩展，它为脚本的编写和调试提供丰富的 PowerShell 编辑器支持。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: SQLvariant
ms.author: aanelson
ms.reviewer: alayu, maghan, sstein
ms.custom: ''
ms.date: 04/19/2019
ms.openlocfilehash: 4de6c653503458ba839147b9a68b457330345755
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048497"
---
# <a name="powershell-editor-support-for-azure-data-studio"></a>Azure Data Studio 的 PowerShell 编辑器支持

此扩展提供了 [Azure Data Studio](https://github.com/Microsoft/azuredatastudio) 中丰富的 PowerShell 编辑器支持。
现在，你可以使用 Azure Data Studio 提供的出色的、类似于 IDE 的接口编写和调试 PowerShell 脚本。

![PowerShell 扩展](media/powershell-extension/powershell-extension.png)

## <a name="features"></a>功能

- 语法突出显示
- 代码片段
- IntelliSense cmdlet 等
- 由 [PowerShell Script Analyzer](http://github.com/PowerShell/PSScriptAnalyzer) 提供的基于规则的分析
- 转到 cmdlet 和变量的定义
- 查找 cmdlet 和变量的引用
- 文档和工作区符号发现
- 使用 <kbd>F8 </kbd> 运行选择的 PowerShell 代码
- 使用 <kbd>Ctrl</kbd>+F1<kbd></kbd> 为游标下的符号启动联机帮助
- 基本交互式控制台支持！

## <a name="installing-the-extension"></a>安装扩展

可以按照 [Azure Data Studio 文档](./add-extensions.md)中的步骤安装 PowerShell 扩展的官方版本。
在“扩展”窗格中，搜索“PowerShell”扩展并将其安装在此处。  未来有任何扩展更新，你都会收到系统自动发送的消息！

你还可以从[发布页](https://github.com/PowerShell/vscode-powershell/releases)安装 VSIX 包，并通过命令行进行安装：

```powershell
azuredatastudio --install-extension PowerShell-<version>.vsix
```

## <a name="platform-support"></a>平台支持

- Windows 7 到 10（带 windows PowerShell v3 及更高版本和 PowerShell Core）
- 带有 PowerShell Core（所有 PowerShell 支持的分发版）的 Linux
- macOS（PowerShell Core）

阅读 [FAQ](https://github.com/PowerShell/vscode-powershell/wiki/FAQ) 获取常见问题的解答。

## <a name="installing-powershell-core"></a>安装 PowerShell Core

如果在 MacOS 或 Linux 上运行 Azure Data Studio，则可能还需要安装 PowerShell Core。

PowerShell Core 是 [GitHub](https://github.com/powershell/powershell) 上的开放源代码项目。
有关在 MacOS 或 Linux 平台上安装 PowerShell Core 的详细信息，请参阅以下文章：

- [在 Linux 上安装 PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-linux)
- [在 macOS 上安装 PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-macos)

## <a name="example-scripts"></a>示例脚本

扩展的 `examples` 文件夹中有一些示例脚本，可用于发现 PowerShell 编辑和调试功能。  请查看其中的 [README.md](https://github.com/PowerShell/vscode-powershell/blob/master/examples/README.md) 文件，了解有关如何使用它们的详细信息。

可在以下路径中找到此文件夹：

```powershell
$HOME/.azuredatastudio/extensions/ms-vscode.PowerShell-<version>/examples
```

或者，如果使用的是扩展的预览版本

 ```powershell
$HOME/.azuredatastudio/extensions/ms-vscode.powershell-preview-<version>/examples
```

要在 Azure Data Studio 中打开/查看扩展的示例，请在 PowerShell 命令提示下运行以下代码：

```powershell
azuredatastudio (Get-ChildItem $Home\.azuredatastudio\extensions\ms-vscode.PowerShell-*\examples)[-1]
```

### <a name="creating-and-opening-files"></a>创建和打开文件

要在编辑器中创建并打开新文件，请使用 PowerShell 集成终端中的 New-EditorFile。

```powershell
PS C:\temp> New-EditorFile ExportData.ps1
```

此命令适用于任何文件类型，不仅仅适用于 PowerShell 文件。

```powershell
PS C:\temp> New-EditorFile ImportData.py
```

要打开 Azure Data Studio 中的一个或多个文件，请使用命令 `Open-EditorFile`。

```powershell
Open-EditorFile ExportData.ps1, ImportData.py
```

### <a name="no-focus-on-console-when-executing"></a>执行时焦点无需置于控制台

对于那些习惯使用 SSMS 的用户，你可以先执行查询，然后可以在无需切换回查询窗格的情况下再次执行它。  在这种情况下，你可能会对代码编辑器的默认行为感到奇怪。  若要在用 <kbd>F8</kbd> 执行时将焦点保持在编辑器中，请更改以下设置：

```json
"powershell.integratedConsole.focusConsoleOnExecute": false
```

访问性目的默认为 `true`。

请注意，即使使用显式调用输入的命令（如 `Get-Credential`），此设置也会阻止将焦点更改到控制台。

## <a name="sql-powershell-examples"></a>SQL PowerShell 示例

若要使用这些示例（如下所示），需要从 [PowerShell 库](https://www.powershellgallery.com/packages/SqlServer)安装 SqlServer 模块。

```powershell
Install-Module -Name SqlServer
```

> [!NOTE]
> 在 `21.1.18102` 及更高版本中，除 Windows PowerShell 以外，`SqlServer` 模块还支持 [PowerShell Core](https://github.com/PowerShell/PowerShell) 6.2 及更高版本。

在此示例中，我们使用 `Get-SqlInstance` cmdlet 来获取 ServerA 和 ServerB 的 Server SMO 对象。  此命令的默认输出将包括实例的实例名称、版本、Service Pack 和 CU 更新级别。

```powershell
Get-SqlInstance -ServerInstance ServerA, ServerB
```

下面是输出的示例：

```console
Instance Name             Version    ProductLevel UpdateLevel  HostPlatform HostDistribution
-------------             -------    ------------ -----------  ------------ ----------------
ServerA                   13.0.5233  SP2          CU4          Windows      Windows Server 2016 Datacenter
ServerB                   14.0.3045  RTM          CU12         Linux        Ubuntu
```

`SqlServer` 模块包含一个名为 `SQLRegistration` 的提供程序，可通过该提供程序以编程方式访问已保存的 SQL Server 连接的以下类型：

+ 数据库引擎服务器（已注册的服务器）
+ 中央管理服务器 (CMS)
+ Analysis Services
+ Integration Services
+ Reporting Services

 在下面的示例中，我们将执行 `dir`（`Get-ChildItem` 的别名）来获取已注册的服务器文件中列出的所有 SQL Server 实例的列表。

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse
```

下面是输出的示例：

```powershell
Mode Name
---- ----
-    ServerA
-    ServerB
-    localhost\SQL2017
-    localhost\SQL2016Happy
-    localhost\SQL2017
```

对于涉及数据库或数据库中对象的许多操作，都可以使用 `Get-SqlDatabase` cmdlet。  如果为 `-ServerInstance` 和 `-Database` 参数都提供值，则仅检索一个数据库对象。  但如果仅指定 `-ServerInstance` 参数，则将返回该实例上所有数据库的完整列表。

下面是输出的示例：

```console
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2017   Normal      336.00 MB   57.01 MB Simple       140 sa
master               Normal        6.00 MB  368.00 KB Simple       140 sa
model                Normal       16.00 MB    5.53 MB Full         140 sa
msdb                 Normal       48.44 MB    1.70 MB Simple       140 sa
PBIRS                Normal      144.00 MB   55.95 MB Full         140 sa
PBIRSTempDB          Normal       16.00 MB    4.20 MB Simple       140 sa
SSISDB               Normal      325.06 MB   26.21 MB Full         140 sa
tempdb               Normal       72.00 MB   61.25 MB Simple       140 sa
WideWorldImporters   Normal         3.2 GB     2.6 GB Simple       130 sa
```

下一个示例使用 `Get-SqlDatabase` cmdlet 来检索 ServerB 实例上的所有数据库列表，然后显示一个网格/表（使用 `Out-GridView` cmdlet），以选择应备份的数据库。  用户单击“确定”按钮后，只会备份突出显示的数据库。

```powershell
Get-SqlDatabase -ServerInstance ServerB |
Out-GridView -PassThru |
Backup-SqlDatabase -CompressionOption On
```

同样，此示例将获取已注册的服务器文件中列出的所有 SQL Server 实例的列表，然后调用 `Get-SqlAgentJobHistory`，以便为每个列出的 SQL Server 实例报告自午夜后每个失败的 SQL 代理作业。

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse |
WHERE {$_.Mode -ne 'd' } |
FOREACH {
    Get-SqlAgentJobHistory -ServerInstance  $_.Name -Since Midnight -OutcomesType Failed
}
```

在此示例中，我们执行 `dir`（`Get-ChildItem` 的别名）来获取已注册的服务器文件中列出的所有 SQL Server 实例的列表，然后使用 `Get-SqlDatabase` cmdlet 获取每个实例的数据库列表。

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse |
WHERE { $_.Mode -ne 'd' } |
FOREACH {
    Get-SqlDatabase -ServerInstance $_.Name
}
```

下面是输出的示例：

```console
Name                 Status           Size  Space     Recovery Compat. Owner
                                            Available Model    Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2017   Normal      336.00 MB   57.01 MB Simple       140 sa
master               Normal        6.00 MB  368.00 KB Simple       140 sa
model                Normal       16.00 MB    5.53 MB Full         140 sa
msdb                 Normal       48.44 MB    1.70 MB Simple       140 sa
PBIRS                Normal      144.00 MB   55.95 MB Full         140 sa
PBIRSTempDB          Normal       16.00 MB    4.20 MB Simple       140 sa
SSISDB               Normal      325.06 MB   26.21 MB Full         140 sa
tempdb               Normal       72.00 MB   61.25 MB Simple       140 sa
WideWorldImporters   Normal         3.2 GB     2.6 GB Simple       130 sa
```

## <a name="reporting-problems"></a>报告问题

如果遇到有关 PowerShell 扩展的任何问题，请参阅[疑难解答文档](https://github.com/PowerShell/vscode-powershell/blob/master/docs/troubleshooting.md)以了解有关诊断和报告问题的信息。

### <a name="security-note"></a>安全说明

有关任何安全问题，请参阅[此处](https://github.com/PowerShell/vscode-powershell/blob/master/docs/troubleshooting.md#note-on-security)。

## <a name="contributing-to-the-code"></a>参与代码

有关如何参与此扩展的更多详细信息，请查看[开发文档](https://github.com/PowerShell/vscode-powershell/blob/master/docs/development.md)！

## <a name="maintainers"></a>维护人员

- [Keith Hill](https://github.com/rkeithhill) - [@r_keith_hill](http://twitter.com/r_keith_hill)
- Tyler Leonhardt - [@TylerLeonhardt](http://twitter.com/tylerleonhardt)
- [Rob Holt](https://github.com/rjmholt)

## <a name="license"></a>许可

此扩展[根据 MIT 许可证获得授权](https://github.com/PowerShell/vscode-powershell/blob/master/LICENSE.txt)。 有关此项目的版本所包含的第三方二进制文件的详细信息，请参阅[第三方通知](https://github.com/PowerShell/vscode-powershell/blob/master/Third%20Party%20Notices.txt)文件。

## <a name="code-of-conduct"></a>行为准则

此项目采用了 [Microsoft 开放源代码行为准则][conduct-code]。
有关详细信息，请参阅[行为准则常见问题解答][conduct-FAQ]，如有任何其他问题或评论，请联系 [opencode@microsoft.com][conduct-email]。

[conduct-code]: https://opensource.microsoft.com/codeofconduct/
[conduct-FAQ]: https://opensource.microsoft.com/codeofconduct/faq/
[conduct-email]: mailto:opencode@microsoft.com