---
title: 了解安全策略 | Microsoft Docs
description: 了解 Reporting Services 安全策略，这些策略指定基于代码组的程序集、扩展和报表表达式的权限。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- code groups [Reporting Services]
- code access security [Reporting Services], security policies
- Execution permission set
- custom assemblies [Reporting Services], security
- permission sets [Reporting Services]
- expressions [Reporting Services], security
- evidence [Reporting Services]
- FullTrust permission set
- security policies [Reporting Services]
- named permission sets [Reporting Services]
ms.assetid: a9bf043a-139a-4929-9a58-244815323df0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 072a2f88ce87c5330614961dc1aed842aae2e058
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065272"
---
# <a name="understanding-security-policies"></a>了解安全策略
  由报表服务器执行的任何代码必须是特定的代码访问安全策略的一部分。 这些安全策略包含多个将证据映射到一组命名权限集的代码组。 通常，代码组与在该组中为代码指定可允许权限的命名权限集关联。 运行时使用受信任主机或加载程序所提供的证据来确定代码所属的代码组，从而确定要授予代码的权限。 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 遵循由 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 公共语言运行时 (CLR) 定义的安全策略体系结构。 下面几节介绍了 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中的各种代码类型以及与它们相关联的策略规则。  
  
## <a name="report-server-assemblies"></a>报表服务器程序集  
 报表服务器程序集是指那些包含作为 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 产品的一部分的代码的程序集。 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 是用托管代码程序集编写的；所有这些程序集都采用了强命名模式（即数字签名模式）。 这些程序集的代码组是使用 StrongNameMembershipCondition 定义的，StrongNameMembershipCondition 为程序集的强名称提供基于公钥信息的证据。 代码组被授予 FullTrust 权限集。  
  
## <a name="report-server-extensions-rendering-data-delivery-and-security"></a>报表服务器扩展插件（呈现扩展插件、数据扩展插件、传递扩展插件和安全扩展插件）  
 报表服务器扩展插件是您或第三方为了扩展 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 的功能而创建的自定义数据扩展插件、传递扩展插件、呈现扩展插件和安全扩展插件。 在与要扩展的 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 组件相关联的策略配置文件中，必须向这些扩展插件或程序集代码授予 FullTrust。 随 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 提供的扩展插件是用报表服务器公钥签名的，它们将接收 FullTrust 权限集。  
  
> [!IMPORTANT]  
>  必须修改 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 策略配置文件才能允许任何第三方扩展插件获得 FullTrust。 如果没有为自定义扩展插件添加具有 FullTrust 的代码组，则这些扩展插件将无法由报表服务器使用。  
  
 有关 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中安全策略配置文件的详细信息，请参阅[使用 Reporting Services 安全策略文件](../../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md)。  
  
## <a name="expressions-used-in-reports"></a>报表中使用的表达式  
 报表表达式是包含在 Code 元素中的内联代码表达式或用户定义方法，该元素位于报表定义语言文件中。 策略配置文件中已经配置了一个代码组，在默认情况下，该代码组向这些表达式授予 Execution 权限集。 该代码组看起来如下所示：  
  
```  
<CodeGroup  
   class="UnionCodeGroup"  
   version="1"  
   PermissionSetName="Execution"  
   Name="Report_Expressions_Default_Permissions"  
   Description="This code group grants default permissions for code in report expressions and Code element. ">  
    <IMembershipCondition  
       class="StrongNameMembershipCondition"  
       version="1"  
       PublicKeyBlob="002400..."  
    />  
</CodeGroup>  
```  
  
 Execution 权限集允许运行（执行）代码，但是不使用受保护的资源。 在报表中找到的所有表达式都编译为一个称作“表达式宿主”的程序集，该程序集作为已编译报表的一部分存储。 在执行报表时，报表服务器会加载并调用表达式宿主程序集以执行相应的表达式。 对表达式宿主程序集进行签名所用的密钥与为所有的表达式宿主定义代码组所用的密钥相同。  
  
 报表表达式引用报表对象模型集合（字段、参数等）并执行简单任务（如算术和字符串运算）。 用来执行这些简单操作的代码只需要 Execution 权限。 在 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中，Code 元素中的用户定义方法以及任何自定义程序集在默认情况下都被授予 Execution 权限 。 因此，对于大多数表达式来说，当前的配置不要求您修改任何安全策略文件。 若要向表达式宿主程序集授予额外的权限，管理员需要修改报表服务器和报表设计器的策略配置文件，并更改报表表达式代码组。 由于权限是全局设置，因此更改表达式宿主的默认权限会影响所有报表。 因此，极力建议您将需要额外安全性的所有代码放在一个自定义程序集内。 只有此程序集将被授予所需的权限。  
  
> [!IMPORTANT]  
>  用来调用外部程序集或受保护资源的代码应当合并到一个自定义程序集内以供报表使用， 这会使您对代码所请求和断言的权限进行更多的控制。 不应当为了保护 Code 元素中的方法而进行调用。 因为这样做会要求你向报表表达式宿主授予 FullTrust，并向所有的自定义代码授予对 CLR 的完全访问权限。  
  
> [!CAUTION]  
>  请不要向报表表达式宿主的代码组授予 FullTrust。 否则将允许所有的报表表达式发出受保护的系统调用。  
  
## <a name="custom-assemblies-referenced-in-reports"></a>报表中引用的自定义程序集  
 某些报表表达式可以调用第三方代码程序集（在 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中又称为自定义程序集）。 报表服务器要求策略配置文件中的这些程序集至少具有 Execution 权限。 默认情况下，[!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 随附的策略文件会从“我的电脑”区域开始，向所有的程序集授予 Execution 权限。 您可以根据需要向自定义程序集授予额外的权限。  
  
 在某些情况下，您可能需要执行某个要求报表表达式中具有特定代码权限的操作。 通常，这意味着报表表达式需要调用受保护的 CLR 库方法（如用来访问文件或系统注册表的方法）。 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 文档介绍了发出此安全调用所需的代码权限；若要执行此调用，必须向调用代码授予这些特定的安全权限。 如果从报表表达式或 Code 元素发出调用，则必须向表达式宿主程序集授予相应的权限。 但是，在向表达式宿主授予相应的权限之后，在任何报表中的任何表达式中运行的所有代码现在都将被授予此特定权限。 从自定义程序集发出调用，并向该自定义程序集授予特定权限的操作要安全得多。  
  
## <a name="see-also"></a>另请参阅  
 [Reporting Services 中的代码访问安全性](../../../reporting-services/extensions/secure-development/code-access-security-in-reporting-services.md)   
 [安全开发 (Reporting Services)](../../../reporting-services/extensions/secure-development/secure-development-reporting-services.md)  
  
  
