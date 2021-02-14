---
description: 功能区域权限 (Master Data Services)
title: 功能区域权限
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- functional area permissions [Master Data Services], about functional area permissions
- functional area permissions [Master Data Services]
- permissions [Master Data Services], functional areas
ms.assetid: a80b87b3-b904-4cda-8582-0761c2617c57
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 45386aaa746522c18f7e3de293ecced923cfa807
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272518"
---
# <a name="functional-area-permissions-master-data-services"></a>功能区域权限 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  可以将权限分配给 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 用户界面 (UI) 的各个功能区域。 功能区域如下所示：  
  
-   **资源管理器**  
  
-   **版本管理**  
  
-   **集成管理**  
  
-   **系统管理**  
  
-   **用户和组权限**  
  
-   **超级用户**  
  
 将权限分配给功能区域时，即使得 UI 的一个区域对用户或组可见。  
  
 在 **“资源管理器”** 功能区域内，分配给模型对象和层次结构成员的附加权限可以确定用户可访问的数据。 在所有其他功能区域内，用户必须是模型管理员才能查看模型和对模型执行操作。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
> [!IMPORTANT]  
>  具有超级用户功能权限的用户可以对所有模型有效地行使管理员权限，并可行使所有其他功能权限。  
  
 在 **“模型”** 选项卡上，用户或组必须至少具有对一个功能区域和一个模型的权限，才能访问 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Master Data Services 分配功能区域权限&#41;](../master-data-services/assign-functional-area-permissions-master-data-services.md)   
 [&#40;Master Data Services 的模型对象权限&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [层次结构成员权限 &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)   
 [如何确定权限 (Master Data Services)](../master-data-services/how-permissions-are-determined-master-data-services.md)  
  
  
