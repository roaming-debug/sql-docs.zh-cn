---
description: 开发自定义连接管理器
title: 开发自定义连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- packages [Integration Services], connections
- custom connection managers [Integration Services], about custom connection managers
- connection managers [Integration Services], custom
- Integration Services packages, connection managers
- SSIS packages, connection managers
- SQL Server Integration Services packages, connection managers
- custom connection managers [Integration Services]
ms.assetid: bda0b29e-57f5-4879-b04d-1396dc56daa8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d92ee03836944fd3a98f89c86369d2c442ee6ea5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348918"
---
# <a name="developing-a-custom-connection-manager"></a>开发自定义连接管理器

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 使用连接管理器封装连接到外部数据源所需的信息。 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 包括可支持与常用数据源（从企业数据库到文本文件和 Excel 工作表）的连接的各种连接管理器。 如果 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 支持的连接管理器和外部数据源不能完全满足您的需要，则可以创建自定义连接管理器。  
  
 若要创建自定义连接管理器，必须创建从 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase> 基类继承的类，再将 <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute> 属性应用到新类，然后重写基类的重要方法和属性，包括 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A> 属性和 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A> 方法。  
  
> [!IMPORTANT]  
>  内置于 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 的大多数任务、源和目标都只能与特定类型的内置连接管理器一起工作。 在开发用于内置任务和组件的自定义连接管理器之前，请检查这些组件是否将可用连接管理器的列表限制为特定类型的连接管理器。 如果您的解决方案需要自定义连接管理器，则还可能必须开发自定义任务或自定义源或目标以与连接管理器一起使用。  
  
## <a name="in-this-section"></a>本节内容  
 本节介绍如何创建、配置和编写自定义连接管理器及其可选自定义用户界面的代码。 本节中演示的代码段来自 SQL Server 自定义连接管理器示例。  
  
 [创建自定义连接管理器](../../../integration-services/extending-packages-custom-objects/connection-manager/creating-a-custom-connection-manager.md)  
 介绍如何为自定义连接管理器项目创建类。  
  
 [编写自定义连接管理器代码](../../../integration-services/extending-packages-custom-objects/connection-manager/coding-a-custom-connection-manager.md)  
 介绍如何通过重写基类的方法和属性实现自定义连接管理器。  
  
 [为自定义连接管理器开发用户界面](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md)  
 介绍如何实现用户界面类，以及用于配置自定义连接管理器的窗体。  
  
## <a name="related-sections"></a>相关章节  
  
### <a name="information-common-to-all-custom-objects"></a>所有自定义对象的通用信息  
 有关可以在 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 中创建的所有类型自定义对象的通用信息，请参阅以下主题：  
  
 [开发 Integration Services 的自定义对象](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)  
 介绍实现 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 的所有自定义对象类型的基本步骤。  
  
 [使自定义对象持久化](../../../integration-services/extending-packages-custom-objects/persisting-custom-objects.md)  
 介绍自定义持久性并在必要时作出解释。  
  
 [生成、部署和调试自定义对象](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)  
 介绍生成、签名、部署和调试自定义对象的技术。  
  
### <a name="information-about-other-custom-objects"></a>其他自定义对象的信息  
 有关可在 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 中创建的其他自定义对象类型的信息，请参阅以下主题：  
  
 [开发自定义任务](../../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)  
 讨论如何对自定义任务进行编程。  
  
 [开发自定义日志提供程序](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-custom-log-provider.md)  
 讨论如何对自定义日志提供程序进行编程。  
  
 [开发自定义 ForEach 枚举器](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-custom-foreach-enumerator.md)  
 讨论如何对自定义枚举器进行编程。  
  
 [开发自定义数据流组件](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)  
 讨论如何对自定义数据流源、转换和目标进行编程。  
  
