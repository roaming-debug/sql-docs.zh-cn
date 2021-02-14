---
description: 取消锁定版本 (Master Data Services)
title: 取消锁定版本
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- unlocking versions [Master Data Services]
- versions [Master Data Services], unlocking
ms.assetid: b4cf4404-40f3-46fb-801d-cbf80a95448c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: fe03c4a0a95462d1dd793eaf82b0c017b3b781e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336222"
---
# <a name="unlock-a-version-master-data-services"></a>取消锁定版本 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，取消锁定某一模型的版本以便实现对该模型的成员及其属性的更改。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“版本管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
-   版本的状态必须是 **“已锁定”**。 有关详细信息，请参阅 [锁定版本 (Master Data Services)](../master-data-services/lock-a-version-master-data-services.md)。  
  
-   你必须有权访问“版本管理”功能区域。 有关详细信息，请参阅[功能区域权限 (Master Data Services)](../master-data-services/functional-area-permissions-master-data-services.md)。  
  
### <a name="to-unlock-a-version"></a>取消锁定版本  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“版本管理”**。  
  
2.  在 **“管理版本”** 页上，选择要取消锁定的版本对应的行。  
  
3.  单击 **“取消锁定”**。  
  
4.  在确认对话框中，单击 **“确定”**。  
  
## <a name="next-steps"></a>后续步骤  
  
-   [锁定版本 (Master Data Services)](../master-data-services/lock-a-version-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [版本 (Master Data Services)](../master-data-services/versions-master-data-services.md)  
  
  
