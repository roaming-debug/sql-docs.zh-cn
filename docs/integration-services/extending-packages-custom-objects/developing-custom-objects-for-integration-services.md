---
description: 开发 Integration Services 的自定义对象
title: 开发 Integration Services 的自定义对象 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom user interface [Integration Services]
- custom objects [Integration Services]
ms.assetid: ca1929a6-0ae6-47d7-b65f-08173b143720
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 15f7d17f7ea8f5a8736052bd43f9b09352597ae2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350326"
---
# <a name="developing-custom-objects-for-integration-services"></a>开发 Integration Services 的自定义对象

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  当随 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供的控制流和数据流对象不能完全满足你的要求时，可以自己开发各种类型的自定义对象，其中包括：  
  
-   **自定义任务**。  
  
-   **自定义连接管理器。** 连接到当前不支持的外部数据源。  
  
-   **自定义日志提供程序。** 以当前不支持的格式记录包事件。  
  
-   **自定义枚举器。** 支持循环访问当前不支持的一组对象或值格式。  
  
-   **自定义数据流组件。** 可配置为源、转换或目标。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型通过一些可为自定义实现提供一致可靠的框架的基类使此自定义开发更加方便。  
  
 如果不需要在多个包中重用自定义功能，则使用脚本任务和脚本组件可充分利用托管编程语言的强大功能，显著减少要编写的基础结构代码。 有关详细信息，请参阅[比较脚本解决方案与自定义对象](../../integration-services/extending-packages-scripting/comparing-scripting-solutions-and-custom-objects.md)。  
  
## <a name="steps-in-developing-a-custom-object-for-integration-services"></a>开发 Integration Services 的自定义对象的步骤  
 开发用于 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的自定义对象时，将开发一个 SSIS 设计器和 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 运行时在设计时和运行时加载的类库（一个 DLL）。 您必须实现的最重要的方法不是从自己的代码调用的方法，而是适当次数的运行库调用，以初始化并验证您的组件和调用其功能的方法。  
  
 下面是在开发自定义对象过程中应遵循的步骤：  
  
1.  使用首选的托管编程语言创建一个类库类型的新项目。  
  
2.  从相应的基类继承，如下表所示。  
  
3.  将相应的属性应用到新类，如下表所示。  
  
4.  根据需要重写基类的方法，并编写对象的自定义功能的代码。  
  
5.  也可以为您的组件生成自定义用户界面。 为了便于部署，您可能会希望在同一解决方案中将用户界面作为独立项目开发，并生成为独立的程序集。  
  
6.  （可选）在“SSIS 工具箱”中显示指向自定义对象的示例和帮助内容的链接。  
  
7.  如[生成、部署和调试自定义对象](../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)中所述，生成、部署和调试新的自定义对象。  
  
## <a name="base-classes-attributes-and-important-methods"></a>基类、属性和重要方法  
 对于您可开发的每种类型的自定义对象，本表提供了 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型中最为重要的元素的简单引用。  
  
|自定义对象|基类|Attribute|重要方法|  
|-------------------|----------------|---------------|-----------------------|  
|任务|<xref:Microsoft.SqlServer.Dts.Runtime.Task>|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>|<xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A>|  
|“ODBC 源编辑器”|<xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase>|<xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute>|<xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ReleaseConnection%2A>|  
|日志提供程序|<xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase>|<xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute>|<xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.OpenLog%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.Log%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.LogProviderBase.CloseLog%2A>|  
|枚举器|<xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator>|<xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute>|<xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A>|  
|数据流组件|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>|<xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute>|<xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>|  
  
## <a name="providing-links-to-samples-and-help-content"></a>提供指向示例和帮助内容的链接  
 若要在“SSIS 工具箱”中显示指向用托管代码编写的自定义对象的示例和帮助内容的链接，请使用以下属性。  
  
-   [Microsoft.SqlServer.Dts.Pipeline.DTSPipelineComponentAttribute.SamplesTag*](/dotnet/api/microsoft.sqlserver.dts.pipeline.dtspipelinecomponentattribute.samplestag)  
  
-   [Microsoft.SqlServer.Dts.Pipeline.DTSPipelineComponentAttribute.HelpCollection*](/dotnet/api/microsoft.sqlserver.dts.pipeline.dtspipelinecomponentattribute.helpcollection)  
  
