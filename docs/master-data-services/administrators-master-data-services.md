---
title: 管理员
description: 了解 Master Data Services 中的管理员类型：模型管理员、实体管理员和超级用户。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- administrators [Master Data Services], about administrators
- administrators [Master Data Services]
- models [Master Data Services], administrators
ms.assetid: d330aa4e-6ade-4b09-b376-1b15d6c78f7d
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: bfe03a31b5d8f35770f8ded41e4d8fdb86fb9d30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339659"
---
# <a name="administrators-master-data-services"></a>管理员 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  本文介绍 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中的管理员类型：模型管理员、实体管理员和超级用户。  
  
## <a name="model-administrators"></a>模型管理员  
 在中 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] ，模型管理员是对 "**模型对象**" 选项卡上的顶级模型对象拥有 "**管理员**" 权限的用户。当用户对特定模型具有管理员权限时，该模型的子对象上的任何其他权限 (模型对象和成员权限) 由模型 **管理员** 权限超越并有效地忽略。  
  
-   如果用户具有对 **“资源管理器”** 功能区域的访问权限，此用户可以添加、删除和更新此区域中的所有主数据。  
  
-   如果用户具有对其他功能区域的访问权限，则此用户可以执行该功能区域中的所有管理任务。  
  
 每个模型可以有多个管理员。 每个用户可以是 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 部署中一个、多个或所有模型的模型管理员。  
  
 可以在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 中或通过编程方式将用户配置为模型管理员。 有关详细信息，请参阅 [创建模型管理员 (Master Data Services)](../master-data-services/create-a-model-administrator-master-data-services.md)。  
  
## <a name="entity-administrators"></a>实体管理员  
 在中 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] ，实体管理员是对 "模型对象" 选项卡上的实体对象具有 "管理员" 权限的用户。当用户对实体具有管理员权限时，该实体的子对象上的任何其他权限 (模型对象和成员权限) 将被管理员权限取代，并将被忽略。  
  
-   如果用户具有对 **“资源管理器”** 功能区域的访问权限，此用户可以添加、删除和更新此区域中的所有主数据。  
  
-   如果实体变更需要管理员批准，则实体管理员可以对变更进行审查，然后批准或拒绝变更集。  
  
 每个实体可以有多个管理员。 每个用户可以是一个、多个或所有实体的实体管理员。  
  
 可以在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 中或通过编程方式将用户配置为实体管理员。 有关详细信息，请参阅[创建实体管理员 (Master Data Services)](../master-data-services/create-an-entity-administrator-master-data-services.md)。  
  
## <a name="master-data-services-super-user"></a>Master Data Services 超级用户  
 在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，可以将用户权限分配给超级用户功能区域。 可以对超级用户功能区域行使权限的用户可以对所有模型有效地行使“管理员”权限，并可对所有其他功能区域行使权限。 有关针对功能区域的权限的信息，请参阅[功能区域权限 (Master Data Services)](../master-data-services/functional-area-permissions-master-data-services.md)。  
  
 使用 [创建数据库向导（Master Data Services 配置管理器）](../master-data-services/create-database-wizard-master-data-services-configuration-manager.md)创建 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库时，会为 **管理员帐户** 指定默认超级用户。  
  
 超级用户可以执行以下操作：  
  
-   访问所有功能区域。  
  
-   添加、删除和更新“资源管理器”功能区域中所有模型的所有主数据。  
  
 可以将“超级用户”权限分配给多个用户和/或用户组。  
  
## <a name="comparing-administrator-types"></a>比较管理员类型  
  
|管理员类型|描述|  
|------------------------|-----------------|  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 超级用户|在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 中分配的权限对于管理员的访问权限没有影响。<br /><br /> 可以是基于显式分配的功能区域权限或从组继承的权限的超级用户。<br /><br /> 会自动拥有对所有模型的所有权限。<br /><br /> 自动对所有功能区域具有访问权限。|  
|模型管理员|可以是基于显式分配的管理员权限或者从组继承的权限的模型管理员。<br /><br /> 仅对向其分配访问权限的功能区域具有访问权限。<br /><br /> 自动对特定模型中的所有对象和成员具有所有权限。|  
|实体管理员|可以是基于显式分配的管理员权限或者从组继承的权限的实体管理员。<br /><br /> 仅对向其分配访问权限的功能区域具有访问权限。<br /><br /> 自动对特定实体中的所有对象和成员具有所有权限。<br /><br /> 如果实体变更需要批准，则可以批准挂起的变更集。|  
  
## <a name="external-resources"></a>外部资源  
 msdn.com 上的博客文章 [安全性改进](/archive/blogs/e7/improvements-to-autoplay)。  
  
## <a name="see-also"></a>另请参阅  
 [创建模型管理员 &#40;Master Data Services&#41;](../master-data-services/create-a-model-administrator-master-data-services.md)   
 [创建 Master Data Services 数据库](../master-data-services/install-windows/create-a-master-data-services-database.md)   
 [通知 (Master Data Services)](../master-data-services/notifications-master-data-services.md)  
  
