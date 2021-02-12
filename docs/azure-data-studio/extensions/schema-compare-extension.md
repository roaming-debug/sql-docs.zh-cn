---
title: 架构比较扩展
description: 了解如何安装和使用 Azure Data Studio 架构比较扩展，以便轻松比较两个数据库，并选择性地更改其中一个数据库，使其与另一个数据库匹配。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 11/04/2019
ms.openlocfilehash: 88f96d75150fc1b3e69d9cf158a50e02110dbae0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048474"
---
# <a name="schema-compare-extension"></a>架构比较扩展

架构比较扩展提供了一种易于使用的体验，以比较两个数据库定义，并将源的区别应用于目标。

## <a name="features"></a>功能

* 比较两个 dacpac 文件或数据库的架构
* 将结果视为一组要匹配源则必须针对目标执行的操作
* 选择性地排除结果中列出的操作
* 设置控制比较范围的选项
* 将更改应用到目标，或生成具有相同效果的脚本
* 保存比较

![架构比较：示例比较](media/schema-compare-extension/schema-compare.png)

## <a name="why-would-i-use-the-schema-compare-extension"></a>为什么要使用架构比较扩展？

手动管理和同步不同的数据库版本可能比较繁琐。 架构比较扩展可以简化数据库比较过程，并在同步数据库时提供完全控制 &mdash; 可以在应用更改之前有选择地筛选特定差异和类别。 架构比较扩展是一个可靠工具，可为你节省时间，让你少写一些代码。

![架构比较：“选项”对话框](media/schema-compare-extension/schema-compare-options.png)

## <a name="install-the-extension"></a>安装扩展

1. 选择“扩展”图标以查看可用扩展。

    ![扩展管理器图标](media/add-extensions/extension-manager-icon.png)

2. 搜索“架构比较”扩展并选择它以查看其详细信息  。 选择“安装”以添加扩展。

3. 安装后，重载以启用 Azure Data Studio 中的扩展（仅在第一次安装扩展时需要进行此操作）  。

## <a name="launch-a-schema-compare"></a>启动“架构比较”

1. 要打开“架构比较”对话框，右键单击对象资源管理器中的数据库，然后选择“架构比较” 。 你选择的数据库将设置为要比较的源数据库。

    ![“架构比较”启动菜单](media/schema-compare-extension/schema-compare-launch.png)

2. 选择其中一个省略号 (...)，更改“架构比较”的源数据库和目标数据库，然后选择“确定”。

    ![schema compare select source/target](media/schema-compare-extension/schema-compare-select-source-target.png)

3. 要自定义比较，请在工具栏中选择“选项”按钮。

4. 选择“比较”以查看比较结果。

## <a name="next-steps"></a>后续步骤

若要详细了解架构比较，请访问[使用架构比较来比较不同数据库定义](../../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)。
在[此处](https://github.com/microsoft/azuredatastudio/issues)报告问题和提出功能请求。