---
description: 创建自定义日志提供程序
title: 创建自定义日志提供程序 | Microsoft Docs
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
- custom log providers [Integration Services], creating
ms.assetid: fc20af96-9eb8-4195-8d3f-8a4d7c753f24
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1f6de779a67df0bdbd81086c1b34992d0fe896ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354371"
---
# <a name="creating-a-custom-log-provider"></a>创建自定义日志提供程序

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 运行时环境具有广泛的日志记录功能。 日志可使您捕获在包执行期间发生的事件。 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 包含各种日志提供程序，可用于创建日志并以诸如 XML、文本、数据库或 Windows 事件日志的格式存储这些日志。 如果这些提供程序或输出格式不能满足您的需要，您可以创建自定义日志提供程序。  
  
 创建自定义日志提供程序的步骤与创建其他任何 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 自定义对象的步骤相似：  
  
-   创建一个从基类继承的新类。 对于日志提供程序，该基类为 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>。  
  
-   将标识对象类型的属性应用于该类。 对于日志提供程序，该属性为 <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute>。  
  
-   替代基类的方法和属性的实现。 对于日志提供程序，所涉及的属性和方法包括 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.ConfigString%2A> 属性以及 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>、<xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A> 方法。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 中不实现自定义日志提供程序的自定义用户界面。  
  
## <a name="getting-started-with-a-custom-log-provider"></a>自定义日志提供程序入门  
  
### <a name="creating-projects-and-classes"></a>创建项目和类  
 由于所有托管日志提供程序都派生自 <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase> 基类，所以创建自定义日志提供程序的第一步是以您的首选托管编程语言创建一个类库项目，然后创建一个从该基类继承的类。 在此派生类中重写基类的属性和方法可实现您的自定义功能。  
  
 将此项目配置为使用强名称密钥文件对将要生成的程序集进行签名。  
  
> [!NOTE]  
>  许多 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 日志提供程序都有一个自定义用户界面，该界面实现 <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsLogProviderUI>，并用已筛选的可用连接管理器下拉列表替换“配置 SSIS 日志”对话框中的“配置”文本框。 但是，[!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 中不实现自定义日志提供程序的自定义用户界面。  
  
### <a name="applying-the-dtslogprovider-attribute"></a>应用 DtsLogProvider 属性  
 将 <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> 属性应用于您创建的类以将其标识为日志提供程序。 此属性提供设计时信息，如日志提供程序的名称和说明。 此属性的 DisplayName 和 Description 属性与“配置 SSIS 日志”对话框中显示的“名称”和“说明”列相对应，该对话框在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] 中为包配置日志记录时显示。  
  
> [!IMPORTANT]  
>  不使用此特性的 <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute.LogProviderType%2A> 属性。 但是，必须为该属性输入一个值，否则自定义日志提供程序将不会显示在可用日志提供程序列表中。  
  
> [!NOTE]  
>  由于自定义日志提供程序的自定义用户界面不在 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 中实现，因此，为 <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute.UITypeName%2A> 的 <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> 属性指定值将不起作用。  
  
```vb  
<DtsLogProvider(DisplayName:="MyLogProvider", Description:="A simple log provider.", LogProviderType:="Custom")> _  
Public Class MyLogProvider  
     Inherits LogProviderBase  
    ' TODO: Override the base class methods.  
End Class  
```  
  
```csharp  
[DtsLogProvider(DisplayName="MyLogProvider", Description="A simple log provider.", LogProviderType="Custom")]  
public class MyLogProvider : LogProviderBase  
{  
    // TODO: Override the base class methods.  
}  
```  
  
## <a name="building-deploying-and-debugging-a-custom-log-provider"></a>生成、部署和调试自定义日志提供程序  
 在 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 中生成、部署和调试自定义日志提供程序的步骤与其他自定义对象类型所需的步骤非常相似。 有关详细信息，请参阅[生成、部署和调试自定义对象](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)。  
  
## <a name="see-also"></a>另请参阅  
 [编写自定义日志提供程序代码](../../../integration-services/extending-packages-custom-objects/log-provider/coding-a-custom-log-provider.md)   
 [为自定义日志提供程序开发用户界面](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-user-interface-for-a-custom-log-provider.md)  
  
  
