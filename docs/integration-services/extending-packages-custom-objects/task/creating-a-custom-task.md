---
description: 创建自定义任务
title: 创建自定义任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom tasks [Integration Services], creating
ms.assetid: 42965c09-1782-4cdb-9ce1-216af4c23e0a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0fa0570ab854d9faf319fc87279e8fcd48afa452
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337676"
---
# <a name="creating-a-custom-task"></a>创建自定义任务

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  创建自定义任务的步骤与创建其他任何 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 自定义对象的步骤相似：  
  
-   创建一个从基类继承的新类。 对于任务，基类是 [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task)。  
  
-   将标识对象类型的属性应用于该类。 对于任务，该属性是 <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>。  
  
-   替代基类的方法和属性的实现。 对于任务，这些方法包括 <xref:Microsoft.SqlServer.Dts.Runtime.Task.Validate%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A>。  
  
-   可以选择开发自定义用户界面。 对于任务，这需要实现 <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI> 接口的类。  
  
## <a name="getting-started-with-a-custom-task"></a>自定义任务入门  
  
### <a name="creating-projects-and-classes"></a>创建项目和类  
 由于所有托管任务都派生自 [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task) 基类，所以创建自定义任务的第一步是创建一个用您首选的托管编程语言编写的类库项目，然后创建一个从该基类继承的类。 在此派生类中重写基类的属性和方法可实现您的自定义功能。  
  
 在同一解决方案中，创建另一个类库项目，用于自定义用户界面。 为便于部署，推荐对用户界面使用单独的程序集，因为这样，您就可以分别更新和重新部署连接管理器或其用户界面。  
  
 将这两个项目配置为使用强名称密钥文件对在生成时产生的程序集进行签名。  
  
### <a name="applying-the-dtstask-attribute"></a>应用 DtsTask 属性  
 将 <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> 属性应用于您创建的类以将其标识为任务。 此属性提供设计时信息，例如任务的名称、说明和任务类型。  
  
 使用 <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.UITypeName%2A> 属性链接任务与其自定义用户界面。 要获取此属性所需的公钥令牌，可使用 sn.exe -t 来显示要用于对用户界面程序集签名的密钥对 (.snk) 文件中的公钥令牌。  
  
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
  
## <a name="building-deploying-and-debugging-a-custom-task"></a>生成、部署和调试自定义任务  
 在 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 中生成、部署和调试自定义任务的步骤与其他自定义对象类型所需的步骤相似。 有关详细信息，请参阅[生成、部署和调试自定义对象](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)。  
  
## <a name="see-also"></a>另请参阅  
 [创建自定义任务](../../../integration-services/extending-packages-custom-objects/task/creating-a-custom-task.md)   
 [编写自定义任务代码](../../../integration-services/extending-packages-custom-objects/task/coding-a-custom-task.md)   
 [为自定义任务开发用户界面](../../../integration-services/extending-packages-custom-objects/task/developing-a-user-interface-for-a-custom-task.md)  
  
  
