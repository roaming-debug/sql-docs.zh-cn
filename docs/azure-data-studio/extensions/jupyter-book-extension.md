---
title: 创建 Jupyter 书籍扩展
description: 了解如何使用扩展生成器将 Jupyter 书籍打包到扩展中。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 37875ecc79d471d1e7226337cec65c3323e19875
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052058"
---
# <a name="create-a-jupyter-book-extension"></a>创建 Jupyter 书籍扩展

本教程演示如何创建新的 Jupyter 书籍 Azure Data Studio 扩展。 该扩展提供了一个可以在 Azure Data Studio 中打开和运行的示例 Jupyter 书籍。

本文介绍如何执行以下操作：

> [!div class="checklist"]
> - 创建一个扩展项目。
> - 安装扩展生成器。
> - 创建 Jupyter 书籍扩展。
> - 运行扩展。
> - 打包扩展。
> - 将扩展发布到市场。

## <a name="apis-used"></a>使用的 API

- `bookTreeView.openBook`

### <a name="extension-use-cases"></a>扩展用例

创建 Jupyter 书籍扩展有以下几点不同的原因：

- 共享已组织和分区的交互式文档
- 共享整个书籍（类似于电子书，但通过 Azure Data Studio 分发）
- 对 Jupyter 书籍进行版本控制并跟踪 Jupyter 书籍更新

Jupyter 书籍扩展与笔记本扩展之间的主要区别在于，Jupyter 书籍提供了结构。 数十个笔记本可以拆分为一本 Jupyter 书籍中的不同章节，但笔记本扩展旨在提供少量单独的笔记本。

## <a name="prerequisites"></a>先决条件

Azure Data Studio 建立在与 Visual Studio Code 相同的框架上，因此 Azure Data Studio 的扩展是使用 Visual Studio Code 生成的。 要开始操作，需满足以下条件：

