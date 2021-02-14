---
description: 要求属性值 (Master Data Services)
title: 要求属性值
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], requiring attribute values
- attributes [Master Data Services], requiring values
ms.assetid: a360ef13-0c34-43b8-a87e-2f5d8732d30e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 8e863605632b82b18f0dbe824fd45adbeaf22274
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353769"
---
# <a name="require-attribute-values-master-data-services"></a>要求属性值 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，在您想要确保主数据完整时要求属性值。  
  
> [!NOTE]  
>  缺少基于域的属性值的成员不显示在基于那些关系的派生层次结构中。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
### <a name="to-require-attribute-values"></a>要求属性值  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”**。  
  
2.  从菜单栏中，指向 **“管理”** ，然后单击 **“业务规则”**。  
  
3.  在 " **业务规则** " 页上，从 " **模型** " 下拉列表中选择一个模型。  
  
4.  从  “实体”下拉列表中选择一个实体。  
  
5.  从“成员类型”  下拉列表中，选择要应用业务规则的成员类型。  
  
6.  单击 **添加**。  
  
7.  在“名称”  框中，键入业务规则的名称。  
  
8.  （可选）在“说明”  字段中，键入业务规则描述。  
  
9. 在“Then”  块下，单击“添加” 。 此时将显示一个面板。  
  
10. 从“运算符”  下拉列表中，选择“必要操作” 。  
  
11. 从  “属性”下拉列表中选择一个属性。  
  
12. 单击“保存”。 此时，系统会在“Then”  网格中新添加一行。  
  
13. 单击“保存”。  
  
14. 单击“全部发布” 。  
  
15. 在确认对话框中，单击 **“确定”**。 “业务规则状态”  列中的值为“有效” 。  
  
## <a name="next-steps"></a>后续步骤  
  
-   通过以下过程之一将业务规则应用到数据：  
  
    -   [针对业务规则验证特定成员 (Master Data Services)](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [针对业务规则验证版本 (Master Data Services)](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a>另请参阅  
 [业务规则 &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [派生层次结构 (Master Data Services)](../master-data-services/derived-hierarchies-master-data-services.md)  
  
  
