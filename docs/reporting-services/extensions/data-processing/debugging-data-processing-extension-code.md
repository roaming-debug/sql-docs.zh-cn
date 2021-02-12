---
title: 调试数据处理扩展插件代码 | Microsoft Docs
description: 了解如何使用 Microsoft .NET Framework 调试工具来分析数据处理扩展插件代码并查找其中的错误。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- debugging data processing extensions [Reporting Services]
- troubleshooting [Reporting Services], data processing extensions
- data processing extensions [Reporting Services], debugging
ms.assetid: e963e205-9ae0-446d-97df-028a1d2727d9
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c389923aade8a22f75180ab0a422a46e71843997
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065420"
---
# <a name="debugging-data-processing-extension-code"></a>调试数据处理扩展插件代码
  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 提供了一些可以帮助分析数据处理扩展插件代码和查找其中错误的调试工具。 哪个工具效果最佳取决于您试图完成的任务。 此示例使用 [!INCLUDE[vsOrcas](../../../includes/vsorcas-md.md)]。  
  
#### <a name="to-debug-your-data-processing-extension-code"></a>调试数据处理扩展插件代码  
  
1.  启动 [!INCLUDE[vsOrcas](../../../includes/vsorcas-md.md)] 并打开数据处理扩展插件项目。  
  
2.  生成项目，并将数据处理扩展插件程序集以及随附的 .pdb 文件部署到报表设计器。 有关部署的详细信息，请参阅[如何：向报表设计器部署数据处理扩展插件](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension-to-report-designer.md)。  
  
3.  在 [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] 中打开新的报表项目，同时在 [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] 的一个单独窗口中让数据处理扩展插件代码处于打开状态。  
  
4.  导航到 [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] 中包含数据处理扩展插件项目的窗口，并在代码中设置一些断点。  
  
5.  在数据处理扩展插件项目窗口仍保持活动状态的同时，在“调试”菜单上，单击“附加到进程” 。  
  
     “附加到进程”对话框会打开。  
  
6.  从进程列表中，选择与报表项目对应的 devenv.exe 进程并单击“附加”。  
  
7.  使用报表项目的“报表数据”选项卡定义报表数据源。 您最有可能使用一般性查询设计器来针对自定义数据源执行查询。 这将调用调试器并执行对应于断点的代码。  
  
8.  使用 F11 键分步执行代码。 有关使用 [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] 进行调试的详细信息，请参阅 [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] 文档。  
  
## <a name="see-also"></a>另请参阅  
 [部署数据处理扩展插件](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)   
 [Reporting Services 扩展插件](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [实现数据处理扩展插件](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Reporting Services 扩展插件库](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
