---
title: 创建和发布业务规则
description: 了解如何在 Master Data Services 中创建业务规则，以确保主数据的准确性。 创建规则后，将其发布以应用规则。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], creating
- creating business rules [Master Data Services]
ms.assetid: 6961d636-4d69-468e-81f7-8d0be6a4a039
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: fbdb07d5a9bbff92b98fca913e2fcf142a1cbe6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339091"
---
# <a name="create-and-publish-a-business-rule-master-data-services"></a>创建和发布业务规则 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，创建业务规则以便确保您的主数据的精确性。 创建规则后，必须首先发布它，然后才能将该规则应用于数据。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
### <a name="to-create-and-publish-a-business-rule"></a>创建和发布业务规则  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  从菜单栏中，指向 **“管理”** ，然后单击 **“业务规则”**。  
  
3.  在“业务规则”页上，从“模型”下拉列表中选择一个模型。  
  
4.  从  “实体”下拉列表中选择一个实体。  
  
5.  从“成员类型”  下拉列表中，选择要应用业务规则的成员类型。  
  
6.  单击 **添加**。  
  
7.  在“名称”  框中，键入业务规则的名称。  
  
8.  （可选）在“说明”  字段中，键入业务规则描述。  
  
9. （可选）选中“发送通知”选项，并从下拉列表中选择要向其发送电子邮件通知的用户或组。  
  
    > [!NOTE]  
    >  仅针对包括验证操作的规则发送通知。  
  
10. 在“If”  块下，单击“添加” 。 此时将显示一个面板。  
  
11. 从  “属性”下拉列表中选择一个属性。  
  
12. 从“运算符”下拉列表中选择一个条件。  
  
13. 填写所有必填字段。  
  
14. 单击“保存”按钮  。 此时，系统会在“If”  网格中新添加一行。  
  
    > [!TIP]  
    >  可以通过右键单击各项并选择“删除”，从业务规则中删除相应项。  
  
15. 也可以向规则添加多个条件。 有关详细信息，请参阅 [向业务规则添加多个条件 (Master Data Services)](../master-data-services/add-multiple-conditions-to-a-business-rule-master-data-services.md)。  
  
16. 在“Then”  块下，单击“添加”  。 此时将显示一个面板。  
  
17. 从  “属性”下拉列表中选择一个属性。  
  
18. 从“运算符”下拉列表中，选择一个操作。  
  
19. 填写所有必填字段。  
  
20. 单击“保存”。 此时，系统会在“Then”  网格中新添加一行。  
  
21. （可选）若要添加“Else”  操作，请完成以下步骤。  
  
    1.  在“Else”  块下，单击“添加” 。 此时将显示一个面板。  
  
    2.  从  “属性”下拉列表中选择一个属性。  
  
    3.  从“运算符”下拉列表中，选择一个操作。  
  
    4.  填写所有必填字段。  
  
    5.  单击“保存”。 此时，系统会在“Else”  网格中新添加一行。  
  
22. 单击“保存”。 此时，系统会在业务规则网格中新添加一行。  
  
23. 单击“全部发布” 。  
  
24. 在确认对话框中，单击 **“确定”**。 “业务规则状态”  列中的值为“有效” 。  
  
## <a name="grid-columns"></a>网格列  
 对于你创建的每个业务规则，系统都会在网格中添加一行（其中包含六列）。 下面介绍了这些列。  
  
|“属性”|说明|  
|----------|-----------------|  
|状态|在你单击“保存”  后，系统会显示下面的图像，指明业务规则正在更新。<br /><br /> ![mds_BR_refresh](../master-data-services/media/mds-br-refresh.png "mds_BR_refresh")<br /><br /> 如果在创建或编辑业务规则时出错，系统会显示下面的图像。<br /><br /> ![mds_br_error](../master-data-services/media/mds-br-error.png "mds_br_error")<br /><br /> 如果状态为“正常”，则将显示下面的图像。<br /><br /> ![mds_BR_success](../master-data-services/media/mds-br-success.png "mds_BR_success")|  
|name|业务规则名称。|  
|描述|业务规则描述。|  
|业务规则状态|以下业务规则状态之一：未定义规则、有效、已排除、待更改、待排除、待删除。|  
|已排除|指定是否排除业务规则。|  
|通知|指定要向其发送电子邮件通知的选定用户或组。|  
  
## <a name="next-steps"></a>后续步骤  
  
-   通过以下过程之一将业务规则应用到数据：  
  
    -   [针对业务规则验证特定成员 (Master Data Services)](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [针对业务规则验证版本 (Master Data Services)](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [配置业务规则以发送通知 &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)   
 [更改业务规则名称 &#40;Master Data Services&#41;](../master-data-services/change-a-business-rule-name-master-data-services.md)   
 [向业务规则添加多个条件 &#40;Master Data Services&#41;](../master-data-services/add-multiple-conditions-to-a-business-rule-master-data-services.md)  
  
  
