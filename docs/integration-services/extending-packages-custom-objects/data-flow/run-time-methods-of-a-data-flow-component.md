---
description: 数据流组件的运行时方法
title: 数据流组件的运行时方法 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- run-time [Integration Services]
- data flow components [Integration Services], run-time methods
ms.assetid: fd9e4317-18dd-43af-bbdc-79db32183ac4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8fa0106c903a509eb2b4f734b667323b6d71fd74
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343632"
---
# <a name="run-time-methods-of-a-data-flow-component"></a>数据流组件的运行时方法

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  在运行时，数据流任务将检查一系列组件、准备执行计划以及管理执行工作计划的工作线程池。 任务先从源加载数据行，再通过转换处理这些行，然后将它们保存到目标。  
  
## <a name="sequence-of-method-execution"></a>方法执行顺序  
 在数据流组件的执行期间，将调用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> 基类中的方法的子集。 除 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法外，所调用的方法及其调用顺序始终相同。 这两个方法是基于组件的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> 对象的存在性及其配置调用的。  
  
 下面的列表以这些方法在组件执行期间的调用顺序来显示它们。 请注意，如果调用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>，则始终在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 之前调用。  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrepareForExecute%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PostExecute%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Cleanup%2A>  
  
### <a name="primeoutput-method"></a>PrimeOutput 方法  
 当组件具有至少一个通过 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.PrimeOutput%2A> 对象附加到下游组件的输出，且该输出的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100> 属性为零时，即调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> 方法。 针对源组件和具有异步输出的转换调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.PrimeOutput%2A> 方法。 与下面介绍的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法不同，<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 方法仅针对每个需要它的组件调用一次。  
  
### <a name="processinput-method"></a>ProcessInput 方法  
 针对至少具有一个由 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.ProcessInput%2A> 对象附加到上游组件的输入的组件调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100> 方法。 针对目标组件和具有同步输出的转换调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.ProcessInput%2A> 方法。 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 可重复调用，直到没有要处理的来自上游组件的行为止。  
  
## <a name="working-with-inputs-and-outputs"></a>使用输入和输出  
 在运行时，数据流组件执行以下任务：  
  
-   源组件添加行。  
  
-   具有同步输出的转换组件接收源组件提供的行。  
  
-   具有异步输出的转换组件接收行和添加行。  
  
-   目标组件接收行，然后将接收到的行加载到目标。  
  
 在执行期间，数据流任务将分配 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> 对象，这些对象包含在一系列组件的输出列集合中定义的所有列。 例如，如果数据流序列中的四个组件都向其输出列集合中添加一个输出列，则提供给每个组件的缓冲区中将包含四个列，分别对应每个组件的输出列。 受此行为影响，组件有时将接收包含不使用的列的缓冲区。  
  
 由于组件接收的缓冲区可能包含该组件不使用的列，因此必须在数据流提供给组件的缓冲区中查找将在组件的输入和输出列集合中使用的列。 可使用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> 属性的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> 方法实现此目的。 出于性能方面的考虑，此任务通常在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> 方法期间执行，而不在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 或 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 中执行。  
  
 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> 在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法之前调用，这是在组件可使用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> 之后第一个组件可执行列查找任务的机会。 在此方法期间，组件应在缓冲区查找列，并内部存储此信息，以便在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 或 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法中使用这些列。  
  
 下面的代码示例演示具有同步输出的转换组件如何在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> 期间在缓冲区查找其输出列。  
  
```csharp  
private int []bufferColumnIndex;  
public override void PreExecute()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
    bufferColumnIndex = new int[input.InputColumnCollection.Count];  
  
    for( int x=0; x < input.InputColumnCollection.Count; x++)  
    {  
        IDTSInputColumn100 column = input.InputColumnCollection[x];  
        bufferColumnIndex[x] = BufferManager.FindColumnByLineageID( input.Buffer, column.LineageID);  
    }  
}  
```  
  
```vb  
Dim bufferColumnIndex As Integer()  
  
    Public Overrides Sub PreExecute()  
  
        Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
  
        ReDim bufferColumnIndex(input.InputColumnCollection.Count)  
  
        For x As Integer = 0 To input.InputColumnCollection.Count  
  
            Dim column As IDTSInputColumn100 = input.InputColumnCollection(x)  
            bufferColumnIndex(x) = BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID)  
  
        Next  
  
    End Sub  
```  
  
