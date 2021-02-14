---
description: 编辑模型 (Master Data Services)
title: 编辑模型
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- models [Master Data Services], changing name
ms.assetid: 399eed32-7c61-4239-9c06-996a65219518
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 9831a53f56d831dbc52c5d7309a365723ca938ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341720"
---
# <a name="edit-model-master-data-services"></a>编辑模型 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，你可以更改模型的名称和说明，并指明所需的事务日志保留期（以天为单位）。  
  
 有关详细信息，请参阅[事务 (Master Data Services)](../master-data-services/transactions-master-data-services.md)。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
### <a name="to-change-a-model"></a>更改模型  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  在 **“模型视图”** 页上，从菜单栏中，指向 **“管理”** ，然后单击 **“模型”**。  
  
3.  在“管理模型”  页上，从网格中选择要更改其名称或说明的模型所在的行。  
  
4.  单击 **“编辑”** 。  
  
5.  在“名称”  框中，键入更新后的模型名称。  
  
6.  在“说明”  字段中，键入更新后的模型说明。  
  
7.  在“日志保留天数”  字段中，选择一个选项来保留日志数据。 默认值是“系统设置” ，指示值继承自 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]中的系统设置。 有关详细信息，请参阅[系统设置 (Master Data Services)](../master-data-services/system-settings-master-data-services.md)。  
  
     若要替代系统设置且不删除事务日志数据，请选择“否” 。 若要仅保留今天的日志数据并截断之前所有日期的日志数据，请选择 **"是"** 并将 " **天数** " 字段设置为0。 若要保留特定天数的日志数据，请选择“是”  并将“天数”  字段设置为相应天数。  
  
8.  单击 **“保存模型”**。  
  
 网格的“状态”列中显示了对模型所执行操作的状态。  单击 " **保存模型** " 按钮后，将显示 " ![更新](../master-data-services/media/mds-model-status-updating.png "更新") " 图像，指示模型正在更新。 如果在创建或编辑模型时出现错误，则会显示 ![错误](../master-data-services/media/mds-model-status-error.png "错误") 映像。 如果状态为“正常”，系统会显示 ![“确定”](../master-data-services/media/mds-model-status-ok.png "确定") 图像。  
  
## <a name="see-also"></a>另请参阅  
 [创建模型 &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md)   
 [删除 &#40;Master Data Services 的模型&#41;](../master-data-services/delete-a-model-master-data-services.md)   
 [模型 (Master Data Services)](../master-data-services/models-master-data-services.md)  
  
  
