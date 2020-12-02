---
description: 为数据流组件开发用户界面
title: 为数据流组件开发用户界面 | Microsoft Docs
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
- data flow components [Integration Services], custom editors
- user interfaces [Integration Services]
- custom data flow components [Integration Services], custom editors
- custom component editors [Integration Services]
- IDtsComponentUI interface
- UITypeName property
- custom user interface [Integration Services], custom data flow component
- editors [Integration Services]
ms.assetid: 10b829a1-609b-42e3-9070-cfe5a2bb698c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 047cbf40a98c2d513c23865dc2b45e00b1fa17b1
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123152"
---
# <a name="developing-a-user-interface-for-a-data-flow-component"></a>为数据流组件开发用户界面

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  组件开发人员可以为组件提供自定义用户界面，编辑该组件时，此界面会显示在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] 中。 通过实现自定义用户界面，您可以在组件添加到数据流任务中或从数据流任务中删除以及请求该组件的帮助时获得通知。  
  
 如果没有为您的组件提供自定义用户界面，用户仍然可以配置该组件及其自定义属性，方法是使用“高级编辑器”。 您可以根据需要使用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.TypeConverter%2A> 的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.UITypeEditor%2A> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100> 属性确保高级编辑器允许用户适当地编辑自定义属性值。 有关详细信息，请参阅[数据流组件的设计时方法](../../../integration-services/extending-packages-custom-objects/data-flow/design-time-methods-of-a-data-flow-component.md)中的“创建自定义属性”。  
  
## <a name="setting-the-uitypename-property"></a>设置 UITypeName 属性  
 若要提供自定义用户界面，开发人员必须将 <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A> 的 <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute> 属性设置为实现 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI> 接口的类的名称。 组件设置此属性时，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 会在该组件在 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器中编辑时加载并调用自定义用户界面。  
  
 <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A> 属性是以逗号分隔的用于标识类型的完全限定名称的字符串。 下面的列表按顺序显示标识类型的元素：  
  
-   类型名称  
  
-   程序集名称  
  
-   文件版本  
  
-   环境  
  
-   公钥标记  
  
 下面的代码示例演示从 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> 基类派生的类，并指定 <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A> 属性。  
  
```csharp  
[DtsPipelineComponent(  
DisplayName="SampleComponent",  
UITypeName="MyNamespace.MyComponentUIClassName,MyAssemblyName,Version=1.0.0.0,Culture=neutral,PublicKeyToken=abcd...",  
ComponentType = ComponentType.Transform)]  
public class SampleComponent : PipelineComponent  
{  
//TODO: Implement the component here.  
}  
```  
  
```vb  
<DtsPipelineComponent(DisplayName="SampleComponent", _  
UITypeName="MyNamespace.MyComponentUIClassName,MyAssemblyName,Version=1.0.0.0,Culture=neutral,PublicKeyToken=abcd...", ComponentType=ComponentType.Transform)> _   
Public Class SampleComponent   
 Inherits PipelineComponent   
End Class  
```  
  
## <a name="implementing-the-idtscomponentui-interface"></a>实现 IDtsComponentUI 接口  
 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI> 接口包含在添加、删除和编辑组件时 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器调用的方法。 组件开发人员可以在其对这些方法的实现中提供代码来与组件的用户进行交互。  
  
 此类通常在独立于组件自身的程序集中实现。 虽然不是必须使用单独的程序集，但是这样做可以使开发人员单独生成和部署组件及用户界面，并保持组件的二进制程序文件较小。  
  
 通过实现自定义用户界面，当组件在 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器中被编辑时，组件开发人员可以对组件实施更多控制。 例如，组件可以将代码添加到在一个组件最初添加到数据流任务时调用的 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.New%2A> 方法中，并显示一个用于指导用户完成组件初始配置的向导。  
  
 创建实现 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI> 接口的类后，必须添加代码以响应用户与组件的交互。 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A> 方法提供组件的 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> 接口，并在调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.New%2A> 和 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A> 方法之前调用。 此引用应存储在私有成员变量中，然后用于修改组件的元数据。  
  
## <a name="modifying-a-component-and-persisting-changes"></a>修改组件并使更改持久化  
 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> 接口作为一个参数提供给 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A> 方法。 此引用应由用户界面代码缓存在成员变量中，然后用于修改组件以响应用户与用户界面的交互。  
  
 虽然可以通过 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> 接口直接修改组件，但是最好使用 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper> 方法创建一个 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.Instantiate%2A> 实例。 使用该接口直接编辑组件时，将绕过组件的验证保护。 通过 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper> 使用组件的设计时实例的优点是可以确保组件可以控制对它的更改。  
  
 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A> 方法的返回值确定对组件的更改是持久的还是被放弃。 此方法返回 false 时，所有更改都被放弃；返回 true 时，将持久化对组件的更改并将包标记为需要保存。  
  
### <a name="using-the-services-of-the-ssis-designer"></a>使用 SSIS 设计器的服务  
 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A> 方法的 IServiceProvider 参数可以访问 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器的以下服务：  
  
|服务|说明|  
|-------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Design.IDtsClipboardService>|用于确定组件是否是通过复制/粘贴或剪切/粘贴操作生成的。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionService>|用于访问包中的现有连接或在包中创建新连接。|  
|<xref:Microsoft.SqlServer.Dts.Design.IErrorCollectionService>|用于在您需要捕获组件引发的所有错误和警告而不是只接收最后一个错误或警告时，从数据流组件中捕获事件。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsVariableService>|用于访问包中的现有变量或在包中创建新变量。|  
|<xref:Microsoft.SqlServer.Dts.Design.IDtsPipelineEnvironmentService>|数据流组件使用它来访问父数据流任务以及数据流中的其他组件。 此功能可用于开发像渐变维度向导一样根据需要创建和连接其他数据流组件的组件。|  
  
 使用这些服务，组件开发人员可以访问组件所加载到的包中的对象以及在该包中创建对象。  
  
