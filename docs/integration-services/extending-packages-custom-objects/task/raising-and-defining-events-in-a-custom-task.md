---
description: 在自定义任务中引发和定义事件
title: 在自定义任务中引发和定义事件 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- SSIS events, custom
- status information [SQL Server], task events
- custom tasks [Integration Services], events
- SSIS custom tasks, events
- IDTSComponentEvents interface
- events [Integration Services], custom
- events [Integration Services], runtime
- custom events [Integration Services]
- SSIS events, runtime
- IDTSEvents interface
ms.assetid: e0898aa1-e90c-4c4e-99d4-708a76efddfd
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1fcafce23542f1d5d1ea1756cdb361d5197286b6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091587"
---
# <a name="raising-and-defining-events-in-a-custom-task"></a>在自定义任务中引发和定义事件

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 运行时引擎提供了一组事件，这些事件提供验证和执行任务时任务进度的状态。 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents> 接口用于定义这些事件，并且该接口可作为 <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Validate%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Execute%2A> 方法的参数提供给任务。  
  
 此外，还有另一组在 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSEvents> 接口中定义的事件，<xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> 可代表任务引发这些事件。 <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> 将引发在验证和执行之前或之后发生的事件，而任务则引发在执行和验证期间发生的事件。  
  
## <a name="creating-custom-events"></a>创建自定义事件  
 自定义任务开发人员可通过在其 <xref:Microsoft.SqlServer.Dts.Runtime.EventInfo> 方法的重写实现中创建新的 <xref:Microsoft.SqlServer.Dts.Runtime.Task.InitializeTask%2A> 来定义新的自定义事件。 创建 <xref:Microsoft.SqlServer.Dts.Runtime.EventInfo> 后，可使用 <xref:Microsoft.SqlServer.Dts.Runtime.EventInfos.Add%2A> 方法将其添加到 **EventInfos** 集合。 <xref:Microsoft.SqlServer.Dts.Runtime.EventInfos.Add%2A> 方法的方法签名如下所示：  
  
 `public void Add(string eventName, string description, bool allowEventHandlers, string[] parameterNames, TypeCode[] parameterTypes, string[] parameterDescriptions);`  
  
 下面的代码示例介绍了自定义任务的 **InitializeTask** 方法，在该自定义任务中，创建了两个自定义事件并设置了其属性。 然后，将新事件添加到 <xref:Microsoft.SqlServer.Dts.Runtime.EventInfos> 集合。  
  
 第一个自定义事件的事件名为 “**OnBeforeIncrement**”，说明为“**更新初始值后触发**”。 下一个参数为 **true** 值，指示此事件应允许创建用于处理事件的事件处理程序容器。 事件处理程序是一个可向任务提供包和服务中的结构的容器，像其他诸如包、序列、ForLoop 和 ForEachLoop 的其他容器一样。 当 *allowEventHandlers* 参数为 **true** 时，会为该事件创建 <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> 对象。 为事件定义的所有参数现在均可在 <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> 的变量集合中用于 <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>。  
  
```csharp  
public override void InitializeTask(Connections connections,  
   VariableDispenser variables, IDTSInfoEvents events,  
   IDTSLogging log, EventInfos eventInfos,  
   LogEntryInfos logEntryInfos, ObjectReferenceTracker refTracker)  
{  
    this.eventInfos = eventInfos;  
    string[] paramNames = new string[1];  
    TypeCode[] paramTypes = new TypeCode[1]{TypeCode.Int32};  
    string[] paramDescriptions = new string[1];  
  
    paramNames[0] = "InitialValue";  
    paramDescriptions[0] = "The value before it is incremented.";  
  
    this.eventInfos.Add("OnBeforeIncrement",   
      "Fires before the task increments the value.",  
      true,paramNames,paramTypes,paramDescriptions);  
    this.onBeforeIncrement = this.eventInfos["OnBeforeIncrement"];  
  
    paramDescriptions[0] = "The value after it has been incremented.";  
    this.eventInfos.Add("OnAfterIncrement",  
      "Fires after the initial value is updated.",  
      true,paramNames, paramTypes,paramDescriptions);  
    this.onAfterIncrement = this.eventInfos["OnAfterIncrement"];  
}  
```  
  
