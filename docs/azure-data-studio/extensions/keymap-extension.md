---
title: 创建键映射扩展
description: 本教程演示如何创建键映射扩展以将自定义功能添加到 Azure Data Studio。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 369685672921a62f44bb81387db0b987aa2e44dc
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495746"
---
# <a name="create-an-azure-data-studio-keymap-extension"></a>创建 Azure Data Studio 键映射扩展

本教程演示如何创建新的 Azure Data Studio 扩展。 此扩展在 Azure Data Studio 中创建常见的 SSMS 键绑定。

本文介绍如何执行以下操作：
> [!div class="checklist"]
> - 创建一个扩展项目
> - 安装扩展生成器
> - 创建扩展
> - 向扩展添加自定义键绑定
> - 测试扩展
> - 打包扩展
> - 将扩展发布到市场

## <a name="prerequisites"></a>先决条件

Azure Data Studio 建立在与 Visual Studio Code 相同的框架上，因此 Azure Data Studio 的扩展是使用 Visual Studio Code 生成的。 要开始操作，需满足以下条件：

- 已在 `$PATH` 中安装 [Node.js](https://nodejs.org) 且可用。 Node.js 包含 [npm](https://www.npmjs.com/)，它是用于安装扩展生成器的 Node.js 包管理器。
- 已有 [Visual Studio Code](https://code.visualstudio.com)，用于调试扩展。
- 有 Azure Data Studio [调试扩展](https://marketplace.visualstudio.com/items?itemName=ms-mssql.sqlops-debug)（可选）。 可使用它测试扩展，且无需将扩展打包并安装到 Azure Data Studio 中。
- 确保 `azuredatastudio` 位于你的路径中。 对于 Windows，请确保选择 setup.exe 中的 `Add to Path` 选项。 对于 Mac 或 Linux，请从 Azure Data Studio 的命令面板运行“在 PATH 中安装 'azuredatastudio' 命令”。

## <a name="install-the-extension-generator"></a>安装扩展生成器

为了简化创建扩展的过程，已使用 Yeoman 构建了一个[扩展生成器](https://code.visualstudio.com/docs/extensions/yocode)。 要安装它，请在命令提示符中运行以下代码：

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-keymap-extension"></a>创建键映射扩展

创建一个扩展：

1. 用以下命令启动扩展生成器：

   `yo azuredatastudio`

2. 从扩展类型列表中选择“新键映射”：

   :::image type="content" source="media/keymap-extension/extension-generator.png" alt-text="扩展生成器":::

3. 按步骤填写扩展名称（对于本教程，请使用 ssmskeymap2）并添加描述。

完成前面的步骤后，系统会创建一个新文件夹。 在 Visual Studio Code 中打开该文件夹，然后便可创建自己的键绑定扩展了！

### <a name="add-a-keyboard-shortcut"></a>添加键盘快捷方式

**步骤 1：查找要替换的快捷方式**

现在已可创建扩展，请在 Azure Data Studio 中添加一些 SSMS 键盘快捷方式（或键绑定）。 我使用了 [Andy Mallon 的 cheatsheet](https://am2.co/2018/02/updated-cheat-sheet/) 和 RedGate 的键盘快捷方式列表来获取灵感。

我发现遗漏了一些重要内容：

- 运行查询，并启用实际的执行计划。 这是 SSMS 中的 Ctrl+M，并且在 Azure Data Studio 中没有绑定。
- 将 CTRL+SHIFT+E 作为运行查询的第二种方式。 用户反馈显示缺少此内容。
- 让 ALT+F1 运行 `sp_help`。 我们在 Azure Data Studio 中添加了此项，但由于该绑定已在使用中，因此我们将其映射到 ALT+F2。
- 切换全屏 (SHIFT+ALT+ENTER)。
- F8，用于显示“对象资源管理器” / “服务器视图”。

可以轻松查找和替换这些键绑定。 运行“打开键盘快捷方式”以显示 Azure Data Studio 中的“键盘快捷方式”选项卡，搜索“查询”，然后选择“更改键绑定”。 更改完键绑定后，可以在 keybindings.json 文件中看到更新的映射（运行“打开键盘快捷方式”可查看该文件）。

:::image type="content" source="media/keymap-extension/keyboard-shortcuts.png" alt-text="键盘快捷方式":::

:::image type="content" source="media/keymap-extension/key-bindings-json.png" alt-text="Keybindings.json 扩展":::

**步骤 2：向扩展添加快捷方式**

若要向扩展添加快捷方式，请打开 package.json 文件（在扩展中），并将 `contributes` 部分替换为以下代码：

```json
"contributes": {
  "keybindings": [
    {
      "key": "shift+cmd+e",
      "command": "runQueryKeyboardAction"
    },
    {
      "key": "ctrl+cmd+e",
      "command": "workbench.view.explorer"
    },
    {
      "key": "alt+f1",
      "command": "workbench.action.query.shortcut1"
    },
    {
      "key": "shift+alt+enter",
      "command": "workbench.action.toggleFullScreen"
    },
    {
      "key": "f8",
      "command": "workbench.view.connections"
    },
    {
      "key": "ctrl+m",
      "command": "runCurrentQueryWithActualPlanKeyboardAction"
    }
  ]
}
```

## <a name="test-your-extension"></a>测试扩展

在 Azure Data Studio 中，通过运行“在 PATH 中安装 azuredatastudio 命令”命令，确保 `azuredatastudio` 位于 PATH 中。

确保 Visual Studio Code 中安装了 Azure Data Studio 调试扩展。

选择“F5”，在调试模式下启动 Azure Data Studio 并运行扩展：

:::image type="content" source="media/keymap-extension/install-extension.png" alt-text="安装扩展":::

:::image type="content" source="media/keymap-extension/test-extension.png" alt-text="测试扩展":::

最快创建出来的扩展之一是键映射，新的扩展现应已成功运行并可进行共享了。

## <a name="package-your-extension"></a>打包扩展

要与他人共享，需要将扩展集中打包到一个文件中。 这样可发布到 Azure Data Studio 扩展市场，或在团队或社区之间共享。 为此，需要从命令行安装另一个 npm 包：

```console
npm install -g vsce
```

导航到扩展的基本目录，并运行 `vsce package`。 我不得不额外添加了几行代码来阻止 vsce 工具发出错误消息：

```json
"repository": {
    "type": "git",
    "url": "https://github.com/kevcunnane/ssmskeymap.git"
},
"bugs": {
    "url": "https://github.com/kevcunnane/ssmskeymap/issues"
},
```

完成后，就生成了 ssmskeymap-0.1.0.vsix 文件，并已可进行安装和与全世界共享了！

:::image type="content" source="media/keymap-extension/extensions.png" alt-text="安装":::

## <a name="publish-your-extension-to-the-marketplace"></a>将扩展发布到市场

Azure Data Studio 扩展市场正在构建中，但当前需要在某个位置（例如 GitHub 发布页面）托管扩展 VSIX，然后提交 PR，请求使用扩展信息更新此 [JSON 文件](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json)。

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何执行以下操作：
> [!div class="checklist"]
> - 创建一个扩展项目
> - 安装扩展生成器
> - 创建扩展
> - 向扩展添加自定义键绑定
> - 测试扩展
> - 打包扩展
> - 将扩展发布到市场

希望本教程能为你提供灵感，构建自己的 Azure Data Studio 扩展。 我们提供仪表板见解支持（针对 SQL Server 运行的很棒的图表）、一些特定于 SQL 的 API，以及从 Visual Studio Code 继承的大量现有扩展点。

如果有想法但不确定如何着手，请提出问题或在推特上 @[azuredatastudio](https://twitter.com/azuredatastudio)。

可持续参考 [Visual Studio Code 扩展指南](https://code.visualstudio.com/docs/extensions/overview)，因为它涵盖了所有现有 API 和模式。

若要了解如何在 Azure Data Studio 中使用 T-SQL，请完成 T-SQL 编辑器教程：

> [!div class="nextstepaction"]
> [使用 Transact-SQL 编辑器创建数据库对象](../tutorial-sql-editor.md)
