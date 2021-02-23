---
title: 在 Azure Data Studio 中对笔记本进行参数化
description: 本教程介绍如何在 ADS 中创建参数化笔记本。
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: vasubhog
ms.author: vasubhog
ms.reviewer: mikeray, alayu, maghan
ms.custom: ''
ms.date: 01/25/2021
ms.openlocfilehash: dc0558d660143de35340010735c74e9abc0cdd56
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343414"
---
# <a name="create-a-parameterized-notebook"></a>创建参数化笔记本

参数化是指用不同的参数执行同一笔记本的功能。

本文介绍如何使用 Python 内核在 Azure Data Studio 中创建和运行参数化笔记本。

## <a name="prerequisites"></a>先决条件

- [Azure Data Studio](../download-azure-data-studio.md)
- [Python](https://www.python.org/downloads/)

## <a name="install-and-set-up-papermill-in-azure-data-studio"></a>在 Azure Data Studio 中安装并设置 Papermill

此部分中的步骤全都在 Azure Data Studio 笔记本中运行。

1. 创建新笔记本并将“内核”更改为“Python 3”。

   ![新建笔记本](media/notebooks-kqlmagic/install-new-notebook.png)

2. 当包需要更新时，系统可能会提示你升级 Python 包。

   ![是](media/notebooks-kqlmagic/install-python-yes.png)

3. 安装 Papermill：

   ```python
   import sys
   !{sys.executable} -m pip install papermill --no-cache-dir --upgrade
   ```

   验证它是否已安装：

   ```python
   import sys
   !{sys.executable} -m pip list
   ```

   :::image type="content" source="media/notebooks-parameterization/install-list-papermill.png" alt-text="列表":::

5. 可以通过检查 Papermill 的版本来测试是否已正确加载 Papermill。

   ```python
   import papermill
   papermill
   ```

   :::image type="content" source="media/notebooks-parameterization/install-validation-papermill.png" alt-text="验证":::

## <a name="set-up-a-parameterized-notebook"></a>设置参数化笔记本

1. 验证“内核”设置为“Python3”。

   ![内核更改](media/notebooks-kqlmagic/change-kernel.png)

2. 创建新的代码单元和标记，将其用作参数单元。

   ```python
   x = 2.0
   y = 5.0
   ```

   :::image type="content" source="media/notebooks-parameterization/make-parameter-cell.png" alt-text="参数单元笔记本":::

3. 添加其他单元以测试不同的参数。

   ```python
   addition = x + y
   multiply = x * y
   ```

   ```python
   print("Addition: " + str(addition))
   print("Multiplication: " + str(multiply))
   ```

   示例输入笔记本中的单元：:::image type="content" source="media/notebooks-parameterization/test-cells.png" alt-text="其他输入笔记本单元":::

4. 将笔记本保存为 Input.ipynb。
   :::image type="content" source="media/notebooks-parameterization/save-notebook.png" alt-text="保存笔记本":::

## <a name="how-to-execute-papermill-notebook"></a>如何执行 Papermill 笔记本

可以通过两种方式执行 Papermill：

- 命令行接口 (CLI)
- Python API

### <a name="parameterized-cli-execution"></a>参数化 CLI 执行

要使用 CLI 执行笔记本，请在带有输入笔记本、输出笔记本位置和选项的终端输入 Papermill 命令。

> [!Note]
   > 可在[此处](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-cli)找到 Papermill 命令行界面文档。

1. 使用新参数执行输入笔记本。

   ```shell
   papermill Input.ipynb Output.ipynb -p x 10 -p y 20
   ```

   此操作将使用参数 x 和 y 的新值执行输入笔记本 。

2. 执行后，查看新的输出参数化笔记本。
   注意有一个标有“# 注入参数”（包含通过 CLI 传入的新参数值）的新单元。

   :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="输出笔记本":::

### <a name="parameterized-python-api-execution"></a>参数化 Python API 执行

> [!Note]
   > 可在[此处](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-the-python-api)找到 Papermill Python API 文档。

1. 创建新笔记本并将“内核”更改为“Python 3”。
   ![新建笔记本](media/notebooks-kqlmagic/install-new-notebook.png)

2. 添加新的代码单元，并使用 Papermill 来使用 execute 方法。

   ```python
   import papermill as pm

   pm.execute_notebook(
   '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Input.ipynb',
   '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Output.ipynb',
   parameters = dict(x = 10, y = 20)
   )
   ```

   ![Papermill Python API 执行](media/notebooks-parameterization/python-api-execute.png)

3. 执行后，查看新的输出参数化笔记本。

   注意有一个标有“# 注入参数”（包含通过 CLI 传入的新参数值）的新单元。

   :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="输出笔记本":::

## <a name="next-steps"></a>后续步骤

了解有关笔记本和参数化的详细信息：

- [如何使用 Azure Data Studio 中的笔记本](./notebooks-guidance.md)
- [Papermill 参数化文档](https://papermill.readthedocs.io/en/latest/index.html)