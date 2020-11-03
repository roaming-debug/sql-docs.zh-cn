---
title: 将参数添加到移动报表 | Reporting Services | Microsoft Docs
description: Reporting Services 移动报表可以具有参数，因此报表阅读器可以筛选报表。 此类报表也可以成为钻取的目标。
ms.date: 07/30/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.topic: conceptual
ms.assetid: 113cb057-deec-40eb-abc8-f35d3900eaa6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 36bf305d4685f18e1c6df9129716ae9de84d4f84
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907285"
---
# <a name="add-parameters-to-a-mobile-report--reporting-services"></a>将参数添加到移动报表 | Reporting Services
可以创建带参数的 [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] 移动报表，以便你和你的报表读者可以筛选你的报表。 带参数的报表还可以是[源报表中的钻取](../../reporting-services/mobile-reports/add-drillthrough-from-a-mobile-report-to-other-mobile-reports-or-urls.md)的目标。 

若要创建带参数的移动报表，请以至少有一个参数的共享数据集开始。 阅读有关 [在共享数据集中创建参数](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)的信息。 移动报表不支持默认参数的 null 值，因此，请确保你的参数具有 null 以外的默认值。

向移动报表添加参数后，创建 URL 用于 [打开具有查询字符串参数的报表](../../reporting-services/mobile-reports/open-a-mobile-report-with-specific-query-string-parameters-reporting-services.md)。 

1. 在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion.md)] Web 门户的上栏中，选择“新建”   > “移动报表”  。  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)  
     
2. 在 [!INCLUDE[SS_MobileReptPub_Long](../../includes/ss-mobilereptpub-long.md)] 的左上角，选择“数据”选项卡  。   
  
3. 在右上角选择“添加数据”  。  
  
4. 选择“报表服务器”，然后选择一个服务器  。  
  
5. 导航到服务器上的共享数据集，并选择一个包含参数的数据集。  
  
   在网格中，你将看到数据集中的数据。 带括号 **{ }** 的绿色圆圈用于标记具有一个参数的数据集。  
     
   ![SSMRP_PforParam](../../reporting-services/mobile-reports/media/ssmrp-pforparam.png)  
  
6. 选择选项卡上的嵌齿，然后选择“Param {}”  。  
  
   ![SSMRP_ParamWheel](../../reporting-services/mobile-reports/media/ssmrp-paramwheel.png)  
  
7. 选择将传递值给参数的报表元素。  
  
   ![SSMRP_SetParam](../../reporting-services/mobile-reports/media/ssmrp-setparam.png)  
     
8. 选择“预览”查看报表的样式  。 在此报表中，选择列表使用的是 Category 参数。

   ![报表预览视图的屏幕截图，其中突出显示了“选择列表 1”。](../../reporting-services/mobile-reports/media/sql-server-mobile-report-publisher-selection-list-view-no-selection.png) 
   
9. 在选择列表中选择一个值时，报表将按该值进行筛选，在本例中，该值为“附件”。

   ![报表预览视图的屏幕截图，其中突出显示了“选择列表 1”，并选择了“配件”选项。](../../reporting-services/mobile-reports/media/sql-server-mobile-report-publisher-selection-list-category-selected.png)   
  
### <a name="see-also"></a>另请参阅  
-  [打开具有特定查询字符串参数的移动报表](../../reporting-services/mobile-reports/open-a-mobile-report-with-specific-query-string-parameters-reporting-services.md)
-  [添加从某个移动报表到其他移动报表或 URL 的钻取](../../reporting-services/mobile-reports/add-drillthrough-from-a-mobile-report-to-other-mobile-reports-or-urls.md)
-  [创建共享数据集或嵌入数据集](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)
- [使用 SQL Server Mobile Report Publisher 创建和发布移动报表](../../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md)  
  
  

