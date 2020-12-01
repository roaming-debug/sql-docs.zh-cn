---
description: 历史属性选项（渐变维度向导）
title: 历史属性选项（渐变维度向导）| Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.loaddimwizard.histattriboption.f1
ms.assetid: a176ec66-ec39-4c99-99d1-c1afa8450e1e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9610a734543c9199e54d3f8489d86e03089f10ed
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88430699"
---
# <a name="historical-attribute-options-slowly-changing-dimension-wizard"></a>历史属性选项（渐变维度向导）

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用 **“历史属性选项”** 对话框按开始日期和结束日期显示历史属性，或者将历史属性记录到专门的列中。  
  
 若要了解有关此向导的详细信息，请参阅 [Slowly Changing Dimension Transformation](../../../integration-services/data-flow/transformations/slowly-changing-dimension-transformation.md)。  
  
## <a name="options"></a>选项  
 **使用单独的列显示当前和过期记录**  
 如果您选择使用单独的列来记录历史属性的状态，则下列选项可用：  
  
|选项|说明|  
|------------|-----------------|  
|**指示当前记录的列**|选择一个要在其中指示当前记录的列。|  
|**表示当前记录的值**|使用 **True** 或 **“当前”** 来显示该记录是否为当前记录。|  
|**表示过期记录的值**|使用 **False** 或 **“过期”** 来显示该记录是否为历史值。|  
  
 **使用开始日期和结束日期确定当前记录和过期记录**  
 此选项的维度表必须包括一个日期列。 如果您选择按开始日期和结束日期显示历史属性，则下列选项可用：  
  
|选项|说明|  
|------------|-----------------|  
|**开始日期列**|在维度表中选择要包含开始日期的列。|  
|**结束日期列**|在维度表中选择要包含结束日期的列。|  
|**用来设置日期值的变量**|从该列表中选择日期变量。|  
  
## <a name="see-also"></a>另请参阅  
 [使用渐变维度向导配置输出](../../../integration-services/data-flow/transformations/configure-outputs-using-the-slowly-changing-dimension-wizard.md)  
  
  
