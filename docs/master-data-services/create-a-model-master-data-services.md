---
title: 创建模型
description: 了解如何在 Master Data Services 中创建一个模型，以包含模型对象。 您必须具有 "系统管理" 区域的权限。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- models [Master Data Services], creating models
- creating models [Master Data Services]
ms.assetid: 9bb3b3b3-bde8-44aa-ad62-eaae21188141
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f153a6b7bf2f4ceef65b06d02a825620ea7b3fb2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272618"
---
# <a name="create-a-model-master-data-services"></a>创建模型 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，创建模型以便包含模型对象。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
### <a name="to-create-a-model"></a>创建模型  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  在 **“模型视图”** 页上，从菜单栏中，指向 **“管理”** ，然后单击 **“模型”**。  
  
3.  在“管理模型”  页上，单击“添加” 。 面板将会在右侧显示。  
  
4.  在“名称”  框中，键入模型的名称。  
  
5.  （可选）在“说明”字段中，输入模型说明。  
  
6.  在“日志保留天数”  字段中，选择一个选项来保留日志数据。 默认值是“系统设置” ，指示值继承自 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]中的系统设置。 有关详细信息，请参阅[系统设置 (Master Data Services)](../master-data-services/system-settings-master-data-services.md)。  
  
     若要替代系统设置且不删除事务日志数据，请选择“否” 。 若要仅保留今天的日志数据并截断之前所有日期的日志数据，请选择 **"是"** 并将 " **天数** " 字段设置为0。 若要保留特定天数的日志数据，请选择“是”  并将“天数”  字段设置为相应天数。  
  
7.  或者，选择 **“使用与模型相同的名称创建实体”** 可以使用与模型相同的名称创建实体。  
  
8.  单击 **“保存模型”**。  
  
 对于创建的每个模型，系统都会在网格中添加一行（其中包含八列）。 八个列是：  
  
-   状态：模型状态。 单击 " **保存模型** " 按钮后，将显示 " ![更新](../master-data-services/media/mds-model-status-updating.png "更新") " 图像，指示模型正在更新。 如果在创建或编辑模型时出现错误，则会显示 ![错误](../master-data-services/media/mds-model-status-error.png "错误") 映像。 如果状态为“正常”，系统会显示 ![“确定”](../master-data-services/media/mds-model-status-ok.png "确定") 图像。  
  
-   **名称**：模型名称。  
  
-   说明：模型说明。  
  
-   日志保留天数：日志可保留该模型的天数。  
  
-   创建者：创建模型的用户的用户名。  
  
-   创建的日期和时间：创建模型的日期和时间。  
  
-   更新者：上次更新模型的用户的用户名。  
  
-   创建的日期和时间：上次更新模型的日期和时间。  
  
## <a name="next-steps"></a>后续步骤  
  
-   [创建实体 (Master Data Services)](../master-data-services/create-an-entity-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [Master Data Services &#40;模型&#41;](../master-data-services/models-master-data-services.md)   
 [实体 &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)   
 [删除 &#40;Master Data Services 的模型&#41;](../master-data-services/delete-a-model-master-data-services.md)   
 [编辑模型 &#40;Master Data Services&#41;](../master-data-services/edit-model-master-data-services.md)   
 [事务 (Master Data Services)](../master-data-services/transactions-master-data-services.md)  
  
  
