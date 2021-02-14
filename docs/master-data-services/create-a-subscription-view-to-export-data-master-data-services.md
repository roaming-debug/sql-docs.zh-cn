---
title: 创建订阅视图以导出数据
description: 了解如何创建订阅视图，以便将 Master Data Services 的数据导出到订阅系统，从而创建数据的视图。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- subscription views [Master Data Services], creating
- creating subscription views [Master Data Services]
ms.assetid: a5e28961-af16-414a-9845-d2e06aac5214
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4181704586d8a0316567ba4548ccd263d4117d79
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272598"
---
# <a name="create-a-subscription-view-to-export-data-master-data-services"></a>创建订阅视图以导出数据 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  创建订阅视图，以便将 Master Data Services 数据导出到订阅系统。 你打算在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库中创建数据的视图。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“集成管理”** 功能区域。 有关详细信息，请参阅[功能区域权限 (Master Data Services)](../master-data-services/functional-area-permissions-master-data-services.md)。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
### <a name="to-create-and-edit-a-subscription-view"></a>创建和编辑订阅视图  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“集成管理”**。  
  
2.  在菜单栏中，单击 **“创建视图”**。  
  
3.  在“订阅视图”  页上，单击“添加”  以创建视图，或者单击“编辑”  以编辑视图。 将在右侧显示一个面板。  
  
4.  在“创建订阅视图”  窗格的“名称”  框中，键入视图的名称。  
  
     在“编辑订阅视图”  窗格的“名称”  框中，键入视图的更新名称。  
  
5.  从 **“模型”** 列表中，选择某一模型。  
  
6.  选择“包括软删除成员” ，以在视图中包括软删除的成员。  
  
7.  在“版本选项”  中选择“版本”  或“版本标志” 选项，然后从相应的列表中进行选择。  
  
    > [!TIP]  
    >  基于版本标志创建订阅视图。 当锁定版本时，可以将该标志重新分配给打开的版本，而无需更新订阅视图。  
  
8.  在“数据源”  选项中选择“实体”  或“派生层次结构”  选项，然后从相应的列表中进行选择。  
  
9. 从 **“格式”** 列表中，选择订阅视图格式。  
  
10. 如果您从 **“格式”** 列表中选择 **“显式级别”** 或 **“派生级别”** ，则键入要包括在视图中的层次结构中的级别数。  
  
11. 单击“保存”。  
  
## <a name="view-information"></a>查看信息  
 对于创建的每个视图，系统都会在网格中添加一行（其中包含十列）。 下表对这些列进行了说明。  
  
|列|说明|  
|------------|-----------------|  
|状态|视图状态。<br /><br /> 单击 " **保存**" 时，将显示 ![更新状态](../master-data-services/media/mds-statusicon-updating.png "用于更新状态的图标") 图像的图标，指示该视图正在更新。<br /><br /> 如果在创建或编辑视图时出现错误，则显示 " ![错误状态](../master-data-services/media/mds-statusicon-error.png "错误状态图标") " 图像图标。<br /><br /> 否则，状态为 "正常"，将显示 !["确定状态](../master-data-services/media/mds-statusicon-ok.png ""正常" 状态图标") " 图像的图标。|  
|名称|订阅视图名称。|  
|建模|模型名称。|  
|版本|版本名称。|  
|版本标志|版本标志名称。|  
|派生层次结构|派生层次结构名称。|  
|实体|实体名称。|  
|格式|指定视图中数据的类型。|  
|级别|在视图中指定级别数，它仅用于显式级别或派生级别视图格式。|  
|包括删除成员|指示视图中是否包括软删除的成员。|  
  
 单击视图后可看到以下信息：  
  
-   “创建者”：创建视图的用户的用户名。  
  
-   “创建时间”：获取创建视图的日期和时间。  
  
-   “更新者”：上次更新索引的用户的用户名。  
  
-   “更新时间”：上次更新索引的日期和时间。  
  
## <a name="see-also"></a>另请参阅  
 [概述：将数据导出 &#40;Master Data Services&#41;](../master-data-services/overview-exporting-data-master-data-services.md)   
 [&#40;Master Data Services 中删除订阅视图&#41;](../master-data-services/delete-a-subscription-view-master-data-services.md)   
 [创建版本标志 (Master Data Services)](../master-data-services/create-a-version-flag-master-data-services.md)  
  
  
