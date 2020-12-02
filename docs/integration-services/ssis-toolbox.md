---
description: SSIS 工具箱
title: SSIS 工具箱 | Microsoft Docs
ms.custom: ''
ms.date: 11/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.toolboxfavorites.F1
- sql13.dts.designer.toolbox.F1
- sql13.dts.designer.toolboxcommon.F1
ms.assetid: 552ff592-eeef-46e8-b4a2-9b2384c869aa
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9c67835df192436ce3b8264849589a1db3f66fee
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92192369"
---
# <a name="ssis-toolbox"></a>SSIS 工具箱

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  本地计算机上安装的所有组件都自动显示在“SSIS 工具箱”中。 当安装其他组件时，在此工具箱内右键单击，然后单击“刷新工具箱”即会添加组件。  
 
 创建新的 SSIS 项目或打开现有项目时，“SSIS 工具箱”自动显示。 也可以通过单击位于包设计图面右上角的工具箱按钮，或通过单击“查看”->“其他窗口”->“SSIS 工具箱”打开 SSIS 工具箱。
 
 > [!NOTE]
> 如果看不到该工具箱，转到“查看”->“其他窗口”->“SSIS 工具箱”。
 
单击组件在工具箱底部查看其说明，了解有关工具箱中组件的详细信息。 对于某些组件，还可访问演示如何配置和使用这些组件的示例。 在 [MSDN](/samples/browse/)上提供这些示例。 若要从 **“SSIS 工具箱”** 访问示例，请单击在说明的下方出现的 **“查找示例”** 链接。  
  
> [!NOTE]
> 不能从工具箱中删除已安装的组件。  

## <a name="toolbox-categories"></a>工具箱类别
 在 **“SSIS 工具箱”** 中，控制流组件和数据流组件组织为不同的类别。  可以展开和折叠类别，并重新排列组件。  通过在此工具箱内右键单击，然后单击“还原工具箱默认值”，可以还原默认组织方式。  
  
 选定 **“控制流”** 、 **“数据流”** 和 **“事件处理程序”** 选项卡时， **“收藏夹”** 和 **“公共”** 类别会显示在此工具箱中。 选定 **“控制流”** 选项卡或 **“事件处理程序”** 选项卡时， **“其他任务”** 类别会显示在此工具箱中。选定 **“数据流”** 选项卡时， **“其他转换”** 、 **“其他源”** 和 **“其他目标”** 类别会显示在此工具箱中。  

 ## <a name="add-azure-components-to-the-toolbox"></a>将 Azure 组件添加到工具箱  
 用于 Integration Services 的 Azure 功能包包含连接到 Azure 数据源的连接管理器，以及用于执行常用 Azure 操作的任务。 安装该功能包，可将这些项添加到工具箱。 有关详细信息，请参阅[用于 Integration Services 的 Azure 功能包 (SSIS)](../integration-services/azure-feature-pack-for-integration-services-ssis.md)。  

## <a name="move-a-toolbox-item-to-another-category"></a>将某一工具箱项移动到另一类别  
  
1.  在 SSIS 工具箱中右键单击某一项，然后单击以下项之一：  
  
    -   移动到收藏夹  
  
    -   移动到公共  
  
    -   移动到其他来源  
  
    -   移动到其他目标  
  
    -   移动到其他转换  
  
    -   移动到其他任务  
  
## <a name="refresh-the-ssis-toolbox"></a>刷新 SSIS 工具箱  
  
1.  右键单击 SSIS 工具箱，然后单击“刷新工具箱”。