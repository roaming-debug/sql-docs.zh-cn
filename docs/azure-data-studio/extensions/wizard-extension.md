---
title: 创建向导扩展
description: 本教程演示如何创建向导扩展以将自定义功能添加到 Azure Data Studio。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 0fc56805427b820dc8ccb8cafedb0c04eb34ab6d
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495726"
---
# <a name="create-an-azure-data-studio-wizard-extension"></a>创建 Azure Data Studio 向导扩展

本教程演示如何创建新的 Azure Data Studio 向导扩展。 该扩展提供了一个向导，用于与 Azure Data Studio 中的用户进行交互。

本文介绍如何执行以下操作：
> [!div class="checklist"]
> - 安装扩展生成器
> - 创建扩展
> - 向扩展添加自定义向导
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

为了简化创建扩展的过程，已使用 Yeoman 构建了一个[扩展生成器](https://code.visualstudio.com/docs/extensions/yocode)。 要安装它，请从命令提示符处运行以下命令：

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-wizard-extension"></a>创建向导扩展

### <a name="introduction-to-wizards"></a>向导简介

向导是一种用户界面类型，它提供了用户完成任务需要填写的分步页面。 常见示例包括软件安装向导和故障排除向导。 例如：

:::image type="content" source="media/wizard-extension/dacpac-wizard.gif" alt-text="Dacpac 向导":::

由于在处理数据和扩展 Azure Data Studio 的功能时，向导通常非常有用，因此 Azure Data Studio 提供了 API 供你创建自己的自定义向导。 我们将逐步介绍如何生成向导模板，并对其进行修改以创建自己的自定义向导。

### <a name="run-the-extension-generator"></a>运行扩展生成器

创建一个扩展：

1. 用以下命令启动扩展生成器：

   `yo azuredatastudio`

2. 从扩展类型列表中选择“新建向导或对话框”。 然后依次选择“向导”、“入门模板” 

3. 按步骤填写扩展名称（对于本教程，请使用“我的测试扩展”），并添加描述。

    :::image type="content" source="media/wizard-extension/extension-generator.png" alt-text="扩展生成器":::

完成前面的步骤后，系统会创建一个新文件夹。 在 Visual Studio Code 中打开该文件夹，然后便可创建自己的向导扩展了！

### <a name="run-the-extension"></a>运行扩展

让我们通过运行扩展来了解向导模板为我们提供的内容。 在运行之前，请确保 Visual Studio Code 中安装了 Azure Data Studio 调试扩展。

在 VS Code 中选择 F5，在调试模式下启动 Azure Data Studio 并运行扩展。 然后，在 Azure Data Studio 中，通过新窗口中的命令面板 (Ctr+Shift+P) 运行“启动向导”命令。 这将启动此扩展提供的默认向导：

:::image type="content" source="media/wizard-extension/wizard-template.gif" alt-text="向导模板":::

接下来，我们将介绍如何修改此默认向导。

### <a name="develop-the-wizard"></a>开发向导

用于扩展开发入门最重要的文件包括 `package.json`、`src/main.ts` 和 `vsc-extension-quickstart.md`：

- `package.json`：这是清单文件，其中注册了“启动向导”命令。 此外，还在此文件中将 `main.ts` 声明为主程序的入口点。
- `main.ts`：包含用于将 UI 元素（例如页面、文本和按钮）添加到向导的代码
- `vsc-extension-quickstart.md`：包含在开发时可能是有用参考的技术文档

让我们对向导进行更改：我们将添加第四个空白页面。 按如下所示修改 `src/main.ts`，在启动向导时，你将看到其他页面。

:::image type="content" source="media/wizard-extension/wizard-main.png" alt-text="向导主页面":::
熟悉模板之后，可以尝试以下一些其他建议：

- 将一个宽度为 300 的按钮添加到新页面
- 添加一个用于放置按钮的弹性布局组件
- 向按钮添加一项操作。 例如，单击按钮后，启动“打开文件”对话框或打开查询编辑器。

## <a name="package-your-extension"></a>打包扩展

要与他人共享，需要将扩展集中打包到一个文件中。 这样可发布到 Azure Data Studio 扩展市场，或在团队或社区之间共享。 为此，需要从命令行安装另一个 npm 包：

```console
npm install -g vsce
```

根据自己喜好编辑 `README.md`，然后导航到扩展的基目录，并运行 `vsce package`。 你可以选择将存储库链接到扩展，也可以选择不链接存储库并继续操作。 若要添加存储库，请在 `package.json` 文件中添加类似的行。

```json
"repository": {
    "type": "git",
    "url": "https://github.com/anjalia/my-test-extension.git"
}
```

添加这些行之后，系统将创建一个 my-test-extension-0.0.1.vsix 文件，你可以直接在 Azure Data Studio 中安装该文件。

:::image type="content" source="media/wizard-extension/install-vsix.png" alt-text="安装 VSIX":::

## <a name="publish-your-extension-to-the-marketplace"></a>将扩展发布到市场

Azure Data Studio 扩展市场尚未完全实现，但当前需要在某处托管扩展 VSIX（例如，GitHub 发布页面），然后提交 PR，请求使用扩展信息更新此 [JSON 文件](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json)。

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何执行以下操作：
> [!div class="checklist"]
> - 安装扩展生成器
> - 创建扩展
> - 向扩展添加自定义向导
> - 测试扩展
> - 打包扩展
> - 将扩展发布到市场

希望本教程能为你提供灵感，构建自己的 Azure Data Studio 扩展。 我们提供仪表板见解支持（针对 SQL Server 运行的很棒的图表）、一些特定于 SQL 的 API，以及从 Visual Studio Code 继承的大量现有扩展点。

如果有想法但不确定如何着手，请在团队 [azuredatastudio](https://twitter.com/azuredatastudio) 处提出问题或发送推文。

可持续参考 [Visual Studio Code 扩展指南](https://code.visualstudio.com/docs/extensions/overview)，因为它涵盖了所有现有 API 和模式。

若要了解如何在 Azure Data Studio 中使用 T-SQL，请完成 T-SQL 编辑器教程：

> [!div class="nextstepaction"]
> [使用 Transact-SQL 编辑器创建数据库对象](../tutorial-sql-editor.md)
