---
description: 开发具有异步输出的自定义转换组件
title: 开发具有异步输出的自定义转换组件 | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom data flow components [Integration Services], transformation components
- asynchronous outputs [Integration Services]
- ProcessInput method
- cache [Integration Services]
- buffer allocations [Integration Services]
- transformation components [Integration Services]
- output columns [Integration Services]
- PrimeOutput method
- data flow components [Integration Services], transformation components
ms.assetid: 1c3e92c7-a4fa-4fdd-b9ca-ac3069536274
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d6eeddfc5c4fff29b27462889d39748c8df2a01e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348249"
---
# <a name="developing-a-custom-transformation-component-with-asynchronous-outputs"></a>开发具有异步输出的自定义转换组件

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  如果某个转换直到组件收到其所有输入行后才输出行，或者该转换不是为收到的每个输入行生成一个输出行，则可以使用具有异步输出的组件。 例如，聚合转换只有在它读取所有行之后才能计算各行的总和。 与之相反，如果可以在每个数据行传递给组件时就修改该行，则可以使用具有同步输出的组件。 您可以就地修改每行的数据，或者创建一个或多个新列，其中每一列的值与每个输入行对应。 有关同步组件和异步组件之间的差异的详细信息，请参阅[了解同步和异步转换](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)。  
  
 具有异步输出的转换组件非常独特，因为它们既充当目标组件又充当源组件。 此类组件从上游组件接收行，然后添加下游组件所使用的行。 其他任何数据流组件都不同时执行这两个操作。  
  
 对于具有同步输出的组件，如果来自其上游组件的列可用于该组件，则对于该组件下游的组件，这些列是自动可用的。 因此，具有同步输出的组件不一定要定义输出列，以便为下一组件提供列和行。 而具有异步输出的组件则必须定义输出列并向下游组件提供行。 因此，具有异步输出的组件在设计时和执行时要执行更多的任务，而组件开发人员要实现更多的代码。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包含多个具有异步输出的转换。 例如，排序转换需要收到所有行才能对这些行进行排序，这是通过异步输出完成的。 该转换收到所有行之后，对这些行进行排序，然后将这些行添加到其输出。  
  
 本节详细说明如何开发具有异步输出的转换。 有关源组件开发的详细信息，请参阅[开发自定义源组件](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md)。  
  
## <a name="design-time"></a>设计时  
  
### <a name="creating-the-component"></a>创建组件  
 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> 对象的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> 属性标识输出是同步的还是异步的。 若要创建异步输出，请将该输出添加到组件，然后将 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> 设置为零。 设置此属性还可以确定数据流任务是为组件的输入和输出都分配 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> 对象，还是只分配一个缓冲区由两个对象共享。  
  
 下面的示例代码演示在其 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> 实现中创建异步输出的组件。  
  
```csharp  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "AsyncComponent",ComponentType = ComponentType.Transform)]  
    public class AsyncComponent : PipelineComponent  
    {  
        public override void ProvideComponentProperties()  
        {  
            // Call the base class, which adds a synchronous input  
            // and output.  
            base.ProvideComponentProperties();  
  
            // Make the output asynchronous.  
            IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
            output.SynchronousInputID = 0;  
        }  
    }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsPipelineComponent(DisplayName:="AsyncComponent", ComponentType:=ComponentType.Transform)> _  
Public Class AsyncComponent  
    Inherits PipelineComponent  
  
    Public Overrides Sub ProvideComponentProperties()  
  
        ' Call the base class, which adds a synchronous input  
        ' and output.  
        Me.ProvideComponentProperties()  
  
        ' Make the output asynchronous.  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
        output.SynchronousInputID = 0  
  
    End Sub  
  
End Class  
```  
  
### <a name="creating-and-configuring-output-columns"></a>创建和配置输出列  
 如上所述，异步组件向其输出列集合添加列来为下游组件提供列。 有几种设计时方法可供选择，具体方法取决于组件的需求。 例如，如果要将来自上游组件的所有列都传递给下游组件，则需要重写 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.OnInputPathAttached%2A> 方法来添加列，因为这是第一个使输入列可用于组件的方法。  
  
 如果组件基于为其输入选择的列来创建输出列，请重写 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.SetUsageType%2A> 方法来选择输出列并指示如何使用它们。  
  
 如果具有异步输出的组件基于来自上游组件的列创建输出列，并且可用的上游列发生变化，则该组件应更新其输出列集合。 这些更改应该由组件在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> 中检测，并在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A> 中修复。  
  
> [!NOTE]  
>  从输出列集合中删除一个输出列时，数据流中引用该列的下游组件会受到不利影响。 必须修复而不是先删除再重新创建该输出列，以防止下游组件断开。 例如，如果该列的数据类型发生变化，必须更新数据类型。  
  
 下面的代码示例演示了一个组件，它对来自上游组件的每个可用列都向其输出列集合添加一个输出列。  
  
