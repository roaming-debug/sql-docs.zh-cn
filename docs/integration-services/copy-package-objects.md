---
description: 复制包对象
title: 复制包对象 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- control flow [Integration Services], copying objects
- copying package objects [Integration Services]
- data flow [Integration Services], copying objects
- connection managers [Integration Services], copying
ms.assetid: 99b85e5c-d6bd-4e7c-afe4-51f6ce151c2f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9d19b63fe653700839f4ea3443fc0676890fdcc9
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127388"
---
# <a name="copy-package-objects"></a>复制包对象

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  此主题介绍如何在包内或包之间复制控制流项、数据流项和连接管理器。  
  
### <a name="to-copy-control-and-data-flow-items"></a>复制控制和数据流项  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中，打开包含要处理的包的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击希望在其间复制的包。  
  
3.  在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器中，单击包含要复制的项的包对应的选项卡，并单击 **“控制流”**、 **“数据流”** 或 **“事件处理程序”** 选项卡。  
  
4.  选择要复制的控制流或数据流项。 可以通过按 Shift 键并单击项以一次一个的方式来选择多项，也可以通过拖动指针扫过希望选择的项来按组选择多项。  
  
    > [!IMPORTANT]  
    >  如果选择的两个项相互连接，则不会自动选择连接这些项的优先级约束和路径。 若要复制经过排序的工作流（一段控制流或数据流），请确保同时复制优先级约束和路径。  
  
5.  右键单击所选项并单击“复制”。  
  
6.  如果将项复制到其他包，请单击要复制到其中的包，然后单击适合该项类型的合适的选项卡。  
  
    > [!IMPORTANT]  
    >  无法将数据流复制到包中，除非包至少包含一个“数据流”任务。  
  
7.  右键单击并单击“粘贴”。  
  
### <a name="to-copy-connection-managers"></a>复制连接管理器  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中，打开包含要处理的包的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击包。  
  
3.  在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器中，单击 **“控制流”**、 **“数据流”** 或 **“事件处理程序”** 选项卡。  
  
4.  在“连接管理器”区域中，右键单击连接管理器，然后单击“复制”。 每次只能复制一个连接管理器。  
  
5.  如果要将项复制到其他包，请单击希望复制到其中的包，然后单击 **“控制流”**、 **“数据流”** 或 **“事件处理程序”** 选项卡。  
  
6.  右键单击“连接管理器”区域，并单击“粘贴”。  
  
## <a name="see-also"></a>另请参阅  
 [控制流](../integration-services/control-flow/control-flow.md)   
 [数据流](../integration-services/data-flow/data-flow.md)   
 [Integration Services (SSIS) 连接](../integration-services/connection-manager/integration-services-ssis-connections.md)   
 [复制项目项](./integration-services-ssis-projects-and-solutions.md)  
  
