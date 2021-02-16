---
description: 以编程方式添加数据流组件
title: 以编程方式添加数据流组件 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- data flow task [Integration Services], components
- components [Integration Services], data flow
- data flow [Integration Services], components
ms.assetid: c06065cf-43e5-4b6b-9824-7309d7f5e35e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cd3a122993913d524547d6c9e57945956f1099de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351006"
---
# <a name="adding-data-flow-components-programmatically"></a>以编程方式添加数据流组件

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  若要生成数据流，则可以先添加组件。 然后配置这些组件并将它们连接在一起，以便在运行时建立数据流。 本节将介绍如何向数据流任务添加组件、如何创建组件的设计时实例以及配置该组件。 有关如何连接组件的详细信息，请参阅[以编程方式连接数据流组件](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md)。  
  
## <a name="adding-a-component"></a>添加组件  
 对 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaDataCollection100.New%2A> 集合调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.MainPipeClass.ComponentMetaDataCollection%2A> 方法可创建新组件并将其添加到数据流任务。 此方法将返回组件的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> 接口。 但是，此时 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> 不包含特定于任一组件的信息。 设置 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A> 属性可标识组件的类型。 数据流任务将使用此属性的值在运行时创建组件的实例。  
  
 在 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A> 属性中指定的值可为 CLSID、PROGID 或组件的 <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfo.CreationName%2A> 属性。 CLSID 通常作为组件的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A> 属性值显示在“属性”窗口中。 有关获取可用组件的此属性和其他属性的信息，请参阅[以编程方式查找数据流组件](../../integration-services/building-packages-programmatically/discovering-data-flow-components-programmatically.md)。  
  
## <a name="adding-a-managed-component"></a>添加托管组件  
 您不能使用 CLSID 或 PROGID 向数据流添加一个托管数据流组件，因为这些值指向包装而不是组件本身。 可以改用 CreationName 属性或 AssemblyQualifiedName 属性，如以下示例中所示。  
  
 若要使用 AssemblyQualifiedName 属性，则必须在 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 项目中添加对包含托管组件的程序集的引用。 这些程序集没有在“添加引用”对话框的 .NET 选项卡上列出。 通常需要浏览至 C:\Program Files\Microsoft SQL Server\100\DTS\PipelineComponents 文件夹查找程序集。  
  
 内置托管数据流组件包括：  
  
-   [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 源  
  
-   XML 源  
  
-   DataReader 目标  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Compact 目标  
  
-   脚本组件  
  
 下面的代码示例演示将托管组件添加到数据流的两种方法：  
  
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
      Microsoft.SqlServer.Dts.Runtime.Package package = new Microsoft.SqlServer.Dts.Runtime.Package();  
      Executable e = package.Executables.Add("STOCK:PipelineTask");  
      Microsoft.SqlServer.Dts.Runtime.TaskHost thMainPipe = (Microsoft.SqlServer.Dts.Runtime.TaskHost)e;  
      MainPipe dataFlowTask = (MainPipe)thMainPipe.InnerObject;  
  
      // The Application object will be used to obtain the CreationName  
      //  of a PipelineComponentInfo from its PipelineComponentInfos collection.  
      Application app = new Application();  
  
      // Add a first ADO NET source to the data flow.  
      //  The CreationName property requires an Application instance.  
      IDTSComponentMetaData100 component1 = dataFlowTask.ComponentMetaDataCollection.New();  
      component1.Name = "DataReader Source";  
      component1.ComponentClassID = app.PipelineComponentInfos["DataReader Source"].CreationName;  
  
      // Add a second ADO NET source to the data flow.  
      //  The AssemblyQualifiedName property requires a reference to the assembly.  
      IDTSComponentMetaData100 component2 = dataFlowTask.ComponentMetaDataCollection.New();  
      component2.Name = "DataReader Source";  
      component2.ComponentClassID = typeof(Microsoft.SqlServer.Dts.Pipeline.DataReaderSourceAdapter).AssemblyQualifiedName;  
    }  
  }  
}  
  
