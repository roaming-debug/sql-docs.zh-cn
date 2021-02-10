---
title: 查看 SQL Server 升级的分析报告
description: 了解如何在数据库实验助手 (DEA) 中查看和了解性能见解的分析报告。
ms.custom: seo-lt-2019
ms.date: 02/04/2020
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: d1666d4ba2279623065dd3c0d8faf5e0653de82e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065942"
---
# <a name="view-analysis-reports-in-database-experimentation-assistant"></a>在数据库实验助手中查看分析报表

使用数据库实验助手 (DEA) [创建分析报表](database-experimentation-assistant-create-report.md)后，可以根据所执行的 A/B 测试来查看性能见解的报表。

## <a name="open-an-existing-analysis-report"></a>打开现有分析报表

1. 在 DEA 中，选择 "列表" 图标，指定服务器名称和身份验证类型，根据具体情况选择或取消选择 " **加密连接** " 和 " **信任服务器证书** " 复选框，然后选择 " **连接**"。

   ![通过报表连接到服务器](./media/database-experimentation-assistant-view-report/dea-connect-to-server-with-report-files.png)

2. 在 " **分析报表** " 屏幕的左侧，选择要查看的报表的条目。

   ![打开现有报表文件](./media/database-experimentation-assistant-view-report/dea-select-report-to-view.png)

## <a name="view-and-understand-the-analysis-report"></a>查看和了解分析报告

本部分将指导你完成分析报表。

在报表的第一页上，将显示有关运行实验的目标服务器的版本和生成信息的信息。 阈值允许调整 A/B 测试分析的灵敏度或容差。 默认情况下，阈值设置为 5%;任何 >= 5% 的性能改进都分类为 "改进的"。  下拉列表允许您评估具有不同性能阈值的报表。

您可以将报表中的数据导出到 CSV 文件中，选择 " **导出** " 按钮。  在分析报表的任何页上，你可以选择 " **打印** " 来打印当前屏幕上显示的内容。

### <a name="query-distribution"></a>查询分发

- 选择饼图的不同切片，以便仅显示属于该类别的查询。

   ![作为饼图切片的报表类别](./media/database-experimentation-assistant-view-report/dea-view-report-pie-slices.png)

  - **降级**：在目标2上执行的比目标1更糟的查询。
  - **错误**：在至少一个目标上至少出现一次错误的查询。
  - **改进**：在目标2上更好地执行的查询，而不是在目标1上。
  - **无法计算**：采样大小太小，无法进行统计分析的查询。 对于 A/B 测试分析，DEA 要求执行相同的查询，才能在每个目标上至少执行15次。
  - **相同**：目标1和目标2之间没有统计差的查询。

  错误查询（如果有）显示在单独的图表中;条形图按类型和饼图分类错误，并按错误 ID 分类错误。

   ![错误查询图表](./media/database-experimentation-assistant-view-report/dea-error-query-charts.png)

  有四种可能的错误类型：

  - **现有错误**：在目标1和目标2上存在的错误。
  - **新错误**：目标2上新的错误。
  - **已解决的错误**：目标1上存在但在目标2上已解决的错误。
  - **升级** 阻止程序：阻止升级到目标服务器的错误。

  单击图表中的任意条形或饼图部分会向下钻取到类别，并显示性能指标，即使对于 " **无法评估** " 类别也是如此。

  此外，仪表板还显示了前5个改进和降级的查询，以提供快速的性能概述。

### <a name="individual-query-drill-down"></a>单个查询向下钻取

您可以选择查询模板链接，以获取有关特定查询的更多详细信息。

![向下钻取到特定查询](./media/database-experimentation-assistant-view-report/dea-query-drill-down-report.png)

- 选择特定查询以打开相关的比较摘要。

   ![比较摘要](./media/database-experimentation-assistant-view-report/dea-view-report-comparison-summary.png)

   您可以找到该查询的摘要统计信息，如执行次数、平均持续时间、平均 CPU、平均读/写和错误计数。  如果查询是错误查询，则 " **错误信息** " 选项卡将显示有关错误的更多详细信息。  在 " **查询计划信息** " 选项卡上，可以找到有关目标1和目标2上的查询所使用的查询计划的信息。

   > [!NOTE]
   > 如果要分析扩展事件 (。.XEL) 文件，则不会收集查询计划信息，以限制用户计算机上的内存压力。

## <a name="see-also"></a>另请参阅

- 若要了解如何在命令提示符下生成分析报告，请参阅 [在命令提示符下运行](database-experimentation-assistant-run-command-prompt.md)。
