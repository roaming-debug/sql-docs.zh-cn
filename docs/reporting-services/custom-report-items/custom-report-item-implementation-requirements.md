---
title: 自定义报表项实现要求 | Microsoft Docs
description: 了解自定义报表项实现所需的开发和部署要求。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- custom report items
ms.assetid: cfacd816-00d6-4a3d-be72-1bba6f7f6886
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 95f33dde355621aecea3a67376b1d28f67b88cdc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062072"
---
# <a name="custom-report-item-implementation-requirements"></a>自定义报表项实现要求
  本主题将讨论开发和部署自定义报表项的先决条件。  
  
## <a name="development-and-deployment-requirements"></a>开发和部署要求  
 为 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 开发自定义报表项要求满足以下条件：  
  
-   对于运行 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以及 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 和 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 的服务器具有管理权限。  
  
-   [!INCLUDE[vsprvsext](../../includes/vsprvsext-md.md)] 或更高版本（已安装 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 软件开发包 (SDK)）。  
  
-   对于 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK 文档的访问权限。  
  
-   熟悉组件创作和 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 中的组件模型命名空间。  
  
## <a name="language-and-namespace-requirements"></a>语言和命名空间要求  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自定义报表项完全支持 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]。 您可以使用您选择的 .NET 兼容的语言开发自定义报表项。  
  
 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 为开发人员提供了许多工具和功能以简化和加速编码、调试和测试的迭代周期，并使部署过程更为容易。 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK 包括 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 和 C# 编译器以及相关工具。  
  
-   自定义报表项使用 Microsoft.ReportDesigner 和 <xref:Microsoft.ReportingServices.Interfaces> 命名空间  。 它们存储在作为 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的一部分安装的 Microsoft.ReportingServices.Designer.DLL 和 Microsoft.ReportingServices.Interfaces.DLL 程序集中。  
  
-   自定义报表项设计时组件需要在 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 中从 <xref:System.ComponentModel> 命名空间实现接口。 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] SDK 文档中有关于 <xref:System.ComponentModel> 的记载。  

## <a name="see-also"></a>另请参阅  
 [创建自定义报表项运行时组件](../../reporting-services/custom-report-items/creating-a-custom-report-item-run-time-component.md)   
 [创建自定义报表项设计时组件](../../reporting-services/custom-report-items/creating-a-custom-report-item-design-time-component.md)   
 [如何：部署自定义报表项](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)   
 [自定义报表项类库](../../reporting-services/custom-report-items/custom-report-item-class-libraries.md)  
  
  
