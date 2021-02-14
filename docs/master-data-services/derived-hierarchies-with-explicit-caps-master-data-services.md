---
description: 具有显式顶端的派生层次结构 (Master Data Services)
title: 具有显式顶端的派生层次结构
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- hierarchies [Master Data Services], derived hierarchies with explicit caps
- explicit hierarchies, derived hierarchies with explicit caps
- derived hierarchies, derived hierarchies with explicit caps
ms.assetid: 6a82ff66-c137-4757-99bb-787d189b4295
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 24996956a4ee2b1fef5904337eaaf2c735770d37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350111"
---
# <a name="derived-hierarchies-with-explicit-caps-master-data-services"></a>具有显式顶端的派生层次结构 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，当来自显式层次结构的级别用作派生层次结构的顶级时，该级别称作具有显式顶端的派生层次结构。  
  
 此显式层次结构必须基于与派生层次结构顶部的实体相同的实体。  
  
 在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] 用户界面 (UI) 中，通过将显式层次结构拖到派生层次结构的顶部来创建此类型的层次结构。  
  
 ![mds_conc_explicit_cap_UI_structure](../master-data-services/media/mds-conc-explicit-cap-ui-structure.gif "mds_conc_explicit_cap_UI_structure")  
  
## <a name="derived-hierarchy-with-explicit-cap-example"></a>具有显式顶端的派生层次结构示例  
 在此示例中，该显式层次结构中的成员来自 Subcategory 实体。 在派生层次结构中，顶级成员也来自 Subcategory 实体。  
  
 ![mds_conc_explicit_cap_UI_example](../master-data-services/media/mds-conc-explicit-cap-ui-example.gif "mds_conc_explicit_cap_UI_example")  
  
 通过在某一派生层次结构的顶部使用显式层次结构，该派生层次结构将成为不规则层次结构。  
  
## <a name="rules"></a>规则  
  
-   不能在具有显式顶端的派生层次结构中具有多个显式层次结构。  
  
-   可以将相同的显式层次结构用作多个派生层次结构的顶端。  
  
-   不能将层次结构成员权限分配给具有显式顶端的派生层次结构。 如果您单独将权限分配给显式层次结构或派生层次结构，则这些权限将影响这两种层次结构。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|创建派生层次结构。|[创建派生层次结构 (Master Data Services)](../master-data-services/create-a-derived-hierarchy-master-data-services.md)|  
|创建显式层次结构。|[创建显式层次结构 (Master Data Services)](../master-data-services/create-an-explicit-hierarchy-master-data-services.md)|  
|删除现有派生层次结构。|[删除派生层次结构 (Master Data Services)](../master-data-services/delete-a-derived-hierarchy-master-data-services.md)|  
|||  
  
## <a name="related-content"></a>相关内容  
  
-   [派生层次结构 (Master Data Services)](../master-data-services/derived-hierarchies-master-data-services.md)  
  
-   [显式层次结构 (Master Data Services)](../master-data-services/explicit-hierarchies-master-data-services.md)  
  
  
