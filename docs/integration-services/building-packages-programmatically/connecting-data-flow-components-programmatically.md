---
description: 以编程方式连接数据流组件
title: 以编程方式连接数据流组件 | Microsoft Docs
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
- data flow task [Integration Services], components
- paths [Integration Services], components
- components [Integration Services], data flow
- data flow [Integration Services], components
ms.assetid: 404ecab7-7698-447b-93d6-dd256beb11ff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 73e8eb7b506050489581ad6b4d00b02a51049f48
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355073"
---
# <a name="connecting-data-flow-components-programmatically"></a>以编程方式连接数据流组件

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  将组件添加到数据流任务后，连接这些组件以创建表示从源通过转换到目标的数据流的执行树。 使用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100> 对象连接数据流中的组件。  
  
## <a name="creating-a-path"></a>创建路径  
 调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.MainPipe> 接口的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.MainPipeClass.PathCollection%2A> 属性的新方法以创建新路径并将其添加到数据流任务中的路径集合。 此方法返回新的断开连接的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100> 对象，然后使用该对象连接两个组件。  
  
 调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100.AttachPathAndPropagateNotifications%2A> 方法连接路径并通知组件参与已连接的路径。 此方法接受上游组件的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> 和下游组件的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100> 作为参数。 默认情况下，对组件的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> 方法的调用为具有输入的组件创建单个输入，为具有输出的组件创建单个输出。 下面的示例使用此默认的源输出和目标输入。  
  
## <a name="next-step"></a>下一步  
 在两个组件之间建立路径后，下一步是映射下游组件中的输入列，这将在下个主题[以编程方式选择输入列](../../integration-services/building-packages-programmatically/selecting-input-columns-programmatically.md)中讨论。  
  
## <a name="sample"></a>示例  
 下面的代码示例演示如何在两个组件之间建立路径。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package package = new Package();  
      Executable e = package.Executables.Add("STOCK:PipelineTask");  
      TaskHost thMainPipe = e as TaskHost;  
      MainPipe dataFlowTask = thMainPipe.InnerObject as MainPipe;  
  
      // Create the source component.    
      IDTSComponentMetaData100 source =  
        dataFlowTask.ComponentMetaDataCollection.New();  
      source.ComponentClassID = "DTSAdapter.OleDbSource";  
      CManagedComponentWrapper srcDesignTime = source.Instantiate();  
      srcDesignTime.ProvideComponentProperties();  
  
      // Create the destination component.  
      IDTSComponentMetaData100 destination =  
        dataFlowTask.ComponentMetaDataCollection.New();  
      destination.ComponentClassID = "DTSAdapter.OleDbDestination";  
      CManagedComponentWrapper destDesignTime = destination.Instantiate();  
      destDesignTime.ProvideComponentProperties();  
  
      // Create the path.  
      IDTSPath100 path = dataFlowTask.PathCollection.New();  
      path.AttachPathAndPropagateNotifications(source.OutputCollection[0],  
        destination.InputCollection[0]);  
    }  
  }  
```  
  
 }  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
Module Module1  
  
  Sub Main()  
  
    Dim package As Microsoft.SqlServer.Dts.Runtime.Package = _  
      New Microsoft.SqlServer.Dts.Runtime.Package()  
    Dim e As Executable = package.Executables.Add("STOCK:PipelineTask")  
    Dim thMainPipe As Microsoft.SqlServer.Dts.Runtime.TaskHost = _  
      CType(e, Microsoft.SqlServer.Dts.Runtime.TaskHost)  
    Dim dataFlowTask As MainPipe = CType(thMainPipe.InnerObject, MainPipe)  
  
    ' Create the source component.    
    Dim source As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    source.ComponentClassID = "DTSAdapter.OleDbSource"  
    Dim srcDesignTime As CManagedComponentWrapper = source.Instantiate()  
    srcDesignTime.ProvideComponentProperties()  
  
    ' Create the destination component.  
    Dim destination As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    destination.ComponentClassID = "DTSAdapter.OleDbDestination"  
    Dim destDesignTime As CManagedComponentWrapper = destination.Instantiate()  
    destDesignTime.ProvideComponentProperties()  
  
    ' Create the path.  
    Dim path As IDTSPath100 = dataFlowTask.PathCollection.New()  
    path.AttachPathAndPropagateNotifications(source.OutputCollection(0), _  
      destination.InputCollection(0))  
  
  End Sub  
  
End Module  
```  
  
## <a name="see-also"></a>另请参阅  
 [以编程方式选择输入列](../../integration-services/building-packages-programmatically/selecting-input-columns-programmatically.md)  
  
  
