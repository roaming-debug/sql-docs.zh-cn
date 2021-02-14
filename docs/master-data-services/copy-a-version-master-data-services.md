---
description: 复制版本 (Master Data Services)
title: 复制版本
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- versions [Master Data Services], copying
- copying versions [Master Data Services]
ms.assetid: f4678a02-bbe9-4f21-9e32-627eae053fe7
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f7871bb7e0f5912e2a1039355d2b5508120a97f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272738"
---
# <a name="copy-a-version-master-data-services"></a>复制版本 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，复制某一模型的版本以便为其创建新版本。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“版本管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
-   你必须有权访问“版本管理”功能区域。 有关详细信息，请参阅[功能区域权限 (Master Data Services)](../master-data-services/functional-area-permissions-master-data-services.md)。  
  
### <a name="to-copy-a-version"></a>复制版本  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“版本管理”**。  
  
2.  在 **“管理版本”** 页上，选择要复制的版本对应的行。  
  
    > [!NOTE]  
    >  根据 [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]中的设置，您可能只能复制状态为 **“已提交”** 的版本。 有关详细信息，请参阅[系统设置 (Master Data Services)](../master-data-services/system-settings-master-data-services.md)。  
  
3.  单击 **“复制”** 。  
  
4.  在确认对话框中，单击 **“确定”**。  
  
## <a name="next-steps"></a>后续步骤  
  
-   [更改版本名称 (Master Data Services)](../master-data-services/change-a-version-name-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [版本 (Master Data Services)](../master-data-services/versions-master-data-services.md)  
  
  
