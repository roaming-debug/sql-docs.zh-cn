---
description: 开发自定义目标组件
title: 开发自定义目标组件 | Microsoft Docs
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
- validation [Integration Services], components
- destinations [Integration Services], components
- ProcessInput method
- external data sources [Integration Services]
- custom data flow components [Integration Services], destination components
- data flow components [Integration Services], destination components
ms.assetid: 24619363-9535-4c0e-8b62-1d22c6630e40
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8a86cfc875bf47d04ef31901866cd7500260ea7e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336340"
---
# <a name="developing-a-custom-destination-component"></a>开发自定义目标组件

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  开发人员可以通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 编写可连接到任意自定义数据源并在其中存储数据的自定义目标组件。 自定义目标组件在您需要连接到无法通过使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中包含的现有源组件来访问的数据源时非常有用。  
  
 目标组件有一个或多个输入，没有输出。 在设计时，目标组件创建和配置连接并从外部数据源读取列元数据。 在执行过程中，它们连接到外部数据源并将从数据流上游组件收到的行添加到外部数据源中。 如果外部数据源在组件执行之前就存在，则目标组件还必须确保组件收到的列的数据类型与外部数据源中列的数据类型相匹配。  
  
 本节详细介绍如何开发目标组件，并提供阐明重要概念的代码示例。 有关数据流组件开发的一般概述，请参阅[开发自定义数据流组件](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)。  
  
## <a name="design-time"></a>设计时  
 实现目标组件的设计时功能包括指定与外部数据源的连接以及验证该组件已经正确配置。 根据定义，目标组件有一个输入，可能有一个错误输出。  
  
### <a name="creating-the-component"></a>创建组件  
 目标组件使用包中定义的 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 对象连接到外部数据源。 目标组件通过将元素添加到 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> 的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A> 集合，向 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器和组件用户指明自己需要连接管理器。 此集合有两个用途：首先，告知 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器它需要连接管理器；然后，在用户选择或创建完连接管理器后，保存对组件正在使用的包中的连接管理器的引用。 将 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100> 添加到该集合后，“高级编辑器”将显示“连接属性”选项卡，以提示用户在包中选择或创建连接以供组件使用。  
  
 下面的代码示例演示 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> 的实现，该实现添加一个输入，然后将 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100> 对象添加到 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A>。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "Destination Component",ComponentType =ComponentType.DestinationAdapter)]  
    public class DestinationComponent : PipelineComponent   
    {  
        public override void ProvideComponentProperties()  
        {  
            // Reset the component.  
            base.RemoveAllInputsOutputsAndCustomProperties();  
            ComponentMetaData.RuntimeConnectionCollection.RemoveAll();  
  
            IDTSInput100 input = ComponentMetaData.InputCollection.New();  
            input.Name = "Input";  
  
            IDTSRuntimeConnection100 connection = ComponentMetaData.RuntimeConnectionCollection.New();  
            connection.Name = "ADO.net";  
        }  
    }  
}  
```  
  
```vb  
Imports System  
Imports System.Data  
Imports System.Data.SqlClient  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Namespace Microsoft.Samples.SqlServer.Dts  
  
    <DtsPipelineComponent(DisplayName:="Destination Component", ComponentType:=ComponentType.DestinationAdapter)> _  
    Public Class DestinationComponent  
        Inherits PipelineComponent  
  
        Public Overrides Sub ProvideComponentProperties()  
  
            ' Reset the component.  
            Me.RemoveAllInputsOutputsAndCustomProperties()  
            ComponentMetaData.RuntimeConnectionCollection.RemoveAll()  
  
            Dim input As IDTSInput100 = ComponentMetaData.InputCollection.New()  
            input.Name = "Input"  
  
            Dim connection As IDTSRuntimeConnection100 = ComponentMetaData.RuntimeConnectionCollection.New()  
            connection.Name = "ADO.net"  
  
        End Sub  
    End Class  
