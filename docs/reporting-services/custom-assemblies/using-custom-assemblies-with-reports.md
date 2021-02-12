---
title: 将自定义程序集用于报表 | Microsoft Docs
description: 使用 Microsoft .NET Framework 开发自定义代码程序集，以便你可以从报表定义文件内引用该程序集。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-assemblies
ms.topic: reference
helpviewer_keywords:
- custom assemblies [Reporting Services]
- assemblies [Reporting Services], custom
- custom assemblies [Reporting Services], about custom assemblies
ms.assetid: 53d141d0-2185-466a-84dc-7b90d284da3d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 19977f4a90ecfb8db81c53aebf7b7ee3e4be68ca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064492"
---
# <a name="using-custom-assemblies-with-reports"></a>将自定义程序集用于报表
  在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中，您可以为报表项值、样式和格式设置编写自定义代码。 例如，您可以使用自定义代码基于区域性设置货币的格式，使用特殊格式设置标记某些值，或者为您的公司应用其他实行中的业务规则。 在报表中添加此代码的一种方法是，使用可以从报表定义文件引用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 创建自定义代码程序集。 报表运行时，服务器将调用自定义程序集中的函数。 自定义程序集可用于检索您计划在报表中使用的指定函数。  
  
## <a name="in-this-section"></a>本节内容  
 [在 RDL 文件中引用程序集](../../reporting-services/custom-assemblies/referencing-assemblies-in-an-rdl-file.md)  
 介绍如何引用报表定义语言文件中的自定义程序集。  
  
 [部署自定义程序集](../../reporting-services/custom-assemblies/deploying-a-custom-assembly.md)  
 介绍如何将自定义程序集部署到报表设计器和报表服务器。  
  
 [使用具有强名称的自定义程序集](../../reporting-services/custom-assemblies/using-strong-named-custom-assemblies.md)  
 介绍如何使用具有强名称的程序集。  
  
 [在自定义程序集中断言权限](../../reporting-services/custom-assemblies/asserting-permissions-in-custom-assemblies.md)  
 介绍如何部署具有有限权限和特定权限的自定义程序集，以及如何在代码中断言这些权限。  
  
 [通过表达式访问自定义程序集](../../reporting-services/custom-assemblies/accessing-custom-assemblies-through-expressions.md)  
 介绍如何在您的报表定义中将自定义程序集方法作为报表表达式调用。  
  
 [初始化自定义程序集对象](../../reporting-services/custom-assemblies/initializing-custom-assembly-objects.md)  
 介绍如何初始化从报表调用的自定义程序集对象的值。  
  
 [如何：调试自定义程序集](../../reporting-services/custom-assemblies/how-to-debug-custom-assemblies.md)  
 介绍如何调试您的自定义程序集代码。  
  
## <a name="see-also"></a>另请参阅  
 [报表定义语言 (SSRS)](../../reporting-services/reports/report-definition-language-ssrs.md)  
  
  
