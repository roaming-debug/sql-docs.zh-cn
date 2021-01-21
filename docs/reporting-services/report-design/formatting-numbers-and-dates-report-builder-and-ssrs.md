---
title: 设置数字和日期的格式（报表生成器）| Microsoft Docs
description: 了解如何通过从数据区域的“属性”对话框的“数字”页选择格式，从而在报表生成器数据区域中设置数字和日期的格式。
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.placeholderproperties.number.f1
- "10127"
- sql13.rtp.rptdesigner.textboxproperties.number.f1
- "10130"
- "10286"
- sql13.rtp.rptdesigner.serieslabelproperties.number.f1
- "10285"
- sql13.rtp.rptdesigner.axisproperties.number.f1
ms.assetid: 6de1a725-9f06-4708-be26-2d55e442e344
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 9df64577305006d406be0f10e060a7ac60ed10df
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596890"
---
# <a name="formatting-numbers-and-dates-report-builder-and-ssrs"></a>设置数字和日期的格式（报表生成器和 SSRS）
  通过从相应数据区域的 **“属性”** 对话框的 **“数字”** 页选择格式，可以在数据区域中设置数字和日期的格式。  
  
 若要指定文本框报表项中的格式字符串，需要选择要对其设置格式的项，右键单击该项，选择 **“文本框属性”**，然后单击 **“数字”**。 可以通过相同方式设置表或矩阵数据区域中的各个单元格的格式，因为表或矩阵中的单元格是单个文本框。  
  
 图表数据区域通常显示沿类别 (x) 轴的日期，和沿值 (y) 轴的值。 若要指定图表中的格式设置，请右键单击某个轴并选择 **“轴属性”** 。 在值轴上，只能为数字指定格式。 有关详细信息，请参阅[设置图表上轴标签的格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md)。  
  
 若要指定仪表数据区域中的格式设置，请右键单击仪表的刻度，并选择 **“径向刻度属性”** 或 **“线性刻度属性”** 。  
  
> [!NOTE]  
>  如果要使用的某些格式设置选项显示为灰色，这表示这些格式设置选项与字段的数据类型（在数据源中设置）不兼容。 例如，如果该字段包含数值，但该字段的数据类型为 String，则无法应用数值数据格式设置选项（如货币或小数）。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="considerations-for-formatting-numbers-and-dates"></a>设置数字和日期格式的注意事项  
 在设置报表中数字和日期的格式之前，应注意以下事项：  
  
-   默认情况下，数字的格式设置反映客户端计算机上的区域性设置。 将格式设置字符串用于指定数字的显示方式，可使格式设置保持一致，而无论用户在何处查看报表。  
  
-   **“数字”** 页上提供的格式是 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 标准数字格式字符串的子集。 若要使用未显示在对话框中的自定义格式来设置数字或日期格式，可以将任何 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 格式字符串用于数字或日期。 有关自定义格式字符串的详细信息，请参阅 MSDN 上的主题 [Formatting Types](/previous-versions/) （设置类型的格式）。  
  
-   如果已指定了自定义格式字符串，则它的优先级比特定于区域性的默认设置的优先级更高。 例如，假定设置了自定义格式字符串“#,###”以将数字 1234 显示为 1,234。 对于美国用户和欧洲的用户而言，它的含义可能是不同的。 在指定自定义格式之前，请考虑您选择的格式将会如何影响那些可能会查看报表的不同区域性的用户。  
  
-   如果指定了无效的格式字符串，则将被格式化文本解释为覆盖格式设置的文字字符串。  
  
-   如果是对同一文本框中的数字和字符组合设置格式，请考虑使用占位符，以将数字和其余文本区分开来单独设置格式。 有关详细信息，请参阅 [设置文本和占位符的格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-text-and-placeholders-report-builder-and-ssrs.md)的详细信息。 如果为文本框的 Format 属性指定了无效格式字符串，则该格式字符串会被忽略。 如果为图表或仪表上的 Format 属性指定了无效格式字符串，则指定的格式字符串将被解释为字符串并且不会应用格式设置。  
  
-   如果选择 **“类别”** 下的 **“货币”** 并选中 **“值的显示位置”**，则可以选择 **“千”**、 **“百万”** 或 **“十亿”** 使用财务格式显示数字。 例如，如果字段值为 1,789,905,394，且您选择 **“十亿”** 并指定 2 个小数位，则该值在报表中显示为 1.78。  
  
## <a name="see-also"></a>另请参阅  
 [设置文本和占位符的格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-text-and-placeholders-report-builder-and-ssrs.md)   
 [设置线条、颜色和图像的格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-lines-colors-and-images-report-builder-and-ssrs.md)   
 [设置图表格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [将轴标签的格式设置为日期或货币（报表生成器和 SSRS）](../../reporting-services/report-design/format-axis-labels-as-dates-or-currencies-report-builder-and-ssrs.md)   
 [设置仪表上刻度的格式（报表生成器和 SSRS）](../../reporting-services/report-design/formatting-scales-on-a-gauge-report-builder-and-ssrs.md)  
  
