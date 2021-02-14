---
title: 集成终端
description: 了解如何打开集成到 Azure Data Studio 的终端。 集成终端比单独的终端更方便。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 054bfd71962a5775374e8faa567dc24242d846c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048337"
---
# <a name="integrated-terminal"></a>集成终端

在 Azure Data Studio 中，可以打开一个集成终端，该终端最初从工作区的根目录启动。 可以便捷实现该操作，因为不必切换窗口或更改现有终端的状态，即可执行一个快速命令行任务。

打开终端：

* 利用反撇号字符，使用 Ctrl+` 键盘快捷方式。
* 使用“视图” | “集成终端”菜单命令 。
* 从命令面板 (Ctrl+Shift+P)，使用“视图: 切换集成终端”命令  。

![终端](media/integrated-terminal/terminal-screen.png)

> [!NOTE]
> 如果更喜欢在 Azure Data Studio 外部工作，仍然可以使用 Explorer 的“在命令提示符中打开”命令（在 Mac 或 Linux 中则为“在终端中打开”命令）打开外部 shell 。

## <a name="managing-multiple-terminals"></a>管理多个终端

可以创建多个打开到不同位置的终端，并轻松在它们之间导航。 可以通过点击 TERMINAL 面板右上角的加号图标或通过触发 Ctrl+Shift+` 命令来添加终端实例 。 这会在下拉列表中创建可用于切换的另一个条目。

![多个终端](media/integrated-terminal/terminal-multiple-instances.png)

按垃圾桶按钮删除终端实例。

> [!TIP]
> 如果要广泛使用多个终端，可以为[“键绑定”部分](#key-bindings)中列出的 `focusNext`、`focusPrevious` 和 `kill` 命令添加键绑定，从而允许只使用键盘在他们之间导航。

## <a name="configuration"></a>配置

所使用的 shell 在 Linux 和 macOS 上默认为 `$SHELL`，在 Windows 10 上为 PowerShell，在早期版本的 Windows 上为 cmd.exe。 这些可在[设置](settings.md)中通过设置 `terminal.integrated.shell.*` 手动覆盖。 可以使用 `terminal.integrated.shellArgs.*` 设置将参数传递到 Linux 和 macOS上 的终端 shell。

### <a name="windows"></a>Windows

在 Windows 上正确配置 shell 需要找到正确的可执行文件并更新设置。 以下是常见 shell 可执行文件及其默认位置的列表：

```json
// 64-bit cmd if available, otherwise 32-bit
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\cmd.exe"
// 64-bit PowerShell if available, otherwise 32-bit
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\WindowsPowerShell\\v1.0\\powershell.exe"
// Git Bash
"terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe"
// Bash on Ubuntu (on Windows)
"terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\bash.exe"
```

> [!NOTE]
> 如果要作为集成终端使用，shell 可执行文件必须是控制台应用程序，以便能够重定向 `stdin/stdout/stderr`。

> [!TIP]
> 集成终端 shell 使用 Azure Data Studio 权限运行。 如果需要使用提升的（管理员）或不同的权限运行 shell 命令，可以在终端内使用平台实用程序，如 `runas.exe`。

### <a name="shell-arguments"></a>Shell 参数

可以在 shell 启动时向其传递参数。

例如，若要实现将 bash 运行为登录 shell（运行 `.bash_profile`），应传入 `-l` 参数（带双引号）：

```json
// Linux
"terminal.integrated.shellArgs.linux": ["-l"]
```

## <a name="terminal-display-settings"></a>终端显示设置

可以通过以下设置自定义集成终端字体和行高：

* `terminal.integrated.fontFamily`
* `terminal.integrated.fontSize`
* `terminal.integrated.lineHeight`

## <a name="terminal-key-bindings"></a><a id="key-bindings"></a>终端键绑定

“视图: 切换集成终端”命令会被绑定到“Ctrl +`”，以快速将该集成终端面板切入和切出视图。

下面是用于在集成终端内快速导航的键盘快捷方式：

|密钥|Command|  
|---|---|  
|**Ctrl+\`**|显示集成终端|  
|**Ctrl+Shift+\`**|创建新终端|  
|**Ctrl+Up**|向上滚动|  
|**Ctrl+Down**|向下滚动|  
|**Ctrl+PageUp**|向上滚动页面|  
|Ctrl+PageDown|向下滚动页面|  
|Ctrl+Home|滚动到顶部|  
|Ctrl+End|滚动到底部|  
|Ctrl+K|清除终端|  

还有其他可用且可以绑定到首选键盘快捷方式的终端命令。

它们分别是：

* `workbench.action.terminal.focus`：聚焦终端。 这类似于切换，不同之处是如果终端可见，则聚焦终端而不是隐藏它。
* `workbench.action.terminal.focusNext`：将焦点放在下一个终端实例上。
* `workbench.action.terminal.focusPrevious`：将焦点放在前一个终端实例上。
* `workbench.action.terminal.kill`：删除当前终端实例。
* `workbench.action.terminal.runSelectedText`：在终端实例中运行选定的文本。
* `workbench.action.terminal.runActiveFile`：在终端实例中运行活动文件。

### <a name="run-selected-text"></a>运行所选文本

要使用 `runSelectedText` 命令，请在编辑器中选择文本，然后通过命令面板 (Ctrl+Shift+P) 运行命令“终端: 在活动终端中运行所选文本” 。 终端随即尝试运行所选文本：

![运行所选文本](media/integrated-terminal/terminal_run_selected.png)

如果在活动编辑器中未选择任何文本，则光标所在的行将在终端中运行。

### <a name="copy--paste"></a>复制和粘贴

复制和粘贴的键绑定遵循平台标准：

* Linux：Ctrl+Shift+C 和 Ctrl+Shift+V
* Mac：Cmd+C 和 Cmd+V
* Windows：Ctrl+C 和 Ctrl+V

### <a name="find"></a>查找

集成终端具有可通过 Ctrl+F 触发的基本查找功能。

如果想通过 Ctrl+F 转到 shell 而不是在 Linux 和 Windows 上启动“查找”小组件，需要删除键绑定，如下所示：

```js
{ "key": "ctrl+f", "command": "-workbench.action.terminal.focusFindWidget",
                      "when": "terminalFocus" },
```

### <a name="rename-terminal-sessions"></a>重命名终端会话

现在可以使用以下命令来重命名集成终端会话：“终端:重命名”(`workbench.action.terminal.rename`)。 新名称显示在终端选择下拉列表中。

### <a name="forcing-key-bindings-to-pass-through-the-terminal"></a>强制通过终端传递键绑定

虽然焦点位于集成终端，但许多键绑定不会起作用，因为击键被传递给终端本身并由终端本身使用。 可通过 `terminal.integrated.commandsToSkipShell` 设置解决此情况。 它包含一组命令名称，其键绑定会跳过 shell 处理，转由 Azure Data Studio 键绑定系统来处理。 默认情况下，除少数常用的键绑定外，其中还包括所有终端键绑定。

