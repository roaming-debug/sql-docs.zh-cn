---
description: 删除属性 (Master Data Services)
title: 删除属性
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- attributes [Master Data Services], deleting
- deleting attributes [Master Data Services]
ms.assetid: ec3e66f7-0e35-43d7-a80d-64899948ebfe
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 75facb60f974ebcd6c233e6527fd3a8fd66c112b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340417"
---
# <a name="delete-an-attribute-master-data-services"></a>删除属性 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，当您希望永久删除某个属性以及所有关联的属性值时，可以删除此属性。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
### <a name="to-delete-an-attribute"></a>删除属性  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  在“管理模型”  页上，从网格中选择一个模型，然后单击“实体” 。  
  
3.  在“管理实体”  页上，选择要为其创建属性的实体所在的行。  
  
4.  单击 **“属性”**。  
  
5.  在“管理属性”  页上，执行下列操作之一。  
  
    -   如果是叶成员的属性，请从“成员类型”  列表框中选择“叶”  。  
  
    -   如果属性是针对合并成员，则从“成员类型”  列表框中选择“合并”  。  
  
    -   如果属性是针对集合，则从“成员类型”  列表框中选择“集合”  。  
  
6.  为你要删除的属性选择行。  
  
    > [!NOTE]  
    >  不能删除 Name 或 Code 属性。  
  
7.  单击 **“删除”** 。  
  
8.  在确认对话框中，单击 **“确定”**。  
  
## <a name="see-also"></a>另请参阅  
 [属性 &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)   
 [基于域的属性 &#40;Master Data Services&#41;](../master-data-services/domain-based-attributes-master-data-services.md)   
 [Master Data Services 创建文本属性 &#40;&#41;](../master-data-services/create-a-text-attribute-master-data-services.md)   
 [创建基于域的属性 (Master Data Services)](../master-data-services/create-a-domain-based-attribute-master-data-services.md)  
  
  
