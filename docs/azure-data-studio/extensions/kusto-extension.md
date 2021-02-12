---
title: Azure Data Studio 的 Kusto (KQL) 扩展
description: 本文介绍如何使用 Azure Data Studio 连接和查询 Azure 数据资源管理器群集。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: jukoesma
ms.custom: ''
ms.date: 10/29/2020
ms.openlocfilehash: 15b2dc331a1387039c79cbab164682f00981c591
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052028"
---
# <a name="kusto-kql-extension-for-azure-data-studio-preview"></a>Azure Data Studio 的 Kusto (KQL) 扩展（预览版）

使用 [Azure Data Studio](../what-is-azure-data-studio.md) 的 Kusto (KQL) 扩展，你可以连接到 [Azure 数据资源管理器](/azure/data-explorer/data-explorer-overview)群集并进行查询。

用户可以使用包含 IntelliSense 的 [Kusto 内核](../notebooks/notebooks-kusto-kernel.md)，编写并运行 KQL 查询，以及创作笔记本。

通过在 Azure Data Studio 中启用本机 Kusto (KQL) 体验版，数据工程师、数据科学家和数据分析师可以根据存储在 Azure 数据资源管理器中的大量数据快速观察趋势和异常情况。

此扩展当前处于预览状态。

## <a name="prerequisites"></a>先决条件

如果还没有 Azure 订阅，可以在开始前创建一个[免费 Azure 帐户](https://azure.microsoft.com/free/)。

还需要以下先决条件：

- [已安装 Azure Data Studio](../download-azure-data-studio.md)。
- [Azure 数据资源管理器群集和数据库](/azure/data-explorer/create-cluster-database-portal)。

## <a name="install-the-kusto-kql-extension"></a>安装 Kusto (KQL) 扩展

若要在 Azure Data Studio 中安装 Kusto (KQL) 扩展，请按照以下步骤操作。

1. 在 Azure Data Studio 中打开扩展管理器。 可以选择扩展图标，也可以在“视图”菜单中选择“扩展”。

2. 在搜索栏中，键入“Kusto”。

3. 选择“Kusto (KQL)”扩展并查看其详细信息。

4. 选择“安装”  。

:::image type="content" source="media/kusto-extension/kusto-extension-icon.png" alt-text="Kusto 扩展":::

## <a name="how-to-connect-to-an-azure-data-explorer-cluster"></a>如何连接到 Azure 数据资源管理器群集

### <a name="find-your-azure-data-explorer-cluster"></a>查找 Azure 数据资源管理器群集。

在 [Azure 门户](https://ms.portal.azure.com/#home)中找到 Azure 数据资源管理器群集，然后找到该群集的 URI。

:::image type="content" source="media/kusto-extension/kusto-extension-adx-cluster-uri.png" alt-text="URI":::

不过，你可以立即开始使用 help.kusto.windows.net 群集。

在本文中，我们将使用 help.kusto.windows.net 群集中的数据作为示例。

### <a name="connection-details"></a>连接详细信息

若要设置要连接到的 Azure Data Explorer 群集，请按照以下步骤操作：

1. 从“连接”窗格选择“新建连接” 。

2. 填写“连接详细信息”信息。
    1. 对于“连接类型”，请选择“Kusto”。
    2. 对于“群集”，请输入 Azure 数据资源管理器群集。

        > [!Note]
        > 输入群集名称时，请勿包含 `https://` 前缀或尾随 `/`。

    3. 对于“身份验证类型”，请使用默认值“具有 MFA 支持的通用 Azure Active Directory”。
    4. 对于“帐户”，请使用你的帐户信息。
    5. 对于“数据集”，请使用“默认”。
    6. 对于“服务器组”，请使用“默认”。
        1. 可以使用此字段来组织特定组中的服务器。
    7. 对于“名称(可选)”，请留空。
        1. 可以使用此字段为服务器指定别名。

    :::image type="content" source="media/kusto-extension/kusto-extension-connection-details.png" alt-text="连接详细信息":::

## <a name="how-to-query-an-azure-data-explorer-database-in-azure-data-studio"></a>如何在 Azure Data Studio 中查询 Azure 数据资源管理器数据库

设置与 Azure 数据资源管理器群集的连接后，接下来可以使用 Kusto (KQL) 查询数据库。

若要创建新查询选项卡，可以选择“文件”>“新建查询”（使用 Ctrl + N），也可以双击该数据库，再选择“新建查询”。

打开新的查询选项卡之后，输入 Kusto 查询。

下面是 KQL 查询的一些示例：

```kusto
StormEvents
| limit 1000
```

```kusto
StormEvents
| where EventType == "Waterspout"
```

有关编写 KQL 查询的详细信息，请访问[编写 Azure 数据资源管理器查询](/azure/data-explorer/write-queries#overview-of-the-query-language)

## <a name="view-extension-settings"></a>查看扩展设置

若要更改 Kusto 扩展的设置，请按照以下步骤操作。

1. 在 Azure Data Studio 中打开扩展管理器。 可以选择扩展图标，也可以在“视图”菜单中选择“扩展”。

2. 找到 Kusto (KQL) 扩展。

3. 选择“管理”图标。

4. 选择“扩展设置”图标。

扩展设置如下所示：

:::image type="content" source="media/kusto-extension/kusto-extension-settings.png" alt-text="Kusto (KQL) 扩展设置":::

## <a name="sanddance-visualization"></a>SandDance 可视化效果

[SandDance 扩展](sanddance-extension.md)和 Azure Data Studio 中的 Kusto (KQL) 扩展结合使用时会带来丰富的交互式可视化效果。 从 KQL 查询结果集中，选择“可视化工具”按钮，启动 [SandDance](https://sanddance.js.org/)。

:::image type="content" source="media/kusto-extension/kusto-extension-sanddance-demo.gif" alt-text="SandDance 可视化效果":::

## <a name="known-issues"></a>已知问题

| 详细信息 | 解决方法 |
|---------|------------|
| [在 Kusto 笔记本中，当代码单元执行中出现错误后，在保存的别名连接中更改数据库连接的操作会停滞](https://github.com/microsoft/azuredatastudio/issues/12384) | 请关闭再重新打开笔记本，然后使用该数据库连接到适当的群集 |
| [在 Kusto 笔记本中，无法在未保存的别名连接上更改数据库连接](https://github.com/microsoft/azuredatastudio/issues/12843) |从连接 Viewlet 创建新连接，并使用别名保存它。 然后，创建一个新的笔记本并连接到新保存的连接 | 
| [在 Kusto 笔记本中，创建新的 ADX 连接时不会填充数据库下拉列表](https://github.com/microsoft/azuredatastudio/issues/12666) | 从连接 Viewlet 创建新连接，并使用别名保存它。 然后，创建一个新的笔记本并连接到新保存的连接 |

你可以提交[功能请求](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=feature_request.md&title=)，向产品团队提供反馈。  
你可以提交 [bug](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=bug_report.md&title=)，向产品团队提供反馈。

## <a name="next-steps"></a>后续步骤

- [创建并运行 Kusto 笔记本](../notebooks/notebooks-kusto-kernel.md)
- [Azure Data Studio 中的 Kqlmagic 笔记本](../notebooks/notebooks-kqlmagic.md)
- [SQL 到 Kusto 备份单](/azure/data-explorer/kusto/query/sqlcheatsheet)
- [什么是 Azure 数据资源管理器？](/azure/data-explorer/data-explorer-overview)
- [使用 SandDance 可视化效果](https://sanddance.js.org/)
