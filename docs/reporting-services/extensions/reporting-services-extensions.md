---
title: Reporting Services 扩展插件 | Microsoft Docs
description: 了解如何通过自定义报表数据、通知机制和安全系统在 Reporting Services 中扩展数据处理功能。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- SQL Server Reporting Services, extending
- extensions [Reporting Services], about extensions
- SSIS, extending
- Reporting Services, extending
- extensions [Reporting Services]
ms.assetid: 2bf17ae4-2292-4a58-a1f0-56e99abd9b69
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b5572aca5e477c8738e46a628ce37f7690239173
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074888"
---
# <a name="reporting-services-extensions"></a>Reporting Services 扩展插件
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的模块化体系结构旨在实现可扩展性。 提供了一个托管代码 API，以便您能够轻松地开发、安装和管理由许多 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 组件使用的扩展插件。 可以使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 创建专用或共享的程序集，并添加新的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 功能以满足不断发展的业务需要。  
  
 通过 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的独树一帜的可扩展体系结构，开发人员可以扩展产品及其组件的特定功能。 目前，全面支持扩展 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的数据处理功能。 数据处理 API 包括大家所熟悉的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口构造和约定，使开发人员能够将附加的数据处理功能内置于 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 中。 这些数据处理扩展插件向报表服务器和报表设计器中添加功能，从而可以将自定义数据无缝集成到报表中。  
  
 另一个支持的扩展插件是传递扩展插件。 传递 API 与 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 体系结构完全集成，从而在将报表通知发送到用户时能够使用多种传递机制。 您可以扩展报表服务器以向用户提供自定义传递，并且可以扩展报表管理器的订阅管理页以便实现使用自定义传递扩展插件的订阅。  
  
 另一个报表服务器扩展插件报表定义自定义扩展插件 (RDCE) 可以在将某一报表定义传递到处理引擎前动态自定义该报表定义。 您可以基于用户或语言之类的因素自定义报表。 例如，您可能要为不同用户（例如经理或部门成员）实现不同的视图，或者可能要自定义某一报表以便在以法语或阿拉伯语呈现时具有不同的布局。  
  
## <a name="in-this-section"></a>本节内容  
 [扩展插件的安全注意事项](../../reporting-services/extensions/security-considerations-for-extensions.md)  
 介绍与开发和部署 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 扩展插件相关的安全问题。  
  
 [实现数据处理扩展插件](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)  
 介绍用于实现 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件的要求和步骤。  
  
 [实现传递扩展插件](../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)  
 介绍用于实现 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 传递扩展插件的要求和步骤。  
  
 [实现呈现扩展插件](../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md)  
 介绍如何开发呈现扩展插件。  
  
 [实现安全扩展插件](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)  
 介绍用于实现 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安全扩展插件的要求和步骤。  
  
 [Reporting Services 扩展插件库](../../reporting-services/extensions/reporting-services-extension-library.md)  
 包含用于 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 扩展功能的扩展插件 API 库的编程参考。  
  
  
