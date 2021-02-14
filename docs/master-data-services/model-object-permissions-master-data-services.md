---
description: 模型对象权限 (Master Data Services)
title: 模型对象权限
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- permissions [Master Data Services], model objects
- models [Master Data Services], object permissions
ms.assetid: fab6335b-4cae-47de-ae7c-6c4743e0680f
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: fc4372748ee9556293eee5c074afc2f404134849
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340320"
---
# <a name="model-object-permissions-master-data-services"></a>模型对象权限 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  模型对象权限是必需的。 这些权限确定用户在用户界面的 **“资源管理器”** 功能区域中可以访问哪些属性。  
  
 例如，如果向某个用户分配对 Product 实体的 **“更新”** 权限，则该用户可以更新 Product 实体的所有属性。 如果分配对单个属性的 **“更新”** 权限，则该用户只能更新该属性。  
  
 若要确定通过每个属性值分配的安全设置，可以将模型对象权限与层次结构成员权限组合，从而确定用户可以访问哪些成员。  
  
 若要授予用户访问“资源管理器” 之外的功能区域的权限，该用户必须是模型管理员，这也会涉及分配对象模型的管理权限。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
 模型对象权限是在用户界面中分配的 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] (UI) ，在 "**模型**" 选项卡上的 "**用户和组权限**" 功能区域中。在此选项卡上，模型表示为树状结构。 将权限分配给树中的对象时，下面的所有对象都将继承该权限。 可以通过为单个对象分配权限来覆盖此类继承。  
  
 你可以分配对模型对象的读取、创建、更新、删除或拒绝权限的组合。 如果没有在 **“模型”** 选项卡上分配任何权限，用户就不能在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中查看任何模型或数据。  
  
## <a name="best-practice"></a>最佳做法  
 一般情况下，应分配对模型对象的“全部”  权限，然后显式分配对其下对象的权限。  
  
## <a name="external-resources"></a>外部资源  
 msdn.com 上的博客文章 [安全性改进](/archive/blogs/e7/improvements-to-autoplay)。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Master Data Services 分配模型对象权限&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)   
 [&#40;Master Data Services 的模型权限&#41;](../master-data-services/model-permissions-master-data-services.md)   
 [功能区域权限 &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md)   
 [层次结构成员权限 &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)   
 [如何确定权限 (Master Data Services)](../master-data-services/how-permissions-are-determined-master-data-services.md)  
  
