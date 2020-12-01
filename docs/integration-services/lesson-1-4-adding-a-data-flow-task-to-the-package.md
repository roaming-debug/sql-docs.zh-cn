---
description: 第 1-4 课：将数据流任务添加到包
title: 步骤 4：将数据流任务添加到包 | Microsoft Docs
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 96af3073-8f11-4444-b934-fe8613a2d084
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4d920b5f78d2d3e6dd3e557cbac4e50ad7952884
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88449648"
---
# <a name="lesson-1-4-add-a-data-flow-task-to-the-package"></a>第 1-4 课：将数据流任务添加到包

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



为源数据和目标数据创建了连接管理器后，你可以将数据流任务添加到包中。 数据流任务将定义在源和目标之间移动数据的数据流引擎，并提供在移动数据时转换、清除和修改数据的功能。 大部分的数据提取、转换和加载 (ETL) 进程均在数据流任务中完成。  
  
> [!NOTE]  
> [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 将数据流从控制流中分隔开来。  
  
## <a name="add-a-data-flow-task"></a>添加数据流任务  
  
1.  选择“控制流”选项卡。  
  
2.  在“SSIS 工具箱”窗格中，展开“收藏夹”，并将一个“数据流任务”拖到“控制流”选项卡的设计图面上。  
  
    > [!NOTE]  
    > 如果“SSIS 工具箱”不可用，请依次选择“SSIS”菜单和“SSIS 工具箱”，以显示它。  

3.  在“控制流”设计图面中，右键单击新的“数据流任务”，再选择“重命名”，然后将名称更改为“Extract Sample Currency Data”。  
  
    为添加到设计图面的所有组件提供唯一的名称。 考虑到易用性和可维护性，名称应说明每个组件的功能。 按照这些命名指南， [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 包可以进行自我说明。 另一个说明包的方法是使用批注。 有关注释的详细信息，请参阅[在包中使用注释](../integration-services/use-annotations-in-packages.md)。  
  
4.  右键单击“数据流”任务，选择“属性”，然后在“属性”窗口中，确保已将 LocaleID 属性设置为“英语(美国)”。  
  
## <a name="go-to-next-task"></a>转到下一个任务
[步骤 5：添加并配置平面文件源](../integration-services/lesson-1-5-adding-and-configuring-the-flat-file-source.md)  
  
## <a name="see-also"></a>另请参阅  
[数据流任务](../integration-services/control-flow/data-flow-task.md)  
  
  
  
