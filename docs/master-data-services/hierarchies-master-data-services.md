---
title: 层次结构
description: 层次结构是一种树结构，可用于将类似成员分组，并合并/汇总成员以便在 Master Data Services 中进行报告和分析。
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- hierarchies [Master Data Services]
- hierarchies [Master Data Services], about hierarchies
ms.assetid: 70dbb1fc-ead7-45be-9552-a45e3ccd8d21
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 9705dbcd81c263573bbc911a22e4464852fcd441
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272528"
---
# <a name="hierarchies-master-data-services"></a>层次结构 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，层次结构是一种树结构，您可以使用它：  
  
-   将类似成员分组以便使结构组织得更好。  
  
-   合并和汇总成员以便进行报告和分析。  
  
## <a name="what-hierarchies-contain"></a>层次结构包含的内容  
 每个层次结构包含一个或多个实体的成员。 添加、更改或删除成员时，将更新所有层次结构。 这可确保数据在所有层次结构中是准确的。 层次结构还有助于确保每个成员计入一次且只计入一次。  
  
 若要创建成员子集的分组，请考虑使用集合。 有关详细信息，请参阅 [集合 (Master Data Services)](../master-data-services/collections-master-data-services.md)。  
  
## <a name="kinds-of-hierarchies"></a>层次结构类型  
 可以创建多个层次结构，以不同的方式查看和组织您的成员。 您可以：  
  
-   从单个实体创建不规则层次结构（称为显式层次结构）。 有关详细信息，请参阅 [显式层次结构 (Master Data Services)](../master-data-services/explicit-hierarchies-master-data-services.md)。  
  
-   从多个实体创建基于级别的层次结构，该层次结构基于实体和其属性之间的现有关系（称为派生层次结构）。 有关详细信息，请参阅 [派生层次结构 (Master Data Services)](../master-data-services/derived-hierarchies-master-data-services.md)。  
  
> [!NOTE]  
>  层次结构中的所有成员都必须在同一模型中。  
  
## <a name="hierarchies-are-not-taxonomies"></a>层次结构不是分类  
 层次结构与分类不同。 分类组织成员时一次处理多个属性，而层次结构组织成员时一次处理一个属性。 分类可以多次包含同一成员，而层次结构只能包含成员一次。  
  
 例如，同一自行车可以包含在一个分类中两次：一次由于它是红色的，一次由于它的规格为 38。 在层次结构中，该自行车只能包含一次，因此您必须决定是依据颜色还是规格来显示它。  
  
## <a name="hierarchy-example"></a>层次结构示例  
 在下面的示例中，product 成员按 subcategory 成员进行分组。  
  
 ![按子类别分组的层次结构示例](../master-data-services/media/mds-conc-hierarchy.gif "按子类别分组的层次结构示例")  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|创建显式层次结构。|[创建显式层次结构 (Master Data Services)](../master-data-services/create-an-explicit-hierarchy-master-data-services.md)|  
|创建派生层次结构。|[创建派生层次结构 (Master Data Services)](../master-data-services/create-a-derived-hierarchy-master-data-services.md)|  
|隐藏或删除现有派生层次结构中的级别。|[隐藏或删除派生层次结构中的级别 (Master Data Services)](../master-data-services/hide-or-delete-levels-in-a-derived-hierarchy-master-data-services.md)|  
  
## <a name="related-content"></a>相关内容  
  
-   [显式层次结构 (Master Data Services)](../master-data-services/explicit-hierarchies-master-data-services.md)  
  
-   [派生层次结构 (Master Data Services)](../master-data-services/derived-hierarchies-master-data-services.md)  
  
-   [递归层次结构 (Master Data Services)](../master-data-services/recursive-hierarchies-master-data-services.md)  
  
-   [具有显式顶端的派生层次结构 &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-with-explicit-caps-master-data-services.md)  
  
-   [集合 (Master Data Services)](../master-data-services/collections-master-data-services.md)  
  
  
