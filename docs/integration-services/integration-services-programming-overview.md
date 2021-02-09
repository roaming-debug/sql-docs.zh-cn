---
description: Integration Services 编程概述
title: Integration Services 编程概述 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Integration Services, programming
- architecture [Integration Services]
- assemblies [Integration Services]
- SSIS, programming
- SQL Server Integration Services, programming
- run-time engine [Integration Services]
- packages [Integration Services], programming
- data flow engine [Integration Services]
- languages [Integration Services]
ms.assetid: 262babc6-eea5-4609-bc65-07d64cbcfee9
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ae083f7e594025449da685ce5f18ed777f88a8a1
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835091"
---
# <a name="integration-services-programming-overview"></a>Integration Services 编程概述

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 具有将数据移动和转换与包控制流和管理分开的体系结构。 有两个截然不同的引擎定义此体系结构，对 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 进行编程可以扩展这两个引擎并使其自动化。 运行时引擎实现控制流和包管理基础结构，该基础结构使开发人员能够控制执行流并为日志记录、事件处理程序和变量设置选项。 数据流引擎是一个专用高性能引擎，专用于提取、转换和加载数据。 对 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 进行编程时，将针对这两个引擎进行编程。  
  
 下图描绘了 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 的体系结构。  
  
 ![集成服务体系结构](../integration-services/media/mw-dts-01.gif "集成服务体系结构")  
  
## <a name="integration-services-run-time-engine"></a>Integration Services 运行时引擎  
 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 运行时引擎通过实现启用执行顺序、日志记录、变量和事件处理的基础结构来控制包的管理和执行。 开发人员对 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 运行时引擎进行编程可自动化包的创建、配置和执行操作，并可创建自定义任务和其他扩展插件。  
  
 有关详细信息，请参阅[使用脚本任务扩展包](../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md)、[开发自定义任务](../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)和[以编程方式生成包](../integration-services/building-packages-programmatically/building-packages-programmatically.md)。  
  
## <a name="integration-services-data-flow-engine"></a>Integration Services 数据流引擎  
 数据流引擎管理数据流任务，是一个专门用于从不同源移动和转换数据的专用高性能任务。 不像其他任务，数据流任务包含称为数据流组件的其他对象，可以是源、转换或目标。 这些组件是移动任务组成部分的核心。 它们定义数据的移动和转换。 开发人员对数据流引擎进行编程可自动化数据流任务中组件的创建和配置，并可创建自定义组件。  
  
 有关详细信息，请参阅[使用脚本组件扩展数据流](../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md)、[开发自定义数据流组件](../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)和[以编程方式生成包](../integration-services/building-packages-programmatically/building-packages-programmatically.md)。  
  
## <a name="supported-languages"></a>支持的语言  
 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 完全支持 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)]。 这使开发人员可以自主选择符合 .NET 的语言来对 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 进行编程。 尽管运行时引擎和数据流引擎都是用本机代码编写的，但是都可以通过完全托管对象模型来使用。  
  
 可以用 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] [!INCLUDE[msCoName](../includes/msconame-md.md)] 或者另一种代码或文本编辑器对 [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 包、自定义任务和组件进行编程。 [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 为开发人员提供了许多工具和功能以简化和加速编码、调试和测试的迭代周期。 [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 还使部署更加容易。 但是，您不需要 [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 编译和生成 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 代码项目。 [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] SDK 包括 [!INCLUDE[vbprvb](../includes/vbprvb-md.md)] 和 [!INCLUDE[csprcs](../includes/csprcs-md.md)] 编译器和相关工具。  
  
> [!IMPORTANT]  
>  默认情况下，[!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] 与 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 一起安装，但不安装 [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] SDK。 除非 SDK 已安装在计算机上，并且 SDK 文档包含在联机丛书集中，否则本部分中指向 SDK 内容的链接将无效。 安装 [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] SDK 后，可以按照[添加或删除 SQL Server 的产品文档](https://www.microsoft.com/download/details.aspx?id=42557)中的说明将 SDK 文档添加到联机丛书集和目录中。  
  
 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 脚本任务和脚本组件将 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] Tools for Applications (VSTA) 用作嵌入式脚本环境。 VSTA 支持 [!INCLUDE[msCoName](../includes/msconame-md.md)] Visual Basic 和 [!INCLUDE[msCoName](../includes/msconame-md.md)] Visual C#。  
  
> [!NOTE]  
>  [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 应用程序编程接口与基于 COM 的脚本语言（如 VBScript）不兼容。  
  
## <a name="locating-assemblies"></a>定位程序集  
 在 [!INCLUDE[ssSQL19](../includes/sssql19-md.md)]中， [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 程序集已升级到 .NET 4.0。 位于 \<drive>:\Windows\Microsoft.NET\assembly 中的 .NET 4 存在单独的全局程序集缓存。 您可在此路径下找到所有 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 程序集，一般位于 GAC_MSIL 文件夹中。  
  
 与之前版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 一样，核心 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 扩展性 .dll 文件也位于 \<drive>:\Program Files\Microsoft SQL Server\100\SDK\Assemblies 中。  
  
## <a name="commonly-used-assemblies"></a>常用程序集  
 下表列出了使用 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 对 [!INCLUDE[dnprdnshort](../includes/dnprdnshort-md.md)] 进行编程时常用的程序集。  
  
|Assembly|说明|  
|--------------|-----------------|  
|Microsoft.SqlServer.ManagedDTS.dll|包含托管运行时引擎。|  
|Microsoft.SqlServer.RuntimeWrapper.dll|包含用于本机运行时引擎的主互操作程序集 (PIA) 或包装。|  
|Microsoft.SqlServer.PipelineHost.dll|包含托管数据流引擎。|  
|Microsoft.SqlServer.PipelineWrapper.dll|包含用于本机数据流引擎的主互操作程序集 (PIA) 或包装。|  
  
  
