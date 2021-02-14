---
title: 使用机器学习扩展管理包
description: 了解如何使用 Azure Data Studio 的机器学习扩展来管理数据库中的 Python 或 R 包。
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 05/19/2020
ms.openlocfilehash: c874317acf39e954a0e640c1954e3a0dac6e9f43
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051958"
---
# <a name="manage-packages-in-database-with-machine-learning-extension-for-azure-data-studio-preview"></a>使用 Azure Data Studio 的机器学习扩展（预览版）来管理数据库中的包

了解如何使用 [Azure Data Studio 的机器学习扩展](machine-learning-extension.md)来管理数据库中的 Python 或 R 包。

> [!IMPORTANT]
> 使用机器学习扩展来管理数据库中的包当前仅支持 SQL Server 2019 上的[机器学习服务](../../machine-learning/sql-server-machine-learning-services.md)。

## <a name="prerequisites"></a>先决条件

- 安装和配置 [Azure Data Studio 的机器学习扩展](machine-learning-extension.md)。 需要指定[扩展设置中的 Python 或 R 安装路径](machine-learning-extension.md#settings)，包管理才能正常工作。

- sqlmlutils 包。 如果尚未安装此包，则机器学习扩展将提示你进行安装。

- 对于 Linux 上的 SQL Server，不支持 Windows 身份验证。 因此，需要使用 SQL 身份验证连接到 Linux 上的 SQL Server。

## <a name="manage-python-packages"></a>管理 Python 包

可以使用机器学习扩展安装和卸载 Python 包。

### <a name="install-new-python-package"></a>安装新 Python 包

按照以下步骤在数据库中安装 Python 包。

1. 选择“管理数据库中的包”。

1. 如果要求安装 sqlmlutils，请选择“是” 。

1. 在“已安装”选项卡下，选择“包类型”下的“Python”，然后在“位置”下选择数据库   。

1. 选择“新增”选项卡。

1. 在“搜索 Python 包”中输入要安装的 Python 包，然后选择“搜索” 。

1. 确认包已在“包名称”下列出，并且所需版本已在“包版本”下列出 。

1. 选择“安装”  。

1. 选择“关闭”，然后在“任务”下验证包是否已成功安装 。

### <a name="uninstall-a-python-package"></a>卸载 Python 包

按照以下步骤在数据库中卸载 Python 包。

> [!NOTE]
> 只能卸载已安装在数据库中的包。 不能卸载已预安装有 SQL Server 机器学习服务的包。

1. 选择“管理数据库中的包”。

1. 如果要求安装 sqlmlutils，请选择“是” 。

1. 在“已安装”选项卡下，选择“包类型”下的“Python”，然后在“位置”下选择数据库   。

1. 选择要卸载的包。

1. 点击“卸载选定的包”。

1. 选择“关闭”，然后在“任务”下验证包是否已成功安装 。

## <a name="manage-r-packages"></a>管理 R 包

可以使用机器学习扩展安装和卸载 R 包。

### <a name="install-new-r-package"></a>安装新 R 包

按照以下步骤在数据库中安装 Python 包。

1. 选择“管理数据库中的包”。

1. 如果要求安装 sqlmlutils，请选择“是” 。

1. 在“已安装”选项卡下，选择“包类型”下的“R”，然后在“位置”下选择数据库   。

1. 选择“新增”选项卡。

1. 在“搜索 R 包”中输入要安装的 R 包，然后选择“搜索” 。

1. 确认包已在“包名称”下列出，并且所需版本已在“包版本”下列出 。

1. 选择“安装”  。

1. 选择“关闭”，然后在“任务”下验证包是否已成功安装 。

### <a name="uninstall-an-r-package"></a>卸载 R 包

按照以下步骤在数据库中卸载 R 包。

> [!NOTE]
> 只能卸载已安装在数据库中的包。 不能卸载已预安装有 SQL Server 机器学习服务的包。

1. 选择“管理数据库中的包”。

1. 如果要求安装 sqlmlutils，请选择“是” 。

1. 在“已安装”选项卡下，选择“包类型”下的“R”，然后在“位置”下选择数据库   。

1. 选择要卸载的包。

1. 点击“卸载选定的包”。

1. 选择“关闭”，然后在“任务”下验证包是否已成功安装 。

## <a name="next-steps"></a>后续步骤

- [Azure Data Studio 中的机器学习扩展](machine-learning-extension.md)
- [作出预测](machine-learning-extension-predictions.md)
- [导入或查看模型](machine-learning-extension-import-view-models.md)
- [Azure Data Studio 中的笔记本](../notebooks/notebooks-guidance.md)
- [SQL 机器学习文档](../../machine-learning/index.yml)