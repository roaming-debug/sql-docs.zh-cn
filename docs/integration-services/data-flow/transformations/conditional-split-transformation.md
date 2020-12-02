---
description: 有条件拆分转换
title: 有条件拆分转换 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.conditionalsplittrans.f1
- sql13.dts.designer.conditionalsplittransformation.f1
helpviewer_keywords:
- Conditional Split transformation
- route rows to different outputs [Integration Services]
ms.assetid: 3f8b5825-226f-413c-ba8f-0bb931ca3770
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2f70d8f963a6f26131099e6cd7d09d5019f9c6bc
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123407"
---
# <a name="conditional-split-transformation"></a>有条件拆分转换

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  有条件拆分转换可以根据数据内容将数据行路由到不同的输出。 有条件拆分转换的实现类似于编程语言中的 CASE 决策结构。 此转换将计算表达式，并且根据结果将数据行定向到指定输出。 此转换还提供默认输出，因此，如果某行与任何表达式都不匹配，它会被定向到默认输出。  
  
## <a name="configuration-of-the-conditional-split-transformation"></a>有条件拆分转换的配置  
 可以按照下列方式配置有条件拆分转换：  
  
-   提供一个表达式，此表达式将转换要测试的每个条件都计算为一个布尔值。  
  
-   指定计算条件的顺序。 顺序很重要，因为行将被发送到对应于第一个计算结果为 true 的条件的输出。  
  
-   指定转换的默认输出。 此转换要求指定默认输出。  
  
 每个输入行只能被发送到一个输出，即第一个计算结果为 true 的条件的输出。 例如，下列条件将 **FirstName** 列中以字母 *A* 开头的所有行定向到一个输出，将以字母 *B* 开头的行定向到另一个输出，将所有其他行定向到默认输出。  
  
 输出 1  
  
 `SUBSTRING(FirstName,1,1) == "A"`  
  
 输出 2  
  
 `SUBSTRING(FirstName,1,1) == "B"`  
  
 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 包含的函数和运算符可用于创建计算输入数据并定向输出数据的表达式。 有关详细信息，请参阅 [Integration Services (SSIS) 表达式](../../../integration-services/expressions/integration-services-ssis-expressions.md)。  
  
 有条件拆分转换包括 **FriendlyExpression** 自定义属性。 加载包时，可以通过属性表达式更新此属性。 有关详细信息，请参阅 [在包中使用属性表达式](../../../integration-services/expressions/use-property-expressions-in-packages.md) 和 [转换自定义属性](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)。  
  
 此转换具有一个输入、一个或多个输出和一个错误输出。  
  
 可以通过 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 **“高级编辑器”** 对话框反映了可以通过编程方式进行设置的属性。 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](../set-the-properties-of-a-data-flow-component.md)  
  
-   [转换自定义属性](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 有关如何设置属性的详细信息，请单击下列主题之一：  
  
-   [使用有条件拆分转换拆分数据集](../../../integration-services/data-flow/transformations/split-a-dataset-by-using-the-conditional-split-transformation.md)  
  
-   [设置数据流组件的属性](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 [使用有条件拆分转换拆分数据集](../../../integration-services/data-flow/transformations/split-a-dataset-by-using-the-conditional-split-transformation.md)  
  
## <a name="conditional-split-transformation-editor"></a>有条件拆分转换编辑器
  可以使用 **“有条件拆分转换编辑器”** 对话框创建表达式，设置表达式计算顺序，以及对有条件拆分输出进行命名。 此对话框包含可以用于生成表达式的数学、字符串和日期/时间函数及运算符。 计算结果为 True 的第一个条件确定了行定向到的输出。  
  
> [!NOTE]  
>  有条件拆分转换仅将每个输入行定向到一个输出。 如果输入多个条件，则转换会将每一行发送到条件为 True 的第一个输出，而忽略各行的后续条件。 如果需要连续计算多个条件，则可能需要在数据流中连接多个有条件拆分转换。  
  
### <a name="options"></a>选项  
 **Order**  
 选择行并使用右边的箭头键，可以更改计算表达式的顺序。  
  
 **输出名称**  
 提供输出名称。 默认为带编号的名称示例列表，不过，您也可以任选一个唯一的描述性名称。  
  
 Condition  
 键入表达式，或通过从可用的列、变量、函数和运算符的列表中拖动相应项来生成表达式。  
  
 此属性的值可以使用属性表达式来指定。  
  
 **相关主题：**  [Integration Services (SSIS) 表达式](../../../integration-services/expressions/integration-services-ssis-expressions.md)、[运算符（SSIS 表达式）](../../../integration-services/expressions/operators-ssis-expression.md)和 [函数（SSIS 表达式）](../../../integration-services/expressions/functions-ssis-expression.md)  
  
 **默认输出名称**  
 为默认输出键入名称，或使用默认名称。  
  
 **配置错误输出**  
 使用 [配置错误输出](../error-handling-in-data.md) 对话框指定处理错误的方式。  
  
## <a name="see-also"></a>另请参阅  
 [数据流](../../../integration-services/data-flow/data-flow.md)   
 [Integration Services 转换](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
