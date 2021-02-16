---
description: 开发具有同步输出的自定义转换组件
title: 开发具有同步输出的自定义转换组件 | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
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
- ProcessInput method
- buffer allocations [Integration Services]
- synchronous outputs [Integration Services]
- transformation components [Integration Services]
- output columns [Integration Services]
- data flow components [Integration Services], transformation components
ms.assetid: b694d21f-9919-402d-9192-666c6449b0b7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 87a53ff91f4ae7d6237fb3f9640420dd964b607f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348231"
---
# <a name="developing-a-custom-transformation-component-with-synchronous-outputs"></a>开发具有同步输出的自定义转换组件

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  具有同步输出的转换组件接收来自上游组件的行，并在将行传递到下游组件时读取或修改这些行的列中的值。 这些转换组件还定义从上游组件提供的列派生的其他输出列，但是它们不会向数据流添加行。 有关同步组件和异步组件之间的差异的详细信息，请参阅[了解同步和异步转换](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)。  
  
 这类组件适用的任务如下：任务中的数据在提供给组件时被内联修改，并且组件在处理这些行之前不必看到所有行。 这是用于开发的最简单组件，因为具有同步输出的转换通常不连接到外部数据源、管理外部元数据列或向输出缓冲区添加行。  
  
 创建具有同步输出的转换组件涉及添加一个将包含为该组件选择的上游列的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100>，和一个可能包含组件创建的派生列的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> 对象。 还包括实现设计时方法以及提供在执行期间用于读取或修改传入缓冲区中的列的代码。  
  
 本节提供了实现自定义转换组件所需的信息，以及有助于您更好地了解这些概念的代码示例。  
  
## <a name="design-time"></a>设计时  
 此组件的设计时代码涉及创建输入和输出、添加组件生成的任何其他输出列以及验证组件的配置。  
  
### <a name="creating-the-component"></a>创建组件  
 组件的输入、输出和自定义属性通常在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> 方法期间创建。 您可以使用两种方式添加具有同步输出的转换组件的输入和输出。 可以使用方法的基类实现，然后修改该方法创建的默认输入和输出，也可以自己显式添加输入和输出。  
  
 下面的代码示例演示显式添加输入和输出对象的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> 实现。 注释中包含对将要完成同样任务的基类的调用。  
  
```csharp  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "SynchronousComponent", ComponentType = ComponentType.Transform)]  
    public class SyncComponent : PipelineComponent  
    {  
  
        public override void ProvideComponentProperties()  
        {  
            // Add the input.  
            IDTSInput100 input = ComponentMetaData.InputCollection.New();  
            input.Name = "Input";  
  
            // Add the output.  
            IDTSOutput100 output = ComponentMetaData.OutputCollection.New();  
            output.Name = "Output";  
            output.SynchronousInputID = input.ID;  
  
            // Alternatively, you can let the base class add the input and output  
            // and set the SynchronousInputID of the output to the ID of the input.  
            // base.ProvideComponentProperties();  
        }  
    }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsPipelineComponent(DisplayName:="SynchronousComponent", ComponentType:=ComponentType.Transform)> _  
Public Class SyncComponent  
    Inherits PipelineComponent  
  
    Public Overrides Sub ProvideComponentProperties()  
  
        ' Add the input.  
        Dim input As IDTSInput100 = ComponentMetaData.InputCollection.New()  
        input.Name = "Input"  
  
        ' Add the output.  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.New()  
        output.Name = "Output"  
        output.SynchronousInputID = Input.ID  
  
        ' Alternatively, you can let the base class add the input and output  
        ' and set the SynchronousInputID of the output to the ID of the input.  
        ' base.ProvideComponentProperties();  
  
    End Sub  
  
End Class  
```  
  
### <a name="creating-and-configuring-output-columns"></a>创建和配置输出列  
 尽管具有同步输出的转换组件不向缓冲区添加行，但是它们可能向自己的输出添加额外的输出列。 通常，当组件添加输出列时，新输出列的值在运行时从一个或多个列中包含的数据派生而来，这些列由上游组件提供给该组件。  
  
 创建输出列后，必须设置该列的数据类型属性。 输出列的数据类型属性的设置需要特殊处理，并通过调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.SetDataTypeProperties%2A> 方法来执行。 此方法是必需的，因为 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A>、<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Length%2A>、<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Precision%2A> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.CodePage%2A> 属性在单独情况下都是只读的，原因是每个属性均取决于另一个属性的设置。 此方法可保证属性值的设置保持一致，并且数据流任务会验证是否正确设置了这些值。  
  
 列的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A> 决定了为其他属性设置的值。 下表说明了对每个 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A> 的依赖属性的要求。 未列出的数据类型的依赖属性设置为零。  
  
