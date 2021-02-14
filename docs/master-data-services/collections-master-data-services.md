---
description: 集合 (Master Data Services)
title: 集合
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- collections [Master Data Services]
- collections [Master Data Services], about collections
ms.assetid: 5aa1d1e0-b4e5-4897-8e74-01dcf418df73
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 574a99b245764a504297829ba3c037186d88672d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272788"
---
# <a name="collections-master-data-services"></a>集合 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  集合是一组来自单个实体的叶成员和合并成员。 不需要完整层次结构并且想要查看不同的成员分组以便进行报告或分析时，或者需要创建分类时，使用集合。  
  
> [!NOTE]  
>  集合已启用。  
  
## <a name="what-collections-contain"></a>集合包含的内容  
 集合不限制可以包含的成员数或成员类型，只要这些成员在同一个实体中即可。 集合可以包含来自多个强制性显式层次结构和非强制性显式层次结构的叶成员和合并成员。  
  
 创建集合时，创建的不是分层结构，而是成员的平面列表。 从层次结构中选择某个节点并将其添加到集合时，您所选的合并成员是唯一添加到集合的成员。  
  
 集合还可以包含其他集合。 可以使用集合的集合来创建分类。  
  
 创建集合时，您将自动作为所有者列出。 如果您是管理员，则可以根据需要为您的集合创建其他属性。  
  
## <a name="subscription-views-for-collections"></a>集合的订阅视图  
 显示集合的订阅视图有两种。 **集合属性** 格式显示集合列表以及与集合相关的任何属性（如说明或所有者）。 **“集合”** 格式显示所有集合中的所有成员以及每个成员权重和排序顺序。 有关详细信息，请参阅 [概述：导出数据 (Master Data Services)](../master-data-services/overview-exporting-data-master-data-services.md)。  
  
 如果为集合中的特定成员设置了权重值，则这些值在相关订阅视图中可用。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|创建新集合。|[创建集合 (Master Data Services)](../master-data-services/create-a-collection-master-data-services.md)|  
|向现有集合添加成员。|[将成员添加到集合 (Master Data Services)](../master-data-services/add-members-to-a-collection-master-data-services.md)|  
  
## <a name="related-content"></a>相关内容  
  
-   [显式层次结构 (Master Data Services)](../master-data-services/explicit-hierarchies-master-data-services.md)  
  
-   [概述：导出数据 (Master Data Services)](../master-data-services/overview-exporting-data-master-data-services.md)  
  
  
