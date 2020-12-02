---
description: 对组件分组或取消分组
title: 对组件分组或取消分组 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- grouping containers
- tasks [Integration Services], grouping
- containers [Integration Services], grouping
- grouping tasks
ms.assetid: 34320838-c271-4f8c-90b3-1254690890bb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7cd88383577694d5bef248baea5004056d239136
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92195196"
---
# <a name="group-or-ungroup-components"></a>对组件分组或取消分组

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  **设计器中的**“控制流” **、**“数据流” **和** “事件处理程序” [!INCLUDE[ssIS](../includes/ssis-md.md)] 选项卡支持可折叠的分组。 如果包包含许多组件，这些选项卡可能变得很拥挤，使您很难一次查看所有组件并找到要使用的项。 可折叠的分组功能可节省工作图面空间，从而简化大型包的处理。  
  
 您可以选择要分组的组件，对其进行分组，然后根据工作需要展开或折叠这些组。 将组展开后，将能够访问组中组件的属性。 连接任务和容器的优先约束会自动包含在组中。  
  
 以下是对组件进行分组需注意的事项。  
  
-   若要对组件分组，控制流、数据流或事件处理程序必须包含多个组件。  
  
-   组还可以嵌套，因此可以在组内创建组。 设计图面可以实现多个非嵌套组，但除非组是嵌套的，否则组件只能属于一个组。  
  
-   保存包时， [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器也保存分组，但是分组对包的执行没有影响。 对组件分组的能力是一项设计时功能；它不影响包的运行时行为。  
  
### <a name="to-group-components"></a>对组件分组  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击 **“控制流”**、 **“数据流”** 或 **“事件处理程序”** 选项卡。  
  
4.  在该选项卡的设计图面上，选择要分组的组件，右键单击所选组件，然后单击“分组”。  
  
5.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
### <a name="to-ungroup-components"></a>对组件取消分组  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击 **“控制流”**、 **“数据流”** 或 **“事件处理程序”** 选项卡。  
  
4.  在该选项卡的设计图面上，选择要取消分组的组件所在的组，单击右键，然后单击“取消分组”。  
  
5.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
## <a name="see-also"></a>另请参阅  
 [在控制流中添加或删除任务或容器](../integration-services/control-flow/add-or-delete-a-task-or-a-container-in-a-control-flow.md)   
 [使用默认优先约束来连接任务和容器](./control-flow/precedence-constraints.md)  
  