- 已在 `$PATH` 中安装 [Node.js](https://nodejs.org) 且可用。 Node.js 包含 [npm](https://www.npmjs.com/)，它是用于安装扩展生成器的 Node.js 包管理器。
- 用于对扩展进行任何更改以及调试扩展的 [Visual Studio Code](https://code.visualstudio.com)。
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

1. 从扩展类型列表中选择“新建 Jupyter 书籍”。

   :::image type="content" source="media/jupyter-book-extension/jupyter-book-extension-generator.png" alt-text="显示扩展生成器的屏幕截图。":::

1. 按照以下步骤填写扩展名称。 在本教程中，请使用测试书籍。 然后填写发布服务器名称。 在本教程中，使用“Microsoft”。 最后，添加说明。

可以选择提供现有的 Jupyter 书籍，使用提供的示例书籍，或者创建新的 Jupyter 书籍。 下面显示了这三个选项。

### <a name="provide-an-existing-book"></a>提供现有书籍

如果想要发布已创建的书籍，请提供书籍内容所在文件夹的绝对文件路径。 然后，可以随时开始学习扩展并进行发布。

:::image type="content" source="media/jupyter-book-extension/jupyter-book-existing-book.png" alt-text="显示现有书籍的屏幕截图。":::

### <a name="use-the-sample-book"></a>使用示例书籍

如果没有现有书籍或笔记本，则可以使用生成器中提供的示例。

:::image type="content" source="media/jupyter-book-extension/jupyter-book-sample-path.png" alt-text="显示示例 Jupyter 书籍的屏幕截图。":::

该示例书籍演示了简单的 Jupyter 书籍。 如果想要了解如何自定义 Jupyter 书籍，请参阅以下部分，了解如何使用现有笔记本创建新书籍。

### <a name="create-a-new-book"></a>创建新书籍

你可以根据需要将笔记本打包到 Jupyter 书籍中。 生成器会询问你是否想要在书籍中包含章节，如果是，还会询问包含多少个章节以及这些章节的标题。 选择过程如下所示。 使用空格键选择要加入每个章节的笔记本。

:::image type="content" source="media/jupyter-book-extension/jupyter-book-create-book.png" alt-text="显示创建 Jupyter 书籍的屏幕截图。":::

完成前面的步骤后，系统会使用新的 Jupyter 书籍创建一个新文件夹。 在 Visual Studio Code 中打开该文件夹，然后便可发布你的 Jupyter 书籍扩展了。

## <a name="understand-your-extension"></a>了解扩展

项目当前应如下所示：

   :::image type="content" source="media/jupyter-book-extension/jupyter-book-file-structure-generator.png" alt-text="显示扩展文件结构的屏幕截图。":::

`vsc-extension-quickstart.md` 文件提供了重要文件的引用。 `README.md` 文件是你可以为新扩展提供文档的位置。 请注意 `package.json`、`jupyter-book.ts`、`content` 和 `toc.yml` 文件。 `content` 文件夹包含所有笔记本文件或 Markdown 文件。 `toc.yml` 建立 Jupyter 书籍结构，该文件在你选择通过扩展生成器创建自定义 Jupyter 书籍时自动生成。

如果你使用生成器创建书籍，并且选择了书籍中的章节，则文件夹结构看起来会稍有不同。 可能存在与你为章节选择的标题相对应的子文件夹，而不是 `content` 文件夹中的 Markdown 文件和 Jupyter Notebook 文件。

如果你不想发布某些文件或文件夹，则可以将其名称包含在 `.vscodeignore` 文件中。

你可以通过查看 `jupyter-book.ts` 了解新构建的扩展的作用。

```javascript
// This function is called when you run the command `Launch Book: Test Book` from the
// command palette in Azure Data Studio. If you want any additional functionality
// to occur when you launch the book, add it to the activate function.
export function activate(context: vscode.ExtensionContext) {
    context.subscriptions.push(vscode.commands.registerCommand('launchBook.test-book', () => {
        processNotebooks();
    }));

    // Add other code here if you want to register another command.
}
```

`activate` 函数是扩展的主要操作。 任何要注册的命令都应出现在 `activate` 函数内，类似于我们的 `launchBook.test-book` 命令。 在 `processNotebooks` 函数内，查找包含 Jupyter 书籍的扩展文件夹，并使用扩展的文件夹作为参数调用 `bookTreeView.openBook`。

在注册扩展的命令时，`package.json` 文件也起着重要作用。

```json
"activationEvents": [
        "onCommand:launchBook.test-book"
    ],
    "main": "./out/notebook.js",
    "contributes": {
        "commands": [
            {
                "command": "launchBook.test-book",
                "title": "Launch Book: Test Book"
            }
        ]
    }
```

激活事件 `onCommand` 会触发我们在调用命令时注册的函数。 还有一些其他激活事件可用于进行其他自定义。 有关详细信息，请参阅[激活事件](https://code.visualstudio.com/api/references/activation-events)。

## <a name="package-your-extension"></a>打包扩展

要与他人共享，需要将扩展集中打包到一个文件中。 扩展可发布到 Azure Data Studio 扩展市场，或与团队或社区共享。 要执行此步骤，需要从命令行安装另一个 npm 包：

```console
npm install -g vsce
```

根据需要编辑 `README.md` 文件。 然后前往扩展的基本目录，并运行 `vsce package`。 你可以选择将存储库链接到扩展，也可以选择不链接存储库并继续操作。 若要添加存储库，请在 `package.json` 文件中添加类似的行。

```json
"repository": {
    "type": "git",
    "url": "https://github.com/laurajjiang/testbook.git"
}
```

添加这些行后，`my test-book-0.0.1.vsix` 文件便创建完毕，可以在 Azure Data Studio 中安装了。

## <a name="run-your-extension"></a>运行扩展

若要运行和测试扩展，请打开 Azure Data Studio 并按 Ctrl+Shift+P 打开命令面板。 查找命令“扩展: Install from VSIX”，并浏览到包含新扩展的文件夹。 该文件夹现在应显示在 Azure Data Studio 的扩展面板中。

   :::image type="content" source="media/jupyter-book-extension/install-vsix.png" alt-text="显示安装 VSIX 的屏幕截图。":::

再次打开命令面板，找到注册的命令“Launch Book:测试笔记本”。 运行后，该命令应该会打开我们打包到扩展中的 Jupyter 书籍。

   :::image type="content" source="media/jupyter-book-extension/jupyter-book-launch-ads.png" alt-text="显示电子书命令的屏幕截图。":::

祝贺你！ 你已构建并且现在可以发布你的第一个 Jupyter 书籍扩展。 有关 Jupyter 书籍的详细信息，请参阅 [Jupyter 书籍](https://jupyterbook.org/intro.html)。

## <a name="publish-your-extension-to-the-marketplace"></a>将扩展发布到市场

Azure Data Studio 扩展市场正在构建中。 若要发布，请在某个位置（例如 GitHub 发布页）托管扩展 VSIX。 提交拉取请求以使用扩展信息更新[此 JSON 文件](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json)。

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何执行以下操作：
> [!div class="checklist"]
> - 创建一个扩展项目。
> - 安装扩展生成器。
> - 创建 Jupyter 书籍扩展。
> - 打包扩展。
> - 将扩展发布到市场。

我们希望在阅读本文后，你将对 Jupyter 书籍有所了解，并且想要将你的想法与 Azure Data Studio 社区分享。

如果有想法但不确定如何着手，请提出问题或在推特上 @[azuredatastudio](https://twitter.com/azuredatastudio)。

有关详细信息，请参考 [Visual Studio Code 扩展指南](https://code.visualstudio.com/docs/extensions/overview)，其中介绍了所有现有 API 和模式。
