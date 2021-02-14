---
title: Azure Arc 扩展（预览版）
description: 了解如何安装和使用 Azure Arc 扩展来试用 Azure Arc 数据服务。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: 195404df404c6fec7ef98f41a9921d937c16eab7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048587"
---
# <a name="azure-arc-extension-for-azure-data-studio-preview"></a>适用于 Azure Data Studio 的 Azure Arc 扩展（预览版）

[Azure Arc 扩展（预览版）](/azure/azure-arc/data/)是用于创建和管理 Azure Arc 数据服务资源的扩展。

**关键操作包括：**
- 创建资源
    - 数据控制器
    - 适用于 Azure Arc 的 SQL 托管实例
    - 适用于 Azure Arc 的 PostgreSQL
- 管理资源
    - 查看“数据控制器”仪表板
    - 查看“适用于 Azure Arc 的 SQL 托管实例”仪表板
    - 查看“适用于 Azure Arc 的 PostgreSQL”仪表板
- 运行 Azure Arc Jupyter Book

## <a name="install-the-extension"></a>安装扩展
- 从库中安装 Azure 数据 CLI 扩展。
- 从库中安装 Azure Arc 扩展。
- 重启 Azure Data Studio

## <a name="sign-in-with-azure-account"></a>使用 Azure 帐户登录
1. 在左下方选择帐户
1. 选择“添加帐户”
1. 此操作将启动浏览器。 登录到你的 Azure 帐户。

## <a name="create-a-resource"></a>创建资源
此扩展支持部署 Azure Arc 数据控制器、适用于 Azure Arc 的 Postgres 和适用于 Azure Arc 的 SQL 托管实例。可以通过内置部署向导完成部署。

1. 在左侧活动栏上选择“连接”viewlet
1. 选择三个点，然后选择“新建部署”
1. 按照提示创建新的 Azure Arc 资源。

## <a name="manage-a-resource"></a>管理资源
从 azdata、Azure 门户或 Azure Data Studio 部署 Azure Arc 数据控制器后，可以从 Azure Data Studio 连接到该控制器。

1. 在左侧活动栏上打开“连接”viewlet。
1. 展开“Azure Arc 控制器”
1. 选择“连接控制器”
1. 填写参数并进行连接。

连接后，可以查看部署到数据控制器上的资源。 然后，可以右键单击并选择“管理”以访问资源仪表板。  

这些仪表板将提供有关资源的额外信息，包括在 Azure 门户中打开的选项。

## <a name="next-steps"></a>后续步骤
若要详细了解 Azure Arc 数据服务，请[查看文档](/azure/azure-arc/data/)。