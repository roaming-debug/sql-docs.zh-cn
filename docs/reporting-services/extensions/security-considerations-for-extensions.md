---
title: 扩展插件的安全注意事项 | Microsoft Docs
description: 了解有关确定报表服务器如何向 Reporting Services 扩展授予权限的安全策略、条件和要求。
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- security [Reporting Services], extensions
- extensions [Reporting Services], security
- permissions [Reporting Services], extensions
ms.assetid: 58cbdfeb-1105-4a7d-a3b8-b897ff95f367
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b8994d24fe6beacfd9dc11417d529575760e7dff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065242"
---
# <a name="security-considerations-for-extensions"></a>扩展插件的安全注意事项
  每种以公共语言运行时 (CLR) 为目标的应用程序都必须与 CLR 的安全系统进行交互。 当此类应用程序运行时，CLR 将自动对它进行计算，然后向它提供一组权限。 应用程序可能会继续运行，或者生成安全性异常，具体取决于应用程序所收到的权限。 针对特定报表服务器的安全策略配置文件中的本地安全设置和策略定义程序集接收的代码权限。  
  
 在请求权限前，您需要知道扩展插件代码计划使用的资源和保护的操作，并且还需要知道哪些权限保护这些资源和操作。 此外，还需要跟踪扩展插件组件调用的所有类库方法访问的所有资源。 有关详细信息，请参阅 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 开发人员指南中的“请求权限”。  
  
 部署到报表服务器的扩展插件必须以完全信任方式运行，这意味着扩展插件需要是授予 FullTrust 权限集的代码组的一部分。 这还意味着根据为特定报表对其验证身份的用户，您的扩展插件可能还要具有对通过 CLR 提供的某些服务器资源和操作的访问权限。 有关详细信息，请参阅 [Reporting Services 中的代码访问安全性](../../reporting-services/extensions/secure-development/code-access-security-in-reporting-services.md)。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 为其所有扩展插件强制 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 安全性。  
  
 以下条件适用于在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中部署数据处理、传递、呈现和安全扩展插件。  
  
-   只有本地管理员才具有部署某一扩展插件的权限。  
  
-   只有具有适当读取/写入权限的用户才能更改要扩展的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 组件的配置文件。  
  
-   只有特权用户才有权编辑扩展插件的安全策略文件和为其启用代码访问安全性。  
  
 有关 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中的代码访问安全性的详细信息，请参阅[安全开发 (Reporting Services)](../../reporting-services/extensions/secure-development/secure-development-reporting-services.md)。  
  
 有关 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 安全性的详细信息，请参阅 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 开发人员指南中的“.NET Framework 安全性”。  
  
## <a name="initialization-of-extension-assemblies"></a>扩展插件程序集的初始化  
 在扩展插件由报表服务器首次加载到内存中时，它们使用服务帐户凭据，因为某些扩展插件程序集要求特定的权限来访问系统资源、读取配置文件和加载其他相关的程序集。 但在已加载和初始化程序集后，对扩展插件程序集的所有后续调用都将使用当前登录的用户帐户的凭据。  
  
## <a name="see-also"></a>另请参阅  
 [Reporting Services 扩展插件](../../reporting-services/extensions/reporting-services-extensions.md)   
 [Reporting Services 扩展插件库](../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
