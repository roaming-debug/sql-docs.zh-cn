---
description: 删除属性组 (Master Data Services)
title: 删除属性组
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- deleting attribute groups [Master Data Services]
- attribute groups [Master Data Services], deleting
ms.assetid: f915e89b-629d-4725-aea6-a7f051978244
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: c1331d00c0c39cbe3a82f4372aaed38a315dc3c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338978"
---
# <a name="delete-an-attribute-group-master-data-services"></a>删除属性组 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，当您不再需要在 **的** “资源管理器” [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]功能区域中显示属性组选项卡时，可以删除属性组。  
  
-   **注意** 存在属性组时，不属于属性组的属性将不在 **“资源管理器”** 中显示。 不存在属性组时，显示所有属性。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
### <a name="to-delete-an-attribute-group"></a>删除属性组  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  在“管理模型”  页上，从网格中选择一个模型，然后单击“实体” 。  
  
3.  在“管理实体”  页上，从网格中选择要为其编辑属性组的实体所在的行。  
  
4.  单击“属性组” 。  
  
5.  在“管理属性组”页上，从“成员类型”下拉列表中选择成员类型，展开“叶”、“合并”或“集合”，具体视你要删除的组类型而定。  
  
6.  单击您要删除的属性组。  
  
7.  单击 **“删除”** 。  
  
8.  在确认对话框中，单击 **“确定”**。  
  
## <a name="see-also"></a>另请参阅  
 [属性组 &#40;Master Data Services&#41;](../master-data-services/attribute-groups-master-data-services.md)   
 [创建属性组 (Master Data Services)](../master-data-services/create-an-attribute-group-master-data-services.md)  
  
  
