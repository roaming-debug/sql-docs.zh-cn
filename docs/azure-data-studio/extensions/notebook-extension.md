---
title: 创建 Jupyter 笔记本扩展
description: 了解如何使用扩展生成器将笔记本打包到扩展中。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 36ed1466decd88686647302e65cf24746393d1f3
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495728"
---
# <a name="create-a-jupyter-notebook-extension"></a>创建 Jupyter 笔记本扩展

本教程演示如何创建新的 Jupyter Notebook Azure Data Studio 扩展。 该扩展提供了一个可以在 Azure Data Studio 中打开和运行的示例 Jupyter 笔记本。

本文介绍如何执行以下操作：

> [!div class="checklist"]
> - 创建一个扩展项目。
> - 安装扩展生成器。
> - 创建笔记本扩展。
> - 运行扩展。
> - 打包扩展。
> - 将扩展发布到市场。

## <a name="apis-used"></a>使用的 API

- `azdata.nb.showNotebookDocument`

### <a name="extension-use-cases"></a>扩展用例

创建笔记本扩展有以下几点不同的原因：
- 共享交互式文档
- 保存并随时访问该笔记本
- 提供用户可遵循的编码问题
- 对笔记本进行版本控制并跟踪笔记本更新

## <a name="prerequisites"></a>先决条件

Azure Data Studio 建立在与 Visual Studio Code 相同的框架上，因此 Azure Data Studio 的扩展是使用 Visual Studio Code 生成的。 要开始操作，需满足以下条件：