End Namespace  
```  
  
### <a name="connecting-to-an-external-data-source"></a>连接外部数据源  
 将连接添加到 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A> 后，请重写 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A> 方法以建立与外部数据源的连接。 此方法在设计时和运行时调用。 组件应先建立与连接管理器（由运行时连接指定）的连接，再建立与外部数据源的连接。 连接建立后，组件就应在内部缓存该连接并在调用 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A> 时释放该连接。 开发人员可重写此方法，释放在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A> 过程中由组件建立的连接。 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A> 方法都在设计时和运行时调用。  
  
 下面的代码示例演示在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A> 方法中连接到 ADO.NET 连接，然后在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A> 中关闭该连接的组件。  
  
```csharp  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
private SqlConnection sqlConnection;  
  
public override void AcquireConnections(object transaction)  
{  
    if (ComponentMetaData.RuntimeConnectionCollection[0].ConnectionManager != null)  
    {  
        ConnectionManager cm = Microsoft.SqlServer.Dts.Runtime.DtsConvert.GetWrapper(ComponentMetaData.RuntimeConnectionCollection[0].ConnectionManager);  
        ConnectionManagerAdoNet cmado = cm.InnerObject as ConnectionManagerAdoNet;  
  
        if (cmado == null)  
            throw new Exception("The ConnectionManager " + cm.Name + " is not an ADO.NET connection.");  
  
        sqlConnection = cmado.AcquireConnection(transaction) as SqlConnection;  
        sqlConnection.Open();  
    }  
}  
  
public override void ReleaseConnections()  
{  
    if (sqlConnection != null && sqlConnection.State != ConnectionState.Closed)  
        sqlConnection.Close();  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
  
Private sqlConnection As SqlConnection  
  
Public Overrides Sub AcquireConnections(ByVal transaction As Object)  
  
    If IsNothing(ComponentMetaData.RuntimeConnectionCollection(0).ConnectionManager) = False Then  
  
        Dim cm As ConnectionManager = DtsConvert.GetWrapper(ComponentMetaData.RuntimeConnectionCollection(0).ConnectionManager)  
        Dim cmado As ConnectionManagerAdoNet = CType(cm.InnerObject,ConnectionManagerAdoNet)  
  
        If IsNothing(cmado) Then  
            Throw New Exception("The ConnectionManager " + cm.Name + " is not an ADO.NET connection.")  
        End If  
  
        sqlConnection = CType(cmado.AcquireConnection(transaction), SqlConnection)  
        sqlConnection.Open()  
  
    End If  
End Sub  
  
Public Overrides Sub ReleaseConnections()  
  
    If IsNothing(sqlConnection) = False And sqlConnection.State <> ConnectionState.Closed Then  
        sqlConnection.Close()  
    End If  
  
End Sub  
```  
  
### <a name="validating-the-component"></a>验证组件  
 目标组件开发人员应按照[组件验证](../../integration-services/extending-packages-custom-objects/data-flow/validating-a-data-flow-component.md)中所述执行验证。 此外，还应该验证组件的输入列集合中定义的列的数据类型属性是否与外部数据源中的列匹配。 有时候，根据外部数据源验证输入列是不可能或不希望的，例如组件或 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器处于断开连接状态时，或者与服务器之间的往返通信不可接受时。 在这些情况下，仍然可以使用输入对象的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100.ExternalMetadataColumnCollection%2A> 来验证输入列集合中的列。  
  
 此集合同时存在于输入对象和输出对象中，必须由组件开发人员从外部数据源的列来填充。 此集合可用于 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器处于离线状态、组件断开连接或 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ValidateExternalMetadata%2A> 属性为“false”时验证输入列。  
  
 下面的示例代码根据现有输入列添加外部元数据列。  
  
```csharp  
private void AddExternalMetaDataColumn(IDTSInput100 input,IDTSInputColumn100 inputColumn)  
{  
    // Set the properties of the external metadata column.  
    IDTSExternalMetadataColumn100 externalColumn = input.ExternalMetadataColumnCollection.New();  
    externalColumn.Name = inputColumn.Name;  
    externalColumn.Precision = inputColumn.Precision;  
    externalColumn.Length = inputColumn.Length;  
    externalColumn.DataType = inputColumn.DataType;  
    externalColumn.Scale = inputColumn.Scale;  
  
    // Map the external column to the input column.  
    inputColumn.ExternalMetadataColumnID = externalColumn.ID;  
}  
```  
  
```vb  
Private Sub AddExternalMetaDataColumn(ByVal input As IDTSInput100, ByVal inputColumn As IDTSInputColumn100)  
  
    ' Set the properties of the external metadata column.  
    Dim externalColumn As IDTSExternalMetadataColumn100 = input.ExternalMetadataColumnCollection.New()  
    externalColumn.Name = inputColumn.Name  
    externalColumn.Precision = inputColumn.Precision  
    externalColumn.Length = inputColumn.Length  
    externalColumn.DataType = inputColumn.DataType  
    externalColumn.Scale = inputColumn.Scale  
  
    ' Map the external column to the input column.  
    inputColumn.ExternalMetadataColumnID = externalColumn.ID  
  
End Sub  
```  
  
## <a name="run-time"></a>运行时  
 在执行过程中，每次上游组件中有了完整的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 时，目标组件都会收到对 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> 方法的调用。 此方法会被重复调用，直到再没有缓冲区可用且 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A> 属性为“true”。 在此方法中，目标组件读取缓冲区中的列和行，并将其添加到外部数据源。  
  
### <a name="locating-columns-in-the-buffer"></a>在缓冲区中查找列  
 组件的输入缓冲区包含数据流中该组件的上游组件的输出列集合中定义的所有列。 例如，如果源组件在其输出中提供三列，下一个组件添加了另外一个输出列，则为目标组件提供的缓冲区中将包含四列，即使目标组件将只写入两列。  
  
 输入缓冲区中列的顺序不是根据目标组件的输入列集合中列的索引定义的。 只有使用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> 的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> 方法才能在缓冲区行中可靠地定位列。 此方法在指定缓冲区中查找具有指定沿袭 ID 的列，并返回其在行中的位置。 输入列的索引通常在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> 方法中查找，并由开发人员缓存供稍后在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 中使用。  
  
 下面的代码示例在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> 中查找输入列在缓冲区中的位置并将其存储在一个数组中。 随后在 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 中使用该数组，以读取缓冲区中列的值。  
  
```csharp  
int[] cols;  
  
public override void PreExecute()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
  
    cols = new int[input.InputColumnCollection.Count];  
  
    for (int x = 0; x < input.InputColumnCollection.Count; x++)  
    {  
        cols[x] = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection[x].LineageID);  
    }  
}  
```  
  
```vb  
Private cols As Integer()  
  
