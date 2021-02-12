---
title: 使用机器学习扩展导入或查看模型
description: 了解如何使用 Azure Data Studio 的机器学习扩展来导入 ONNX 模型或查看数据库中已导入的模型。
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 06/09/2020
ms.openlocfilehash: 3f55f793391e408d96b8c6d07a31c7e790d54853
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048527"
---
# <a name="import-or-view-models-with-machine-learning-extension-for-azure-data-studio-preview"></a>使用 Azure Data Studio 的机器学习扩展（预览版）导入或查看模型

了解如何使用 [Azure Data Studio 的机器学习扩展](machine-learning-extension.md)来导入 ONNX 模型或查看数据库中已导入的模型。

> [!IMPORTANT]
> 目前，在数据库中使用机器学习扩展进行导入和查看时，仅支持 [Azure SQL 托管实例中的机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)和[具有 ONNX 的 Azure SQL Edge](/azure/azure-sql-edge/onnx-overview)。

## <a name="prerequisites"></a>先决条件

- 安装和配置 [Azure Data Studio 的机器学习扩展](machine-learning-extension.md)。 需要[在扩展设置中指定 Python 安装路径](machine-learning-extension.md#settings)。

- onnxruntime、mlflow 和 mlflow-dbstore Python 包。 如果尚未安装这些包，则机器学习扩展将提示你进行安装。

## <a name="view-models"></a>查看模型

请执行以下步骤查看数据库中存储的 ONNX 模型。

1. 选择“导入或查看模型”。

1. 如果要求安装 onnxruntime、mlflow 和 mlflow-dbstore，请选择“是”。

1. 选择其中存储了模型的“模型数据库”和“模型表”。

此时将显示模型列表。 可以编辑模型名称和描述，或从列表中删除模型。

## <a name="import-a-new-model"></a>导入新模型

请执行以下步骤在数据库中导入 ONNX 模型。

1. 选择“导入或查看模型”。

1. 如果要求安装 onnxruntime、mlflow 和 mlflow-dbstore，请选择“是”。

1. 选择“导入模型”。

1. 选择要用于存储导入模型的“模型数据库”。

1. 选择要用于存储导入模型的“模型表”。 可以选择“现有表”，也可以创建“新表”。 选择“**下一页**”。

1. 选择模型的位置，然后选择“下一步”。 可用工具如下：
    - “文件上传”。 选择此选项可使用文件中的模型。 在“源文件”下选择模型文件，然后选择“下一步”。
    - “Azure 机器学习”。 选择此选项可使用 Azure 机器学习中的模型。 首先，登录到 Azure。 然后选择“Azure 帐户”、“Azure 订阅”、“Azure 资源组”和“Azure ML 工作区”。 选择要使用的模型，然后选择“下一步”。

1. 输入模型“名称”和“描述‘，然后选择”部署“，以将模型存储在数据库中。

> [!NOTE]
> 机器学习扩展目前为预览版。 因此，存储模型的表架构在将来可能会更改。

## <a name="next-steps"></a>后续步骤

- [Azure Data Studio 中的机器学习扩展](machine-learning-extension.md)
- [管理数据库中的包](machine-learning-extension-manage-packages.md)
- [作出预测](machine-learning-extension-predictions.md)
- [SQL 机器学习文档](../../machine-learning/index.yml)
- [Azure SQL 托管实例中的机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)
- [在 SQL Edge（预览版）中将机器学习和 AI 与 ONNX 结合使用](/azure/azure-sql-edge/onnx-overview)
