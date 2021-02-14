---
description: 更改业务规则名称 (Master Data Services)
title: 更改业务规则名称
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], changing name
ms.assetid: cffcae43-a208-443f-9f43-a0ec9e05f79c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: cada2e43ffb4d987c54a0e8b5079b32879a47bf2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351949"
---
# <a name="change-a-business-rule-name-master-data-services"></a>更改业务规则名称 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，当分配的业务规则名称不能满足您的业务需要时，可以更改此名称。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
-   业务规则必须存在。 有关详细信息，请参阅[创建和发布业务规则 (Master Data Services)](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)。  
  
### <a name="to-change-the-name-of-a-business-rule"></a>更改业务规则的名称  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  从菜单栏中，指向 **“管理”** ，然后单击 **“业务规则”**。  
  
3.  在 " **业务规则** " 页上，从 " **模型** " 下拉列表中选择一个模型。  
  
4.  从  “实体”下拉列表中选择一个实体。  
  
5.  从“成员类型”列表中选择一个成员类型。  
  
6.  在网格中，选择要更改名称的业务规则所对应的行并单击“编辑” 。  
  
7.  为业务规则键入新名称。  
  
8.  单击“保存”。  
  
9. 单击“全部发布” 。  
  
10. 在确认对话框中，单击 **“确定”**。 “业务规则状态” 列中的值为“有效” 。  
  
## <a name="see-also"></a>另请参阅  
 [业务规则 (Master Data Services)](../master-data-services/business-rules-master-data-services.md)  
  
  
