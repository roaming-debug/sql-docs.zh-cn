---
title: 使用钻取功能创建选项卡式移动报表 | Reporting Services 移动报表 | Microsoft Docs
description: 了解如何使用钻取功能和参数创建外观和行为与选项卡式报表类似的 Reporting Services 移动报表。
ms.date: 03/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.topic: conceptual
ms.assetid: c4d5d80d-370a-4a6d-8b76-698bd5ba5ba6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 95b153be1b4dc5a45effeb678ca0ccef83f06e6e
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907335"
---
# <a name="create-a-tabbed-mobile-report-by-using-drillthrough"></a>使用钻取功能创建选项卡式移动报表
了解如何使用钻取功能和参数创建外观和行为与选项卡式报表类似的 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 移动报表。

例如，在此报表中，顶部仪表的行为与选项卡类似。 单击“运输”仪表时，图表其余部分中的数据将筛选为运输数据。

![屏幕截图显示了“财务 - 运输”报表，其中选择了“运输”仪表。](../../reporting-services/mobile-reports/media/tabbed-mobile-report-web-viewer-transportation-complete.png)

在后台，这实际上是五个单独报表的集合，每个报表都具有不同的参数，用于筛选报表以匹配在报表顶部所选的仪表。 首先创建五个报表，然后针对其中的每个报表，将其他四个仪表设置为分别钻取到其他四个报表中。

此示例的步骤如下。

## <a name="create-the-basic-report"></a>创建基本报表

1. 使用五个仪表创建名为 Sales 的报表：

    * Sales
    * 运输
    * 燃料
    * 存储
    * 杂项费用

   ![销售报表的屏幕截图，其中包含五个仪表。](../../reporting-services/mobile-reports/media/01-sales-mobile-report-publisher.png)
    
2. 针对“销售”仪表将“主题色”设置为“开”，使其相对于报表的其余部分更加醒目--在本示例中，为黑底白色   。

    ![“销售”仪表的屏幕截图，其中有一个红色箭头指向位于“开启”位置的“重点”滑块。](../../reporting-services/mobile-reports/media/01a-sales-accent-mobile-report-publisher.png)
    
3. 将它保存到 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 报表服务器。

## <a name="make-copies-of-the-report"></a>创建报表的副本

1. 创建销售报表的四份副本并分别命名： 

    * 运输
    * 燃料
    * 存储
    * 杂项费用

3. 将它们保存到 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 报表服务器。

## <a name="set-the-gauge-as-a-drillthrough"></a>将仪表设置为钻取

在本部分中，将把每个仪表（“销售”仪表除外）设置为钻取到相应的报表。

1. 在“销售”报表中，选择“运输”仪表。

    ![销售报表的屏幕截图，其中有一个红色箭头从“运输”仪表指向“钻取目标”选项。](../../reporting-services/mobile-reports/media/02-sales-create-drillthrough-mobile-report-publisher.png)

2. 选择“布局”选项卡后，在“视觉对象属性”窗格中，选择“钻取目标”    。

3. 选择“移动报表”  。

4. 导航到将作为钻取目标的报表然后选中 - 在本示例中为“财务 - 运输”。

    ![“从服务器打开”对话框的屏幕截图，其中突出显示了“财务 - 运输”选项。](../../reporting-services/mobile-reports/media/03-sales-select-dashboard-mobile-report-publisher.png)

5. 在“配置目标报表”中，选择用于筛选报表的参数，并选择“应用”   。

   ![“配置目标报表”部分的屏幕截图，其中显示了“财务 - 运输”报表参数。](../../reporting-services/mobile-reports/media/04-sales-apply-parameters-mobile-report-publisher.png)
   
6. 分别对“销售”报表中的其他每个仪表重复上述步骤。 

## <a name="set-the-gauges-for-the-other-reports"></a>设置其他报表的仪表

1.  打开“运输”报表，将“销售”仪表设置为钻取到“销售”报表，然后将其他三个仪表设置为分别钻取到各自报表。

2. 同样，在“运输”报表中，将“运输”仪表的“主题色”设置为“开”，使其相对于报表其余部分更加醒目   。

3. 对“燃料”、“存储”和“杂项支出”报表执行上述步骤。 

## <a name="view-the-report-in-the-web-portal"></a>在 Web 门户中查看报表

1. 转到 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 报表服务器并打开其中一个报表。 

2. 请注意，每个仪表在右上角均有一个钻取图标。

    ![“燃料”仪表的屏幕截图。](../../reporting-services/mobile-reports/media/web-viewer-drillthrough-icon-mobile-report-builder.png)

3. 选择其中一个仪表，转到筛选到该仪表数据的报表。

   ![屏幕截图显示了“财务 - 运输”报表，其中有一个红色箭头指向“运输”仪表，该仪表同时也用红框框出。](../../reporting-services/mobile-reports/media/06-mobile-report-web-viewer-transportation.png)

### <a name="see-also"></a>另请参阅
    
* [将参数添加到移动报表](../../reporting-services/mobile-reports/add-parameters-to-a-mobile-report-reporting-services.md)
* [添加从某个移动报表到其他移动报表或 URL 的钻取](../../reporting-services/mobile-reports/add-drillthrough-from-a-mobile-report-to-other-mobile-reports-or-urls.md)




  

