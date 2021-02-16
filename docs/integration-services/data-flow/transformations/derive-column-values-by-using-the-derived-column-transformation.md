---
description: 使用派生列转换派生列值
title: 使用派生列转换派生列值 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- columns [Integration Services]
- derived columns
- columns [Integration Services], values
- Derived Column transformation
ms.assetid: 28b07746-fc6f-42b2-b741-9de6fac3f29c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 06ecff11e6fc32ad59e6457ec2404292a9c22955
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353723"
---
# <a name="derive-column-values-with-the-derived-column-transformation"></a>使用派生列转换派生列值

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  若要添加和配置派生列转换，包必须已包含至少一个数据流任务和一个源。  
  
 派生列转换使用表达式来更新现有值或向新列中添加值。 当您选择向新列中添加值时， **“派生列转换编辑器”** 对话框会对表达式求值并相应地定义列的元数据。 例如，如果一个表达式连接两列（每列的数据类型均为 DT_WSTR，长度均为 50），两列值之间有一个空格，则新列的数据类型为 DT_WSTR，长度为 101。 您可以更新新列的数据类型。 唯一的要求是数据类型与插入的数据兼容。 例如，当您将日期值分配给数据类型为整数的列时， **“派生列转换编辑器”** 对话框将生成验证错误。 根据所选数据类型，您可以指定列的长度、精度、小数位数和代码页。  
  
### <a name="to-derive-column-values"></a>派生列值  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击 **“数据流”** 选项卡，然后将派生列转换从 **“工具箱”** 拖动到设计图面。  
  
4.  将连接线从源或前一转换拖到派生列转换，从而将派生列转换连接到数据流。  
  
5.  双击派生列转换。  
  
6.  在 **“派生列转换编辑器”** 对话框中，将变量、列、函数和运算符拖动到网格中的 **“表达式”** 列，从而生成要用作条件的表达式。 或者，也可以在 **“表达式”** 列中键入表达式。  
  
    > [!NOTE]  
    >  如果表达式无效，表达式文本将突出显示，列上的工具提示将对错误进行说明。  
  
7.  在“派生列”列表中，选择“\<add as new column>”以将表达式的计算结果写入新列，或选择一个现有列以用计算结果对其进行更新 。  
  
     如果选择使用新列， **“派生列转换编辑器”** 对话框将对表达式求值，并根据数据类型、长度、精度、小数位数和代码页为列指定数据类型。  
  
8.  如果使用新列，请在 **“数据类型”** 列表中选择数据类型。 根据所选的数据类型，可选择更新 **“长度”** 列、 **“精度”** 列、 **“小数位数”** 列和 **“代码页”** 列中的值。 现有列的元数据不能更改。  
  
9. 还可以在 **“派生列名称”** 列中修改这些值。  
  
10. 若要配置错误输出，请单击 **“配置错误输出”** 。 有关详细信息，请参阅 [Debugging Data Flow](../../../integration-services/troubleshooting/debugging-data-flow.md)。  
  
11. 单击“确定”。  
  
12. 若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
## <a name="see-also"></a>另请参阅  
 [Derived Column Transformation](../../../integration-services/data-flow/transformations/derived-column-transformation.md)   
 [Integration Services 数据类型](../../../integration-services/data-flow/integration-services-data-types.md)   
 [Integration Services 转换](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Integration Services 路径](../../../integration-services/data-flow/integration-services-paths.md)   
 [数据流任务](../../../integration-services/control-flow/data-flow-task.md)   
 [Integration Services (SSIS) 表达式](../../../integration-services/expressions/integration-services-ssis-expressions.md)  
  
  
