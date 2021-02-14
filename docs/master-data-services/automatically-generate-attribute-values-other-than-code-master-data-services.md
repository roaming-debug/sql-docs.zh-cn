---
description: 自动生成 Code 之外的属性值 (Master Data Services)
title: 自动生成属性值
titleSuffix: Master Data Services
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: b82f6f81-6e9c-4918-9ea9-4ab5f5d11b15
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 90302e701b8975d300e77cc7ca028c76825a79f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339548"
---
# <a name="automatically-generate-attribute-values-other-than-code-master-data-services"></a>自动生成 Code 之外的属性值 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，当希望在每次应用业务规则时自动分配一个整数作为值时，自动为实体的属性值生成值。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
-   数值属性必须存在。 有关详细信息，请参阅[创建数字属性 (Master Data Services)](../master-data-services/create-a-numeric-attribute-master-data-services.md)。  
  
### <a name="to-automatically-generate-an-attribute-value"></a>自动生成属性值  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  从菜单栏中，指向 **“管理”** ，然后单击 **“业务规则”**。  
  
3.  在 **“业务规则维护”** 页上，从 **“模型”** 列表中，选择某一模型。  
  
4.  从 **“实体”** 列表中，选择某一实体。  
  
5.  从 **“成员类型”** 列表中，为要应用于的业务规则选择成员类型。  
  
6.  从 **“属性”** 列表中，保留默认值 **“全部”**。  
  
7.  单击 **“添加业务规则”**。  
  
8.  单击 **“编辑所选业务规则”**。  
  
9. 在 **“组件”** 窗格中，展开 **“操作”** 节点。  
  
10. 在“默认值”节点中，单击“默认为生成的值”并将其拖到“THEN”窗格的“操作”标签。  
  
11. 在 **“属性”** 窗格中，单击要生成其值的属性并将其拖到 **“编辑操作”** 窗格的 **“选择属性”** 标签。  
  
12. 在 **“起始”** 和 **“增量”** 框中键入值。 如果成员已存在，则将基于最大的现有值设置值。 例如，如果最大的现有值为 299 并且将“增量”设置为“1”，则下一个成员的值将设置为 300。  
  
13. 在 **“编辑操作”** 窗格中，单击 **“保存项”**。  
  
14. 单击 **“上一步”**。  
  
15. 或者，在“业务规则维护”页上，对于包含业务规则的行，双击“名称”、“说明”或“通知”列中的单元以便更新值。  
  
16. 单击 **“发布业务规则”**。  
  
17. 在确认对话框中，单击 **“确定”**。 规则的状态将更改为 **“活动”**。  
  
## <a name="next-steps"></a>后续步骤  
  
-   [针对业务规则验证特定成员 (Master Data Services)](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
-   [针对业务规则验证版本 (Master Data Services)](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [自动创建代码 &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md)   
 [业务规则 &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [验证 (Master Data Services)](../master-data-services/validation-master-data-services.md)  
  
  
