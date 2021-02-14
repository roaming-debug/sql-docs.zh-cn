---
title: 用户和组
description: 了解如何授予对主数据管理器 web 应用程序的访问权限。 用户必须具有合适的帐户。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- users [Master Data Services]
- groups [Master Data Services]
- users [Master Data Services], about users
- groups [Master Data Services], about groups
ms.assetid: ed08dd2d-248e-4b68-91d4-e9961cb50eed
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 580b4f7f098f65391f88cb2e39b57642cbc0eafa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336201"
---
# <a name="users-and-groups-master-data-services"></a>用户和组 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  若要访问 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Web 应用程序，用户必须具有 Windows 域帐户或者安装了 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 的服务器计算机上的帐户。 要授予对 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 的访问权限，可以执行以下任一操作：  
  
-   将用户帐户添加到域组或本地组，再将该组添加到 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中的组列表。  
  
-   将用户帐户添加到 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中的用户列表。  
  
    > [!NOTE]  
    >  如果用户属于具有对 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]的访问权限的组，当该用户第一次访问 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 或 MDS [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]时，自动将其名称添加到用户列表。  
  
 若要在用户界面的 **“资源管理器”** 功能区域中执行操作，必须为组或用户分配对 **“资源管理器”** 功能区域的访问权限以及对模型对象的相应权限。  
  
 如果用户或组需要访问其他功能区域，必须给该用户或组分配访问特定功能区域的权限。  
  
## <a name="best-practice"></a>最佳做法  
 为了简化管理，请创建组并为每个组分配针对功能区域和模型对象的权限。 然后，您可以从这些组中添加和删除用户，而无需访问 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 用户界面。  
  
 不要向单个用户分配其他权限，也不要将一个用户包括在对 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]具有访问权限的多个组中。 此外，不要使用层次结构成员权限，除非您希望某个组对特定成员具有受限的访问权限。  
  
## <a name="see-also"></a>另请参阅  
 [添加用户 &#40;Master Data Services&#41;](../master-data-services/add-a-user-master-data-services.md)   
 [添加组 &#40;Master Data Services&#41;](../master-data-services/add-a-group-master-data-services.md)   
 [删除用户或组 &#40;Master Data Services&#41;](../master-data-services/delete-users-or-groups-master-data-services.md)   
 [测试用户权限 &#40;Master Data Services&#41;](../master-data-services/test-a-user-s-permissions-master-data-services.md)  
  
  