## <a name="sample"></a>示例  
 下面的代码示例演示如何将实现 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI> 接口的自定义用户界面类与用作组件编辑器的 Windows 窗体相集成。  
  
### <a name="custom-user-interface-class"></a>自定义用户界面类  
 下面的代码演示实现 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI> 接口的类。 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A> 方法创建组件编辑器，然后显示该窗体。 窗体的返回值确定对组件的更改是否是持久的。  
  
```csharp  
using System;  
using System.Windows.Forms;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Pipeline.Design;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    public class SampleComponentUI : IDtsComponentUI  
    {  
        IDTSComponentMetaData100 md;  
        IServiceProvider sp;  
  
        public void Help(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public void New(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public void Delete(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public bool Edit(System.Windows.Forms.IWin32Window parentWindow, Variables vars, Connections cons)  
        {  
            // Create and display the form for the user interface.  
            SampleComponentUIForm componentEditor = new SampleComponentUIForm(cons, vars, md);  
  
            DialogResult result  = componentEditor.ShowDialog(parentWindow);  
  
            if (result == DialogResult.OK)  
                return true;  
  
            return false;  
        }  
        public void Initialize(IDTSComponentMetaData100 dtsComponentMetadata, IServiceProvider serviceProvider)  
        {  
            // Store the component metadata.  
            this.md = dtsComponentMetadata;  
        }  
    }  
}  
```  
  
```vb  
Imports System   
Imports System.Windows.Forms   
Imports Microsoft.SqlServer.Dts.Runtime   
Imports Microsoft.SqlServer.Dts.Pipeline.Design   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
  
Namespace Microsoft.Samples.SqlServer.Dts   
  
 Public Class SampleComponentUI   
 Implements IDtsComponentUI   
   Private md As IDTSComponentMetaData100   
   Private sp As IServiceProvider   
  
   Public Sub Help(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Sub New(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Sub Delete(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Function Edit(ByVal parentWindow As System.Windows.Forms.IWin32Window, ByVal vars As Variables, ByVal cons As Connections) As Boolean   
     ' Create and display the form for the user interface.  
     Dim componentEditor As SampleComponentUIForm = New SampleComponentUIForm(cons, vars, md)   
     Dim result As DialogResult = componentEditor.ShowDialog(parentWindow)   
     If result = DialogResult.OK Then   
       Return True   
     End If   
     Return False   
   End Function   
  
   Public Sub Initialize(ByVal dtsComponentMetadata As IDTSComponentMetaData100, ByVal serviceProvider As IServiceProvider)   
     Me.md = dtsComponentMetadata   
   End Sub   
 End Class   
  
End Namespace  
```  
  
### <a name="custom-editor"></a>自定义编辑器  
 下面的代码演示在调用 <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A> 方法的过程中显示的 Windows 窗体的实现。  
  
```csharp  
using System;  
using System.Drawing;  
using System.Collections;  
using System.ComponentModel;  
using System.Windows.Forms;  
using System.Data;  
  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    public partial class SampleComponentUIForm : System.Windows.Forms.Form  
    {  
        private Connections connections;  
        private Variables variables;  
        private IDTSComponentMetaData100 metaData;  
        private CManagedComponentWrapper designTimeInstance;  
        private System.ComponentModel.IContainer components = null;  
  
        public SampleComponentUIForm( Connections cons, Variables vars, IDTSComponentMetaData100 md)  
        {  
            variables = vars;  
            connections = cons;  
            metaData = md;  
        }  
  
        private void btnOk_Click(object sender, System.EventArgs e)  
        {  
            if (designTimeInstance == null)  
                designTimeInstance = metaData.Instantiate();  
  
            designTimeInstance.SetComponentProperty( "CustomProperty", txtCustomPropertyValue.Text);  
  
            this.Close();  
        }  
  
        private void btnCancel_Click(object sender, System.EventArgs e)  
        {  
            this.Close();  
        }  
    }  
}  
```  
  
```vb  
Imports System   
Imports System.Drawing   
Imports System.Collections   
Imports System.ComponentModel   
Imports System.Windows.Forms   
Imports System.Data   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Imports Microsoft.SqlServer.Dts.Runtime   
  
Namespace Microsoft.Samples.SqlServer.Dts   
  
 Public Partial Class SampleComponentUIForm   
  Inherits System.Windows.Forms.Form   
   Private connections As Connections   
   Private variables As Variables   
   Private metaData As IDTSComponentMetaData100   
   Private designTimeInstance As CManagedComponentWrapper   
   Private components As System.ComponentModel.IContainer = Nothing   
  
   Public Sub New(ByVal cons As Connections, ByVal vars As Variables, ByVal md As IDTSComponentMetaData100)   
     variables = vars   
     connections = cons   
     metaData = md   
   End Sub   
  
   Private Sub btnOk_Click(ByVal sender As Object, ByVal e As System.EventArgs)   
     If designTimeInstance Is Nothing Then   
       designTimeInstance = metaData.Instantiate   
     End If   
     designTimeInstance.SetComponentProperty("CustomProperty", txtCustomPropertyValue.Text)   
     Me.Close   
   End Sub   
  
   Private Sub btnCancel_Click(ByVal sender As Object, ByVal e As System.EventArgs)   
     Me.Close   
   End Sub   
 End Class   
  
End Namespace  
```
  
## <a name="see-also"></a>另请参阅  
 [创建自定义数据流组件](../../../integration-services/extending-packages-custom-objects/data-flow/creating-a-custom-data-flow-component.md)  
  
  
