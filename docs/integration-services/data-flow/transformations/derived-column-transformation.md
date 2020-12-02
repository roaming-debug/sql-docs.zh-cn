---
description: 派生列转换
title: 派生列转换 | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.derivedcolumntrans.f1
- sql13.dts.designer.derivedcolumntransformation.f1
helpviewer_keywords:
- multiple derived columns
- expressions [Integration Services], derived columns
- derived columns
- columns [Integration Services], derivations
- Derived Column transformation
ms.assetid: 8eba755e-8e48-4233-bd1e-09a46bf2692f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: df48a338c2fe6cbc938284ed85b3b08fdc06f1cc
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123379"
---
# <a name="derived-column-transformation"></a>派生列转换

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  派生列转换通过对转换输入列应用表达式来创建新列值。 表达式可以包含来自转换输入的变量、函数、运算符和列的任意组合。 结果可作为新列添加，也可作为替换值插入到现有列。 派生列转换可定义多个派生列，任何变量或输入列都可以出现在多个表达式中。  
  
 可以使用此转换执行下列任务：  
  
-   将不同列的数据连接到一个派生列中。 例如，可以使用表达式 **将** FirstName **和** LastName **列中的值组合到名为** FullName `FirstName + " " + LastName`的单个派生列中。  
  
-   通过使用 SUBSTRING 之类的函数从字符串数据中提取字符，然后将结果存储到派生列中。 例如，可以使用表达式 **从** FirstName `SUBSTRING(FirstName,1,1)`列提取人名的首字母。  
  
-   对数值数据应用数学函数，然后将结果存储到派生列中。 例如，可以使用表达式 **将数值列** SalesTax `ROUND(SalesTax, 2)`的值更改为精确到小数点后两位。  
  
-   创建比较输入列和变量的表达式。 例如，可以使用表达式 **来比较变量** Version **与** ProductVersion **列中的数据，然后根据比较结果决定选用** Version **还是** ProductVersion `ProductVersion == @Version? ProductVersion : @Version`的值。  
  
-   提取日期时间值的某部分。 例如，可以通过表达式 `DATEPART("year",GETDATE())`使用 GETDATE 和 DATEPART 函数提取当前年份。  
  
-   使用表达式将日期字符串转换为特定格式。  
  
## <a name="configuration-of-the-derived-column-transformation"></a>派生列转换的配置  
 可以按照下列方式配置派生列转换：  
  
-   为每个输入列或即将更改的新列提供表达式。 有关详细信息，请参阅 [Integration Services (SSIS) 表达式](../../../integration-services/expressions/integration-services-ssis-expressions.md)。  
  
    > [!NOTE]  
    >  如果表达式引用了由派生列转换覆盖的输入列，则表达式使用列的原始值，而不是派生值。  
  
-   如果向新列中添加结果并且数据类型为 **string**，则需指定代码页。 有关详细信息，请参阅 [Comparing String Data](../../../integration-services/data-flow/comparing-string-data.md)。  
  
 派生列转换包括 FriendlyExpression 自定义属性。 加载包时，可以通过属性表达式更新此属性。 有关详细信息，请参阅 [在包中使用属性表达式](../../../integration-services/expressions/use-property-expressions-in-packages.md)和 [转换自定义属性](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)。  
  
 此转换有一个输入、一个常规输出和一个错误输出。  
  
 可以通过 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 **“高级编辑器”** 对话框反映了可以通过编程方式进行设置的属性。 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](../set-the-properties-of-a-data-flow-component.md)  
  
-   [转换自定义属性](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 有关如何设置属性的详细信息，请单击下列主题之一：  
  
-   [设置数据流组件的属性](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [使用派生列转换派生列值](../../../integration-services/data-flow/transformations/derive-column-values-by-using-the-derived-column-transformation.md)  
  
## <a name="derived-column-transformation-editor"></a>派生列转换编辑器
  可以使用 **“派生列转换编辑器”** 对话框，创建填充新列或替换列的表达式。  
  
### <a name="options"></a>选项  
 **变量和列**  
 通过将变量或输入列从可用变量和列的列表中拖到下面窗格中的现有表行，或者拖到列表底部的新行中，生成使用变量或输入列的表达式。  
  
 **函数和运算符**  
 通过将函数和运算符从列表中拖到下面的窗格中，生成使用函数或运算符的表达式，以计算输入数据和直接输出数据。  
  
 **派生列名称**  
 提供派生列名称。 默认值为带编号的派生列列表；不过，您也可以任选一个唯一的描述性名称。  
  
 **派生列**  
 从列表中选择派生列。 选择是将派生列添加为新的输出列，还是替换现有列中的数据。  
  
 **表达式**  
 键入表达式，或通过从可用列、变量、函数和运算符的以前列表中进行拖动来生成表达式。  
  
 此属性的值可以使用属性表达式来指定。  
  
 **相关主题**：[Integration Services (SSIS) 表达式](../../../integration-services/expressions/integration-services-ssis-expressions.md)、[运算符（SSIS 表达式）](../../../integration-services/expressions/operators-ssis-expression.md)和 [函数（SSIS 表达式）](../../../integration-services/expressions/functions-ssis-expression.md)  
  
 **数据类型**  
 如果向新列中添加数据，“派生列转换编辑器”对话框将自动计算表达式的值并设置相应的数据类型。 该列的值是只读的。 有关详细信息，请参阅 [Integration Services 数据类型](../../../integration-services/data-flow/integration-services-data-types.md)。  
  
 **长度**  
 如果向新列中添加数据，“派生列转换编辑器”对话框将自动计算表达式的值并设置字符串数据的列长度。 该列的值是只读的。  
  
 **精度**  
 如果向新列中添加数据，“派生列转换编辑器”对话框将自动根据数据类型来设置数值数据的精度。 该列的值是只读的。  
  
 **缩放**  
 如果向新列中添加数据，“派生列转换编辑器”对话框将自动根据数据类型来设置数值数据的小数位数。 该列的值是只读的。  
  
 **代码页**  
 如果向新列中添加数据，“派生列转换编辑器”对话框将自动设置 DT_STR 数据类型的代码页。 可以更新 **“代码页”**。  
  
 **配置错误输出**  
 使用 [配置错误输出](../error-handling-in-data.md) 对话框指定处理错误的方式。  
  
## <a name="related-content"></a>相关内容  
 social.technet.microsoft.com 上的技术文章 [SSIS 表达式示例](https://go.microsoft.com/fwlink/?LinkId=220761)