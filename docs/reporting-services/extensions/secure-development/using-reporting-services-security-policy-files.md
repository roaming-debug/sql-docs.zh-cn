---
title: 使用 Reporting Services 安全策略文件 | Microsoft Docs
description: 了解 Reporting Services 用于存储代码程序集的安全策略信息的配置文件。 了解如何修改它们。
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- code groups [Reporting Services]
- CodeGroup elements
- configuration files [Reporting Services]
- code access security [Reporting Services], security policies
- security policies [Reporting Services]
- security configuration files [Reporting Services]
- named permission sets [Reporting Services]
ms.assetid: 2280fff6-3de7-44b1-87da-5db0ec975928
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e5f82b3db5f52656e88c54062f203d273d8ab24c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065262"
---
# <a name="using-reporting-services-security-policy-files"></a>使用 Reporting Services 安全策略文件
  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 将组件安全策略信息存储在三个配置文件中，而这三个配置文件会在安装过程中复制到文件系统。 这些配置文件可以包含 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中代码程序集的内部使用安全策略和用户定义安全策略的组合。 三个配置文件与 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中的三个安全对象组件相对应：报表服务器和 Windows 服务、报表管理器 Web 应用程序以及报表设计器预览窗口。  
  
> [!NOTE]  
>  报表设计器有两种预览模式：预览选项卡以及在以 DebugLocal 模式启动报表项目时启动的弹出式预览窗口。 “预览”选项卡不是安全对象组件，不应用安全策略设置。 预览窗口旨在模拟报表服务器的功能，因此具有策略配置文件，您或管理员必须修改该文件才能在报表设计器中使用自定义程序集和自定义扩展插件。  
  
 这些安全策略配置文件包含 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中的程序集的安全类信息、一些默认的命名权限集以及代码组。 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 的策略配置文件与 Security.config 文件相似，该 Security.config 文件确定与计算机和 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 中的企业级策略关联的代码组层次结构和权限集。 该文件的位置是 C:\WINDOWS\Microsoft.NET\Framework\v2.0.50727\CONFIG\security.config。  
  
## <a name="policy-files-in-reporting-services"></a>Reporting Services 中的策略文件  
 下表列出了 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中的策略配置文件、文件位置（假定默认安装）及其各自的功能。  
  
|文件名|位置（默认安装）|说明|  
|---------------|---------------------------------------|-----------------|  
|rssrvpolicy.config|C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer|报表服务器策略配置文件。 在将报表部署到报表服务器之后，这些安全策略主要影响报表表达式和自定义程序集。 此策略文件还影响部署到报表服务器的自定义数据、传递、呈现和安全扩展插件。|  
|rsmgrpolicy.config|C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportManager|报表管理器策略配置文件。 这些安全策略影响扩展报表管理器的所有程序集，例如用于自定义传递的订阅用户界面扩展插件。|  
|rspreviewpolicy.config|C:\Program Files\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies|报表设计器独立预览策略配置文件。 这些安全策略影响预览和开发期间报表中使用的自定义程序集和报表表达式。 这些策略还影响部署到报表设计器的自定义扩展插件，例如事件处理扩展插件。|  
  
## <a name="modifying-configuration-files"></a>修改配置文件  
 将配置设置指定为 XML 元素或属性。 如果您了解 XML 和配置文件，则可以使用文本编辑器或代码编辑器来修改可以由用户定义的设置。 安全配置文件包含有关与 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中策略级别关联的代码组层次结构和权限集的信息。 建议先使用 .NET Framework 配置实用工具 (Mscorcfg.msc) 或代码访问安全性策略实用工具 (Caspol.exe) 来修改 Security.config 文件中的安全策略，以确保策略更改与策略文件的有效 XML 配置元素相对应。 完成该操作后，可以从 Security.config 中剪切新代码组和权限集并粘贴到要向其添加代码权限的组件的策略文件。  
  
> [!IMPORTANT]  
>  进行更改之前应备份策略配置文件。  
  
 使用此方法可完成两个任务。 首先，此方法使您能够使用可视化工具来为 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 生成代码组和权限集。 这比从头开始编写 XML 配置元素容易得多。 其次，此方法确保不会用格式不正确的 XML 元素和属性损坏安全策略配置文件。 有关代码访问安全性策略实用工具的详细信息，请参阅 MSDN 上的“Using Reporting Services Security Policy Files”（使用 Reporting Services 安全策略文件）。  
  
 在修改策略配置文件之前，您应该阅读本节和相关主题中的所有信息。 修改 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 的策略配置可能对 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 组件如何执行外部代码模块具有重大的安全影响。  
  
## <a name="placement-of-codegroup-elements-for-extensions"></a>扩展插件的 CodeGroup 元素的位置  
 安全策略文件中 CodeGroup 元素的位置非常重要。 对于您开发的扩展插件和自定义程序集，建议将自定义代码组紧靠在 URL 成员身份“$CodeGen$/*”的现有条目的下面，如下所示：  
  
```  
<CodeGroup  
    class="UnionCodeGroup"  
    version="1"  
    PermissionSetName="FullTrust">  
    <IMembershipCondition   
        class="UrlMembershipCondition"  
        version="1"  
        Url="$CodeGen$/*"  
    />  
</CodeGroup>  
<CodeGroup   
    class="UnionCodeGroup"  
    version="1"  
    PermissionSetName="FullTrust"  
    Name="MyCustomCodeGroup"  
    Description="Code group for my custom extension">  
        <IMembershipCondition class="UrlMembershipCondition"  
        version="1"  
        Url="C:\Program Files\Microsoft SQL Server\MSSQL\Reporting Services\ReportServer\bin\MyAssembly.dll"  
        />  
</CodeGroup>  
```  
  
 可以逐个添加其他代码组。  
  
## <a name="see-also"></a>另请参阅  
 [了解安全策略](../../../reporting-services/extensions/secure-development/understanding-security-policies.md)  
  
  