Public Overrides Sub PreExecute()  
  
    Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
  
    ReDim cols(input.InputColumnCollection.Count)  
  
    For x As Integer = 0 To input.InputColumnCollection.Count  
  
        cols(x) = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection(x).LineageID)  
    Next x  
  
End Sub  
```  
  
### <a name="processing-rows"></a>处理行  
 在缓冲区中找到输入列后，可以读取这些列并将其写入外部数据源。  
  
 在目标组件将行写入外部数据源时，您可能希望调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.IncrementPipelinePerfCounter%2A> 方法来更新“Rows read”或“BLOB bytes read”性能计数器。 有关详细信息，请参阅 [性能计时器](../../integration-services/performance/performance-counters.md)。  
  
 下面的示例演示从 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> 中提供的缓冲区读取行的组件。 缓冲区中列的索引已在前面的代码示例中的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> 内找到。  
  
```csharp  
public override void ProcessInput(int inputID, PipelineBuffer buffer)  
{  
        while (buffer.NextRow())  
        {  
            foreach (int col in cols)  
            {  
                if (!buffer.IsNull(col))  
                {  
                    //  TODO: Read the column data.  
                }  
            }  
        }  
}  
```  
  
```vb  
Public Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)  
  
        While (buffer.NextRow())  
  
            For Each col As Integer In cols  
  
                If buffer.IsNull(col) = False Then  
  
                    '  TODO: Read the column data.  
                End If  
  
            Next col  
        End While  
