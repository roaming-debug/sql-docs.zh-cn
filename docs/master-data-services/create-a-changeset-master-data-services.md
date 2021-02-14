---
description: 创建变更集 (Master Data Services)
title: 创建变更集
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: cfad6f1c-9125-4896-b5f5-a4b9f9593cc4
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 7aa771612709a47f5dce24038af6307dc41d95e7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272728"
---
# <a name="create-a-changeset-master-data-services"></a>创建变更集 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  变更集是主数据的挂起更改的集合。 如果实体需要批准更改，则挂起的更改必须保存在变更集中，然后提交以供管理员批准。  
  
## <a name="prerequisites"></a>先决条件  
  
-   你必须有权访问“资源管理器”功能区域。 有关详细信息，请参阅 [功能区域权限 &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md)  
  
-   你必须至少拥有实体或其属性之一的读取访问权限。  
  
## <a name="to-create-a-local-changeset"></a>创建本地变更集  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 主页中，选择模型和版本，然后单击“资源管理器” 。  
  
2.  单击“实体”菜单中的某个实体  。  
  
3.  在右窗格中，选择“变更集”  ，然后单击“创建” 。  
  
4.  输入变更集的名称，然后单击“保存” 。  
  
     变更集的名称在模型中必须是唯一的。  
  
## <a name="to-create-a-changeset-for-approval"></a>创建供审批的变更集  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 主页中，选择模型和版本，然后单击“资源管理器” 。  
  
2.  单击“实体”菜单中的某个实体  。  
  
3.  对实体进行更改，然后单击“确定”。  
  
4.  将显示“选择 A 变更集”对话框。  
  
5.  单击“新建” ，输入变更集的名称，然后单击“保存” 。 变更集名称在模型中必须是唯一的。  
  
6.  若要使用现有变更集，请单击“现有”  并从列表中选择变更集。 只有处于打开或已拒绝状态的变更集才可用。  
  
## <a name="next-steps"></a>后续步骤  
 [应用并更新变更集 (Master Data Services)](../master-data-services/apply-and-update-a-changeset-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [提交或提交变更集 &#40;Master Data Services&#41;](../master-data-services/commit-or-submit-a-changeset-master-data-services.md)   
 [批准或拒绝变更集 (Master Data Services)](../master-data-services/approve-or-reject-a-changeset-master-data-services.md)  
  
  