- 已在 `$PATH` 中安装 [Node.js](https://nodejs.org) 且可用。 Node.js 包含 [npm](https://www.npmjs.com/)，它是用于安装扩展生成器的 Node.js 包管理器。
- 已有 [Visual Studio Code](https://code.visualstudio.com)，用于调试扩展。
- 确保 `azuredatastudio` 位于你的路径中。 对于 Windows，请确保选择 setup.exe 中的“添加到路径”选项。 对于 Mac 或 Linux，请从 Azure Data Studio 的命令面板运行“在 PATH 中安装 'azuredatastudio' 命令”。

## <a name="install-the-extension-generator"></a>安装扩展生成器

为了简化创建扩展的过程，已使用 Yeoman 构建了一个[扩展生成器](https://www.npmjs.com/package/generator-azuredatastudio)。 要安装它，请在命令提示符下运行以下命令：

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-extension"></a>创建扩展

创建一个扩展：

1. 用以下命令启动扩展生成器：

   `yo azuredatastudio`

2. 从扩展类型列表中选择“新建笔记本(个人)”。

   :::image type="content" source="media/notebook-extension/notebook-extension-generator.png" alt-text="笔记本扩展生成器":::

3. 按照以下步骤填写扩展名称。 在本教程中，请使用测试笔记本。 然后填写发布服务器名称。 在本教程中，使用“Microsoft”。 最后，添加说明。

现在，这里就是某些分支所在的位置。 你可以添加已创建的 Jupyter Notebook，也可以使用生成器为你提供的示例笔记本。

在本教程中，我们将使用示例 Python 笔记本：

   :::image type="content" source="media/notebook-extension/notebook-sample-generator.png" alt-text="选择 python 示例":::

如果你有想要发布的笔记本，请回答你有要发布的现有笔记本。 提供所有笔记本或 Markdown 文件的实际文件路径。

完成前面的步骤后，系统会使用示例笔记本创建一个新文件夹。 在 Visual Studio Code 中打开该文件夹，然后便可发布你的新笔记本扩展了。

## <a name="understand-your-extension"></a>了解扩展

项目当前应如下所示：

   :::image type="content" source="media/notebook-extension/notebook-file-structure-generator.png" alt-text="扩展文件结构":::

`vsc-extension-quickstart.md` 文件提供了重要文件的引用。 `README.md` 文件是你可以为新扩展提供文档的位置。 请注意 `package.json`、`notebook.ts` 和 `pySample.ipynb` 文件。

如果你不想发布某些文件或文件夹，则可以将其名称包含在 `.vscodeignore` 文件中。

你可以通过查看 `notebook.ts`，了解新构建的扩展的作用。

```javascript
// This function is called when you run the command `Launch Notebooks: Test Notebook` from the
// command palette in Azure Data Studio. If you want any additional functionality
// to occur when you launch the book, add it to the activate function.
export function activate(context: vscode.ExtensionContext) {
    context.subscriptions.push(vscode.commands.registerCommand('launchNotebooks.test-notebook', () => {
        let notebooksToDisplay: Array<string> = processNotebooks();
        notebooksToDisplay.forEach(name => {
            azdata.nb.showNotebookDocument(vscode.Uri.file(name));
        });
    }));

    // Add other code here if you want to register another command.
}
```

这是 `notebook.ts` 中的主要函数，每当我们通过命令“Launch Notebooks:测试笔记本”时，`notebook.ts` 文件也起着重要作用。 我们使用 `vscode.commands.registerCommand` API 创建新的命令。 以下大括号中的定义是每次调用命令时运行的代码。 对于通过 `processNotebooks` 函数找到的每个笔记本，我们使用 `azdata.nb.showNotebookDocument` 在 Azure Data Studio 中打开它。

在注册命令“Launch Notebooks:测试笔记本”时，`package.json` 文件也起着重要作用。

```json
"activationEvents": [
        "onCommand:launchNotebooks.test-notebook"
    ],
    "main": "./out/notebook.js",
    "contributes": {
        "commands": [
            {
                "command": "launchNotebooks.test-notebook",
                "title": "Launch Notebooks: Test Notebook"
            }
        ]
    }
```

我们具有针对该命令的激活事件，并且还添加了特定的贡献点。 当用户查看你的扩展时，这些贡献点将显示在扩展市场中，也就是发布扩展的位置。 如果要添加更多命令，请确保将它们添加到 `activationEvents` 字段。 有关更多选项，请参阅[激活事件](https://code.visualstudio.com/api/references/activation-events)。

## <a name="package-your-extension"></a>打包扩展

要与他人共享，需要将扩展集中打包到一个文件中。 扩展可发布到 Azure Data Studio 扩展市场，或与团队或社区共享。 要执行此步骤，需要从命令行安装另一个 npm 包：

```console
npm install -g vsce
```

根据需要编辑 `README.md` 文件。 然后前往扩展的基本目录，并运行 `vsce package`。 你可以选择将存储库链接到扩展，也可以选择不链接存储库并继续操作。 若要添加存储库，请在 `package.json` 文件中添加类似的行。

```json
"repository": {
    "type": "git",
    "url": "https://github.com/laurajjiang/testnotebook.git"
}
```

添加这些行后，`my test-notebook-0.0.1.vsix` 文件便创建完毕，可以进行安装并与他人共享。

## <a name="run-your-extension"></a>运行扩展

若要运行和测试扩展，请打开 Azure Data Studio 并按 Ctrl+Shift+P 打开命令面板。 查找命令“扩展: Install from VSIX”，并浏览到包含新扩展的文件夹。

   :::image type="content" source="media/notebook-extension/install-vsix.png" alt-text="安装 VSIX":::

该扩展现在应显示在 Azure Data Studio 的扩展面板中。 再次打开命令面板，你将发现我们通过扩展创建的新命令“Launch Book:测试书籍”。 运行后，该命令应该会打开我们打包到扩展中的 Jupyter 书籍。

   :::image type="content" source="media/notebook-extension/notebook-launch-ads.png" alt-text="笔记本命令":::

祝贺你！ 你已构建并且现在可以发布你的第一个 Jupyter 笔记本扩展。

## <a name="publish-your-extension-to-the-marketplace"></a>将扩展发布到市场

Azure Data Studio 扩展市场正在构建中。 若要发布，请在某个位置（例如 GitHub 发布页）托管扩展 VSIX。 然后，提交拉取请求以使用扩展信息更新[此 JSON 文件](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json)。

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何执行以下操作：
> [!div class="checklist"]
> - 创建一个扩展项目。
> - 安装扩展生成器。
> - 创建笔记本扩展。
> - 创建扩展。
> - 打包扩展。
> - 将扩展发布到市场。

希望本文能为你提供灵感，构建自己的 Azure Data Studio 扩展。

如果有想法但不确定如何着手，请提出问题或在推特上 @[azuredatastudio](https://twitter.com/azuredatastudio)。

有关详细信息，请参考 [Visual Studio Code 扩展指南](https://code.visualstudio.com/docs/extensions/overview)，其中介绍了所有现有 API 和模式。
