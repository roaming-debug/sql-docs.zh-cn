---
title: Azure Data Studio 中使用 Kusto 内核的笔记本
description: 本教程介绍如何创建并运行 Kusto 笔记本。
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: markingmyname
ms.author: maghan
ms.reviewer: jukoesma
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: 1362e6a5ccd6152a1cb2597076d25c2da4149fd0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048227"
---
# <a name="create-and-run-a-kusto-kql-notebook-preview"></a>创建并运行 Kusto (KQL) 笔记本（预览）

本文介绍如何使用连接到 Azure 数据资源管理器群集的 [Kusto (KQL) 扩展](../extensions/kusto-extension.md)创建并运行 [Azure Data Studio 笔记本](./notebooks-guidance.md)

使用 Kusto (KQL) 扩展，你可以更改 Kusto 的内核选项。

此功能目前处于预览状态。

## <a name="prerequisites"></a>先决条件

如果还没有 Azure 订阅，可以在开始前创建一个[免费 Azure 帐户](https://azure.microsoft.com/free/)。

- [你可连接到的带数据库的 Azure 数据资源管理器群集](/azure/data-explorer/create-cluster-database-portal).
- [Azure Data Studio](../download-azure-data-studio.md)。
- [Azure Data Studio 的 Kusto (KQL) 扩展](../extensions/kusto-extension.md)。

## <a name="create-a-kusto-kql-notebook"></a>创建 Kusto (KQL) 笔记本

以下步骤演示如何在 Azure Data Studio 中创建笔记本文件：

1. 在 Azure Data Studio 中，连接到 Azure 数据资源管理器群集。

2. 导航到“连接”窗格，在“服务器”窗口中右键单击 Kusto 数据库，然后选择“新建笔记本” 。 还可以转到“文件” > “新建笔记本” 。

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-new-notebook.png" alt-text="打开笔记本":::

3. 对于“内核”，选择 Kusto。 确认“附加到”菜单中设置了群集名称和数据库。 本文使用 help.kusto.windows.net 群集和示例数据库数据。

   :::image type="content" source="media/notebooks-kusto-kernel/set-kusto-kernel.png" alt-text="设置 Kernel 和“附加到”":::

可使用“文件”菜单中的“保存”或“另存为…”命令保存笔记本  。

若要打开笔记本，可以使用“文件”菜单中的“打开文件…”命令，选择“欢迎”页上的“打开文件”，或使用命令面板中的“文件：    打开”命令。

## <a name="change-the-connection"></a>更改连接

更改笔记本的 Kusto 连接：

1. 选择笔记本工具栏中的“附加到”菜单，然后选择“更改连接”   。

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-select-attach-to-change-connections.png" alt-text="更改连接":::

   > [!Note]
   > 确保填充数据库值。 Kusto 笔记本需要指定数据库。

2. 现在你可以选择一个最近的连接服务器，也可以输入新的连接详细信息进行连接。

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-change-connection-cluster.png" alt-text="选择其他群集":::

   > [!Note]
   > 指定不带 `https://` 的群集名称。

## <a name="run-a-code-cell"></a>运行代码单元格

可通过选择单元格左侧的“运行单元格”按钮，创建包含可就地运行的 KQL 查询的单元格。 单元格完成运行后，结果会显示在笔记本中。

例如：

1. 选择工具栏中的“+代码”命令，添加一个新的代码单元格  。

   :::image type="content" source="media/notebooks-kusto-kernel/kusto-kernel-code.png" alt-text="Kusto 内核代码块":::

2. 将以下示例复制并粘贴到单元格，然后选择“运行单元格”。 此示例查询 StormEvents 数据以获取特定的事件类型。

   ```kusto
    StormEvents
    | where EventType == "Waterspout"
   ```

   :::image type="content" source="media/notebooks-kusto-kernel/run-kusto-notebook-cell.png" alt-text="运行单元格":::

## <a name="save-the-result-or-show-chart"></a>保存结果或显示图表

如果运行返回结果的脚本，则可以使用结果上方显示的工具栏将结果保存为不同的格式。

- 另存为 CSV
- 另存为 Excel
- 另存为 JSON
- 另存为 XML
- 显示图表

```kusto
    StormEvents
    | limit 10
```

:::image type="content" source="media/notebooks-kusto-kernel/run-notebook-save-results.png" alt-text="保存结果":::

## <a name="known-issues"></a>已知问题

| 详细信息 | 解决方法 |
|---------|------------|
| [查询结果仅显示列标题](https://github.com/microsoft/azuredatastudio/issues/12565)。 | 空值 |

你可以提交[功能请求](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=feature_request.md&title=)，向产品团队提供反馈。  
你可以提交 [bug](https://github.com/microsoft/azuredatastudio/issues/new?assignees=&labels=&template=bug_report.md&title=)，向产品团队提供反馈。

## <a name="next-steps"></a>后续步骤

了解有关笔记本的详细信息：

- [Azure Data Studio 的 Kusto (KQL) 扩展](../extensions/kusto-extension.md)
- [如何使用 Azure Data Studio 中的笔记本](./notebooks-guidance.md)
- [创建和运行 Python 笔记本](./notebooks-python-kernel.md)
- [创建和运行 SQL Server 笔记本](./notebooks-sql-kernel.md)