```vb  
Public Overrides Sub InitializeTask(ByVal connections As Connections, _  
ByVal variables As VariableDispenser, ByVal events As IDTSInfoEvents, _  
ByVal log As IDTSLogging, ByVal eventInfos As EventInfos, _  
ByVal logEntryInfos As LogEntryInfos, ByVal refTracker As ObjectReferenceTracker)   
  
    Dim paramNames(0) As String  
    Dim paramTypes(0) As TypeCode = {TypeCode.Int32}  
    Dim paramDescriptions(0) As String  
  
    Me.eventInfos = eventInfos  
  
    paramNames(0) = "InitialValue"  
    paramDescriptions(0) = "The value before it is incremented."  
  
    Me.eventInfos.Add("OnBeforeIncrement", _  
      "Fires before the task increments the value.", _  
      True, paramNames, paramTypes, paramDescriptions)  
    Me.onBeforeIncrement = Me.eventInfos("OnBeforeIncrement")  
  
    paramDescriptions(0) = "The value after it has been incremented."  
    Me.eventInfos.Add("OnAfterIncrement", _  
      "Fires after the initial value is updated.", True, _  
      paramNames, paramTypes, paramDescriptions)  
    Me.onAfterIncrement = Me.eventInfos("OnAfterIncrement")  
  
End Sub  
```  
  
## <a name="raising-custom-events"></a>引发自定义事件  
 通过调用 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireCustomEvent%2A> 方法引发自定义事件。 下面的代码行引发了一个自定义事件。  
  
```csharp  
componentEvents.FireCustomEvent(this.onBeforeIncrement.Name,  
   this.onBeforeIncrement.Description, ref arguments,  
   null, ref bFireOnBeforeIncrement);  
```  
  
```vb  
componentEvents.FireCustomEvent(Me.onBeforeIncrement.Name, _  
Me.onBeforeIncrement.Description, arguments, _  
Nothing,  bFireOnBeforeIncrement)  
```  
  
## <a name="sample"></a>示例  
 下面的示例演示了一个任务，该任务在 **InitializeTask** 方法中定义一个自定义事件，再将该自定义事件添加到 <xref:Microsoft.SqlServer.Dts.Runtime.EventInfos> 集合，然后通过调用 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireCustomEvent%2A> 方法在其 **Execute** 方法中引发该自定义事件。  
  
```csharp  
[DtsTask(DisplayName = "CustomEventTask")]  
    public class CustomEventTask : Task  
    {  
        public override DTSExecResult Execute(Connections connections,   
          VariableDispenser variableDispenser, IDTSComponentEvents componentEvents,  
           IDTSLogging log, object transaction)  
        {  
            bool fireAgain;  
            object[] args = new object[1] { "The value of the parameter." };  
            componentEvents.FireCustomEvent( "MyCustomEvent",   
              "Firing the custom event.", ref args,  
              "CustomEventTask" , ref fireAgain );  
            return DTSExecResult.Success;  
        }  
  
        public override void InitializeTask(Connections connections,  
          VariableDispenser variableDispenser, IDTSInfoEvents events,  
          IDTSLogging log, EventInfos eventInfos,  
          LogEntryInfos logEntryInfos, ObjectReferenceTracker refTracker)  
        {  
            string[] names = new string[1] {"Parameter1"};  
            TypeCode[] types = new TypeCode[1] {TypeCode.String};  
            string[] descriptions = new string[1] {"Parameter description." };  
  
            eventInfos.Add("MyCustomEvent",  
             "Fires when my interesting event happens.",  
             true, names, types, descriptions);  
  
        }  
   }  
```  
  
```vb  
<DtsTask(DisplayName = "CustomEventTask")> _   
    Public Class CustomEventTask  
     Inherits Task  
        Public Overrides Function Execute(ByVal connections As Connections, _  
          ByVal variableDispenser As VariableDispenser, _  
          ByVal componentEvents As IDTSComponentEvents, _  
          ByVal log As IDTSLogging, ByVal transaction As Object) _  
          As DTSExecResult  
  
            Dim fireAgain As Boolean  
            Dim args() As Object =  New Object(1) {"The value of the parameter."}  
  
            componentEvents.FireCustomEvent("MyCustomEvent", _  
              "Firing the custom event.", args, _  
              "CustomEventTask" ,  fireAgain)  
            Return DTSExecResult.Success  
        End Function  
  
        Public Overrides  Sub InitializeTask(ByVal connections As Connections, _  
          ByVal variableDispenser As VariableDispenser,  
          ByVal events As IDTSInfoEvents,  
          ByVal log As IDTSLogging, ByVal eventInfos As EventInfos, ByVal logEnTryInfos As LogEnTryInfos, ByVal refTracker As ObjectReferenceTracker)  
  
            Dim names() As String =  New String(1) {"Parameter1"}  
            Dim types() As TypeCode =  New TypeCode(1) {TypeCode.String}  
            Dim descriptions() As String =  New String(1) {"Parameter description."}  
  
            eventInfos.Add("MyCustomEvent", _  
              "Fires when my interesting event happens.", _  
              True, names, types, descriptions)  
  
        End Sub  
  
    End Class  
```  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services (SSIS) 事件处理程序](../../../integration-services/integration-services-ssis-event-handlers.md)   
 [在包中添加事件处理程序](../../integration-services-ssis-event-handlers.md)  
  
