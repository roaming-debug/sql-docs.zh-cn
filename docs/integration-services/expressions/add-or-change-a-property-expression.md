---
description: 添加或更改属性表达式
title: 添加或更改属性表达式 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- expressions [Integration Services], creating
- expressions [Integration Services], property expressions
ms.assetid: cb5da499-065f-4fa6-9f6d-5bc5f385241e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3ef6580fb5a6983412343137e5cdf9e0d4ecbcf0
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123319"
---
# <a name="add-or-change-a-property-expression"></a>添加或更改属性表达式

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  可以为包、任务、Foreach 循环容器、For 循环容器、序列容器、事件处理程序、包和项目级别连接管理器以及日志提供程序创建属性表达式。  
  
 若要创建或更改属性表达式，您可以使用 **“属性表达式编辑器”** 或 **“表达式生成器”**。 可以从任务和容器使用的自定义编辑器中访问 **“属性表达式编辑器”** ，也可以从 **“属性”** 窗口中进行访问。 可以从 **“属性表达式编辑器”** 内部访问 **“表达式生成器”**。 在 **“属性表达式编辑器”** 或 **“表达式生成器”** 中编写表达式时， **“表达式生成器”** 提供一组图形工具，可以非常容易地生成复杂表达式。  
  
 若要了解 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供的语法、运算符和函数的详细信息，请参阅[运算符（SSIS 表达式）](../../integration-services/expressions/operators-ssis-expression.md)和[函数（SSIS 表达式）](../../integration-services/expressions/functions-ssis-expression.md)。 每个运算符或函数的主题都包括在表达式中使用该运算符或函数的示例。 有关更复杂的表达式示例，请参阅 [在包中使用属性表达式](../../integration-services/expressions/use-property-expressions-in-packages.md)。  
  
### <a name="to-create-or-change-a-property-expression"></a>创建或更改属性表达式  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开，再执行下列操作之一：  
  
    -   如果该项是一个任务或一个容器，请双击该项，再单击编辑器中的“表达式”。  
  
    -   右键单击项，再单击“属性”。  
  
3.  单击“表达式”框，再单击省略号 (…)。  
  
4.  在 **“属性表达式编辑器”** 中的 **“属性”** 列表中选择某个属性，然后执行下列操作之一：  
  
    -   在 **“表达式”** 列中直接键入或更改属性表达式，然后单击 **“确定”**。  
  
         -或-  
  
    -   单击属性的表达式行中的省略号 (...) 以打开“表达式生成器”。  
  
5.  （可选）在“表达式生成器”中，执行下列任一任务：  
  
    -   若要访问系统和用户定义的变量，请展开“变量”。  
  
    -   若要访问 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 表达式语言提供的函数、转换和运算符，请展开“数学函数”、“字符串函数”、“日期/时间函数”、“NULL 函数”、“类型转换”和“运算符”。  
  
    -   若要在 **“表达式生成器”** 中生成或更改表达式，请将变量、列、函数、运算符和转换拖到 **“表达式”** 框中，也可以在该框中键入表达式。  
  
    -   若要查看表达式的计算结果，请在 **“表达式生成器”** 中单击 **“计算表达式”**。  
  
         如果表达式无效，则将出现描述表达式语法错误的警报。  
  
        > [!NOTE]  
        >  计算表达式时不会将计算结果分配给属性。  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services (SSIS) 表达式](../../integration-services/expressions/integration-services-ssis-expressions.md)   
 [在包中使用属性表达式](../../integration-services/expressions/use-property-expressions-in-packages.md)   
 [Integration Services (SSIS) 包](../../integration-services/integration-services-ssis-packages.md)   
 [Integration Services 容器](../../integration-services/control-flow/integration-services-containers.md)   
 [Integration Services 任务](../../integration-services/control-flow/integration-services-tasks.md)   
 [Integration Services (SSIS) 事件处理程序](../../integration-services/integration-services-ssis-event-handlers.md)   
 [Integration Services (SSIS) 连接](../../integration-services/connection-manager/integration-services-ssis-connections.md)   
 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)  
  
  
