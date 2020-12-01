---
description: 渐变维度列（渐变维度向导）
title: 渐变维度列（渐变维度向导）| Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.loaddimwizard.scdsupport.f1
ms.assetid: 36de99d5-5368-48e0-b876-17e9c6862c6c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8b27c9c0e575775449d2f3a25c5a20bd6667b5cd
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88425719"
---
# <a name="slowly-changing-dimension-columns-slowly-changing-dimension-wizard"></a>渐变维度列（渐变维度向导）

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用 **“渐变维度列”** 对话框，为每个渐变维度列选择更改类型。  
  
 若要了解有关此向导的详细信息，请参阅 [Slowly Changing Dimension Transformation](../../../integration-services/data-flow/transformations/slowly-changing-dimension-transformation.md)。  
  
## <a name="options"></a>选项  
 **维度列**  
 从列表中选择维度列。  
  
 **更改类型**  
 选择“固定的属性”，或从两种变化的属性类型中选择一种类型。 在不需要更改列中的值时，请使用 **“固定的属性”** ；设置后， [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 会将更改视为错误。 若要使用更改后的值覆盖现有的值，请使用 **“变化的属性”** 。 若要在新记录中保存更改后的值，并将以前的记录标记为已过时，请使用 **“历史属性”** 。  
  
 **Remove**  
 选择一个维度列，通过单击“删除”可以将其从映射列的列表中删除。  
  
## <a name="see-also"></a>另请参阅  
 [使用渐变维度向导配置输出](../../../integration-services/data-flow/transformations/configure-outputs-using-the-slowly-changing-dimension-wizard.md)  
  
  