|数据类型|长度|缩放|精度|CodePage|  
|--------------|------------|-----------|---------------|--------------|  
|DT_DECIMAL|0|大于 0 且小于或等于 28。|0|0|  
|DT_CY|0|0|0|0|  
|DT_NUMERIC|0|大于 0 且小于或等于 28，并且小于 Precision。|大于或等于 1 且小于或等于 38。|0|  
|DT_BYTES|大于 0。|0|0|0|  
|DT_STR|大于 0 且小于 8000。|0|0|不为 0，并且是一个有效的代码页。|  
|DT_WSTR|大于 0 且小于 4000。|0|0|0|  
  
 由于对数据类型属性的限制是基于输出列的数据类型的，因此使用托管类型时必须选择正确的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 数据类型。 基类提供了三个帮助器方法：<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ConvertBufferDataTypeToFitManaged%2A>、<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferTypeToDataRecordType%2A> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.DataRecordTypeToBufferType%2A>，它们可协助托管组件开发人员在给定托管类型的情况下选择 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 数据类型。 这些方法用于将托管数据类型转换为 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 数据类型，反之亦然。  
  
## <a name="run-time"></a>运行时  
 通常，组件的运行时部分的实现分为两个任务：在缓冲区中查找组件的输入和输出列，以及在传入缓冲区行中读取或写入这些列的值。  
  
### <a name="locating-columns-in-the-buffer"></a>在缓冲区中查找列  
 在执行期间提供给组件的缓冲区中的列数将很可能超过组件的输入或输出集合中的列数。 这是因为每个缓冲区都包含在数据流的组件中定义的所有输出列。 为了确保缓冲区列与输出或输出的列正确匹配，组件开发人员必须使用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> 的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> 方法。 此方法会根据其沿袭 ID 在已定义的缓冲区中查找列。 这些列通常在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> 期间进行查找，因为这是第一个其 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> 属性可用的运行时方法。  
  
 下面的代码示例演示了一个组件，该组件在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> 期间查找其输入和输出列集合中的列的索引。 列的索引存储在整数数组中，可在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 期间由组件访问。  
  
```csharp  
int []inputColumns;  
int []outputColumns;  
  
public override void PreExecute()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
    IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
    inputColumns = new int[input.InputColumnCollection.Count];  
    outputColumns = new int[output.OutputColumnCollection.Count];  
  
    for(int col=0; col < input.InputColumnCollection.Count; col++)  
    {  
        IDTSInputColumn100 inputColumn = input.InputColumnCollection[col];  
        inputColumns[col] = BufferManager.FindColumnByLineageID(input.Buffer, inputColumn.LineageID);  
    }  
  
    for(int col=0; col < output.OutputColumnCollection.Count; col++)  
    {  
        IDTSOutputColumn100 outputColumn = output.OutputColumnCollection[col];  
        outputColumns[col] = BufferManager.FindColumnByLineageID(input.Buffer, outputColumn.LineageID);  
    }  
  
}  
```  
  
```vb  
Public Overrides Sub PreExecute()  
  
    Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
    ReDim inputColumns(input.InputColumnCollection.Count)  
    ReDim outputColumns(output.OutputColumnCollection.Count)  
  
    For col As Integer = 0 To input.InputColumnCollection.Count  
  
        Dim inputColumn As IDTSInputColumn100 = input.InputColumnCollection(col)  
        inputColumns(col) = BufferManager.FindColumnByLineageID(input.Buffer, inputColumn.LineageID)  
    Next  
  
    For col As Integer = 0 To output.OutputColumnCollection.Count  
  
        Dim outputColumn As IDTSOutputColumn100 = output.OutputColumnCollection(col)  
        outputColumns(col) = BufferManager.FindColumnByLineageID(input.Buffer, outputColumn.LineageID)  
    Next  
  
End Sub  
```  
  
### <a name="processing-rows"></a>处理行  
 组件可接收 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> 对象，这些对象包含 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法中的行和列。 在此方法中，将循环访问缓冲区中的行，并读取和修改 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> 期间标识的列。 数据流任务将重复调用该方法，直到上游组件不再提供行。  
  
 可使用数组索引器访问方法或者使用 Get 或 Set 方法之一来读取或写入缓冲区中的单个列。 Get 和 Set 方法的效率更高，应在缓冲区中的列的数据类型已知时使用。  
  
 下面的代码示例演示 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 方法的实现，该方法用于处理传入的行。  
  
