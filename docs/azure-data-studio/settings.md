---
title: 用户和工作区设置
description: 了解如何根据自己的喜好使用各项设置来自定义 Azure Data Studio 的编辑器、用户界面和功能行为。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 76f6cedaa26165d14ce96b234b0d6cd8129806e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048197"
---
# <a name="modify-user-and-workspace-settings"></a>修改用户和工作区设置

可以通过设置根据自己的喜好轻松配置 Azure Data Studio。 Azure Data Studio 的编辑器、用户界面和功能行为的几乎每个部分都有可以修改的选项。

Azure Data Studio 提供两种不同的设置范围：

* **用户** - 这些设置全局适用于打开的任何 Azure Data Studio 实例。
* **工作区** -“工作区”设置是特定于计算机上文件夹的设置，且仅当在资源管理器边栏中打开文件夹时可用。 此范围中定义的设置会替代用户范围。

## <a name="creating-user-and-workspace-settings"></a>创建用户和工作区设置

菜单命令“文件” > “首选项” > “设置”（Mac 上为“Code”[代码] > “Preferences”[首选项] > “Settings”[设置]）提供用于配置用户和工作区设置的入口点     。 你将获得默认设置的列表。 将想要更改的任何设置复制到相应的 `settings.json` 文件。 右侧选项卡允许在用户设置文件和工作区设置文件之间快速切换。

还可从“命令面板”（Ctrl+Shift+P）（使用“首选项:打开用户设置”和“首选项:打开工作区设置”），或使用键盘快捷键（Ctrl+,）打开用户和工作区设置。

以下示例在编辑器中禁用行号，并将代码行配置为自动缩进。

![示例设置](media/settings/sample-settings.png)

保存经过修改的 `settings.json` 文件后，Azure Data Studio 会重新加载对设置的更改。

> [!NOTE]
> 工作区设置对于在团队中共享项目特定设置非常有用。

## <a name="settings-file-locations"></a>设置文件位置

根据平台，用户设置文件位于：

* **Windows** `%APPDATA%\azuredatastudio\User\settings.json`
* **Mac** `$HOME/Library/Application Support/azuredatastudio/User/settings.json`
* **Linux** `$HOME/.config/azuredatastudio/User/settings.json`

工作区设置文件位于项目的 `.Azure Data Studio` 文件夹下。

## <a name="hot-exit"></a>热退出

默认情况下，Azure Data Studio 会在退出时记住未保存的文件更改。 在 Visual Studio Code 中，这与热退出功能一样。

默认情况下，热退出处于关闭状态。 通过编辑 `files.hotExit` 设置启用热退出。 有关详细信息，请参阅[热退出（在 Visual Studio Code 文档中）](https://code.visualstudio.com/docs/editor/codebasics#_hot-exit)。

## <a name="tab-color"></a>选项卡颜色

若要简化对所使用的连接的标识，可将编辑器中打开的选项卡的颜色设置为与连接所属的服务器组的颜色匹配。 默认情况下，选项卡颜色处于关闭状态。 通过编辑 `sql.tabColorMode` 设置启用选项卡颜色。

## <a name="additional-resources"></a>其他资源

由于 Azure Data Studio 从 Visual Studio Code 继承了其用户和工作区设置功能，有关设置的详细信息请参阅 [Visual Studio Code 设置](https://code.visualstudio.com/docs/getstarted/settings)一文。
