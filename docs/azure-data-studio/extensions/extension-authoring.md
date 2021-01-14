---
title: 创建扩展
description: 你可以使用扩展向 Azure Data Studio 添加功能。 了解如何创建扩展并将其发布到扩展库。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: ''
ms.date: 08/26/2020
ms.openlocfilehash: d23e37065eb18a7a22d5717a95379161c10870a0
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151233"
---
# <a name="extend-functionality-by-creating-azure-data-studio-extensions"></a>通过创建 Azure Data Studio 扩展来扩展功能

借助 Azure Data Studio 中的扩展，可轻松地向基本 Azure Data Studio 安装添加更多功能。

扩展由 Azure Data Studio 团队 (Microsoft) 以及第三方社区（你）提供。

## <a name="author-an-extension"></a>编写扩展

如果你对扩展 Azure Data Studio 感兴趣，则可创建自己的扩展并将其发布到扩展库。

### <a name="write-an-extension"></a>编写扩展

#### <a name="prerequisites"></a>先决条件

若要开发扩展，你需要在 `$PATH` 中安装并提供 [Node.js](https://nodejs.org/)。 Node.js 包含 npm，它是用于安装扩展生成器的 Node.js 包管理器。

若要创建新的扩展，可使用 Azure Data Studio 扩展生成器。 Yeoman [扩展生成器](https://www.npmjs.com/package/generator-azuredatastudio)是扩展项目的一个有用起点。 若要启动生成器，请在命令提示符处输入以下命令：

```console
npm install -g yo generator-azuredatastudio
yo azuredatastudio
```

有关如何开始使用扩展模板的深入指南，请参阅[键映射扩展](keymap-extension.md)，这将指导你完成创建扩展。

### <a name="extensibility-references"></a>扩展性参考

若要了解 Azure Data Studio 扩展性，请参阅[扩展性概述](../extensibility.md)。 还可在现有[示例](https://github.com/Microsoft/azuredatastudio/tree/main/samples)中查看如何使用 API 的示例。

## <a name="debug-an-extension"></a>调试扩展

可使用 Visual Studio Code 扩展 [Azure Data Studio 调试](https://github.com/kevcunnane/sqlops-debug)来调试新扩展。

若要调试扩展，请执行以下操作：

1. 使用 [Visual Studio Code](https://code.visualstudio.com/) 打开扩展。
2. 安装 Azure Data Studio 调试扩展。
3. 按 F5 或选择“调试”图标，然后选择“开始”  。
4. Azure Data Studio 的新实例在特殊模式（扩展开发主机）下启动。 此新实例现在可以识别你的扩展。

## <a name="create-an-extension-package"></a>创建扩展包

编写扩展后，需要创建 VSIX 包，以便在 Azure Data Studio 中进行安装。 可以使用 [vscode-vsce](https://github.com/Microsoft/vscode-vsce)（Visual Studio Code 扩展）创建 VSIX 包。

```console
npm install -g vsce
cd myExtensionName
vsce package
# The myExtensionName.vsix file has now been generated
```

借助 VSIX 包，可以通过共享 .vsix 文件并使用命令面板中的命令“Extensions:Install From VSIX File”将扩展安装到 Azure Data Studio 中，从而以本地和非公开形式共享扩展。

## <a name="publish-an-extension"></a>发布扩展

若要将新扩展发布到 Azure Data Studio，请执行以下操作：

1. 将扩展添加到[扩展库](https://github.com/Microsoft/azuredatastudio/blob/release/extensions/extensionsGallery.json)。
2. 我们目前不支持托管第三方扩展。 Azure Data Studio 不会下载扩展，但提供了浏览到下载页面的选项。 若要为扩展设置下载页，请设置资产“Microsoft.AzureDataStudio.DownloadPage”的值。
3. 针对发布/扩展分支创建 PR。
4. 向团队发送评审请求。

你的扩展将接受评审并添加到扩展库中。

### <a name="publish-extension-updates"></a>发布扩展更新

发布更新的过程与发布扩展的过程类似。 请确保在 package.json 中更新版本。

## <a name="next-steps"></a>后续步骤

有关如何入门的分步说明，请参阅以下扩展创作教程之一：

- [Keymap 扩展教程](keymap-extension.md)
- [仪表板扩展教程](dashboard-extension.md)
- [笔记本扩展教程](notebook-extension.md)
- [Jupyter 书籍扩展教程](jupyter-book-extension.md)
- [向导扩展教程](wizard-extension.md)
