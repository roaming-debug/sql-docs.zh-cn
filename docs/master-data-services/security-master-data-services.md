---
title: 安全性
description: 了解 Master Data Services 中的安全性，包括用户类型、如何在 Excel 外接程序中设置安全性、安全和相关任务。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 56bc41ea-de28-4184-aa7e-99111ae55af5
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 97209bd6d906a73f6199f06a09e14e1ab4d0ac22
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352995"
---
# <a name="security-master-data-services"></a>安全性 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]在  中，使用安全设置可以确保用户可以访问完成自己工作所需的特定主数据，并防止他们访问自己不应查看的数据。  
  
 还可以使用安全设置使某人成为特定模型和功能区域的管理员（例如，允许某人创建 Customer 模型的版本或使某人可以设置安全权限）。  
  
 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 安全设置基于本地或 Active Directory 域用户和组。 MDS 安全设置允许您在确定用户可以访问的数据时使用不同的详细粒度级别。 由于存在这种粒度，安全设置很容易复杂化，因此在使用重叠的用户和组时应慎用。 有关详细信息，请参阅 [重叠的用户和组权限 (Master Data Services)](../master-data-services/overlapping-user-and-group-permissions-master-data-services.md)。  
  
  可以在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Web 应用程序的“用户和组权限”功能区域中分配安全访问权限或使用 Web 服务进行分配。  
  
## <a name="types-of-users"></a>用户类型  
 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中有两种类型的用户：  
  
-    在“资源管理器”功能区域中访问数据的用户。  
  
-   可以在“资源管理器”之外的区域中执行管理任务的用户。 这些用户称为 [管理员 (Master Data Services)](../master-data-services/administrators-master-data-services.md)。  
  
## <a name="how-to-set-security"></a>如何设置安全性  
 要在 MDS 中授予用户或组访问数据或功能的权限，您必须分配：  
  
-   [功能区域访问权限](../master-data-services/functional-area-permissions-master-data-services.md)，该权限确定用户可以访问用户界面的五个功能区域中的哪些区域。  
  
-   [模型对象权限](../master-data-services/model-object-permissions-master-data-services.md)，该权限决定了用户可以访问哪些属性，以及对这些属性的访问权限类型（读取、创建和更新）。 用户还可以在模型级别分配管理员权限。  
  
-   [层次结构成员权限](../master-data-services/hierarchy-member-permissions-master-data-services.md)（可选），该权限决定了用户可以访问哪些成员，以及对这些成员的访问权限类型（读取、更新和删除）。  
  
 分配对属性和成员的权限时，权限可能交叉，此时由规则确定哪个权限优先。 有关详细信息，请参阅 [如何确定权限 (Master Data Services)](../master-data-services/how-permissions-are-determined-master-data-services.md)。  
  
## <a name="security-in-the-add-in-for-excel"></a>Excel 外接程序中的安全设置  
 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Web 应用程序中的安全设置同样适用于 [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]。 用户只能查看和使用拥有权限的数据。 管理员可以执行管理任务。  
  
 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 唯一需要注意的是，中分配的所有安全设置在经过 20 分钟的间隔时间后才会在 Excel 中生效。 *MdsMaximumUserInformationCacheInterval* 间隔时间由 web.config 文件中的  设置进行定义。 若要更改间隔时间，可以更改该设置并重新启动 IIS。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|创建对模型具有完全权限的用户。|[创建模型管理员 (Master Data Services)](../master-data-services/create-a-model-administrator-master-data-services.md)|  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]将 Active Directory 组添加到 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] ；这是授予组权限以访问  Web 应用程序中的数据的第一步。|[添加组 (Master Data Services)](../master-data-services/add-a-group-master-data-services.md)|  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 分配对  Web 应用程序的功能区域的权限。|[分配功能区域权限 (Master Data Services)](../master-data-services/assign-functional-area-permissions-master-data-services.md)|  
|通过将权限分配给模型对象来将权限分配给属性值。|[分配模型对象权限 (Master Data Services)](../master-data-services/assign-model-object-permissions-master-data-services.md)|  
|通过将权限分配给层次结构节点来将权限分配给成员值。|[分配层次结构成员权限 (Master Data Services)](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)|  
  
## <a name="see-also"></a>另请参阅  
 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)   
 [用户和组 &#40;Master Data Services&#41;](../master-data-services/users-and-groups-master-data-services.md)   
 [功能区域权限 &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md)   
 [&#40;Master Data Services 的模型对象权限&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [层次结构成员权限 &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)   
 [如何确定权限 (Master Data Services)](../master-data-services/how-permissions-are-determined-master-data-services.md)  
  
  
