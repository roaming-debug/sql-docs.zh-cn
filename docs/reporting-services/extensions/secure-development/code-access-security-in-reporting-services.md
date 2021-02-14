---
title: Reporting Services 中的代码访问安全性 | Microsoft Docs
description: 了解 Reporting Services 中的代码访问安全性。 了解证据、代码组和命名权限集如何影响安全策略。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- code groups [Reporting Services]
- code access security [Reporting Services]
- permission sets [Reporting Services]
- evidence [Reporting Services]
- code access security [Reporting Services], about code access security
- named permission sets [Reporting Services]
ms.assetid: 97480368-1fc3-4c32-b1b0-63edfb54e472
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: dccf6bacfaf431cb69ba4ec8dc031f4e817f4650
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065282"
---
# <a name="code-access-security-in-reporting-services"></a>Reporting Services 中的代码访问安全性
  代码访问安全性以下面几个核心概念为中心：证据、代码组和命名权限集。 在 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中，报表管理器、报表设计器和报表服务器组件均有一个策略文件，该文件用来为自定义程序集配置代码访问安全性，还用来配置数据扩展插件、传递扩展插件、呈现扩展插件和安全扩展插件。 下面几节提供了代码访问安全性的概述。 有关本节中所涵盖主题的更详细信息，请参阅 [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] SDK 文档中的“安全策略模型”。  
  
 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 使用代码访问安全性的原因在于，尽管报表服务器基于 [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] 技术而构建，但是典型的 [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] 应用程序和报表服务器之间有着显著的区别。 典型的 [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] 应用程序不执行用户代码。 相比之下，[!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 使用开放式可扩展体系结构，该体系结构允许用户使用报表定义语言的 Code 元素来针对报表定义文件编程，并在自定义程序集内进行专用功能开发，以供报表使用。 此外，开发人员可以设计和部署功能强大的扩展插件来增强报表服务器的功能。 由于具有这种强大的功能和灵活性，因此将需要提供尽可能多的保护和安全性。  
  
 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 开发人员可以使用其报表中的任何 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 程序集并在本机调用部署到全局程序集缓存中的所有程序集功能。 报表服务器唯一能够控制的内容就是向报表表达式和已加载的自定义程序集授予的权限。 在 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中，自定义程序集在默认情况下接收仅 Execute 权限。  
  
## <a name="evidence"></a>证据  
 证据是公共语言运行时 (CLR) 用来为代码程序集确定安全策略的信息。 证据向公共语言运行时指示代码具有特定的特征。 证据的常见形式包括数字签名和程序集位置， 但也可以自定义设计证据来表示其他对应用程序有意义的信息。  
  
 程序集和应用程序域都接收基于证据的权限。 例如，[!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 尝试访问的程序集的位置是一种常见形式的弱名称程序集证据。 这样的证据称作 URL 证据。 部署到报表服务器上的自定义数据处理扩展插件的 URL 证据可以是“C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer\bin\Microsoft.Samples.ReportingServices.FsiDataExtension.dll”。 程序集的强名称或数字签名是另一种常见形式的证据。 在这种情况下，证据是程序集的公钥信息。  
  
## <a name="code-groups"></a>代码组  
 代码组是代码的逻辑分组，该分组具有指定的成员身份条件。 所有满足成员身份条件的代码均包括在该组中。 管理员通过管理代码组及其关联权限集来配置安全策略。  
  
 代码组的成员身份条件基于证据。 例如，代码组的 URL 成员身份基于 URL 证据。 公共语言运行时 (CLR) 使用标识特征（如 URL 证据）来描述代码并确定是否已符合组的成员身份条件。 例如，如果代码组的成员身份条件是“程序集 C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer\bin\Microsoft.Samples.ReportingServices.FsiDataExtension.dll 中的代码”，则公共语言运行时会检查证据以确定代码是否源自该位置。 这种类型的代码组的配置条目示例看上去可能如下所示：  
  
```  
<CodeGroup class="UnionCodeGroup"  
   version="1"  
   PermissionSetName="FullTrust"  
   Name="MyCodeGroup"  
   Description="Code group for my data processing extension">  
      <IMembershipCondition class="UrlMembershipCondition"  
         version="1"  
         Url="C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer\bin\Microsoft.Samples.ReportingServices.FsiDataExtension.dll"  
       />  
</CodeGroup>  
```  
  
 您应当与系统管理员或应用程序开发专家一起来确定代码访问安全性的类型以及自定义程序集或 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 扩展插件所需的代码组。  
  
## <a name="named-permission-sets"></a>命名权限集  
 命名权限集是管理员可以将其与某个代码组关联的权限集。 大多数命名权限集都至少包含一个权限、一个名称以及对该权限集的说明。 管理员可以使用命名权限集来创建或修改代码组的安全策略。 可以将多个代码组与同一个命名权限集关联。 CLR 提供内置的命名权限集，其中包括 Nothing、Execution、Internet、LocalIntranet、Everything 和 FullTrust     。  
  
> [!NOTE]  
>  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中的自定义数据扩展插件、传递扩展插件、呈现扩展插件和安全扩展插件必须以 FullTrust 权限集运行。 可以与系统管理员一起为 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 扩展插件添加适当的代码组和成员身份条件。  
  
 可以将自定义的权限级别与那些和报表一起使用的自定义程序集相关联。 例如，如果您希望允许程序集访问特定的文件，则可以新建一个具有特定文件 I/O 访问权限的命名权限集，然后将该权限集分配给您的代码组。 下面的权限集向 MyFile.xml 文件授予只读访问权限：  
  
```  
<PermissionSet class="NamedPermissionSet"  
   version="1"  
   Name="MyNewFilePermissionSet"  
   Description="A special permission set that grants read access to my file.">  
    <IPermission class="FileIOPermission"  
       version="1"  
       Read="C:\MyFile.xml"/>  
    <IPermission class="SecurityPermission"  
       version="1"  
       Flags="Assertion, Execution"/>  
</PermissionSet>  
```  
  
 被授予此权限集的代码组看上去可能如下所示：  
  
```  
<CodeGroup class="UnionCodeGroup"  
   version="1"  
   PermissionSetName="MyNewFilePermissionSet"  
   Name="MyNewCodeGroup"  
   Description="A special code group for my custom assembly.">  
   <IMembershipCondition class="UrlMembershipCondition"  
      version="1"  
      Url="C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer\bin\MyCustomAssembly.dll"/>  
</CodeGroup>  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全开发 (Reporting Services)](../../../reporting-services/extensions/secure-development/secure-development-reporting-services.md)  
  
  
