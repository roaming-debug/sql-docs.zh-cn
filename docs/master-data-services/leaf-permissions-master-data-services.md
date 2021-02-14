---
description: 叶权限（主数据服务）
title: 叶权限
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- attribute groups [Master Data Services], permissions
- members [Master Data Services], leaf member permissions
- permissions [Master Data Services], leaf members
- leaf members [Master Data Services], attribute permissions
- attributes [Master Data Services], leaf member attribute permissions
ms.assetid: bde16e8c-bcd4-4041-8130-55c5450e5f72
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 1924b33891b36d68a7b79c9046e0375b92f1d940
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338212"
---
# <a name="leaf-permissions-master-data-services"></a>叶权限（主数据服务）

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  叶权限应用到实体的所有叶成员的属性值。  
  
 对于未启用显式层次结构的实体，将权限分配给 **“叶”** 等同于将权限分配给实体。  
  
 **注意：**  
  
-   叶权限仅应用到用户界面的 **“资源管理器”** 功能区域。  
  
-   不强制向 **Name** 和 **Code** 属性分配权限。  
  
|权限|说明|  
|----------------|-----------------|  
|**读取**|用户可以读取叶成员、属性。|  
|**创建**|用户可以创建叶成员，并在创建过程中指定属性值。|  
|**更新**|用户可以更新叶成员和属性。|  
|**删除**|用户可以删除叶成员。|  
|**拒绝**|拒绝对叶成员的所有访问权限。|  
  
 读取、创建、更新和删除权限可以合并。 如果已分配创建、更新和删除权限，那么系统会自动分配读取权限。  
  
## <a name="attribute-permissions"></a>属性权限  
 属性权限应用到该属性用于特定实体的值。 仅具有属性权限的用户不能添加或删除成员。  
  
|权限|说明|  
|----------------|-----------------|  
|**读取**|用户可以读取属性。|  
|**创建**|用户可以在创建成员时分配值。|  
|**更新**|用户可以更新属性。|  
|**删除**|无效。|  
|**拒绝**|不显示属性。<br /><br /> 注意：不能明确拒绝对 Name 和 Code 属性的访问权限。|  
  
### <a name="example"></a>示例  
 对于 Product 实体，将 **“更新”** 权限分配给 Subcategory 属性。 向所有其他属性分配“拒绝”权限。  
  
|name|代码|Subcategory（更新）|  
|----------|----------|----------------------------|  
|Mountain-100|BK-M101|{5} 山地自行车|  
|Mountain-100|BK-M201|{5} 山地自行车|  
  
 在 **“资源管理器”** 中，可以更新 Subcategory 列中的任何属性值。 如果您没有对属性的权限，则不显示该属性。  
  
> [!NOTE]  
>  在本示例中，Subcategory 是基于域的属性，基于 SubcategoryList 实体。 可以为 Mountain-100 选择不同子类别，但是不能向 SubcategoryList 实体添加成员或从中删除成员。  
  
## <a name="see-also"></a>另请参阅  
 [分配模型对象权限 (Master Data Services)](../master-data-services/assign-model-object-permissions-master-data-services.md)   
    
 [&#40;Master Data Services 的模型对象权限&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [成员 &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)   
 [属性 (Master Data Services)](../master-data-services/attributes-master-data-services.md)  
  
  
