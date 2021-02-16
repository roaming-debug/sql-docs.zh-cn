---
description: 在数据流组件中引发和定义事件
title: 在数据流组件中引发和定义事件 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- data flow components [Integration Services], events
- events [Integration Services], custom
- custom events [Integration Services]
- custom data flow components [Integration Services], events
- events [Integration Services], raising
- predefined events [Integration Services]
ms.assetid: 1d8c5358-9384-47a8-b7cb-7b0650384119
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1d6e0a5d7694323f22a9262df37353f8117b9666
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343666"
---
# <a name="raising-and-defining-events-in-a-data-flow-component"></a>在数据流组件中引发和定义事件

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  组件开发人员可通过调用对 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents> 属性公开的方法，引发在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> 接口中定义的一部分事件。 还可以使用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.EventInfos%2A> 集合定义自定义事件，并使用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A> 方法在执行过程中引发这些事件。 本节介绍如何创建和引发事件，并提供在设计时应于何时引发事件的指南。  
  
## <a name="raising-events"></a>引发事件  
 组件使用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> 接口的 Fire\<X> 方法引发事件。 您可以在组件设计和执行的过程中引发事件。 通常，在组件设计过程中，验证期间会调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireError%2A> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireWarning%2A> 方法。 这些事件在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] 的“错误列表”窗格中显示消息，并在某个组件配置不正确时，向该组件的用户提供反馈。  
  
 组件也可以在执行过程中的任一点引发事件。 组件开发人员可通过事件在组件执行时向该组件的用户提供反馈。 在执行过程中调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireError%2A> 方法可能导致包失败。  
  
## <a name="defining-and-raising-custom-events"></a>定义和引发自定义事件  
  
### <a name="defining-a-custom-event"></a>定义自定义事件  
 调用 <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.IDTSEventInfos100.Add%2A> 集合的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.EventInfos%2A> 方法可创建自定义事件。 此集合由数据流任务设置，并作为属性通过 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> 基类提供给组件开发人员。 此类包含调用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.RegisterEvents%2A> 方法的过程中由数据流任务定义的自定义事件和组件定义的自定义事件。  
  
 组件的自定义事件不会持久保留在包 XML 中。 因此，在设计和执行过程中都调用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.RegisterEvents%2A> 方法，以允许组件定义所引发的事件。  
  
 <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.IDTSEventInfos100.Add%2A> 方法的 allowEventHandlers 参数指定组件是否允许为事件创建 <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> 对象。 请注意，<xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandlers> 是同步的。 因此，直到附加到自定义事件的 <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> 执行完毕后，组件才会继续执行。 如果 allowEventHandlers 参数为真，则任何 *对象都可以通过由*  **运行时自动创建和填充的变量使用事件的每个参数**<xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]。  
  
### <a name="raising-a-custom-event"></a>引发自定义事件  
 通过调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A> 方法并提供事件的名称、文本和参数，组件可引发自定义事件。 如果 allowEventHandlers 参数为 true，则为自定义事件创建的所有 <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandlers> 都由 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 运行时引擎执行。  
  
### <a name="custom-event-sample"></a>自定义事件示例  
 下面的代码示例演示了在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.RegisterEvents%2A> 方法中定义自定义事件，然后通过调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A> 方法在运行时引发该事件的组件。  
  
```csharp  
public override void RegisterEvents()  
{  
    string [] parameterNames = new string[2]{"RowCount", "StartTime"};  
    ushort [] parameterTypes = new ushort[2]{ DtsConvert.VarTypeFromTypeCode(TypeCode.Int32), DtsConvert.VarTypeFromTypeCode(TypeCode.DateTime)};  
    string [] parameterDescriptions = new string[2]{"The number of rows to sort.", "The start time of the Sort operation."};  
    EventInfos.Add("StartingSort","Fires when the component begins sorting the rows.",false,ref parameterNames, ref parameterTypes, ref parameterDescriptions);  
}  
public override void ProcessInput(int inputID, PipelineBuffer buffer)  
{  
    while (buffer.NextRow())  
    {  
       // Process buffer rows.  
    }  
  
    IDTSEventInfo100 eventInfo = EventInfos["StartingSort"];  
    object []arguments = new object[2]{buffer.RowCount, DateTime.Now };  
    ComponentMetaData.FireCustomEvent("StartingSort", "Beginning sort operation.", ref arguments, ComponentMetaData.Name, ref FireSortEventAgain);  
}  
```  
  
```vb  
Public  Overrides Sub RegisterEvents()   
  Dim parameterNames As String() = New String(2) {"RowCount", "StartTime"}   
  Dim parameterTypes As System.UInt16() = New System.UInt16(2) {DtsConvert.VarTypeFromTypeCode(TypeCode.Int32), DtsConvert.VarTypeFromTypeCode(TypeCode.DateTime)}   
  Dim parameterDescriptions As String() = New String(2) {"The number of rows to sort.", "The start time of the Sort operation."}   
  EventInfos.Add("StartingSort", "Fires when the component begins sorting the rows.", False, parameterNames, parameterTypes, parameterDescriptions)   
End Sub   
  
Public  Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)   
  While buffer.NextRow   
  End While   
  Dim eventInfo As IDTSEventInfo100 = EventInfos("StartingSort")   
  Dim arguments As Object() = New Object(2) {buffer.RowCount, DateTime.Now}   
  ComponentMetaData.FireCustomEvent("StartingSort", _  
    "Beginning sort operation.", arguments, _  
    ComponentMetaData.Name, FireSortEventAgain)   
End Sub  
```  

## <a name="see-also"></a>另请参阅  
 [Integration Services (SSIS) 事件处理程序](../../../integration-services/integration-services-ssis-event-handlers.md)   
 [在包中添加事件处理程序](../../integration-services-ssis-event-handlers.md)  
  