```csharp  
public override void ProcessInput( int InputID, PipelineBuffer buffer)  
{  
       while( buffer.NextRow())  
       {  
            for(int x=0; x < inputColumns.Length;x++)  
            {  
                if(!buffer.IsNull(inputColumns[x]))  
                {  
                    object columnData = buffer[inputColumns[x]];  
                    // TODO: Modify the column data.  
                    buffer[inputColumns[x]] = columnData;  
                }  
            }  
  
      }  
}  
```  
  
```vb  
Public Overrides Sub ProcessInput(ByVal InputID As Integer, ByVal buffer As PipelineBuffer)  
  
        While (buffer.NextRow())  
  
            For x As Integer = 0 To inputColumns.Length  
  
                if buffer.IsNull(inputColumns(x)) = false then  
  
                    Dim columnData As Object = buffer(inputColumns(x))  
                    ' TODO: Modify the column data.  
                    buffer(inputColumns(x)) = columnData  
  
                End If  
            Next  
  
        End While  
End Sub  
```  
  
## <a name="sample"></a>示例  
 下面的示例演示一个具有同步输出的简单转换组件，该组件将所有字符串列的值转换为大写。 此示例未演示本主题中讨论的全部方法和功能。 它演示了每个具有同步输出的自定义转换组件都必须重写的重要方法，但不包含用于设计时验证的代码。  
  
```csharp  
using System;  
using System.Collections;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
namespace Uppercase  
{  
  [DtsPipelineComponent(DisplayName = "Uppercase")]  
  public class Uppercase : PipelineComponent  
  {  
    ArrayList m_ColumnIndexList = new ArrayList();  
  
    public override void ProvideComponentProperties()  
    {  
      base.ProvideComponentProperties();  
      ComponentMetaData.InputCollection[0].Name = "Uppercase Input";  
      ComponentMetaData.OutputCollection[0].Name = "Uppercase Output";  
    }  
  
    public override void PreExecute()  
    {  
      IDTSInput100 input = ComponentMetaData.InputCollection[0];  
      IDTSInputColumnCollection100 inputColumns = input.InputColumnCollection;  
  
      foreach (IDTSInputColumn100 column in inputColumns)  
      {  
        if (column.DataType == DataType.DT_STR || column.DataType == DataType.DT_WSTR)  
        {  
          m_ColumnIndexList.Add((int)BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID));  
        }  
      }  
    }  
  
    public override void ProcessInput(int inputID, PipelineBuffer buffer)  
    {  
      while (buffer.NextRow())  
      {  
        foreach (int columnIndex in m_ColumnIndexList)  
        {  
          string str = buffer.GetString(columnIndex);  
          buffer.SetString(columnIndex, str.ToUpper());  
        }  
      }  
    }  
  }  
}  
```  
  
```vb  
Imports System   
Imports System.Collections   
Imports Microsoft.SqlServer.Dts.Pipeline   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper   
Namespace Uppercase   
  
 <DtsPipelineComponent(DisplayName="Uppercase")> _   
 Public Class Uppercase   
 Inherits PipelineComponent   
   Private m_ColumnIndexList As ArrayList = New ArrayList   
  
   Public  Overrides Sub ProvideComponentProperties()   
     MyBase.ProvideComponentProperties   
     ComponentMetaData.InputCollection(0).Name = "Uppercase Input"   
     ComponentMetaData.OutputCollection(0).Name = "Uppercase Output"   
   End Sub   
  
   Public  Overrides Sub PreExecute()   
     Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
     Dim inputColumns As IDTSInputColumnCollection100 = input.InputColumnCollection   
     For Each column As IDTSInputColumn100 In inputColumns   
       If column.DataType = DataType.DT_STR OrElse column.DataType = DataType.DT_WSTR Then   
         m_ColumnIndexList.Add(CType(BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID), Integer))   
       End If   
     Next   
   End Sub   
  
   Public  Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)   
     While buffer.NextRow   
       For Each columnIndex As Integer In m_ColumnIndexList   
         Dim str As String = buffer.GetString(columnIndex)   
         buffer.SetString(columnIndex, str.ToUpper)   
       Next   
     End While   
   End Sub   
 End Class   
End Namespace  
```  
  
## <a name="see-also"></a>另请参阅  
 [开发具有异步输出的自定义转换组件](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md)   
 [了解同步和异步转换](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)   
 [使用脚本组件创建同步转换](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)  
  
  