-   [Microsoft.SqlServer.Dts.Pipeline.DTSPipelineComponentAttribute.HelpKeyword*](/dotnet/api/microsoft.sqlserver.dts.pipeline.dtspipelinecomponentattribute.helpkeyword)  
  
-   [Microsoft.SqlServer.Dts.Runtime.DTSTaskAttribute.SamplesTag*](/dotnet/api/microsoft.sqlserver.dts.runtime.dtstaskattribute.samplestag)  
  
-   [Microsoft.SqlServer.Dts.Runtime.DTSTaskAttribute.HelpCollection*](/dotnet/api/microsoft.sqlserver.dts.runtime.dtstaskattribute.helpcollection)  
  
-   [Microsoft.SqlServer.Dts.Runtime.DTSTaskAttribute.HelpKeyword*](/dotnet/api/microsoft.sqlserver.dts.runtime.dtstaskattribute.helpkeyword)  
  
 若要显示指向用本机代码编写的自定义对象的示例和帮助内容的链接，请在注册表脚本 (.rgs) 文件中为 SamplesTag、HelpKeyword 和 HelpCollection 添加项。 下面是一个示例。  
  
 `val HelpKeyword = s 'sql11.dts.designer.executepackagetask.F1'`  
  
 `val SamplesTag = s 'ExecutePackageTask'`  
  
## <a name="providing-a-custom-user-interface"></a>提供自定义用户界面  
 为了允许自定义对象的用户配置自定义对象的属性，您可能还需要开发一个自定义用户界面。 对于那些自定义用户界面要求不是很严格的情况，您可选择创建一个自定义用户界面，以提供比默认编辑器更加友好的用户界面。  
  
 在自定义用户界面项目或程序集中，通常有两个类：一个用于实现特定类型自定义对象的用户界面的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 接口的类和用于显示以收集用户信息的 Windows 窗体。 您实现的接口只有少量方法，因此开发自定义用户界面并不困难。  
  
> [!NOTE]  
>  许多 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 日志提供程序都有一个自定义用户界面，该界面实现 <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsLogProviderUI>，并使用已筛选的可用连接管理器下拉列表替换“配置”文本框。 但是，此版本的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中不实现自定义日志提供程序的自定义用户界面。 为 <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute.UITypeName%2A> 的 <xref:Microsoft.SqlServer.Dts.Runtime.DtsLogProviderAttribute> 属性指定值不起作用。  
  
 下表提供了对一些接口的简单引用，您在为每种类型的自定义对象开发自定义用户界面时必须实现这些接口。 该表还介绍了当你选择不为对象开发自定义用户界面，或者无法在对象的特性中使用 **UITypeName** 属性将对象链接到其用户界面时，用户所能看到的内容。 尽管功能强大的高级编辑器能够满足数据流组件的要求，但对于任务和连接管理器，“属性”窗口解决方案的用户友好性较差，并且如果没有自定义窗体，将根本就无法配置自定义 ForEach 枚举器。  
  
|自定义对象|用户界面的基类|不提自定义供用户界面时的默认编辑行为|  
|-------------------|-----------------------------------|----------------------------------------------------------------------|  
|任务|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI>|仅“属性”窗口|  
|“ODBC 源编辑器”|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI>|仅“属性”窗口|  
|日志提供程序|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsLogProviderUI><br /><br /> （未在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中实现）|“配置”列中的文本框|  
|枚举器|<xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumeratorUI>|仅“属性”窗口。 编辑器的“枚举器配置”区域为空。|  
|数据流组件|<xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>|“高级编辑器”|  
  
## <a name="external-resources"></a>外部资源  
  
-   blogs.msdn.com 上的博客文章：[Visual Studio solution build process give a warning about indirect dependency on the .NET Framework assembly due to SSIS references](/archive/blogs/jason_howell/visual-studio-2010-solution-build-process-give-a-warning-about-indirect-dependency-on-the-net-framework-assembly-due-to-ssis-references)（Visual Studio 解决方案生成流程由于 SSIS 引用而针对 .NET Framework 程序集上的间接依赖关系发出警告）。  
  
## <a name="see-also"></a>另请参阅  
 [使自定义对象持久化](../../integration-services/extending-packages-custom-objects/persisting-custom-objects.md)   
 [生成、部署和调试自定义对象](../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)  
  
