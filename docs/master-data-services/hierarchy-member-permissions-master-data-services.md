---
description: 层次结构成员权限 (Master Data Services)
title: 层次结构成员权限
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- members [Master Data Services], permissions
- permissions [Master Data Services], members
ms.assetid: b3880eed-1bf6-4f65-ab23-b08c194cc858
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 58c354eae5dadcc34d160b756e936dd82c3844e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347745"
---
# <a name="hierarchy-member-permissions-master-data-services"></a>层次结构成员权限 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  层次结构成员权限是可选的，仅当您希望某个用户对特定成员具有受限的访问权限时才应使用。 如果您未在 **“层次结构成员”** 选项卡上分配权限，则用户的权限仅基于在 **“模型”** 选项卡上分配的权限。  
  
 层次结构成员权限在 " [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] **层次结构成员**" 选项卡上的 "**用户和组权限**" 功能区域 (UI) 中分配。这些权限确定用户在用户界面的 "**资源管理器**" 功能区域中可以访问哪些成员。  
  
 在 **“层次结构成员”** 选项卡上，每个层次结构均表示为一个树形结构。 将权限分配给树中的节点时，所有子节点都将继承该权限，除非在更低级别显式分配权限。  
  
> [!NOTE]  
>  如果向层次结构中的节点分配权限，则处于同一级别或更高级别的其他节点中的所有成员都将被隐式拒绝。  
  
 在 **“资源管理器”** 中，将成员权限应用到显示成员的任何位置。 例如，具有“读取”  权限的成员可以读取它所属的任何实体、层次结构和集合。  
  
 层次结构权限应用于您向其分配权限的模型版本，并应用于版本的任何将来副本。 它们不应用于比您向其分配权限的版本更早的版本。  
  
|权限|说明|  
|----------------|-----------------|  
|**读取**|显示成员。<br /><br /> <br /><br /> 注意：如果仅将“读取”权限分配给“根”，则“根”下的成员为只读的；但是，在显式层次结构和集合中，用户可以将成员移到“根”并可以将新成员添加到“根”。|  
|**创建**|层次结构成员权限中不提供创建权限。|  
|**更新**|显示成员，用户可以更改它们。 用户还可以在成员所属的任何显式层次结构或集合中移动成员。|  
|**删除**|显示成员，用户可以删除它们。|  
|**拒绝**|不显示成员。|  
  
 在 **“层次结构成员”** 选项卡上，分配的权限不立即生效。 应用该权限的频率取决于 **数据库中“系统设置”表的** “成员安全处理间隔设置” [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 。 可以按照 [立即应用成员权限 (Master Data Services)](../master-data-services/immediately-apply-member-permissions-master-data-services.md)中的以下步骤立即应用成员权限。  
  
> [!NOTE]  
>  不能将层次结构成员权限分配给递归层次结构、具有显式顶端的派生层次结构和具有隐藏级别的层次结构。  
  
## <a name="possible-overlapping-permissions"></a>可能重叠的权限  
 给成员分配权限时，必须解决重叠的权限问题。  
  
### <a name="when-a-member-belongs-to-multiple-hierarchies"></a>成员属于多个层次结构时  
 两个或多个层次结构可以包含同一成员。  
  
-   如果给一个层次结构节点分配“更新”  权限，而给另一个节点分配“读取” 权限，则该节点中的成员为“读取” 权限。  
  
-   如果给一个层次结构节点分配“更新”  和“创建”  权限，而给另一个节点分配“更新”  和“删除”  权限，则该节点中的成员可以更新。  
  
-   如果为一个层次结构节点分配 **创建** / **读取** / **更新** / **删除** 权限的任意组合，并为另一个节点分配了 "**拒绝**" 权限，则对该节点中的成员的访问被拒绝。  
  
## <a name="external-resources"></a>外部资源  
 msdn.com 上的博客文章 [安全性改进](/archive/blogs/e7/improvements-to-autoplay)。  
  
## <a name="see-also"></a>另请参阅  
 [将层次结构成员权限分配 &#40;Master Data Services&#41;](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)   
 [如何 Master Data Services &#40;确定权限&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)   
 [成员 &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)   
 [层次结构 &#40;Master Data Services&#41;](../master-data-services/hierarchies-master-data-services.md)   
 [立即应用成员权限 (Master Data Services)](../master-data-services/immediately-apply-member-permissions-master-data-services.md)  
  
