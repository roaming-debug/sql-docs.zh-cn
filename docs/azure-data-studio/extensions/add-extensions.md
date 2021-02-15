---
title: 添加扩展
description: Microsoft 和第三方提供了很多扩展，请了解如何选择和安装它们来将功能添加到 Azure Data Studio。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: ''
ms.date: 10/03/2019
ms.openlocfilehash: 3cc965ffd169d53e59706ca4ba68f2bcdddb5aa9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052117"
---
# <a name="extend-the-functionality-of-azure-data-studio"></a>扩展 Azure Data Studio 的功能

Azure Data Studio 中的扩展提供向基本 Azure Data Studio 安装添加更多功能的简便方法。

扩展由 Azure Data Studio 团队 (Microsoft) 以及第三方社区（你）提供。 有关创建扩展的详细信息，请参阅[扩展创作](./extension-authoring.md)。

## <a name="add-azure-data-studio-extensions"></a>添加 Azure Data Studio 扩展

1. 通过选择扩展图标或选择“视图”菜单中的“扩展”来访问可用扩展 。 可以使用“视图 **：显示扩展”命令，该命令可在“命令面板”中找到（F1 或 `Ctrl+Shift+P`）** 。

    ![扩展管理器图标](media/add-extensions/extension-manager-icon.png)

    还可通过按 `Ctrl+Shift+X` (Windows/Linux) 或 `Command+Shift+X` (Mac) 来快速访问扩展管理器。

2. 选择某个可用扩展以查看其详细信息。

    ![扩展详细信息](media/add-extensions/extension-details.png)

3. 选择所需的扩展并“安装”它。

4. 安装后，重载以启用 Azure Data Studio 中的扩展（仅在第一次安装扩展时需要进行此操作）。

如果访问 Azure Data Studio 上的扩展管理器时遇到问题，则可以在 [GitHub Wiki](https://github.com/microsoft/azuredatastudio/wiki/List-of-Extensions) 上下载所需的扩展。

## <a name="manage-extensions"></a>管理扩展

### <a name="list-installed-extensions"></a>列出已安装的扩展

默认的“扩展”视图显示当前启用的扩展、推荐的所有扩展以及所有当前禁用的扩展的折叠容器。 “扩展 **：显示已安装扩展”命令，该命令可以在“命令面板”或“更多操作”`(...)` 下拉菜单中找到，显示所有已安装扩展的列表，包括已禁用的扩展**  。

### <a name="uninstall-an-extension"></a>卸载扩展

若要卸载扩展，请单击扩展条目右侧的齿轮图标，然后从下拉菜单中选择“卸载”。 这将卸载选定的扩展，并提示你重新加载 Azure Data Studio。

 ![扩展下拉列表](media/add-extensions/extension-gear-dropdown.png)

### <a name="disable-an-extension"></a>禁用一个扩展

可以暂时禁用扩展，而不是永久删除扩展。 可以禁用所有 Azure Data Studio 会话的扩展（“禁用”），也可以仅禁用当前工作区的扩展（“禁用(工作区)”） 。 还可以通过命令面板使用命令“扩展 **：禁用所有扩展”和“扩展** **：禁用所有扩展(工作区)”来禁用所有当前安装的扩展程序**。

### <a name="enable-an-extension"></a>添加扩展

如果扩展已禁用，它将位于扩展列表的“已禁用”部分，并标记为“已禁用” ***。可以使用下拉菜单中的“启用”或“启用(工作区)”命令来重新启用它*** 。 通过命令面板，还可使用命令“扩展 **：启用所有扩展”和“扩展** **：启用所有扩展(工作区)”来启用所有扩展**。

![启用扩展](media/add-extensions/extensions-enable.png)

### <a name="updating-an-extension"></a>更新扩展

Azure Data Studio 会自动检查并安装任何已安装扩展的更新。 若要关闭自动更新功能，则可以使用“扩展 **：禁用自动更新扩展”命令来禁用自动更新**。

若要手动更新扩展，可以使用“扩展 **：显示过期的扩展”命令检查是否有扩展更新，该命令使用 `@outdated` 筛选器在扩展列表中进行搜索**。 这将显示所有当前安装的扩展的任何可用更新。 在过期的扩展上单击“更新”按钮可安装更新。 系统将提示你重载 Azure Data Studio。 还可以使用“扩展 **：更新所有扩展”命令同时更新所有过期的扩展**。

“扩展 **：检查是否有扩展更新”命令是另一种检查哪些扩展具有可用更新的方法**。

## <a name="install-from-a-vsix"></a>从 VSIX 安装

可以使用“扩展”视图命令下拉列表中的“从 VSIX 安装”命令，或者命令面板中的“扩展`.vsix` **：从 VSIX 安装”命令，手动安装打包在  文件中的 Azure Data Studio 扩展，并指向扩展的  文件。

## <a name="access-installed-azure-data-studio-extensions"></a>访问已安装的 Azure Data Studio 扩展

每个扩展都以不同的方式增强你在 Azure Data Studio 中的体验。 因此，扩展的入口点可能会有所不同。 请参阅已安装的扩展的单独文档，了解关于如何在安装该扩展后访问其功能的信息。

## <a name="extensions-view-filters"></a>扩展视图筛选器

“扩展”视图搜索框支持筛选器，以帮助你查找和管理扩展。 命令“显示安装的扩展”和“显示建议的扩展”在搜索框中使用 `@installed` 和 `@recommended` 等筛选器 。

通过在扩展搜索框中键入 @ 并浏览建议，可以查看所有筛选器和排序命令的完整列表：

![扩展排序](media/add-extensions/extension-sort.png)

以下是“扩展”视图筛选器：

- `@builtin` - 显示 Azure Data Studio 附带的扩展。 按类型（编程语言、主题等）分组。
- `@disabled` - 显示已禁用的安装扩展。
- `@enabled` - 显示已启用的安装扩展。 可以单独启用/禁用扩展。
- `@installed` - 显示已安装的扩展。
- `@outdated` - 显示已过期的扩展。 市场现在提供新版本。
- `@recommended` - 显示建议的扩展。 按工作区特定用途或常规用途分组。
- `@category` - 显示属于指定类别的扩展。 下面是几个受支持的类别。 要获得完整的列表，请键入 @category 并按照建议列表中的选项进行操作：
    - `@category:themes`
    - `@category:formatters`
    - `@category:snippets` 这些筛选器也可以组合。 例如，`@installed @category:themes` 显示所有已安装的主题。

如果未提供筛选器，则“扩展”视图将显示当前已安装和建议的扩展。

### <a name="sorting"></a>排序

可以使用 `@sort` 筛选器对扩展进行排序，它采用以下值：

- `installs` - 按扩展库的安装计数降序排序。
- `rating` - 按扩展库等级（1 - 5 星）降序排序。
- `name` - 按扩展名的字母顺序排序。

## <a name="common-questions"></a>常见问题

### <a name="where-are-extensions-installed"></a>扩展安装在哪里？

扩展安装在每用户扩展文件夹中。 根据你的平台，该位置位于以下文件夹中：

- Windows `%USERPROFILE%\.azuredatastudio\extensions`
- macOS `~/.azuredatastudio/extensions`
- Linux `~/.azuredatastudio/extensions`
