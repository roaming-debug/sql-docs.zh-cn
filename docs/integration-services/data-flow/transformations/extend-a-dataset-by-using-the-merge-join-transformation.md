---
description: 使用合并联接转换扩展数据集
title: 使用合并联接转换扩展数据集 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Merge Join transformation
- datasets [Integration Services], joining
- datasets [Integration Services], extending
- joining datasets [Integration Services]
ms.assetid: 9e512c3c-f89b-45f3-8281-cdb8f35a2b1f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7000b1593f20305c4d09f1f8ee11eb4b874ee3aa
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123357"
---
# <a name="extend-a-dataset-by-using-the-merge-join-transformation"></a>使用合并联接转换扩展数据集

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  若要添加和配置合并联接转换，则包中必须已包含至少一个数据流任务和为合并联接转换提供输入的两个数据流组件。  
  
 合并联接转换需要两个已排序的输入。 有关详细信息，请参阅 [为合并转换和合并联接转换排序数据](../../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)。  
  
### <a name="to-extend-a-dataset"></a>扩展数据集  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击 **“数据流”** 选项卡，然后从 **“工具箱”** 中将合并联接转换拖动到设计图面上。  
  
4.  将连接线从数据源或前一个转换拖到合并联接转换，从而将合并联接转换连接到数据流。  
  
5.  双击合并联接转换。  
  
6.  在 **“合并联接转换编辑器”** 对话框的 **“联接类型”** 列表中，选择要用的联接类型。  
  
    > [!NOTE]  
    >  如果选择了“左外部联接”类型，那么可以单击“交换输入”来切换输入，将左外部联接转换为右外部联接。  
  
7.  将左输入中的列拖动到右输入中的列，以指定联接列。 如果这些列名称相同，则可以选中 **“联接键”** 复选框，合并联接转换将自动创建联接。  
  
    > [!NOTE]  
    >  只能在具有相同排序位置的列之间创建联接，而且必须按照排序位置所指定的顺序创建联接。 如果尝试不按顺序创建联接， **“合并联接转换编辑器”** 会提示您为跳过的排序顺序位置创建其他联接。  
  
    > [!NOTE]  
    >  默认情况下，输出根据联接列进行排序。  
  
8.  在左输入和右输入中，选中要包含在输出中的其他列的复选框。 默认情况下包含联接列。  
  
9. 还可以更新 **“输出别名”** 列中的输出列名称。  
  
10. 单击“确定”。  
  
11. 若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
## <a name="see-also"></a>另请参阅  
 [合并联接转换](../../../integration-services/data-flow/transformations/merge-join-transformation.md)   
 [Integration Services 转换](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Integration Services 路径](../../../integration-services/data-flow/integration-services-paths.md)   
 [数据流任务](../../../integration-services/control-flow/data-flow-task.md)  
  
  
