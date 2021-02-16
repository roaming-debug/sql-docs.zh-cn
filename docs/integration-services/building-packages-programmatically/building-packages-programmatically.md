---
description: 以编程方式生成包
title: 以编程方式生成包 | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
ms.assetid: 7474b1f4-7607-4f28-a6fd-67f7db1dd3f8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: daf54aa01dcea6d5c63fba21a3b4ffaf61f2cdd9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351012"
---
# <a name="building-packages-programmatically"></a>以编程方式生成包

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  如果您需要动态创建包，或需要在开发环境之外管理和执行 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包，则可以采用编程方式对包进行操作。 如果采用这种方法，则有一系列选择：  
  
-   加载并执行现有包，不进行修改。  
  
-   加载现有包，对其进行重新配置（例如，指定一个不同的数据源），然后执行。  
  
-   创建一个新包，添加并配置组件（逐个对象和属性），保存并执行。  
  
 您可以使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型，以任何托管编程语言编写可创建、配置和执行包的代码。 例如，您可能希望创建元数据驱动的包，它们可以基于所选数据源及其表和列，配置它们的连接或数据源、转换和目标。  
  
 本节逐行介绍并演示如何以编程方式创建和配置包。 如果选择复杂性最低的包编程方式，只需加载并运行现有包，不需要进行[以编程方式运行和管理包](../../integration-services/run-manage-packages-programmatically/running-and-managing-packages-programmatically.md)中所介绍的修改。  
  
 这里没有介绍一种中间方式，即将一个现有包加载为模板，对其进行重新配置（例如，指定一个不同的数据源），然后执行。 您也可以使用本节中的信息，修改包中的现有对象。  
  
> [!NOTE]  
>  将现有包用作模板并修改数据流中的现有列时，您可能必须删除现有列并调用受影响组件的 <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A> 方法。  
  
## <a name="in-this-section"></a>本节内容  
 [以编程方式创建包](../../integration-services/building-packages-programmatically/creating-a-package-programmatically.md)  
 介绍如何以编程方式创建包。  
  
 [以编程方式添加任务](../../integration-services/building-packages-programmatically/adding-tasks-programmatically.md)  
 介绍如何向包中添加任务。  
  
 [以编程方式连接任务](../../integration-services/building-packages-programmatically/connecting-tasks-programmatically.md)  
 介绍如何基于前一个任务或容器的执行结果来控制包中容器或任务的执行。  
  
 [以编程方式添加连接](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md)  
 介绍如何向包中添加连接管理器。  
  
 [以编程方式使用变量](../../integration-services/building-packages-programmatically/working-with-variables-programmatically.md)  
 介绍如何在包执行期间添加和使用变量。  
  
 [以编程方式处理事件](../../integration-services/building-packages-programmatically/handling-events-programmatically.md)  
 介绍如何处理包和任务事件。  
  
 [以编程方式启用日志记录](../../integration-services/building-packages-programmatically/enabling-logging-programmatically.md)  
 介绍如何为包或任务启用日志记录，以及如何对日志事件应用自定义筛选器。  
  
 [以编程方式添加数据流任务](../../integration-services/building-packages-programmatically/adding-the-data-flow-task-programmatically.md)  
 介绍如何添加和配置数据流任务及其组件。  
  
 [以编程方式查找数据流组件](../../integration-services/building-packages-programmatically/discovering-data-flow-components-programmatically.md)  
 介绍如何检测本地计算机上安装的组件。  
  
 [以编程方式添加数据流组件](../../integration-services/building-packages-programmatically/adding-data-flow-components-programmatically.md)  
 介绍如何向数据流任务添加组件。  
  
 [以编程方式连接数据流组件](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md)  
 介绍如何连接两个数据流组件。  
  
 [以编程方式选择输入列](../../integration-services/building-packages-programmatically/selecting-input-columns-programmatically.md)  
 介绍如何从数据流中的上游组件提供给组件的输入列中选择输入列。  
  
 [以编程方式保存包](../../integration-services/building-packages-programmatically/saving-a-package-programmatically.md)  
 介绍如何以编程方式保存包。  
  
## <a name="reference"></a>参考  
 [Integration Services 错误和消息引用](../../integration-services/integration-services-error-and-message-reference.md)  
 列出预定义的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 错误代码及其符号名称和说明。  
  
## <a name="related-sections"></a>相关章节  
 [用脚本扩展包](../../integration-services/extending-packages-scripting/extending-packages-with-scripting.md)  
 讨论如何使用脚本任务扩展控制流，以及如何使用脚本组件扩展数据流。  
  
 [用自定义对象扩展包](../../integration-services/extending-packages-custom-objects/extending-packages-with-custom-objects.md)  
 讨论如何创建用于多个包的编程自定义任务、数据流组件以及其他包对象。  
  
 [以编程方式运行和管理包](../../integration-services/run-manage-packages-programmatically/running-and-managing-packages-programmatically.md)  
 讨论如何枚举、运行和管理包以及存储包的文件夹。  
  
## <a name="external-resources"></a>外部资源  
  
-    [www.codeplex.com/MSFTISProdSamples](www.codeplex.com/MSFTISProdSamples) 上的 CodePlex 示例 [Integration Services 产品示例](https://go.microsoft.com/fwlink/?LinkID=131204)  
  
-   blogs.msdn.com 上的博客文章[自定义扩展插件性能探查](https://techcommunity.microsoft.com/t5/sql-server-integration-services/performance-profiling-your-custom-extensions/ba-p/387490)  

## <a name="see-also"></a>另请参阅  
 [SQL Server Integration Services](../../integration-services/sql-server-integration-services.md)  
  
  
