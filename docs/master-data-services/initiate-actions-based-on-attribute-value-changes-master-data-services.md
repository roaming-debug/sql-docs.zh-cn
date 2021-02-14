---
description: 基于属性值更改启动操作 (Master Data Services)
title: 基于属性值更改启动操作
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], tracking attribute changes
- change tracking groups [Master Data Services], initiating actions
ms.assetid: 5e4402ce-31db-4774-a2a1-552335f87693
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f9258c1019909cecd67573fad575a37b6e9d1d8f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347761"
---
# <a name="initiate-actions-based-on-attribute-value-changes-master-data-services"></a>基于属性值更改启动操作 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，创建业务规则以便基于对属性值的更改启动操作。 例如，在某个特定的属性值发生更改时，您可能需要更改值、发送通知或启动外部工作流。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
-   您的属性必须处于更改跟踪组中。 有关详细信息，请参阅 [向更改跟踪组添加属性 (Master Data Services)](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md) 。  
  
### <a name="to-create-a-business-rule-to-initiate-actions-based-on-attribute-value-changes"></a>创建业务规则以便基于属性值更改启动操作  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  从菜单栏中，指向 **“管理”** ，然后单击 **“业务规则”**。  
  
3.  在 **“业务规则维护”** 页上，从 **“模型”** 列表中，选择某一模型。  
  
4.  从 **“实体”** 列表中，选择某一实体。  
  
5.  从 **“成员类型”** 列表中，为要应用于的业务规则选择成员类型。  
  
6.  从 **“属性”** 列表中，选择某一属性或保持默认值 **“全部”**。  
  
7.  单击 **“添加业务规则”**。  
  
8.  单击 **“编辑所选业务规则”**。  
  
9. 在 **“组件”** 窗格中，展开 **“条件”** 节点。  
  
10. 在 **“值比较”** 节点下，将 **“已更改”** 拖到 **IF** 窗格的 **“条件”** 标签。  
  
11. 在 **“编辑条件”** 窗格的 **“更改跟踪组”** 框中，键入您作为必备组件的一部分分配的更改跟踪组的编号。  
  
12. 在 **“编辑条件”** 窗格中，单击 **“保存项”**。  
  
13. 在 **“组件”** 窗格中，展开 **“操作”** 节点。  
  
14. 单击某一操作，然后将其拖到 **THEN** 窗格的 **“操作”** 标签。  
  
15. 在 **“属性”** 窗格中，单击某一属性并且将其拖到 **“编辑操作”** 窗格的 **“选择属性”** 标签。  
  
16. 在 **“编辑操作”** 窗格中，完成必填字段。  
  
17. 在 **“编辑操作”** 窗格中，单击 **“保存项”**。  
  
18. 单击 **“上一步”**。  
  
19. 或者，在“业务规则维护”页上，对于包含业务规则的行，双击“名称”、“说明”或“通知”列中的单元以便更新值。  
  
    > [!NOTE]  
    >  仅针对包括验证操作的规则发送通知。  
  
20. 单击 **“发布业务规则”**。  
  
21. 在确认对话框中，单击 **“确定”**。 规则的状态将更改为 **“活动”**。  
  
## <a name="next-steps"></a>后续步骤  
  
-   通过以下过程之一将业务规则应用到数据：  
  
    -   [针对业务规则验证特定成员 (Master Data Services)](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [针对业务规则验证版本 (Master Data Services)](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [将属性添加到更改跟踪组 &#40;Master Data Services&#41;](../master-data-services/add-attributes-to-a-change-tracking-group-master-data-services.md)   
 [业务规则 (Master Data Services)](../master-data-services/business-rules-master-data-services.md)  
  
  
