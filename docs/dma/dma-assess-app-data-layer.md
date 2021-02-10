---
title: 使用数据迁移助手评估应用程序的数据访问层
description: 了解如何使用数据迁移助手来评估应用程序的数据访问层。
ms.date: 01/23/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: f56976c9140cb4ac18c048d7d57d6949fc4ae74d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065156"
---
# <a name="assess-an-apps-data-access-layer-with-data-migration-assistant"></a>使用数据迁移助手评估应用的数据访问层

通常，应用程序将数据连接并保存到数据库。 应用程序的数据访问层可以简化对此数据的访问。 数据迁移助手 (DMA) 使用户能够评估其数据库和相关对象。 最新版本的 DMA (v 5.0) 在应用程序代码中引入了对分析数据库连接和嵌入式 SQL 查询的支持。

请考虑以下 c # 代码段：

![示例 c # 代码段](../dma/media/dma-assess-app-data-layer/dma-sample-c-sharp-code-segment.png)

在这种情况下，可以看到该应用程序正在使用 SQL 查询来获取雇员的姓名。

![示例 c # 代码段详细信息](../dma/media/dma-assess-app-data-layer/dma-sample-c-sharp-code-detail.png)

作为应用程序所有者，我需要能够识别应用程序可以连接到的各种数据库和应用程序的数据访问层中嵌入的查询。 此外，我还需要确定将应用程序现代化到 Azure 数据服务所需的任何更改。

## <a name="assess-an-app-with-data-access-migration-toolkit"></a>使用数据访问迁移工具包评估应用

为实现此评估，我们最近引入了数据访问迁移工具包 (DAMT) ，Visual Studio Code 扩展。 此扩展的最新版本 (v 0.2) 增加了对 .Net 应用程序和 T-sql 方言的支持。

1. 从 [此处](https://code.visualstudio.com/download)下载并安装 VS Code。
2. 从 Extensions Marketplace 启用 [数据访问迁移工具包扩展](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) 。

   ![数据访问迁移工具包扩展页](../dma/media/dma-assess-app-data-layer/dma-damt-extension-page.png)

3. 在 Visual Studio Code 中打开应用程序项目。

   ![Visual Studio Code 中的应用程序项目](../dma/media/dma-assess-app-data-layer/dma-app-project-in-vscode.png)

4. 启动扩展控制台 (Shft-P) ，然后运行 **数据访问：分析工作区** 命令。

   ![Visual Studio Code 中的扩展控制台](../dma/media/dma-assess-app-data-layer/dma-vscode-extension-console.png)

5. 选择 SQL Server 方言。

   ![SQL Server 方言选择](../dma/media/dma-assess-app-data-layer/dma-sql-server-dialect.png)

   在分析结束时，该命令将生成 SQL 连接命令和查询的报告。

   ![数据访问报表](../dma/media/dma-assess-app-data-layer/dma-data-access-report.png)

6. 查看报表中的数据连接组件和嵌入在应用程序代码中的 SQL 查询（突出显示）。

   ![应用程序代码中的 SQL 查询](../dma/media/dma-assess-app-data-layer/dma-sql-queries-in-app-code.png)

   可以通过 DMA 分析这些查询，以了解基于目标 SQL 平台的兼容性和功能奇偶校验问题。

7. 若要评估应用程序的数据层，请将报表导出为 json 文件。

   ![json 文件导出](../dma/media/dma-assess-app-data-layer/dma-json-file-export.png)

   在这种情况下，生成的文件为：

   ![查看 json 文件的内容](../dma/media/dma-assess-app-data-layer/dma-json-file-contents.png)

   数据迁移助手可以在将数据库现代化到 Azure 数据平台的上下文中对应用程序中标识的查询进行评估。

8. 开始数据迁移助手，然后创建评估项目。

   ![数据迁移助手新的评估项目](../dma/media/dma-assess-app-data-layer/dma-new-assessment-project.png)

9. 选择 "源 SQL Server 实例"。

   ![数据迁移助手选择 SQL Server 源](../dma/media/dma-assess-app-data-layer/dma-select-sql-source.png)

10. 选择应用程序要连接到的数据库。

    ![数据访问迁移选择应用程序数据库](../dma/media/dma-assess-app-data-layer/dma-select-app-database.png)

    为了便于进行数据访问评估，DMA 引入了包含 json 文件和应用程序查询的功能。 接下来，我们将包括我们在应用程序查询之前编写的 json 文件。

11. 选择数据库并浏览到从数据访问迁移工具包导出的 json 文件，以包含应用程序中用于评估的查询。

    ![数据迁移助手打开 DMAT json 文件](../dma/media/dma-assess-app-data-layer/dma-open-damt-json-file.png)

12. 开始评估。

    ![数据迁移助手开始评估](../dma/media/dma-assess-app-data-layer/dma-start-assessment.png)

13. 查看评估报告。 生成的报告将包含在应用程序查询中检测到的任何兼容性或功能奇偶校验问题，如下所示。

    ![数据迁移助手评估报表](../dma/media/dma-assess-app-data-layer/dma-assessment-report.png)

现在，除了数据库的迁移角度外，用户还可以从应用程序角度获得一个视图。

## <a name="see-also"></a>另请参阅

* [数据迁移助手概述](../dma/dma-overview.md)
* [数据迁移助手：配置设置](../dma/dma-configurationsettings.md)
* [数据迁移助手：最佳实践](../dma/dma-bestpractices.md)
* [数据访问迁移工具包](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)