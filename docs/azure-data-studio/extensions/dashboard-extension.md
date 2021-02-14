---
title: 创建面板扩展
description: 本教程演示如何创建仪表板扩展以将自定义功能添加到 Azure Data Studio。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan
ms.custom: ''
ms.date: 08/28/2020
ms.openlocfilehash: 400dae97922503d37b1ca1c2b726264032fa12cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048597"
---
# <a name="create-an-azure-data-studio-dashboard-extension"></a>创建 Azure Data Studio 面板扩展

本教程演示如何创建新的 Azure Data Studio 面板扩展。 该扩展可对 Azure Data Studio 连接仪表板提供帮助，因此，你可以通过用户很容易看到的方式扩展 Azure Data Studio 的功能。

本文介绍如何执行以下操作：

> [!div class="checklist"]
> - 安装扩展生成器。
> - 创建扩展。
> - 向扩展中的面板提供内容。
> - 测试扩展。
> - 打包扩展。
> - 将扩展发布到市场。

## <a name="prerequisites"></a>先决条件

Azure Data Studio 建立在与 Visual Studio Code 相同的框架上，因此 Azure Data Studio 的扩展是使用 Visual Studio Code 生成的。 要开始操作，需满足以下条件：

- 已在 `$PATH` 中安装 [Node.js](https://nodejs.org) 且可用。 Node.js 包含 [npm](https://www.npmjs.com/)，它是用于安装扩展生成器的 Node.js 包管理器。
- 已有 [Visual Studio Code](https://code.visualstudio.com)，用于调试扩展。
- 有 Azure Data Studio [调试扩展](https://marketplace.visualstudio.com/items?itemName=ms-mssql.sqlops-debug)（可选）。 借助调试扩展，无需将扩展打包并安装到 Azure Data Studio 中也可测试扩展。
- 确保 `azuredatastudio` 位于你的路径中。 对于 Windows，请确保选择 setup.exe 中的“添加到路径”选项。 对于 Mac 或 Linux，请从 Azure Data Studio 的命令面板运行“在 PATH 中安装 'azuredatastudio' 命令”。

## <a name="install-the-extension-generator"></a>安装扩展生成器

为了简化创建扩展的过程，已使用 Yeoman 构建了一个[扩展生成器](https://code.visualstudio.com/docs/extensions/yocode)。 要安装它，请在命令提示符下运行以下命令：

```console
npm install -g yo generator-azuredatastudio
```

## <a name="create-your-dashboard-extension"></a>创建仪表板扩展

### <a name="introduction-to-the-dashboard"></a>仪表板简介

Azure Data Studio 连接仪表板是一种功能强大的工具，可对用户的连接进行汇总并针对此类连接提供见解。

面板具有两种变体： 对整个服务器进行汇总的“服务器仪表板”和对单个数据库进行汇总的“数据库仪表板” 。 可以通过以下方式访问任一仪表板：右键单击 Azure Data Studio“连接”Viewlet 中的服务器或数据库，然后单击“管理” 。

:::image type="content" source="media/dashboard-extension/dashboard-summary.gif" alt-text="显示仪表板简介的屏幕截图。":::

有 3 个关键贡献点可以帮助扩展将功能添加到仪表板：

1. **“完整仪表板”选项卡**：仪表板中用于扩展的一个单独选项卡。 可以添加到服务器仪表板或数据库仪表板。 可以通过小组件、工具栏和导航部分进行自定义。
2. **主页操作**：连接工具栏顶部的操作按钮。
3. **小组件**：针对 SQL Server 运行的图表。

   :::image type="content" source="media/dashboard-extension/dashboard-contrib-points.png" alt-text="显示贡献点的屏幕截图。":::

### <a name="run-the-extension-generator"></a>运行扩展生成器

创建一个扩展：

1. 用以下命令启动扩展生成器：

   `yo azuredatastudio`

1. 从扩展类型列表中选择“新建仪表板”。

1. 如图所示，填写提示，创建一个向服务器仪表板提供选项卡的扩展。

   :::image type="content" source="media/dashboard-extension/dashboard-generator.png" alt-text="显示扩展生成器的屏幕截图。":::

   由于存在很多提示，因此在下面介绍了有关每个问题含义的详细信息：

   :::image type="content" source="media/dashboard-extension/dashboard-flowchart.png" alt-text="显示仪表板流程图的屏幕截图。":::

完成前面的步骤后，系统会创建一个新文件夹。 在 Visual Studio Code 中打开该文件夹，然后便可创建自己的仪表板扩展了。

### <a name="run-the-extension"></a>运行扩展

让我们通过运行扩展来了解仪表板模板为我们提供的内容。 在运行之前，请确保 Visual Studio Code 中安装了 Azure Data Studio 调试扩展。

在 Visual Studio Code 中选择 F5，在调试模式下启动 Azure Data Studio 并运行扩展。 然后，可以查看此默认模板如何向仪表板提供内容。

接下来，我们将介绍如何修改此默认仪表板。

### <a name="develop-the-dashboard"></a>开发仪表板

用于扩展开发入门的最重要文件是 `package.json`。 该文件是清单文件，其中注册了仪表板贡献。 请注意 `dashboard.tabs`、`dashboard.insights` 和 `dashboard.containers` 部分。

下面是要尝试的一些更改：

- 试用见解类型，包括 bar、horizontalBar 和 timeSeries。
- 编写自己的查询，对 SQL Server 连接运行该查询。
- 请参阅此[示例见解教程](../tutorial-qds-sql-server.md)或[此教程](../tutorial-table-space-sql-server.md)，了解特定的见解教程。

## <a name="package-your-extension"></a>打包扩展

要与他人共享，需要将扩展集中打包到一个文件中。 扩展可发布到 Azure Data Studio 扩展市场，或与团队或社区共享。 要执行此步骤，需要从命令行安装另一个 npm 包：

```console
npm install -g vsce
```

根据需要编辑 `README.md` 文件。 然后前往扩展的基本目录，并运行 `vsce package`。 你可以选择将存储库链接到扩展，也可以选择不链接存储库并继续操作。 若要添加存储库，请在 `package.json` 文件中添加类似的行。

```json
"repository": {
    "type": "git",
    "url": "https://github.com/anjalia/my-test-extension.git"
}
```

添加这些行后，`my-test-extension-0.0.1.vsix` 文件便创建完毕，可以在 Azure Data Studio 中安装了。

:::image type="content" source="media/dashboard-extension/install-vsix.png" alt-text="显示安装 VSIX 的屏幕截图。":::

## <a name="publish-your-extension-to-the-marketplace"></a>将扩展发布到市场

Azure Data Studio 扩展市场正在构建中。 当前需要在某个位置（例如 GitHub 发布页）托管扩展 VSIX。 然后，你将提交拉取请求以使用扩展信息更新[此 JSON 文件](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json)。

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何执行以下操作：
> [!div class="checklist"]
> - 安装扩展生成器。
> - 创建扩展。
> - 向扩展中的面板提供内容。
> - 测试扩展。
> - 打包扩展。
> - 将扩展发布到市场。

希望本文能为你提供灵感，构建自己的 Azure Data Studio 扩展。 我们提供仪表板见解支持（针对 SQL Server 运行的实用图表）、一些特定于 SQL 的 API，以及从 Visual Studio Code 继承的大量现有扩展点。

如果有想法但不确定如何着手，请提出问题或在推特上 @[azuredatastudio](https://twitter.com/azuredatastudio)。

有关详细信息，请参考 [Visual Studio Code 扩展指南](https://code.visualstudio.com/docs/extensions/overview)，其中介绍了所有现有 API 和模式。

若要了解如何在 Azure Data Studio 中使用 T-SQL，请完成 T-SQL 编辑器教程：

> [!div class="nextstepaction"]
> [使用 Transact-SQL 编辑器创建数据库对象](../tutorial-sql-editor.md)
