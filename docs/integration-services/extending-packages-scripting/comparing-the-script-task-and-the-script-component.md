---
description: 比较脚本任务和脚本组件
title: 比较脚本任务和脚本组件 | Microsoft Docs
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
- Script task [Integration Services], comparing to Script component
- Script component [Integration Services], comparing to Script task
ms.assetid: 4b73753a-4239-491b-b7a6-abc63ba83d2d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 732de7f6d7c9e75d436dca721b370f63a46a4c60
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96125632"
---
# <a name="comparing-the-script-task-and-the-script-component"></a>比较脚本任务和脚本组件

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  脚本任务在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 设计器的“控制流”窗口中，脚本组件在“数据流”窗口中，它们在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包中的用途非常不同。 任务是一般用途的控制流工具，而组件用作数据流中的源、转换或目标。 尽管它们的用途不同，但是脚本任务和脚本组件在所使用的编码工具和在包中供开发人员使用的对象方面还是有一些相似之处的。 了解它们的相似与不同之处有助于您更有效地使用任务和组件。  
  
## <a name="similarities-between-the-script-task-and-the-script-component"></a>脚本任务和脚本组件之间的相似之处  
 脚本任务和脚本组件有以下相同特性。  
  
|功能|说明|  
|-------------|-----------------|  
|两个设计时模式|在任务和组件中，都是从在编辑器中指定属性开始，然后切换到开发环境编写代码。|  
|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA)|任务和组件都使用相同的 VSTA IDE，并且支持在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic 或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# 中编写代码。|  
|预编译的脚本|从 [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] 开始，所有脚本都要进行预编译。 在早期版本中，您可以指定是否对脚本进行预编译。<br /><br /> 脚本被预编译为二进制代码会使执行更加快速，但代价是会增加包大小。|  
|调试|任务和组件都支持在设计环境中进行调试时设置断点和单步执行代码。 有关详细信息，请参阅[脚本任务的编码和调试](../../integration-services/extending-packages-scripting/task/coding-and-debugging-the-script-task.md)和[脚本组件的编码和调试](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md)。|  
  
## <a name="differences-between-the-script-task-and-the-script-component"></a>脚本任务和脚本组件之间的不同之处  
 脚本任务和脚本组件有以下值得注意的不同之处。  
  