```csharp  
public override void OnInputPathAttached(int inputID)  
{  
   IDTSInput100 input = ComponentMetaData.InputCollection.GetObjectByID(inputID);  
   IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
   IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
   foreach (IDTSVirtualInputColumn100 vCol in vInput.VirtualInputColumnCollection)  
   {  
      IDTSOutputColumn100 outCol = output.OutputColumnCollection.New();  
      outCol.Name = vCol.Name;  
      outCol.SetDataTypeProperties(vCol.DataType, vCol.Length, vCol.Precision, vCol.Scale, vCol.CodePage);  
   }  
}  
```  
  
```vb  
Public Overrides Sub OnInputPathAttached(ByVal inputID As Integer)  
  
    Dim input As IDTSInput100 = ComponentMetaData.InputCollection.GetObjectByID(inputID)  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
    Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput()  
  
    For Each vCol As IDTSVirtualInputColumn100 In vInput.VirtualInputColumnCollection  
  
        Dim outCol As IDTSOutputColumn100 = output.OutputColumnCollection.New()  
        outCol.Name = vCol.Name  
        outCol.SetDataTypeProperties(vCol.DataType, vCol.Length, vCol.Precision, vCol.Scale, vCol.CodePage)  
  
    Next  
End Sub  
```  
  
## <a name="run-time"></a>运行时  
 具有异步输出的组件在运行时执行的一系列方法也与其他类型的组件不同。 首先，只有这种组件既接受对 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 方法的调用又接受对 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法的调用。 其次，具有异步输出的组件还需要在开始处理前访问所有传入行；因此，这些组件必须在内部缓存输入行直到读取所有行为止。 最后，与其他组件不同，具有异步输出的组件既接收输入缓冲区又接收输出缓冲区。  
  
### <a name="understanding-the-buffers"></a>了解缓冲区  
 组件在调用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 期间接收输入缓冲区。 此缓冲区包含由上游组件添加到该缓冲区中的行。 该缓冲区还包含组件的输入列以及上游组件输出中提供的但是没有添加到异步组件输入集合中的列。  
  
 输出缓冲区在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 中提供给组件，该缓冲区最初不包含行。 组件会向此缓冲区添加行，并在其写满时将其提供给下游组件。 输出缓冲区包含在组件输出列集合中定义的列，还包含其他下游组件添加到其输出中的所有列。  
  
 此行为与具有同步输出的组件不同，后者只接收一个共享缓冲区。 具有同步输出的组件的共享缓冲区包含该组件的输入列和输出列，还包含添加到上游和下游组件输出中的列。  
  
### <a name="processing-rows"></a>处理行  
  
#### <a name="caching-input-rows"></a>缓存输入行  
 编写具有异步输出的组件时，有三种方法可向输出缓冲区添加行。 您可以在接收输入行时添加它们，可以在接收到来自上游组件的所有行之后缓存它们，也可以在适合组件之时添加它们。 所选择的方法取决于组件的需要。 例如，排序组件需要在接收到所有上游行之后才能对其进行排序。 因此，它会等到读取所有行之后再向输出缓冲区添加行。  
  
 在输入缓冲区中接收到的行必须由组件在内部缓存，直到它可以处理这些行为止。 传入缓冲区行可缓存在数据表、多维数组或任何其他内部结构中。  
  
#### <a name="adding-output-rows"></a>添加输出行  
 无论是在接收行的同时将行添加到输出缓冲区中还是在接收所有行之后再将其添加到输出缓冲区中，您都可以通过对输出缓冲区调用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddRow%2A> 方法来实现。 添加行之后，您可以在新行中设置每列的值。  
  
 由于有时输出缓冲区中的列多于组件输出列集合中的列，因此您必须先找到相应列在缓冲区中的索引，然后才能设置其值。 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> 属性的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> 方法返回缓冲区行中具有指定沿袭 ID 的列的索引，该索引随后用于给该缓冲区列赋值。  
  
 在调用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> 方法或 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> 方法之前调用的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法是可使用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> 属性的第一个方法，并且是可以在输入和输出缓冲区中查找列索引的第一个机会。  
  
