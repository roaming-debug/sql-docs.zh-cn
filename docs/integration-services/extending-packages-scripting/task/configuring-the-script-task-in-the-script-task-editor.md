---
description: 在脚本任务编辑器中配置脚本任务
title: 在脚本任务编辑器中配置脚本任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Script task [Integration Services], configuring
- Script Task Editor
- SSIS Script task, configuring
ms.assetid: 232de0c9-b24d-4c38-861d-6c1f4a75bdf3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8b4a052ea59e3c72adda887c3084bca278059f0b
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122930"
---
# <a name="configuring-the-script-task-in-the-script-task-editor"></a>在脚本任务编辑器中配置脚本任务

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  在脚本任务中编写自定义代码之前，请在“脚本任务编辑器”的三个页面中配置它的主要属性。 使用“属性”窗口可以配置非脚本任务特有的其他任务属性。  
  
> [!NOTE]  
>  在早期版本中，您可以指示是否对脚本进行预编译，而从 [!INCLUDE[ssISversion10](../../../includes/ssisversion10-md.md)] 开始，不同的是，所有脚本都要预编译。  
  
## <a name="general-page-of-the-script-task-editor"></a>脚本任务编辑器的“常规”页  
 在“脚本任务编辑器”的“常规”页中，可以为脚本任务分配唯一的名称和说明。  
  
## <a name="script-page-of-the-script-task-editor"></a>脚本任务编辑器的“脚本”页  
 “脚本任务编辑器”的“脚本”页显示脚本任务的自定义属性。  
  
### <a name="scriptlanguage-property"></a>ScriptLanguage 属性  
 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA) 支持 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic 或 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C# 编程语言。 在脚本任务中创建脚本后，不能更改“ScriptLanguage”属性的值。  
  
 若要设置脚本任务和脚本组件的默认脚本语言，请使用“选项”对话框的“常规”页上的“ScriptLanguage”属性。 有关详细信息，请参阅 [General Page](../../general-page-of-integration-services-designers-options.md)。  
  
### <a name="entrypoint-property"></a>EntryPoint 属性  
 EntryPoint 属性对 VSTA 项目中的 ScriptMain 类指定一种方法，[!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 运行时会将该方法作为脚本任务代码的入口点来调用。 ScriptMain 类是脚本模板生成的默认类。  
  
 若要更改 VSTA 项目中方法的名称，则必须更改 **EntryPoint** 属性的值。  
  
### <a name="readonlyvariables-and-readwritevariables-properties"></a>ReadOnlyVariables 和 ReadWriteVariables 属性  
 可以输入以逗号分隔的现有变量列表作为这些属性的值，使这些变量在脚本任务代码中可用于只读或读/写访问。 这两种类型的变量都可以在代码中通过 Dts 对象的 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A> 属性来访问。 有关详细信息，请参阅 [Using Variables in the Script Task](../../../integration-services/extending-packages-scripting/task/using-variables-in-the-script-task.md)。  
  
> [!NOTE]  
>  变量名称区分大小写。  
  
 若要选择变量，请单击属性字段旁边的省略号 (…) 按钮。 有关详细信息，请参阅[“选择变量”页](../../../integration-services/control-flow/select-variables-page.md)。  
  
### <a name="edit-script-button"></a>“编辑脚本”按钮  
 “编辑脚本”按钮用于启动 VSTA 开发环境，可以在其中编写自定义脚本。 有关详细信息，请参阅[脚本任务的编码和调试](../../../integration-services/extending-packages-scripting/task/coding-and-debugging-the-script-task.md)。  
  
## <a name="expressions-page-of-the-script-task-editor"></a>脚本任务编辑器的“表达式”页  
 在“脚本任务编辑器”的“表达式”页中，可以使用表达式为上面列出的脚本任务属性和其他许多任务属性提供值。 有关详细信息，请参阅 [Integration Services (SSIS) 表达式](../../../integration-services/expressions/integration-services-ssis-expressions.md)。  
  
## <a name="see-also"></a>另请参阅  
 [脚本任务的编码和调试](../../../integration-services/extending-packages-scripting/task/coding-and-debugging-the-script-task.md)  
  
  
