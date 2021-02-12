---
title: 使用机器学习扩展进行预测
description: 了解如何使用 Azure Data Studio 的机器学习扩展来通过数据库中的 ONNX 模型进行预测。
ms.prod: azure-data-studio
ms.technology: machine-learning
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: sstein
ms.custom: ''
ms.date: 06/09/2020
ms.openlocfilehash: 6da9918b4ebb3ed571d82d23c123c8e477741c0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048537"
---
# <a name="make-predictions-with-machine-learning-extension-for-azure-data-studio-preview"></a>使用 Azure Data Studio 的机器学习扩展（预览版）进行预测

了解如何使用 [Azure Data Studio 的机器学习扩展](machine-learning-extension.md)来通过数据库中的 ONNX 模型进行预测。 该扩展将使用 [PREDICT](../../t-sql/queries/predict-transact-sql.md) 生成 T-SQL 脚本，以使用先前导入、驻留在本地文件中或来自 Azure 机器学习的模型对存储在表中的数据集进行预测。

> [!IMPORTANT]
> 使用机器学习扩展进行预测当前仅支持 [Azure SQL 托管实例中的机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)和[使用 ONNX 的 Azure SQL Edge](/azure/azure-sql-edge/onnx-overview)。

## <a name="prerequisites"></a>先决条件

- 安装和配置 [Azure Data Studio 的机器学习扩展](machine-learning-extension.md)。 需要[在“扩展设置”中指定 Python 安装路径](machine-learning-extension.md#settings)。

- onnxruntime、mlflow 和 mlflow-dbstore Python 包  。 如果尚未安装这些包，机器学习扩展将提示你进行安装。

## <a name="make-predictions-from-onnx-model"></a>通过 ONNX 模型进行预测

按照以下步骤使用 ONNX 模型进行预测。

1. 选择“进行预测”。

1. 如果要求安装 onnxruntime、mlflow 和 mlflow-dbstore，请选择“是”。

1. 选择模型的位置，然后选择“下一步”。 可用工具如下：
    - **导入的模型**。 选择此项可使用已存储在数据库中的模型。 选择模型所在的“模型数据库”和“模型表”，选择要使用的模型，然后选择“下一步”  。
    - **文件上传**。 选择此项可使用文件中的模型。 在“源文件”下选择模型文件，然后选择“下一步”。
    - **Azure 机器学习**。 选择此项可使用 Azure 机器学习中的模型。 首先，登录到 Azure。 然后选择你的“Azure 帐户”、“Azure 订阅”、“Azure 资源组”和“Azure ML 工作区”   。 选择要使用的模型，然后选择“下一步”。

1. 将源数据映射到模型。
    - 选择包含要对其应用预测的数据集的“源数据库”和“源表” 。
    - 映射“模型输入映射”和“模型输出”下的列 。 该扩展将自动映射具有相同名称和数据类型的列。

1. 选择“预测”。

Azure Data Studio 将使用 [PREDICT](../../t-sql/queries/predict-transact-sql.md) 创建新的 T-SQL 查询，该查询可用于对数据进行预测。

## <a name="next-steps"></a>后续步骤

- [Azure Data Studio 中的机器学习扩展](machine-learning-extension.md)
- [管理数据库中的包](machine-learning-extension-manage-packages.md)
- [导入或查看模型](machine-learning-extension-import-view-models.md)
- [Azure Data Studio 中的笔记本](../notebooks/notebooks-guidance.md)
- [SQL 机器学习文档](../../machine-learning/index.yml)
- [Azure SQL 托管实例中的机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)
- [在 SQL Edge（预览版）中将机器学习和 AI 与 ONNX 结合使用](/azure/azure-sql-edge/onnx-overview)