End Sub  
```  
  
## <a name="sample"></a>示例  
 下面的示例演示一个简单的目标组件，该组件使用文件连接管理器将数据流中的二进制数据保存到文件中。 此示例未演示本主题中讨论的全部方法和功能。 它演示了每个自定义目标组件都必须重写的重要方法，但不包含用于设计时验证的代码。  
  
```csharp  
using System;  
using System.IO;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace BlobDst  
{  
  [DtsPipelineComponent(DisplayName = "BLOB Extractor Destination", Description = "Writes values of BLOB columns to files")]  
  public class BlobDst : PipelineComponent  
  {  
    string m_DestDir;  
    int m_FileNameColumnIndex = -1;  
    int m_BlobColumnIndex = -1;  
  
    public override void ProvideComponentProperties()  
    {  
      IDTSInput100 input = ComponentMetaData.InputCollection.New();  
      input.Name = "BLOB Extractor Destination Input";  
      input.HasSideEffects = true;  
  
      IDTSRuntimeConnection100 conn = ComponentMetaData.RuntimeConnectionCollection.New();  
      conn.Name = "FileConnection";  
    }  
  
    public override void AcquireConnections(object transaction)  
    {  
      IDTSRuntimeConnection100 conn = ComponentMetaData.RuntimeConnectionCollection[0];  
      m_DestDir = (string)conn.ConnectionManager.AcquireConnection(null);  
  
      if (m_DestDir.Length > 0 && m_DestDir[m_DestDir.Length - 1] != '\\')  
        m_DestDir += "\\";  
    }  
  
    public override IDTSInputColumn100 SetUsageType(int inputID, IDTSVirtualInput100 virtualInput, int lineageID, DTSUsageType usageType)  
    {  
      IDTSInputColumn100 inputColumn = base.SetUsageType(inputID, virtualInput, lineageID, usageType);  
      IDTSCustomProperty100 custProp;  
  
      custProp = inputColumn.CustomPropertyCollection.New();  
      custProp.Name = "IsFileName";  
      custProp.Value = (object)false;  
  
      custProp = inputColumn.CustomPropertyCollection.New();  
      custProp.Name = "IsBLOB";  
      custProp.Value = (object)false;  
  
      return inputColumn;  
    }  
  
    public override void PreExecute()  
    {  
      IDTSInput100 input = ComponentMetaData.InputCollection[0];  
      IDTSInputColumnCollection100 inputColumns = input.InputColumnCollection;  
      IDTSCustomProperty100 custProp;  
  
      foreach (IDTSInputColumn100 column in inputColumns)  
      {  
        custProp = column.CustomPropertyCollection["IsFileName"];  
        if ((bool)custProp.Value == true)  
        {  
          m_FileNameColumnIndex = (int)BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID);  
        }  
  
        custProp = column.CustomPropertyCollection["IsBLOB"];  
        if ((bool)custProp.Value == true)  
        {  
          m_BlobColumnIndex = (int)BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID);  
        }  
      }  
    }  
  
    public override void ProcessInput(int inputID, PipelineBuffer buffer)  
    {  
      while (buffer.NextRow())  
      {  
        string strFileName = buffer.GetString(m_FileNameColumnIndex);  
        int blobLength = (int)buffer.GetBlobLength(m_BlobColumnIndex);  
        byte[] blobData = buffer.GetBlobData(m_BlobColumnIndex, 0, blobLength);  
  
        strFileName = TranslateFileName(strFileName);  
  
        // Make sure directory exists before creating file.  
        FileInfo fi = new FileInfo(strFileName);  
        if (!fi.Directory.Exists)  
          fi.Directory.Create();  
  
        // Write the data to the file.  
        FileStream fs = new FileStream(strFileName, FileMode.Create, FileAccess.Write, FileShare.None);  
        fs.Write(blobData, 0, blobLength);  
        fs.Close();  
      }  
    }  
  
    private string TranslateFileName(string fileName)  
    {  
      if (fileName.Length > 2 && fileName[1] == ':')  
        return m_DestDir + fileName.Substring(3, fileName.Length - 3);  
      else  
        return m_DestDir + fileName;  
    }  
  }  
}  
```  
  
```vb  
Imports System   
Imports System.IO   
Imports Microsoft.SqlServer.Dts.Pipeline   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Namespace BlobDst   
  
 <DtsPipelineComponent(DisplayName="BLOB Extractor Destination", Description="Writes values of BLOB columns to files")> _   
 Public Class BlobDst   
 Inherits PipelineComponent   
   Private m_DestDir As String   
   Private m_FileNameColumnIndex As Integer = -1   
   Private m_BlobColumnIndex As Integer = -1   
  
   Public  Overrides Sub ProvideComponentProperties()   
     Dim input As IDTSInput100 = ComponentMetaData.InputCollection.New   
     input.Name = "BLOB Extractor Destination Input"   
     input.HasSideEffects = True   
     Dim conn As IDTSRuntimeConnection100 = ComponentMetaData.RuntimeConnectionCollection.New   
     conn.Name = "FileConnection"   
   End Sub   
  
   Public  Overrides Sub AcquireConnections(ByVal transaction As Object)   
     Dim conn As IDTSRuntimeConnection100 = ComponentMetaData.RuntimeConnectionCollection(0)   
     m_DestDir = CType(conn.ConnectionManager.AcquireConnection(Nothing), String)   
     If m_DestDir.Length > 0 AndAlso Not (m_DestDir(m_DestDir.Length - 1) = "\"C) Then   
       m_DestDir += "\"   
     End If   
   End Sub   
  
   Public  Overrides Function SetUsageType(ByVal inputID As Integer, ByVal virtualInput As IDTSVirtualInput100, ByVal lineageID As Integer, ByVal usageType As DTSUsageType) As IDTSInputColumn100   
     Dim inputColumn As IDTSInputColumn100 = MyBase.SetUsageType(inputID, virtualInput, lineageID, usageType)   
     Dim custProp As IDTSCustomProperty100   
     custProp = inputColumn.CustomPropertyCollection.New   
     custProp.Name = "IsFileName"   
     custProp.Value = CType(False, Object)   
     custProp = inputColumn.CustomPropertyCollection.New   
     custProp.Name = "IsBLOB"   
     custProp.Value = CType(False, Object)   
     Return inputColumn   
   End Function   
  
   Public  Overrides Sub PreExecute()   
     Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
     Dim inputColumns As IDTSInputColumnCollection100 = input.InputColumnCollection   
     Dim custProp As IDTSCustomProperty100   
     For Each column As IDTSInputColumn100 In inputColumns   
       custProp = column.CustomPropertyCollection("IsFileName")   
       If CType(custProp.Value, Boolean) = True Then   
         m_FileNameColumnIndex = CType(BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID), Integer)   
       End If   
       custProp = column.CustomPropertyCollection("IsBLOB")   
       If CType(custProp.Value, Boolean) = True Then   
         m_BlobColumnIndex = CType(BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID), Integer)   
       End If   
     Next   
   End Sub   
  
   Public  Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)   
     While buffer.NextRow   
       Dim strFileName As String = buffer.GetString(m_FileNameColumnIndex)   
       Dim blobLength As Integer = CType(buffer.GetBlobLength(m_BlobColumnIndex), Integer)   
       Dim blobData As Byte() = buffer.GetBlobData(m_BlobColumnIndex, 0, blobLength)   
       strFileName = TranslateFileName(strFileName)   
       Dim fi As FileInfo = New FileInfo(strFileName)   
       ' Make sure directory exists before creating file.  
       If Not fi.Directory.Exists Then   
         fi.Directory.Create   
       End If   
       ' Write the data to the file.  
       Dim fs As FileStream = New FileStream(strFileName, FileMode.Create, FileAccess.Write, FileShare.None)   
       fs.Write(blobData, 0, blobLength)   
       fs.Close   
     End While   
   End Sub   
  
   Private Function TranslateFileName(ByVal fileName As String) As String   
     If fileName.Length > 2 AndAlso fileName(1) = ":"C Then   
       Return m_DestDir + fileName.Substring(3, fileName.Length - 3)   
     Else   
       Return m_DestDir + fileName   
     End If   
   End Function   
 End Class   
End Namespace  
```  
  
## <a name="see-also"></a>另请参阅  
 [开发自定义源组件](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md)   
 [使用脚本组件创建目标](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md)  
  
  
