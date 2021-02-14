---
title: Azure Data Studio 中使用 Python 内核的笔记本
description: 本教程介绍如何创建和运行 Python 笔记本。
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: garyericson
ms.author: garye
ms.reviewer: mikeray, alayu, maghan
ms.custom: ''
ms.date: 07/01/2020
ms.openlocfilehash: e1859c5e1af823e067210c5b69247702e8c78f89
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040294"
---
# <a name="create-and-run-a-python-notebook"></a>创建和运行 Python 笔记本

[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

本教程演示如何使用 Python 内核在 Azure Data Studio 中创建和运行笔记本。

## <a name="prerequisites"></a>先决条件

- [已安装 Azure Data Studio](../download-azure-data-studio.md)

## <a name="create-a-notebook"></a>创建笔记本

以下步骤演示如何在 Azure Data Studio 中创建笔记本文件：

1. 打开 Azure Data Studio。 如果系统提示连接到 SQL Server，则可以进行连接或单击“取消”  。

1. 在“文件”菜单中，选择“新建笔记本”。

1. 对“内核”  选择“Python 3”  。 “附加到”设置为“localhost”。

   :::image type="content" source="media/notebooks-python-kernel/set-kernel-and-attach-to-python.png" alt-text="设置内核":::

可使用“文件”菜单中的“保存”或“另存为…”命令保存笔记本  。

若要打开笔记本，可以使用“文件”菜单中的“打开文件…”命令，选择“欢迎”页上的“打开文件”，或使用命令面板中的“文件：    打开”命令。

## <a name="change-the-python-kernel"></a>更改 Python 内核

第一次连接到笔记本中的 Python 内核时，将显示“为笔记本配置 Python”页面。 可以选择以下任一项：

- “新 Python 安装”  ，用于为 Azure Data Studio 安装 Python 的新副本，或是
- “使用现有 Python 安装”  ，用于指定供 Azure Data Studio 使用的现有 Python 安装的路径

若要查看活动 Python 内核的位置和版本，请创建一个代码单元格，并运行以下 Python 命令：

```python
import os
import sys
print(sys.version_info)
print(os.path.dirname(sys.executable))
```

连接到不同的 Python 安装：

1. 在“文件”菜单中，选择“首选项”，然后选择“设置”    。
1. 在“扩展”下滚动到“笔记本配置”   。
1. 在“使用现有 Python”下，取消选中“笔记本使用的预先存在的 python 安装的本地路径”选项  。
1. 重启 Azure Data Studio。

当 Azure Data Studio 启动并且你连接到 Python 内核时，将显示“为笔记本配置 Python”页面，你可以选择创建新 Python 安装或指定现有安装的路径。

## <a name="run-a-code-cell"></a>运行代码单元格

可单击单元格左侧的“运行单元格”按钮（圆形黑色箭头），创建包含可就地运行的 SQL 代码的单元格。 单元格完成运行后，结果会显示在笔记本中。

例如：

1. 选择工具栏中的“+代码”命令，添加一个新的 Python 代码单元格  。

   :::image type="content" source="media/notebooks-python-kernel/notebook-toolbar-python.png" alt-text="笔记本工具栏":::

1. 将以下示例复制并粘贴到单元格，然后单击“运行单元格”。 此示例执行简单的数学运算，结果如下所示。

   ```python
   a = 1
   b = 2
   c = a/b
   print(c)
   ```

   :::image type="content" source="media/notebooks-python-kernel/run-notebook-cell-python.png" alt-text="运行笔记本单元格":::

## <a name="next-steps"></a>后续步骤

了解有关笔记本的详细信息：

- [通过 Kqlmagic 扩展 Python](./notebooks-kqlmagic.md)
- [如何使用 Azure Data Studio 中的笔记本](./notebooks-guidance.md)
- [创建和运行 SQL Server 笔记本](./notebooks-sql-kernel.md)
