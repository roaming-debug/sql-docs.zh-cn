---
title: 自定义报表项 | Microsoft Docs
description: 了解自定义报表项以及它们如何由运行时组件和设计时组件组成。
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- extending Reporting Services
- Reporting Services, extending
- custom report items
ms.assetid: 64dcaf2c-1af5-4937-8ff7-98f1ec3b367e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6ca390146b560e8de105661097637a42ff3ddfea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061922"
---
# <a name="custom-report-items"></a>自定义报表项
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 提供大量工具，用于生成和发布企业报表，管理安全性和订阅，以及通过全面的 API 扩展报表功能。 报表使用称作报表定义语言 (RDL) 的基于 XML 的语言定义。 RDL 提供一组指令，用于描述报表的布局、查询信息和项类型。 可以通过编写自定义报表项来扩展 RDL。 自定义报表项由运行时组件（由报表处理器在运行时调用）和设计时组件（允许在报表设计器中使用该自定义报表项）构成。  
  
 有关完全实现的自定义报表项的示例，请参阅 [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889)（SQL Server Reporting Services 产品示例）。  
  
## <a name="custom-report-item-scenarios"></a>自定义报表项应用场景  
 需要将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 集成到其应用程序中的开发人员可能要求在 RDL 中不固有支持的功能。 这可能包括如下项：映射控件、水平列表、垂直列表和透视表矩阵。 可以开发运行时自定义报表项组件并向应用程序分发，以便满足此需求。  
  
 除了提供不固有支持的功能外，某些开发人员可能要通过随 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 一起提供的控件的替代版本扩展现有功能。 在此应用场景中，开发人员可以提供三个组件：运行时组件、设计时组件和在需要时将现有报表项转换为自定义报表项的设计时报表项转换组件。  
  
## <a name="in-this-section"></a>本节内容  
 [自定义报表项体系结构](../../reporting-services/custom-report-items/custom-report-item-architecture.md)  
 描述组成自定义报表项的组件。  
  
 [自定义报表项实现要求](../../reporting-services/custom-report-items/custom-report-item-implementation-requirements.md)  
 描述用于创建自定义报表项的先决条件。  
  
 [创建自定义报表项运行时组件](../../reporting-services/custom-report-items/creating-a-custom-report-item-run-time-component.md)  
 描述如何创建自定义报表项运行时组件。  
  
 [创建自定义报表项设计时组件](../../reporting-services/custom-report-items/creating-a-custom-report-item-design-time-component.md)  
 描述如何创建自定义报表项设计时组件。  
  
 [如何：部署自定义报表项](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)  
 描述如何部署自定义报表项。  
  
 [自定义报表项类库](../../reporting-services/custom-report-items/custom-report-item-class-libraries.md)  
 描述 Microsoft.ReportDesigner 命名空间中的自定义报表项基础结构类和托管包装类  。  
  
## <a name="see-also"></a>另请参阅  
 [技术参考 (SSRS)](../../reporting-services/technical-reference-ssrs.md)  
  
  
