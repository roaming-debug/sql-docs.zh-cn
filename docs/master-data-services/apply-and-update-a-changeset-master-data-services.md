---
description: 应用并更新变更集 (Master Data Services)
title: 应用并更新变更集
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 3a6a3cf2-1e77-43d3-a64a-855ae51258e7
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ad71934e2eca7e7db651859123eb6562565b4bcc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339594"
---
# <a name="apply-and-update-a-changeset-master-data-services"></a>应用并更新变更集 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  变更集是主数据的挂起更改的集合。 可以在本地应用变更集以查看、添加、更新和删除变更集中挂起的更改。  
  
## <a name="prerequisites"></a>先决条件  
  
-    您必须有权访问“资源管理器”功能区域。 有关详细信息，请参阅[功能区域权限 (Master Data Services)](../master-data-services/functional-area-permissions-master-data-services.md)。  
  
-   你必须至少拥有实体或其属性之一的更新访问权限。  
  
-   如果你是实体管理员，只可以查看你拥有的变更集或提交以供审核的变更集。  
  
-   在变更集处于打开或拒绝状态时，你仅可以修改自己拥有的变更集。  
  
## <a name="to-apply-and-update-a-changeset"></a>应用并更新变更集  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 主页中，选择模型和版本，然后单击“资源管理器” 。  
  
2.  单击“实体”菜单中的某个实体  。  
  
3.  在右窗格中，选择“变更集”，然后双击你想要查看或更改的变更集。  
  
4.  单击“应用”。  
  
     挂起的更改应用于网格中的实体成员。 挂起的更改会突出显示。  
  
     创建、删除和更新成员会导致变更集发生变化。  
  
5.  若要还原挂起的更改，右键单击“变更集”窗格中的网格，然后单击“还原”。  
  
## <a name="next-steps"></a>后续步骤  
 [确认或提交变更 (Master Data Services)](../master-data-services/commit-or-submit-a-changeset-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [创建变更集 &#40;Master Data Services&#41;](../master-data-services/create-a-changeset-master-data-services.md)   
 [批准或拒绝变更集 (Master Data Services)](../master-data-services/approve-or-reject-a-changeset-master-data-services.md)  
  
  