### <a name="adding-rows"></a>添加行  
 组件通过向 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> 对象添加行来向下游组件提供行。 数据流任务将一组输出缓冲区作为参数提供给 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> 方法，其中一个缓冲区对应一个连接到下游组件的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 对象。 源组件和具有异步输出的转换组件向缓冲区中添加行，并在完成行添加后调用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetEndOfRowset%2A> 方法。 数据流任务将管理其提供给组件的输出缓冲区，并在缓冲区写满之后自动将缓冲区中的行移动至下一个组件。 与可重复调用的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 方法不同，<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法仅为每个组件调用一次。  
  
 下面的代码示例演示组件如何在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 方法期间向其输出缓冲区添加行，然后调用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetEndOfRowset%2A> 方法。  
  
```csharp  
public override void PrimeOutput( int outputs, int []outputIDs,PipelineBuffer []buffers)  
{  
    for( int x=0; x < outputs; x++ )  
    {  
        IDTSOutput100 output = ComponentMetaData.OutputCollection.GetObjectByID( outputIDs[x]);  
        PipelineBuffer buffer = buffers[x];  
  
        // TODO: Add rows to the output buffer.  
    }  
    foreach( PipelineBuffer buffer in buffers )  
    {  
        /// Notify the data flow task that no more rows are coming.  
        buffer.SetEndOfRowset();  
    }  
}  
```  
  
```vb  
public overrides sub PrimeOutput( outputs as Integer , outputIDs() as Integer ,buffers() as PipelineBuffer buffers)  
  
    For x As Integer = 0 To outputs.MaxValue  
  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.GetObjectByID(outputIDs(x))  
        Dim buffer As PipelineBuffer = buffers(x)  
  
        ' TODO: Add rows to the output buffer.  
  
    Next  
  
    For Each buffer As PipelineBuffer In buffers  
  
        ' Notify the data flow task that no more rows are coming.  
        buffer.SetEndOfRowset()  
  
    Next  
  
End Sub  
```  
  
 有关开发用于向输出缓冲区添加行的组件的详细信息，请参阅[开发自定义源组件](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md)和[开发具有异步输出的自定义转换组件](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md)。  
  
### <a name="receiving-rows"></a>接收行  
 组件在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> 对象中接收来自上游组件的行。 数据流任务将 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> 对象作为参数提供给 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法，该对象包含上游组件添加到数据流的行。 此输出缓冲区可用于检查和修改缓冲区中的行和列，但不能用于添加或删除行。 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法可重复调用，直到没有可用的缓冲区为止。 最后一次调用时，<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A> 属性为 **true**。 可使用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.NextRow%2A> 方法（可将缓冲区移至下一行）循环访问缓冲区中的行集合。 当缓冲区位于集合中的最后一行时，此方法返回 **false**。 您不必检查 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A> 属性，除非在处理最后一行数据后需要执行其他操作。  
  
 下面的文本显示了 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.NextRow%2A> 方法和 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A> 属性的正确用法：  
  
 `while (buffer.NextRow())`  
  
 `{`  
  
 `// Do something with each row.`  
  
 `}`  
  
 `if (buffer.EndOfRowset)`  
  
 `{`  
  
 `// Optionally, do something after all rows have been processed.`  
  
 `}`  
  
 下面的代码示例演示组件如何在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法期间处理输入缓冲区中的行。  
  
```csharp  
public override void ProcessInput( int inputID, PipelineBuffer buffer )  
{  
    {  
        IDTSInput100 input = ComponentMetaData.InputCollection.GetObjectByID(inputID);  
        while( buffer.NextRow())  
        {  
            // TODO: Examine the columns in the current row.  
        }  
}  
```  
  
```vb  
Public Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)  
  
        Dim input As IDTSInput100 = ComponentMetaData.InputCollection.GetObjectByID(inputID)  
        While buffer.NextRow() = True  
  
            ' TODO: Examine the columns in the current row.  
        End While  
  
End Sub  
```  
  
 有关开发用于接收输入缓冲区中的行的组件的详细信息，请参阅[开发自定义目标组件](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-destination-component.md)和[开发具有同步输出的自定义转换组件](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)。  
  
## <a name="see-also"></a>另请参阅  
 [数据流组件的设计时方法](../../../integration-services/extending-packages-custom-objects/data-flow/design-time-methods-of-a-data-flow-component.md)  
  
  
