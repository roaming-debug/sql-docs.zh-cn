---
description: 创建自定义 Foreach 枚举器
title: 创建自定义 Foreach 枚举器 | Microsoft Docs
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
- custom foreach enumerators [Integration Services], creating
ms.assetid: 050e8455-2ed0-4b6d-b3ea-4e80e6c28487
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ab6fe83aca3ddcbf5aad1826016bdc2f017537d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350278"
---
# <a name="creating-a-custom-foreach-enumerator"></a>创建自定义 Foreach 枚举器

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  创建自定义 foreach 枚举器的步骤与创建其他任何 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 自定义对象的步骤相似。  
  
-   创建一个从基类继承的新类。 对于 foreach 枚举器，基类是 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator>。  
  
-   将标识对象类型的属性应用于该类。 对于 foreach 枚举器，该属性为 <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute>。  
  
-   替代基类的方法和属性的实现。 对于 foreach 枚举器，最重要的是 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A> 方法。  
  
-   可以选择开发自定义用户界面。 对于 foreach 枚举器，这需要实现 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSForEachEnumeratorUI> 接口的类。  
  
 自定义枚举器由 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop> 容器承载。 在运行时，<xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop> 容器会调用自定义枚举器的 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A> 方法。 自定义枚举器会返回一个实现 IEnumerable 接口的对象，如 ArrayList。 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop> 随后循环访问该集合中的每个元素，并通过一个用户定义的变量将当前元素的值提供给控制流，然后在容器中执行控制流。  
  
## <a name="getting-started-with-a-custom-foreach-enumerator"></a>自定义 ForEach 枚举器入门  
  
### <a name="creating-projects-and-classes"></a>创建项目和类  
 由于所有托管 foreach 枚举器都是从基类 <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator> 派生的，因此创建自定义 foreach 枚举器的第一步是创建一个用您首选的托管编程语言编写的类库项目，然后创建一个从该基类继承的类。 在此派生类中重写基类的属性和方法可实现您的自定义功能。  
  
 在同一解决方案中，创建另一个类库项目，用于自定义用户界面。 为便于部署，推荐对用户界面使用单独的程序集，因为这样，您就可以分别更新和重新部署 foreach 枚举器或其用户界面。  
  
 将这两个项目配置为使用强名称密钥文件对在生成时产生的程序集进行签名。  
  
### <a name="applying-the-dtsforeachenumerator-attribute"></a>应用 DtsForEachEnumerator 属性  
 将 <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute> 属性应用于您创建的类以将其标识为 foreach 枚举器。 此属性提供设计时信息，如 foreach 枚举器的名称和说明。 Name 属性显示在“Foreach 循环编辑器”对话框的“集合”选项卡上的可用枚举器下拉列表中。  
  
 使用 <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute.UITypeName%2A> 属性链接 foreach 枚举器与其自定义用户界面。 要获取此属性所需的公钥令牌，可使用 sn.exe -t 来显示要用于对用户界面程序集签名的密钥对 (.snk) 文件中的公钥令牌。  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
Namespace Microsoft.Samples.SqlServer.Dts  
    <DtsForEachEnumerator(DisplayName = "MyEnumerator", Description="A sample custom enumerator", UITypeName="FullyQualifiedTypeName,AssemblyName,Version=1.00.000.00,Culture=Neutral,PublicKeyToken=<publickeytoken>")> _   
    Public Class MyEnumerator  
     Inherits ForEachEnumerator  
        'Insert code here.  
    End Class  
End Namespace  
```  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsForEachEnumerator( DisplayName = "MyEnumerator", Description="A sample custom enumerator", UITypeName="FullyQualifiedTypeName,AssemblyName,Version=1.00.000.00,Culture=Neutral,PublicKeyToken=<publickeytoken>")]  
    public class MyEnumerator : ForEachEnumerator  
    {  
        //Insert code here.  
    }  
}  
```  
  
## <a name="building-deploying-and-debugging-a-custom-enumerator"></a>生成、部署和调试自定义枚举器  
 在 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 中生成、部署和调试自定义 foreach 枚举器的步骤与其他自定义对象类型所需的步骤非常相似。 有关详细信息，请参阅[生成、部署和调试自定义对象](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)。  
  
## <a name="see-also"></a>另请参阅  
 [编写自定义 Foreach 枚举器代码](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/coding-a-custom-foreach-enumerator.md)   
 [为自定义 ForEach 枚举器开发用户界面](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-user-interface-for-a-custom-foreach-enumerator.md)  
  
  
