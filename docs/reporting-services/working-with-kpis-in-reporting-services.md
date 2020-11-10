---
title: 使用 Reporting Services 中的 KPI | Microsoft Docs
description: 了解如何使用 SQL Server Reporting Services 中的 KPI 来轻松度量状态和性能。
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.date: 07/02/2017
ms.openlocfilehash: e661fee4e9b5afe5f78cae444ff8d6574a536bb9
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243774"
---
# <a name="working-with-kpis-in-reporting-services"></a>使用 Reporting Services 中的 KPI

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

关键绩效指标 (KPI) 是一个视觉提示，用于传达某个目标的进度量。  关键绩效指标对于团队、经理和企业很有价值，可快速评估针对可度量的目标所进行的工作进度。
  
通过使用 SQL Server Reporting Services 中的 KPI，你可以轻松地将以下问题的答案可视化：  
  
- 我是超前还是落后？  
  
- 我超前或落后多少？  
  
- 我已完成的最小工作量是多少？  

> [!NOTE]
> 只有在 SSRS 门户的企业版（开发人员）版本中才能访问 KPI。

## <a name="creating-a-dataset"></a>创建数据集

KPI 将只使用共享数据集的第一行数据。 请确保你想要使用的数据位于第一行上。 若要创建共享数据集，可以使用报表生成器或 SQL Server Data Tools。  
  
> **注意** ：该 KPI 的数据集不必位于相同文件夹中。  
  
## <a name="placement-of-kpis"></a>KPI 的位置  
  
可以在报表服务器中的任何文件夹中创建 KPI。  创建 KPI 前，需要考虑 KPI 的正确放置位置。 可以将它放在用户可见、同时可与周围其他报表和 KPI 相关的文件夹中。  
## <a name="adding-a-kpi"></a>添加 KPI
  
在确定 KPI 的位置之后，转到该文件夹并从顶部菜单中选择“新建” > “KPI” 。  
  
![屏幕截图：已调出“KPI”选项的“新建”下拉列表。](../reporting-services/media/rscreatekpi1.png)  
  
这将为你显示“新建 KPI”  屏幕。  
  
![显示“新建 KPI”屏幕的屏幕截图。](../reporting-services/media/rscreatekpi2.png)  
  
你可以分配静态值，或使用共享数据集的数据。 在创建新的 KPI 时，它将使用一组随机的人工输入数据填充它们。  
  
| 字段 | 描述 |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| 值格式 | 用于更改要显示的值的格式。 |
| 值 | 要显示的 KPI 的值。 |
| 目标 | 用作数值比较并显示为一个百分比差值。 |
| 状态 | 用于确定 KPI 磁贴颜色的数值。 有效值为 1（绿色）、0（琥珀色）和 -1（红色）。 |
| 趋势集 | 用于图表可视化的逗号分隔的数值。 也可以将其设置为具有表示走向的值的数据集列。 |
| 相关内容 | 设置钻取链接的功能。 此链接可以是在门户上发布的移动报表或自定义 URL。 |
  
> **警告** ：虽然可以在设计时使用用于“状态”  字段的单词，如果要刷新数据集，还是应当使用数值。 如果使用单词刷新数据集，而不是使用数值，则可能会损坏服务器上的 KPI。  
>
> **注意** ：“值”、“目标”和“状态”字段只能从数据集结果的第一行选择一个值  。 但是，“趋势集”  字段中，可以选择反映这种趋势的具体列。  
  
若要使用来自共享数据集的数据，可以执行以下步骤。
  
1. 将字段下拉框从“手动设置” ，或“未设置” ，更改为“数据集字段” 。  
  
    ![屏幕截图：“值”选项设置为“数据集字段”，“选择数据集”字段设置为“未设置”。](../reporting-services/media/rscreatekpi3.png)  
  
2. 在数据框中选择省略号 (…)。 此时会弹出“选择数据集”  屏幕。  
  
    ![“选择数据集”部分的屏幕截图，其中已选择“Finance_KPI”选项。](../reporting-services/media/rscreatekpi4.png)  
  
3. 选择包含要显示的数据的数据集。  
  
4. 选择想要使用的字段。 选择“确定”。  
  
    ![显示“从 Finance_KPI 选择字段”部分的屏幕截图，其中 Sum_Amount 选项处于选定状态。](../reporting-services/media/rscreatekpi5.png)  
  
5. 更改“值格式”  以匹配你的值的格式。 在此示例中，值是一种货币。  
  
    ![KPI 预览的屏幕截图，其中“值格式”选项设置为“货币”。](../reporting-services/media/rscreatekpi6.png)  
  
6. 选择“应用”。  
  
    ![KPI 的屏幕截图，其中显示数据集具有两个项。](../reporting-services/media/rscreatekpi7.png)

## <a name="configuring-related-content"></a>配置相关内容

选择“移动报表”时，可以在对话框中选择目标。

   ![屏幕截图：“相关内容”选项设置为“移动报表”，并将“选择移动报表”选项设置为“未设置”。](media/rscreatekpi-related-content-mobile-report.png)

现在，单击门户中的 KPI 后，“相关内容”下拉列表下会显示移动报表的缩略图。 单击此缩略图可以直接导航到此报表。

还可以指定自定义 URL。 此任务可以是任何内容：网站、SharePoint 站点、SSRS 报表的 URL（这将允许你通过硬编码参数传递）。

![屏幕截图：将“相关内容”选项设置为“自定义 URL”，并将“输入 URL”选项设置为“http://”。](media/rscreatekpi-related-content-custom-url.png)

当你现在单击 KPI 时，“相关内容”下会显示 URL。

只能添加一个移动报表或一个自定义 URL。
  
## <a name="removing-a-kpi"></a>删除 KPI  
  
若要删除 KPI，可以执行以下步骤。
  
1. 选择要删除的 KPI 的省略号 (…)。 选择“管理”。  
  
    ![已选择的 KPI 的省略号选项的屏幕截图，其中已调出“管理”选项。](../reporting-services/media/rsremovekpi1.png)  
  
2. 选择“删除”。 在确认对话框中再次选择“删除”  。  
  
    ![“删除”选项的屏幕截图。](../reporting-services/media/rsremovekpi2.png)  
  
## <a name="refreshing-a-kpi"></a>刷新 KPI  
  
若要刷新 KPI，请配置共享数据集缓存。 有关缓存刷新计划的详细信息，请参阅[使用共享数据集](../reporting-services/work-with-shared-datasets-web-portal.md)。  
  
## <a name="next-steps"></a>后续步骤
  
[Web 门户](../reporting-services/web-portal-ssrs-native-mode.md)  
[使用共享数据集](../reporting-services/work-with-shared-datasets-web-portal.md)

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
