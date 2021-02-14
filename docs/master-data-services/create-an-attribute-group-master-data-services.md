---
description: 创建属性组 (Master Data Services)
title: 创建属性组
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- attribute groups [Master Data Services], creating
- creating attribute groups [Master Data Services]
ms.assetid: 798c325e-e8d8-412a-b02e-118f2741d1c7
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 3d2e129bdd1a995fb5d0092bc15136ca9e1726f9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348092"
---
# <a name="create-an-attribute-group-master-data-services"></a>创建属性组 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，当您想要在 **“资源管理器”** 网格中的单独选项卡上显示属性时，创建属性组。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
-   必须至少存在一个属性。 有关详细信息，请参阅 [创建文本属性 (Master Data Services)](../master-data-services/create-a-text-attribute-master-data-services.md)。  
  
### <a name="to-create-an-attribute-group"></a>创建属性组  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  在“管理模型”  页上，从网格中选择一个模型，然后单击“实体” 。  
  
3.  在“管理实体”  页上，从网格中选择要为其创建属性组的实体所在的行。  
  
4.  单击“属性组” 。  
  
5.  在“管理属性组”页上，执行下列操作之一，然后单击“添加” 。  
  
     如果是叶成员的属性组，请从页顶部的“成员类型”下拉列表中选择“叶”。  
  
     如果是合并成员的属性组，请从“成员类型”下拉列表中选择“合并”。  
  
     如果是集合的属性组，请从“成员类型”下拉列表中选择“集合”。  
  
6.  单击 **“叶组”**、 **“合并组”** 或 **“集合组”** ，以便为叶成员、合并成员或集合相应创建属性组。  
  
7.  在  “名称”框中，键入属性组的名称。 此名称显示在 “资源管理器”中的选项卡上。  
  
8.  若要添加属性，依次单击“可用属性”  框中的属性和“添加”  箭头。 若要添加全部属性，请单击“全部添加”  箭头。  
  
9. 单击“向上”和“向下”箭头可以更改属性的从左到右顺序。  
  
10. 单击  “可用用户”框中的用户，然后单击  “添加”箭头。 若要添加所有用户，请单击  “全部添加”箭头。  
  
11. 单击  “可用组”框中的组，然后单击  “添加”箭头。 若要添加所有组，请单击  “全部添加”箭头。  
  
12. 单击“ **保存**”。  
  
## <a name="next-steps"></a>后续步骤  
  
-   [使属性组对用户可见 (Master Data Services)](../master-data-services/make-an-attribute-group-visible-to-users-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [属性组 &#40;Master Data Services&#41;](../master-data-services/attribute-groups-master-data-services.md)   
 [属性 &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)   
 [更改属性组名称 &#40;Master Data Services&#41;](../master-data-services/change-an-attribute-group-name-master-data-services.md)   
 [删除 &#40;Master Data Services 的属性组&#41;](../master-data-services/delete-an-attribute-group-master-data-services.md)   
 [叶权限 (Master Data Services)](../master-data-services/leaf-permissions-master-data-services.md)   
   
  
  
