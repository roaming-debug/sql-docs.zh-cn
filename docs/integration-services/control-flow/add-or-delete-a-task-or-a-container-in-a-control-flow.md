---
description: 在控制流中添加或删除任务或容器
title: 在控制流中添加或删除任务或容器 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- containers [Integration Services], adding
- adding tasks
- adding containers
- tasks [Integration Services], adding
ms.assetid: 653084c6-87a3-45d5-b458-914ecf24d56a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 41a59d34bd6b84388f9fc32443b703bdf94f3239
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123689"
---
# <a name="add-or-delete-a-task-or-a-container-in-a-control-flow"></a>在控制流中添加或删除任务或容器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  在控制流设计器中工作时， [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中的工具箱会列出 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 为在包中生成控制流而提供的任务。 有关这些工具箱的详细信息，请参阅 [SSIS Toolbox](../../integration-services/ssis-toolbox.md)。  
  
 包可以包含同一任务的多个实例。 任务的每个实例在包中都唯一标识，因此可以分别配置每个实例。  
  
 如果删除任务，则将该任务连接到控制流中其他任务和容器的优先约束也将被删除。  
  
 下面的过程介绍如何在包的控制流中添加或删除任务或容器。  
  
## <a name="add-a-task-or-a-container-to-a-control-flow"></a>向控制流添加任务或容器  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击 **“控制流”** 选项卡。  
  
4.  若要打开 **“工具箱”**，请单击 **“查看”** 菜单上的 **“工具箱”** 。  
  
5.  展开 **“控制流项”** 和 **“维护任务”**。  
  
6.  将任务和容器从 **“工具箱”** 拖动到 **“控制流”** 选项卡的设计图面。  
  
7.  将任务或容器的连接线拖动到控制流中的另一组件，从而连接包控制流内的任务或容器。  
  
8.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
## <a name="delete-a-task-or-a-container-from-a-control-flow"></a>从控制流中删除任务或容器  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。 执行下列操作之一：  
  
    -   单击“控制流”选项卡，右键单击要删除的任务或容器，然后单击“删除”。  
  
    -   打开 **包资源管理器**。 在“可执行文件”文件夹上，右键单击要删除的任务或容器，然后单击“删除”。  
  
3.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  

## <a name="set-the-properties-of-a-task-or-container"></a>设置任务或容器的属性
可以使用 **“属性”** 窗口设置任务和容器的大多数属性。 但任务集合的属性以及因过于复杂而无法使用 **“属性”** 窗口设置的属性不在此列。 例如，不能在 **“属性”** 窗口中配置 Foreach 循环容器使用的枚举器。 您必须使用任务或容器编辑器来设置这些复杂的属性。 大多数任务和容器编辑器都有多个节点，而每个节点都包含相关属性。 节点的名称指出了节点所包含属性的主题。  
  
 下面的过程介绍如何使用 **“属性”** 窗口或者任务或容器编辑器来设置相应任务或容器的属性。  
  
### <a name="set-the-properties-of-a-task-or-container-with-the-properties-window"></a>使用“属性”窗口设置任务或容器的属性  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击 **“控制流”** 选项卡。  
  
4.  在“控制流”选项卡的设计图面上，右键单击任务或容器，然后单击“属性”。  
  
5.  在 **“属性”** 窗口中，更新属性值。  
  
    > [!NOTE]  
    >  大部分属性可以通过直接在文本框中键入值或者从列表中选择值的方式进行设置。 但是，某些属性比较复杂，并且具有自定义的属性编辑器。 若要设置属性，请在文本框中单击，然后单击生成按钮 (…) 打开自定义编辑器。  
  
6.  也可以创建属性表达式来动态更新任务或容器的属性。 有关详细信息，请参阅 [添加或更改属性表达式](../../integration-services/expressions/add-or-change-a-property-expression.md)。  
  
7.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
### <a name="set-the-properties-of-a-task-or-container-with-the-task-or-container-editor"></a>使用任务或容器编辑器设置任务或容器的属性  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击 **“控制流”** 选项卡。  
  
4.  在“控制流”选项卡的设计图面上，右键单击任务或容器，然后单击“编辑”以打开相应的任务或容器编辑器。  
  
     有关如何配置 For 循环容器的信息，请参阅[配置 For 循环容器](./for-loop-container.md)。  
  
     有关如何配置 Foreach 循环容器的信息，请参阅 [配置 Foreach 循环容器](./foreach-loop-container.md)。  
  
    > [!NOTE]  
    >  序列容器没有自定义编辑器。  
  
5.  如果任务或容器编辑器具有多个节点，单击包含要设置的属性的节点。  
  
6.  根据需要，也可以单击 **“表达式”** ，并在 **“表达式”** 页上创建属性表达式，以动态更新任务或容器的属性。 有关详细信息，请参阅 [添加或更改属性表达式](../../integration-services/expressions/add-or-change-a-property-expression.md)。  
  
7.  更新属性值。  
  
8.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services 任务](../../integration-services/control-flow/integration-services-tasks.md)   
 [Integration Services 容器](../../integration-services/control-flow/integration-services-containers.md)   
 [控制流](../../integration-services/control-flow/control-flow.md)  
  
