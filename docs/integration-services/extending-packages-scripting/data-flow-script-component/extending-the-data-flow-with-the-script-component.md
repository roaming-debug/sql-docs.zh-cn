---
description: 使用脚本组件扩展数据流
title: 使用脚本组件扩展数据流 | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- data flow task [Integration Services], components
- data flow [Integration Services], extending
- debugging [Integration Services], components
- code design mode [Integration Services]
- metadata [Integration Services]
- scripts [Integration Services], extending data flow
- data flow components [Integration Services], Script component
- components [Integration Services], data flow
- Script component [Integration Services], about Script component
- Script component [Integration Services]
- data flow [Integration Services], components
ms.assetid: 072bc4b8-363a-4131-87c3-240338e2fa5c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 82f34ab83935bee2972a4dbb2b007eb2632b9fba
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122938"
---
# <a name="extending-the-data-flow-with-the-script-component"></a>使用脚本组件扩展数据流

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  脚本组件通过以 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic 或 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C# 编写、在包运行时编译和执行的自定义代码来扩展 [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 包的数据流功能。 当 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 包含的数据流源、转换或目标不能完全满足您的需求时，脚本组件可简化自定义数据流源、转换或目标的开发。 用预期输入和输出配置该组件后，它将为您编写所有必需的基础结构代码，这样您就可以只将注意力集中于自定义处理所需的代码。  
  
 脚本组件通过 ComponentWrapper 和 BufferWrapper 项目项中自动生成的类来与包含包和数据流进行交互，这两个项目项分别是 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer> 类的实例。 这些类使连接、变量和其他包项成为类型化对象，并管理输入和输出。 脚本组件还可以使用 [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] 命名空间、[!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 类库以及自定义程序集来实现自定义功能。  
  
 脚本组件及其生成的基础结构代码可以大大简化自定义数据流组件的开发过程。 但是，若要了解脚本组件的工作方式，你会发现阅读[开发自定义数据流组件](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)一节是很有帮助的，从该节中可了解开发自定义数据流组件的步骤。  
  
 如果您创建的是计划在多个包中重用的源、转换或目标，则应考虑开发自定义组件，而不是使用脚本组件。 有关详细信息，请参阅 [开发自定义数据流组件](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)。  
  
## <a name="in-this-section"></a>本节内容  
 下列主题提供有关脚本组件的详细信息。  
  
 [在脚本组件编辑器中配置脚本组件](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md)  
 在“脚本转换编辑器”中配置的属性会影响脚本组件代码的功能和性能。  
  
 [脚本组件的编码和调试](../../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md)  
 可以使用 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA) 开发环境来开发包含在脚本组件中的脚本。  
  
 [了解脚本组件对象模型](../../../integration-services/extending-packages-scripting/data-flow-script-component/understanding-the-script-component-object-model.md)  
 新脚本组件项目包含三个带有多个类和自动生成的属性及方法的项目项。  
  
 [在脚本组件中使用变量](../../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md)  
 ComponentWrapper 项目项包含包变量的强类型取值函数属性。  
  
 [在脚本组件中连接数据源](../../../integration-services/extending-packages-scripting/data-flow-script-component/connecting-to-data-sources-in-the-script-component.md)  
 ComponentWrapper 项目项还包含在包中定义的连接的强类型取值函数属性。  
  
 [在脚本组件中引发事件](../../../integration-services/extending-packages-scripting/data-flow-script-component/raising-events-in-the-script-component.md)  
 您可以引发事件来提供问题和错误的通知。  
  
 [脚本组件中的日志记录](../../../integration-services/extending-packages-scripting/data-flow-script-component/logging-in-the-script-component.md)  
 您可以向包中启用的日志提供程序记录信息。  
  
 [开发特定类型的脚本组件](../../../integration-services/extending-packages-scripting-data-flow-script-component-types/developing-specific-types-of-script-components.md)  
 这些简单示例说明和演示如何使用脚本组件来开发数据流源、转换和目标。  
  
 [其他脚本组件示例](../../../integration-services/extending-packages-scripting-data-flow-script-component-examples/additional-script-component-examples.md)  
 这些简单示例说明和演示脚本组件的一些可能的用法。  
  
## <a name="see-also"></a>另请参阅  
 [脚本组件](../../../integration-services/data-flow/transformations/script-component.md)   
 [比较脚本任务和脚本组件](../../../integration-services/extending-packages-scripting/comparing-the-script-task-and-the-script-component.md)  
  
  