## <a name="sample"></a>示例  
 下面的示例演示一个具有异步输出的简单转换组件，该组件在接收到行时将其添加到输出缓冲区中。 此示例未演示本主题中讨论的全部方法和功能。 它演示了每个具有异步输出的自定义转换组件都必须重写的重要方法，但不包含用于设计时验证的代码。 此外，<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 中的代码假定对于输入列集合中的每一列，输出列集合中都有相应的一列。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
   [DtsPipelineComponent(DisplayName = "AsynchronousOutput")]  
   public class AsynchronousOutput : PipelineComponent  
   {  
      PipelineBuffer outputBuffer;  
      int[] inputColumnBufferIndexes;  
      int[] outputColumnBufferIndexes;  
  
      public override void ProvideComponentProperties()  
      {  
         // Let the base class add the input and output objects.  
         base.ProvideComponentProperties();  
  
         // Name the input and output, and make the  
         // output asynchronous.  
         ComponentMetaData.InputCollection[0].Name = "Input";  
         ComponentMetaData.OutputCollection[0].Name = "AsyncOutput";  
         ComponentMetaData.OutputCollection[0].SynchronousInputID = 0;  
      }  
      public override void PreExecute()  
      {  
         IDTSInput100 input = ComponentMetaData.InputCollection[0];  
         IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
         inputColumnBufferIndexes = new int[input.InputColumnCollection.Count];  
         outputColumnBufferIndexes = new int[output.OutputColumnCollection.Count];  
  
         for (int col = 0; col < input.InputColumnCollection.Count; col++)  
            inputColumnBufferIndexes[col] = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection[col].LineageID);  
  
         for (int col = 0; col < output.OutputColumnCollection.Count; col++)  
            outputColumnBufferIndexes[col] = BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection[col].LineageID);  
  
      }  
  
      public override void PrimeOutput(int outputs, int[] outputIDs, PipelineBuffer[] buffers)  
      {  
         if (buffers.Length != 0)  
            outputBuffer = buffers[0];  
      }  
      public override void ProcessInput(int inputID, PipelineBuffer buffer)  
      {  
            // Advance the buffer to the next row.  
            while (buffer.NextRow())  
            {  
               // Add a row to the output buffer.  
               outputBuffer.AddRow();  
               for (int x = 0; x < inputColumnBufferIndexes.Length; x++)  
               {  
                  // Copy the data from the input buffer column to the output buffer column.  
                  outputBuffer[outputColumnBufferIndexes[x]] = buffer[inputColumnBufferIndexes[x]];  
               }  
            }  
         if (buffer.EndOfRowset)  
         {  
            // EndOfRowset on the input buffer is true.  
            // Set EndOfRowset on the output buffer.  
            outputBuffer.SetEndOfRowset();  
         }  
      }  
   }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
  
Namespace Microsoft.Samples.SqlServer.Dts  
  
    <DtsPipelineComponent(DisplayName:="AsynchronousOutput")> _  
    Public Class AsynchronousOutput  
  
        Inherits PipelineComponent  
  
        Private outputBuffer As PipelineBuffer  
        Private inputColumnBufferIndexes As Integer()  
        Private outputColumnBufferIndexes As Integer()  
  
        Public Overrides Sub ProvideComponentProperties()  
  
            ' Let the base class add the input and output objects.  
            Me.ProvideComponentProperties()  
  
            ' Name the input and output, and make the  
            ' output asynchronous.  
            ComponentMetaData.InputCollection(0).Name = "Input"  
            ComponentMetaData.OutputCollection(0).Name = "AsyncOutput"  
            ComponentMetaData.OutputCollection(0).SynchronousInputID = 0  
        End Sub  
  
        Public Overrides Sub PreExecute()  
  
            Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
            Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
            ReDim inputColumnBufferIndexes(input.InputColumnCollection.Count)  
            ReDim outputColumnBufferIndexes(output.OutputColumnCollection.Count)  
  
            For col As Integer = 0 To input.InputColumnCollection.Count  
                inputColumnBufferIndexes(col) = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection(col).LineageID)  
            Next  
  
            For col As Integer = 0 To output.OutputColumnCollection.Count  
                outputColumnBufferIndexes(col) = BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection(col).LineageID)  
            Next  
  
        End Sub  
        Public Overrides Sub PrimeOutput(ByVal outputs As Integer, ByVal outputIDs As Integer(), ByVal buffers As PipelineBuffer())  
  
            If buffers.Length <> 0 Then  
                outputBuffer = buffers(0)  
            End If  
  
        End Sub  
  
        Public Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)  
  
                ' Advance the buffer to the next row.  
                While (buffer.NextRow())  
  
                    ' Add a row to the output buffer.  
                    outputBuffer.AddRow()  
                    For x As Integer = 0 To inputColumnBufferIndexes.Length  
  
                        ' Copy the data from the input buffer column to the output buffer column.  
                        outputBuffer(outputColumnBufferIndexes(x)) = buffer(inputColumnBufferIndexes(x))  
  
                    Next  
                End While  
  
            If buffer.EndOfRowset = True Then  
                ' EndOfRowset on the input buffer is true.  
                ' Set the end of row set on the output buffer.  
                outputBuffer.SetEndOfRowset()  
            End If  
        End Sub  
    End Class  
End Namespace  
```  
  
## <a name="see-also"></a>另请参阅  
 [开发具有同步输出的自定义转换组件](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)   
 [了解同步和异步转换](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)   
 [使用脚本组件创建异步转换](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
  
  
