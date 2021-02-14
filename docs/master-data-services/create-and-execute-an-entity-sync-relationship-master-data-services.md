---
description: 创建和执行实体同步关系（主数据服务）
title: 创建和执行实体同步关系
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 0ddceab4-d2b3-4bc1-bd9c-6b852200b414
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: df91e4bc3d0dab5566abfecd8ef5a8039339d2c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339100"
---
# <a name="create-and-execute-an-entity-sync-relationship-master-data-services"></a>创建和执行实体同步关系（主数据服务）

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  实体同步是实体版本间的单向可重复同步。 它提供了一种在不同模型间共享实体数据的方法。  
  
## <a name="prerequisites"></a>先决条件  
 创建实体同步关系的先决条件：  
  
-   你必须有权访问“系统管理”功能区域。 有关详细信息，请参阅[功能区域权限 (Master Data Services)](../master-data-services/functional-area-permissions-master-data-services.md)。  
  
-   你必须是目标模型的模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
-   至少需要拥有对源实体及其所有属性和成员的读取访问权限。  
  
 执行实体同步关系的先决条件：  
  
-   你必须有权访问“系统管理”功能区域。 有关详细信息，请参阅[功能区域权限 (Master Data Services)](../master-data-services/functional-area-permissions-master-data-services.md)。  
  
-   你必须是目标模型的模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
 在创建实体同步关系时，请注意以下事项。  
  
-   源实体和目标实体必须处于不同的模型中。  
  
-   不得确认目标实体版本状态。  
  
-   建立同步关系后，目标会立即与源进行同步。  
  
-   目标实体版本不能是另一同步关系中的源实体版本。  
  
 在执行实体同步关系时，请注意以下事项。  
  
-   仅复制叶成员  
  
-   不会复制基于域的属性。  
  
-   不会复制软删除的成员。  
  
-   同步不会生成目标实体事务/历史记录。  
  
 **创建实体同步关系**  
  
1.  在主数据管理器中，单击“系统管理” 。  
  
2.  在“模型视图”  页上，从菜单栏中指向“管理”  ，然后单击“实体同步” 。  
  
3.  在“实体同步维护”  页上，单击“添加” 。 右侧将显示一个面板。  
  
4.  从“模型”  列表中，选择某一模型。  
  
5.  从源“版本”  列表中，选择某一版本。  
  
6.  从“实体”  列表中，选择某一实体。  
  
7.  从目标“模型”  列表中，选择某一模型。  
  
8.  从目标“版本”  列表中，选择某一版本。  
  
9. 如果想要同步某个现有实体，则选择“现有实体”  并从实体列表中选择一个实体，或者如果想要同步到新实体，则选择“新实体”  ，然后输入目标实体名称。  
  
10. 选择“按需同步” ，或选择“自动同步”  并设置频率。  
  
11. 单击“保存”。  
  
 **执行实体同步关系**  
  
1.  在主数据管理器中，单击“系统管理” 。  
  
2.  在“模型视图”  页上，从菜单栏中指向“管理”  ，然后单击“实体同步” 。  
  
3.  在“实体同步维护”  页上，选择网格中的同步关系。  
  
4.  单击“执行” 。  
  
## <a name="sync-relationship-information"></a>同步关系信息  
 对于创建的每个同步关系，系统都会在网格中添加一行（其中包含十列）。 下表对这些列进行了说明。  
  
|列|说明|  
|------------|-----------------|  
|状态|同步关系状态。<br /><br /> 单击 " **保存** " 或 "执行同步关系" 时，将显示 ![更新状态](../master-data-services/media/mds-statusicon-updating.png "用于更新状态的图标") 图像的图标，指示同步关系正在更新。<br /><br /> 如果在创建、编辑或执行同步关系时出现错误，则会显示 ![错误状态图标图标](../master-data-services/media/mds-statusicon-error.png "错误状态图标") 。<br /><br /> 否则，状态为 "正常"，将显示 !["确定状态](../master-data-services/media/mds-statusicon-ok.png ""正常" 状态图标") " 图像的图标。|  
|源模型|源模型名称。|  
|源版本|源版本名称。|  
|源实体|源实体名称。|  
|目标模型|目标模型名称。|  
|目标版本|目标版本名称。|  
|目标实体|目标实体名称。|  
|频率|指定同步关系的频率。|  
|上次尝试时间|显示上次同步尝试的时间。|  
|上次成功的时间|显示上次同步尝试成功的时间。|  
  
 单击索引后可看到以下信息：  
  
-   “上次尝试错误”：显示有关上次同步尝试的错误信息。  
  
-   “创建者”：创建同步的用户的用户名。  
  
-   “创建时间”：创建同步的日期和时间。  
  
-   “更新者”：上次更新同步的用户的用户名。  
  
-   “更新时间”：上次更新同步的日期和时间。  
  
## <a name="next-steps"></a>后续步骤  
 [编辑和删除实体同步关系 (Master Data Services)](../master-data-services/edit-and-delete-an-entity-sync-relationship-master-data-services.md)  
  
  
