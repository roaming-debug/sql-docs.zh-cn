---
description: 配置业务规则以发送通知 (Master Data Services)
title: 配置业务规则以发送通知
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], configuring notifications
- e-mail [Master Data Services], configuring business rules
- notifications [Master Data Services], configuring business rules
ms.assetid: b24f7b11-ab53-4642-999c-e17b543b3558
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 0b7bbf98bd65399a6eb2235a0acb73c28af277f9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272768"
---
# <a name="configure-business-rules-to-send-notifications-master-data-services"></a>配置业务规则以发送通知 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，当您要向用户通知有关属性值更改时，请配置业务规则以发送通知。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域和 **“用户和组权限”** 功能区域。 如果您对 **“用户和组权限”** 功能区域没有权限，则无法查看要向其发送通知的用户和组的列表。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
-   使用验证操作的业务规则必须已经存在。 有关详细信息，请参阅[创建和发布业务规则 (Master Data Services)](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)。  
  
-   接收通知的用户或组对未能通过验证的属性必须至少具有“只读”权限。 显式或隐式拒绝对属性的权限的用户或组将接收电子邮件，但将无法访问 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中的属性。  
  
-   如果将邮件发送到组，则组中只有已访问了 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 的成员才会获得电子邮件。  
  
### <a name="to-configure-business-rules-to-send-notifications"></a>配置业务规则以发送通知  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  从菜单栏中，指向 **“管理”** ，然后单击 **“业务规则”**。  
  
3.  在“业务规则”  页上，从“模型”  列表中选择一个模型。  
  
4.  从  “实体”下拉列表中选择一个实体。  
  
5.  从“成员类型”列表中选择一个成员类型。  
  
6.  在网格中，选择要编辑的业务规则所对应的行并单击“编辑” 。  
  
7.  选中“发送通知”复选框，并从下拉列表中选择要向其发送电子邮件通知的用户或组。  
  
8.  单击“保存”。  
  
9. 单击“全部发布” 。  
  
10. 在确认对话框中，单击 **“确定”**。 “业务规则状态”  列中的值更改为“活动”  ，且“通知”  列显示想要向其发送通知的选中用户或组。  
  
## <a name="next-steps"></a>后续步骤  
  
-   通过以下过程之一将业务规则应用到数据：  
  
    -   [针对业务规则验证特定成员 (Master Data Services)](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [针对业务规则验证版本 (Master Data Services)](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
-   按如下方式配置电子邮件协议：  
  
    -   [配置电子邮件通知 (Master Data Services)](../master-data-services/configure-email-notifications-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [通知 &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md)   
 [配置电子邮件通知 (Master Data Services)](../master-data-services/configure-email-notifications-master-data-services.md)  
  
  
