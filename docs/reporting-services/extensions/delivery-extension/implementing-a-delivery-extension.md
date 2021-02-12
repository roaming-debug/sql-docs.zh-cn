---
title: 实现传递扩展插件 | Microsoft Docs
description: 阅读概述，了解如何通过实现自定义传递扩展插件，在 Reporting Services 中扩展传递的功能。
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- delivery [Reporting Services]
- custom delivery extensions [Reporting Services]
- extensions [Reporting Services], delivery
- delivery extensions [Reporting Services]
ms.assetid: 600cd229-efcd-480e-8c95-3c3c39ff4e7a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 902d5d19e4073cfcaf41f62c702da9ce59a191ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064482"
---
# <a name="implementing-a-delivery-extension"></a>实现传递扩展插件
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 使得用户能够创建和发布报表，一旦创建和发布，就可以将报表传递到不同位置。 此外，[!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 包括多个传递扩展插件和一个传递 API，它们使得开发人员能够创建其他传递扩展插件，以便在 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中进一步扩展传递的功能。  
  
 有关传递扩展插件的示例实现，请参阅 [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889)（SQL Server Reporting Services 产品示例）。  
  
## <a name="in-this-section"></a>本节内容  
 [传递扩展插件概述](../../../reporting-services/extensions/delivery-extension/delivery-extensions-overview.md)  
 介绍如何为 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 编写自定义传递扩展插件。  
  
 [准备实现传递扩展插件](../../../reporting-services/extensions/delivery-extension/preparing-to-implement-a-delivery-extension.md)  
 介绍在实现 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 传递扩展插件时可用的接口和类，以及在实现前要考虑的问题。  
  
 [创建传递扩展插件库](../../../reporting-services/extensions/delivery-extension/creating-a-delivery-extension-library.md)  
 介绍如何为 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 传递扩展插件分配命名空间以及如何将传递扩展插件编译成库 DLL。  
  
 [为传递扩展插件实现 IDeliveryExtension 接口](../../../reporting-services/extensions/delivery-extension/implementing-the-ideliveryextension-interface-for-a-delivery-extension.md)  
 介绍传递扩展插件的属性以及如何实现您自己的传递扩展插件类。  
  
 [将通知类用于传递扩展插件](../../../reporting-services/extensions/delivery-extension/using-a-notification-class-for-a-delivery-extension.md)  
 介绍 Notification 类的属性以及如何在传递扩展插件实现中使用该类。  
  
 [将 Setting 类用于传递扩展插件](../../../reporting-services/extensions/delivery-extension/using-the-setting-class-for-a-delivery-extension.md)  
 介绍 Setting 类的属性以及如何在传递扩展插件实现中使用该类。  
  
 [将 Report 类用于传递扩展插件](../../../reporting-services/extensions/delivery-extension/using-the-report-class-for-a-delivery-extension.md)  
 介绍 Report 类的属性以及如何在传递扩展插件实现中使用该类。  
  
 [将 RenderedOutputFile 类用于传递扩展插件](../../../reporting-services/extensions/delivery-extension/using-the-renderedoutputfile-class-for-a-delivery-extension.md)  
 介绍 RenderedOutputFile 类的属性以及如何在传递扩展插件实现中使用该类。  
  
 [部署传递扩展插件](../../../reporting-services/extensions/delivery-extension/deploying-a-delivery-extension.md)  
 介绍如何部署传递扩展插件。  
  
 [调试传递扩展插件代码](../../../reporting-services/extensions/delivery-extension/debugging-delivery-extension-code.md)  
 介绍如何调试传递扩展插件中的代码。  
  
 [删除传递扩展插件](../../../reporting-services/extensions/delivery-extension/removing-a-delivery-extension.md)  
 说明如何从报表服务器上删除传递扩展插件。  
  
## <a name="see-also"></a>另请参阅  
 [Reporting Services 扩展插件](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Reporting Services 扩展插件库](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
