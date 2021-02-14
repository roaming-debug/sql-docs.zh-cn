---
description: 模型权限 (Master Data Services)
title: 模型权限
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- models [Master Data Services], permissions
- permissions [Master Data Services], models
ms.assetid: 210f440b-2cc1-4c49-94b1-3a97e2af7bc3
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 029a3f624dea71567f34a26cdd7093cc96be36d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340330"
---
# <a name="model-permissions-master-data-services"></a>模型权限 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  模型权限应用到模型中存在的所有实体、派生层次结构、显式层次结构和集合。 对于任何单个对象，可以覆盖分配给模型的权限。  
  
> [!NOTE]  
>  如果用户是模型管理员，则在用户界面的所有功能区域显示该模型。 否则，仅在 **“资源管理器”** 功能区域显示该模型。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
|权限|说明|  
|----------------|-----------------|  
|**读取**|用户可以读取成员、属性、层次结构成员身份或集合成员身份。|  
|**创建**|用户可以创建成员，并在创建过程中指定属性值。|  
|**更新**|用户可以更新成员、属性、层次结构成员身份或集合成员身份。|  
|**删除**|用户可以删除成员|  
|**拒绝**|拒绝对模型的所有访问|  
|**管理员**|针对模型的管理员权限。 管理员权限仅在模型级别可用。|  
  
 读取、创建、更新和删除权限可以彼此合并。 当分配创建、更新和删除权限时，将自动分配读取权限。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Master Data Services 分配模型对象权限&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)   
 [&#40;Master Data Services 的模型对象权限&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [实体权限 &#40;Master Data Services&#41;](../master-data-services/entity-permissions-master-data-services.md)   
 [集合权限 &#40;Master Data Services&#41;](../master-data-services/collection-permissions-master-data-services.md)  
  
  
