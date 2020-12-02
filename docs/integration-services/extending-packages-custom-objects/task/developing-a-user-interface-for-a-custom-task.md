---
description: 为自定义任务开发用户界面
title: 为自定义任务开发用户界面 | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom user interfaces [Integration Services]
- IDtsTaskUI interface
- DtsTaskAttribute attribute
- custom tasks [Integration Services], user interface
- custom user interface [Integration Services], custom tasks
- user interface [Integration Services]
- SSIS custom tasks, user interface
ms.assetid: 1e940cd1-c5f8-4527-b678-e89ba5dc398a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2819e52dfe651cb7fde80560c3edd28745376179
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123059"
---
# <a name="developing-a-user-interface-for-a-custom-task"></a>为自定义任务开发用户界面

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  自定义任务开发人员使用 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 对象模型可以轻松地为任务创建自定义用户界面，该任务随后可以集成并显示在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] 中。 用户界面可在 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器中向用户提供有用的信息，并指导用户正确配置自定义任务的属性和设置。  
  
 为任务开发自定义用户界面包括使用两个重要的类。 下表介绍了这两个类。  
  
|类|说明|  
|-----------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>|标识托管任务的特性，该特性通过其属性提供设计时信息以控制 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器显示对象和与对象交互的方式。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI>|任务所用的接口，用来将任务与其自定义用户界面相关联。|  
  
 本节介绍为自定义任务开发用户界面时，<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> 特性和 <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI> 接口的角色，并提供有关如何在 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器中创建、集成、部署和调试任务的详细信息。  
  
 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器为任务提供用户界面的多个入口点：用户可以从快捷菜单中选择“编辑”，双击任务，或者单击属性表底部的“显示编辑器”链接。 当用户访问其中一个入口点时，[!INCLUDE[ssIS](../../../includes/ssis-md.md)] 编辑器会定位并加载包含该任务的用户界面的程序集。 任务的用户界面负责创建在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] 中向用户显示的属性对话框。  
  
 任务及其用户界面是不同的实体。 应该在不同的程序集中实现它们，以减少本地化、部署和维护的工作。 除了在任务中编码的 <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> 特性值中包含的信息外，任务 DLL 不会加载、调用或通常的包含其用户界面的任何知识。 此信息是任务与其用户界面相关联的唯一方式。  
  
## <a name="the-dtstask-attribute"></a>DtsTask 特性  
 <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> 特性包含在任务类代码中，用于关联任务与其用户界面。 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器使用该特性的属性来确定任务在设计器中的显示方式。 这些属性包含要显示的名称和图标（如果有）。  
  
 下表介绍了 <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> 特性的各属性。  
  