```  
  
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
  
    ' The Application object will be used to obtain the CreationName  
    '  of a PipelineComponentInfo from its PipelineComponentInfos collection.  
    Dim app As New Application()  
  
    ' Add a first ADO NET source to the data flow.  
    '  The CreationName property requires an Application instance.  
    Dim component1 As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    component1.Name = "DataReader Source"  
    component1.ComponentClassID = app.PipelineComponentInfos("DataReader Source").CreationName  
  
    ' Add a second ADO NET source to the data flow.  
    '  The AssemblyQualifiedName property requires a reference to the assembly.  
    Dim component2 As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    component2.Name = "DataReader Source"  
    component2.ComponentClassID = _  
      GetType(Microsoft.SqlServer.Dts.Pipeline.DataReaderSourceAdapter).AssemblyQualifiedName  
  
  End Sub  
  
End Module  
```  
  
## <a name="creating-the-design-time-instance-of-the-component"></a>创建组件的设计时实例  
 调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.Instantiate%2A> 方法可以创建由 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A> 属性标识的组件的设计时实例。 此方法将返回 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper> 对象，它是 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSDesigntimeComponent100> 接口的托管包装。  
  
 应尽可能使用设计时实例的方法而不是通过直接修改组件元数据来修改组件。 尽管元数据中存在必须直接设置的项（如连接），但是通常不建议直接修改元数据，因为这样将跳过组件监视和验证更改的功能。  
  
## <a name="assigning-connections"></a>分配连接  
 某些组件（如 OLE DB 源组件）需要连接到外部数据并使用包中的现有 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 对象来实现此分配目的。 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnectionCollection100.Count%2A> 集合的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A> 属性指示组件所需的运行时 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 对象的数量。 如果该计数大于零，则组件需要连接。 可通过指定 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100.ConnectionManager%2A> 中的第一个连接的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100.Name%2A> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A> 属性，将包中的连接管理器分配给组件。 请注意，运行时连接集合中的连接管理器的名称必须与从包中引用的连接管理器的名称匹配。  
  
## <a name="setting-the-values-of-custom-properties"></a>设置自定义属性的值  
 创建组件的设计时实例后，可调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ProvideComponentProperties%2A> 方法。 此方法类似于构造函数，因为该方法通过创建其自定义属性及其输入和输出对象来初始化新创建的组件。 请不要对组件多次调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ProvideComponentProperties%2A>，因为该组件可能会对自己进行重置，从而丢失之前对其元数据所做的所有修改。  
  
 组件的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.CustomPropertyCollection%2A> 包含特定于该组件的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100> 对象的集合。 与对象属性在对象中始终可见的其他编程模型不同，组件在您调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ProvideComponentProperties%2A> 方法时只填充其自定义属性集合。 调用该方法后，使用组件的设计时实例的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.SetComponentProperty%2A> 方法可将值分配给组件的自定义属性。 此方法接受用于标识自定义属性并提供其新值的名称/值对。  
  
## <a name="initializing-output-columns"></a>初始化输出列  
 向任务添加组件并对该组件进行配置后，将初始化对象的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> 中的列的集合。 此步骤特别适用于源组件，但是可能不会初始化转换和目标组件的列，因为它们通常依赖于从上游组件接收到的列。  
  
 调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ReinitializeMetaData%2A> 方法可初始化源组件的输出中的列。 由于组件不会自动连接到外部数据源，所以应先调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.AcquireConnections%2A> 方法，然后再调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ReinitializeMetaData%2A>，以提供对其外部数据源的组件访问以及填充组件列元数据的功能。 最后，调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ReleaseConnections%2A> 方法以释放连接。  
  
## <a name="next-step"></a>下一步  
 添加并配置组件之后，下一步是创建组件之间的路径，这将在[在两个组件之间创建路径](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md)主题中讨论。  
  