|功能|脚本任务|脚本组件|  
|-------------|-----------------|----------------------|  
|控制流/数据流|脚本任务在设计器的“控制流”选项卡中配置，在包的数据流外部运行。|脚本组件在设计器的“数据流”页中配置，表示数据流任务中的源、转换或目标。|  
|目的|脚本任务可完成几乎所有一般用途的任务。|必须指定是否要使用脚本组件创建源、转换或目标。|  
|执行|脚本任务在包工作流中的某个点运行自定义代码。 除非将其放在循环容器或事件处理程序中，否则它只运行一次。|脚本组件也运行一次，但是通常它为数据流中的每行数据运行一次主处理例程。|  
|编辑器|“脚本任务编辑器”有三个页面：“常规”、“脚本”和“表达式”。 只有 **ReadOnlyVariables**、**ReadWriteVariables** 和 **ScriptLanguage** 属性对用户可编写的代码有直接影响。|“脚本转换编辑器”最多有四个页面：“输入列”、“输入和输出”、“脚本”和“连接管理器”。 在其中每个页面上配置的元数据和属性将决定自动生成以供您在编码中使用的基类的成员。|  
|与包交互|在为脚本任务编写的代码中，使用 **Dts** 属性访问包的其他功能。 **Dts** 属性是 **ScriptMain** 类的成员。|在脚本组件代码中，使用类型化的取值函数属性访问特定包功能，如变量和连接管理器。<br /><br /> **PreExecute** 方法仅可访问只读变量。 **PostExecute** 方法既可访问只读变量，又可访问读/写变量。<br /><br /> 有关这些方法的详细信息，请参阅[脚本组件的编码和调试](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md)。|  
|使用变量|脚本任务使用 Dts 对象的 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A> 属性访问具备任务的 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask.ReadOnlyVariables%2A> 和 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask.ReadWriteVariables%2A> 属性的变量。 例如：<br /><br /> [Visual Basic]<br /><br /> `Dim myVar as String` <br /> `myVar = Dts.Variables("MyStringVariable").Value.ToString`<br /><br /> [C#]<br /><br /> `string myVar;` <br /> `myVar = Dts.Variables["MyStringVariable"].Value.ToString();`|脚本组件使用自动生成的基类的类型化取值函数属性，这些属性基于组件的 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ReadOnlyVariables%2A> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ReadWriteVariables%2A> 属性创建。 例如：<br /><br /> [Visual Basic]<br /><br /> `Dim myVar as String` <br /> `myVar = Me.Variables.MyStringVariable`<br /><br /> [C#]<br /><br /> `string myVar;` <br /> `myVar = this.Variables.MyStringVariable;`|  
|使用连接|脚本任务使用 **Dts** 对象的 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Connections%2A> 属性访问在包中定义的连接管理器。 例如：<br /><br /> [Visual Basic]<br /><br /> `Dim myFlatFileConnection As String` <br /> `myFlatFileConnection = _     DirectCast(Dts.Connections("Test Flat File Connection").AcquireConnection(Dts.Transaction), _     String)`<br /><br /> [C#]<br /><br /> `string myFlatFileConnection;` <br /> `myFlatFileConnection = (Dts.Connections["Test Flat File Connection"].AcquireConnection(Dts.Transaction) as String);`|脚本组件使用自动生成的基类的类型化取值函数属性，这些属性基于用户在编辑器的“连接管理器”页上输入的连接管理器列表创建。 例如：<br /><br /> [Visual Basic]<br /><br /> `Dim connMgr As IDTSConnectionManager100` <br /> `connMgr = Me.Connections.MyADONETConnection`<br /><br /> [C#]<br /><br /> `IDTSConnectionManager100 connMgr;` <br /> `connMgr = this.Connections.MyADONETConnection;`|  
|引发事件|脚本任务使用 **Dts** 对象的 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A> 属性引发事件。 例如：<br /><br /> [Visual Basic]<br /><br /> `Dts.Events.FireError(0, "Event Snippet", _     ex.Message & ControlChars.CrLf & ex.StackTrace, _     "", 0)`<br /><br /> [C#]<br /><br /> `Dts.Events.FireError(0, "Event Snippet", ex.Message + "\r" + ex.StackTrace, "", 0);`|脚本组件使用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> 属性返回的 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ComponentMetaData%2A> 接口的方法引发错误、警告和信息性消息。 例如：<br /><br /> [Visual Basic]<br /><br /> `Dim myMetadata as IDTSComponentMetaData100 myMetaData = Me.ComponentMetaData myMetaData.FireError(...)`|  
|Logging|脚本任务使用 **Dts** 对象的 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A> 方法将信息记录到已启用的日志提供程序。 例如：<br /><br /> [Visual Basic]<br /><br /> `Dim bt(0) As Byte Dts.Log("Test Log Event", _     0, _     bt)`<br /><br /> [C#]<br /><br /> `byte[] bt = new byte[0];` <br /> `Dts.Log("Test Log Event", 0, bt);`|脚本组件使用自动生成的基类的 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.Log%2A> 方法将信息记录到已启用的日志提供程序。 例如：<br /><br /> [Visual Basic]<br /><br /> `Dim bt(0) As Byte`<br /><br /> `Me.Log("Test Log Event", _`<br /><br /> `0, _`<br /><br /> `bt)`<br /><br /> [C#]<br /><br /> `byte[] bt = new byte[0]; this.Log("Test Log Event", 0, bt);`|  
|返回结果|脚本任务使用 **Dts** 对象的 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.TaskResult%2A> 属性和可选的 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.ExecutionValue%2A> 属性通知运行时其结果。|脚本组件作为数据流任务的组成部分运行，不会使用这两个属性中的任何一个报告结果。|  
  
## <a name="see-also"></a>另请参阅  
 [使用脚本任务扩展包](../../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md)   
 [使用脚本组件扩展数据流](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md)   
  
  