|属性|说明|  
|--------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.DisplayName%2A>|在“控制流”工具箱中显示任务名称。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.Description%2A>|任务说明（继承自 <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute>）。 此属性显示在工具提示中。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A>|显示在 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器中的图标。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.RequiredProductLevel%2A>|如果使用，请将其设置为 <xref:Microsoft.SqlServer.Dts.Runtime.DTSProductLevel> 枚举中的一个值。 例如，`RequiredProductLevel = DTSProductLevel.None`。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.TaskContact%2A>|保存联系信息，以备任务需要技术支持。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.TaskType%2A>|为任务分配类型。|  
|Attribute.TypeId|在派生类中实现时，获取此特性的唯一标识符。 有关详细信息，请参阅 .NET Framework 类库中的 **Attribute.TypeID** 属性。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.UITypeName%2A>|[!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器用于加载程序集的程序集类型名。 此属性用于查找任务的用户界面程序集。|  
  
 下面的代码示例演示按照上面的类定义进行编码以得到所需显示效果的 <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
namespace Microsoft.SSIS.Samples  
{  
  [DtsTask  
  (  
   DisplayName = "MyTask",  
   IconResource = "MyTask.MyTaskIcon.ico",  
   UITypeName = "My Custom Task," +  
   "Version=1.0.0.0," +  
   "Culture = Neutral," +  
   "PublicKeyToken = 12345abc6789de01",  
   TaskType = "PackageMaintenance",  
   TaskContact = "MyTask; company name; any other information",  
   RequiredProductLevel = DTSProductLevel.None  
   )]  
  public class MyTask : Task  
  {  
    // Your code here.  
  }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsTask(DisplayName:="MyTask", _  
 IconResource:="MyTask.MyTaskIcon.ico", _  
 UITypeName:="My Custom Task," & _  
 "Version=1.0.0.0,Culture=Neutral," & _  
 "PublicKeyToken=12345abc6789de01", _  
 TaskType:="PackageMaintenance", _  
 TaskContact:="MyTask; company name; any other information", _  
 RequiredProductLevel:=DTSProductLevel.None)> _  
Public Class MyTask  
  Inherits Task  
  
  ' Your code here.  
  
End Class 'MyTask  
```  
  
 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器使用包含程序集名称、类型名称、版本、区域性和公钥标记的特性的 <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.UITypeName%2A> 属性在全局程序集缓存 (GAC) 中查找程序集并加载该程序集，以供设计器使用。  
  
 找到程序集后，[!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器使用该特性中的其他属性在 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器中显示任务的其他信息，例如任务的名称、图标和说明。  
  
 <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.DisplayName%2A>、<xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.Description%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A> 属性指定将任务呈现给用户的方式。 <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A> 属性包含嵌入在用户界面程序集中的图标的资源 ID。 设计器根据 ID 从程序集中加载图标资源，并且在任务添加到包后，将其显示在工具箱中和设计器图面上任务名称的旁边。 如果任务未提供图标资源，设计器将对该任务使用默认图标。  
  
## <a name="the-idtstaskui-interface"></a>IDTSTaskUI 接口  
 <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI> 接口定义 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器为初始化和显示与任务关联的用户界面而调用的方法和属性的集合。 调用任务的用户界面时，设计器调用 <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.Initialize%2A> 方法（该方法在您写入时由任务用户界面实现），然后分别提供任务和包的 <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> 集合和 <xref:Microsoft.SqlServer.Dts.Runtime.Connections> 集合作为参数。 这些集合存储在本地，随后用在 <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A> 方法中。  
  
 设计器调用 <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A> 方法以请求显示在 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器中的窗口。 任务会创建一个包含任务的用户界面的窗口实例，并将用户界面返回设计器以供显示。 通常，<xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> 和 <xref:Microsoft.SqlServer.Dts.Runtime.Connections> 对象通过重载构造函数提供给窗口，以便可以用于配置任务。  
  
 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器调用任务 UI 的 <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A> 方法来显示任务的用户界面。 任务用户界面从此方法返回 Windows 窗体，[!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器将此窗体显示为模式对话框。 关闭窗体时，[!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器会检查该窗体的 **DialogResult** 属性值，以确定任务是否已修改以及是否应保存这些修改。 如果 **DialogResult** 属性值是 **OK**，[!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器将调用任务的持久性方法以保存更改；否则，将放弃更改。  
  
 下面的代码实例实现 <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI> 接口，并假定存在名为 SampleTaskForm 的 Windows 窗体类。  
  
```csharp  
using System;  
using System.Windows.Forms;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Runtime.Design;  
  
namespace Sample  
{  
   public class HelloWorldTaskUI : IDtsTaskUI  
   {  
      TaskHost   taskHost;  
      Connections connections;  
      public void Initialize(TaskHost taskHost, IServiceProvider serviceProvider)  
      {  
         this.taskHost = taskHost;  
         IDtsConnectionService cs = serviceProvider.GetService  
         ( typeof( IDtsConnectionService ) ) as   IDtsConnectionService;   
         this.connections = cs.GetConnections();  
      }  
      public ContainerControl GetView()  
      {  
        return new HelloWorldTaskForm(this.taskHost, this.connections);  
      }  
     public void Delete(IWin32Window parentWindow)  
     {  
     }  
     public void New(IWin32Window parentWindow)  
     {  
     }  
   }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Runtime.Design  
Imports System.Windows.Forms  
  
Public Class HelloWorldTaskUI  
  Implements IDtsTaskUI  
  
  Dim taskHost As TaskHost  
  Dim connections As Connections  
  
  Public Sub Initialize(ByVal taskHost As TaskHost, ByVal serviceProvider As IServiceProvider) _  
    Implements IDtsTaskUI.Initialize  
  
    Dim cs As IDtsConnectionService  
  
    Me.taskHost = taskHost  
    cs = DirectCast(serviceProvider.GetService(GetType(IDtsConnectionService)), IDtsConnectionService)  
    Me.connections = cs.GetConnections()  
  
  End Sub  
  
  Public Function GetView() As ContainerControl _  
    Implements IDtsTaskUI.GetView  
  
    Return New HelloWorldTaskForm(Me.taskHost, Me.connections)  
  
  End Function  
  
  Public Sub Delete(ByVal parentWindow As IWin32Window) _  
    Implements IDtsTaskUI.Delete  
  
  End Sub  
  
  Public Sub [New](ByVal parentWindow As IWin32Window) _  
    Implements IDtsTaskUI.[New]  
  
  End Sub  
  
End Class  
```  
 
## <a name="see-also"></a>另请参阅  
 [创建自定义任务](../../../integration-services/extending-packages-custom-objects/task/creating-a-custom-task.md)   
 [编写自定义任务代码](../../../integration-services/extending-packages-custom-objects/task/coding-a-custom-task.md)   
 [为自定义任务开发用户界面](../../../integration-services/extending-packages-custom-objects/task/developing-a-user-interface-for-a-custom-task.md)  
  
  
