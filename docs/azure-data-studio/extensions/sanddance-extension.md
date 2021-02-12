---
title: 适用于 Azure Data Studio 的 SandDance
description: 了解如何使用 Azure Data Studio 扩展快速创建数据的可视化效果（提供见解的可视化效果）。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: seodec18
ms.date: 07/03/2019
ms.openlocfilehash: d031ddea065f035bf4d1f4ffc70bd65d90f1bd0f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040332"
---
# <a name="sanddance-for-azure-data-studio-preview"></a>适用于 Azure Data Studio 的 SandDance（预览版）

Azure Data Studio 现在可为数据创建快速可视化效果。 当你尝试查看数据并了解发生的情况时，此扩展会非常有用。 我们使用 Microsoft Research 的 SandDance 技术生成现成可用的数据可视化效果。

![sanddance-animation](https://user-images.githubusercontent.com/11507384/54236654-52d42800-44d1-11e9-859e-6c5d297a46d2.gif)

通过使用易于理解的视图，SandDance 可帮助你获取关于数据的见解，进而帮助你根据数据制定决策、基于论证构建方案、测试假设、深入探索表面论证背后的依据、为购买决策提供支持或将数据与更广泛、真实的背景进行关联。

SandDance 使用单位可视化效果，在数据库中的行与屏幕上的标记之间应用一对一映射。
视图间的平滑动画转换有助于在与数据进行交互时维持上下文。

## <a name="usage"></a>使用情况

### <a name="view-csv-or-tsv-files"></a>查看 .csv 或 .tsv 文件
这包括 SQL Server 2019 大数据群集中的本地文件或 HDFS 上的文件。
 
从“文件”菜单开始，使用“打开文件夹”或 [Ctrl + K Ctrl + O] 打开包含 .CSV 文件的目录。  接下来，在“资源管理器”面板中，右键单击 .csv 或 tsv 文件，然后选择“在 SandDance 中查看”  。

如果已连接到 SQL Server 2019 大数据群集，请在 HDFS 中右键单击 .csv 或 tsv 文件，并选择“在 SandDance 中查看”  。

### <a name="view-sql-query-results"></a>查看 SQL 查询结果

从 SQL 查询窗口开始，执行查询以获取结果网格。 单击“结果”窗格右侧的“可视化工具”图标。

## <a name="known-issues"></a>已知问题

当前，我们不限制可视化的行计数。 但是，内存消耗与行数成比例，因此建议将数据集或视图限制为 10 万行左右。

请参阅[已知问题](https://microsoft.github.io/SandDance/#known-issues)

## <a name="release-notes"></a>发行说明

### <a name="100"></a>1.0.0

azdata-sanddance 的初始版本

## <a name="next-steps"></a>后续步骤
若要了解详细信息，请[访问 GitHub 存储库。](https://github.com/Microsoft/SandDance)
