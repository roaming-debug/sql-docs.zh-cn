---
description: 对包执行进行故障排除的报告
title: 包执行的疑难解答报告 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 8fc476ac-bd69-434e-9636-70776e0b3b6c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7c59111f0626aa9506c0cab17be903da21ff5245
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342216"
---
# <a name="troubleshooting-reports-for-package-execution"></a>对包执行进行故障排除的报告

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  在当前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]中， [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 提供了标准报告，可帮助您监视部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包并排除其问题。 这些包报告中有两个报告尤其有助您查看包的执行状态，并确定执行失败的原因。  
  
-   **Integration Services** 面板 - 此报告提供过去 24 小时内 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上所有包执行情况的概览。 该报告显示每个包的状态、操作类型、包名称之类的信息。  
  
     开始时间、结束时间和持续时间可以解释如下：  
  
    -   如果包仍在运行，则持续时间 = 当前时间 - 开始时间  
  
    -   如果包已完成操作，则持续时间 = 结束时间 - 开始时间  
  
     对于已在服务器上运行的每个包，该面板允许您“放大”以便查找有关可能已发生的包执行错误的具体细节。 例如，单击“概述”可以显示执行中各任务的状态的高级概览，或者选择“所有消息”可以显示作为包执行的一部分捕获的详细消息。  
  
     您可以通过单击 **“筛选器”** ，然后在 **“筛选设置”** 对话框中选择条件，对在任何页上显示的表进行筛选。 可用的筛选条件依赖于要显示的数据。 您可以通过在 **“筛选设置”** 对话框中单击排序图标，更改报告的排序顺序。  
  
-   **“活动 - 所有执行”报告** - 此报告显示已在服务器上执行的所有 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 执行的摘要。 摘要中显示每次执行的信息，如状态、开始时间和结束时间。 每个摘要条目都包含指向有关执行的详细信息的连接，包括在执行期间生成的消息和性能数据。 与 Integration Services 面板一样，您可以将筛选器应用于表，以便缩小显示的信息的范围。  
  
## <a name="related-content"></a>相关内容  
 [Integration Services 服务器的报告](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports)  
  
 [包执行的疑难解答工具](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)  
  
  