## <a name="sample"></a>示例  
 下面的代码示例向数据流任务添加 OLE DB 源组件、创建组件的设计时实例并配置组件的属性。 此示例需要对 Microsoft.SqlServer.DTSRuntimeWrap 程序集的附加引用。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Runtime.Package package = new Runtime.Package();  
      Executable e = package.Executables.Add("STOCK:PipelineTask");  
      Runtime.TaskHost thMainPipe = e as Runtime.TaskHost;  
      MainPipe dataFlowTask = thMainPipe.InnerObject as MainPipe;  
  
      // Add an OLEDB connection manager to the package.  
      ConnectionManager cm = package.Connections.Add("OLEDB");  
      cm.Name = "OLEDB ConnectionManager";  
      cm.ConnectionString = "Data Source=(local);" +   
        "Initial Catalog=AdventureWorks;Provider=SQLOLEDB.1;" +   
        "Integrated Security=SSPI;"  
  
      // Add an OLE DB source to the data flow.  
      IDTSComponentMetaData100 component =   
        dataFlowTask.ComponentMetaDataCollection.New();  
      component.Name = "OLEDBSource";  
      component.ComponentClassID = "DTSAdapter.OleDbSource.1";  
      // You can also use the CLSID of the component instead of the PROGID.  
      //component.ComponentClassID = "{2C0A8BE5-1EDC-4353-A0EF-B778599C65A0}";  
  
      // Get the design time instance of the component.  
      CManagedComponentWrapper instance = component.Instantiate();  
  
      // Initialize the component  
      instance.ProvideComponentProperties();  
  
      // Specify the connection manager.  
      if (component.RuntimeConnectionCollection.Count > 0)  
      {  
        component.RuntimeConnectionCollection[0].ConnectionManager =   
          DtsConvert.GetExtendedInterface(package.Connections[0]);  
        component.RuntimeConnectionCollection[0].ConnectionManagerID =   
          package.Connections[0].ID;      }  
  
      // Set the custom properties.  
      instance.SetComponentProperty("AccessMode", 2);  
      instance.SetComponentProperty("SqlCommand",   
        "Select * from Production.Product");  
  
      // Reinitialize the metadata.  
      instance.AcquireConnections(null);  
      instance.ReinitializeMetaData();  
      instance.ReleaseConnections();  
  
      // Add other components to the data flow and connect them.  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
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
  
    ' Add an OLEDB connection manager to the package.  
    Dim cm As ConnectionManager = package.Connections.Add("OLEDB")  
    cm.Name = "OLEDB ConnectionManager"  
    cm.ConnectionString = "Data Source=(local);" & _  
      "Initial Catalog=AdventureWorks;Provider=SQLOLEDB.1;" & _  
      "Integrated Security=SSPI;"  
  
    ' Add an OLE DB source to the data flow.  
    Dim component As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    component.Name = "OLEDBSource"  
    component.ComponentClassID = "DTSAdapter.OleDbSource.1"  
    ' You can also use the CLSID of the component instead of the PROGID.  
    'component.ComponentClassID = "{2C0A8BE5-1EDC-4353-A0EF-B778599C65A0}";  
  
    ' Get the design time instance of the component.  
    Dim instance As CManagedComponentWrapper = component.Instantiate()  
  
    ' Initialize the component.  
    instance.ProvideComponentProperties()  
  
    ' Specify the connection manager.  
    If component.RuntimeConnectionCollection.Count > 0 Then  
      component.RuntimeConnectionCollection(0).ConnectionManager = _  
        DtsConvert.GetExtendedInterface(package.Connections(0))  
      component.RuntimeConnectionCollection(0).ConnectionManagerID = _  
        package.Connections(0).ID  
    End If  
  
    ' Set the custom properties.  
    instance.SetComponentProperty("AccessMode", 2)  
    instance.SetComponentProperty("SqlCommand", _  
      "Select * from Production.Product")  
  
    ' Reinitialize the metadata.  
    instance.AcquireConnections(vbNull)  
    instance.ReinitializeMetaData()  
    instance.ReleaseConnections()  
  
    ' Add other components to the data flow and connect them.  
  
  End Sub  
  
End Module  
```  
  
## <a name="external-resources"></a>外部资源  
 blogs.msdn.com 上的博客文章 [EzAPI - 为 SQL Server 2012 更新](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ezapi-updated-for-sql-server-2012/ba-p/388042)。  

## <a name="see-also"></a>另请参阅  
 [以编程方式连接数据流组件](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md)  
  
  
