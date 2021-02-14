---
description: 弃用的 Master Data Services 功能
title: 弃用的 Master Data Services 功能
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: d8506bda-66dd-45a4-bfc9-3a10fa665acc
author: lrtoyou1223
ms.author: lle
manager: erikre
ms.openlocfilehash: d8cded1c88278ca67426eaf40df7bdd87474312c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350185"
---
# <a name="deprecated-master-data-services-features"></a>弃用的 Master Data Services 功能

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  本主题介绍 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 中仍然可用但不推荐使用的 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]功能。 按照计划， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]未来版本将不再具有这些功能。 在新的应用程序中不应使用这些不推荐使用的功能。  
  
## <a name="explicit-hierarchies-collections-and-related-components"></a>显式层次结构、集合和相关组件  
 已弃用显式层次结构、集合和相关组件。 之前已建模为合并成员类型（显式层次结构父项）和集合成员类型的成员将被建模为派生层次结构中的叶成员。 以下新功能使派生层次结构能够代替显式层次结构。  
  
-   现在可以使用递归派生层次结构来分配成员安全权限。  
  
     显式层次结构与递归派生层次结构类似，递归级别下具有一个非递归级别。 在递归级别以上和/或以下纳入级别会使递归派生层次结构变得复杂。  
  
-   在资源管理器中，派生层次级别页面现在将显示每个层次结构级别的未分配（未使用）成员。 根据层次结构级别对未使用的节点进行分组。 通过拖放或剪切和粘贴操作可在“未使用”节点和“根”节点之间移动成员。  
  
     在“系统管理”中，未使用的节点在“预览”  窗格中可见。 在“安全性”中，未使用的节点在“层次结构成员权限”  窗格中可见。 可以向“根”  节点下或“未使用”  节点下的任何成员分配权限。 也可以向“根”成员 、“未使用”成员 和“未使用伪”成员  分配权限。  
  
-   存储过程 (mdm.udpConvertCollectionAndConsolidatedMembersToLeaf) 会将显式层次结构转换为递归派生层次结构，并将合并成员和集合成员转换为叶成员。  
  
     仍支持显式层次结构、合并成员类型和集合成员类型，因此可以选择运行存储过程。 但是，如果使用已弃用的项，则建议运行存储过程。  
  
 有关显式层次结构、集合和合并成员的信息，请参阅以下主题。  
  
-   [显式层次结构 (Master Data Services)](../master-data-services/explicit-hierarchies-master-data-services.md)  
  
-   [集合 (Master Data Services)](../master-data-services/collections-master-data-services.md)  
  
-   [成员 &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)  
  
## <a name="attribute-entity-transaction-log-type"></a>属性实体事务日志类型  
“属性”实体事务日志类型已被弃用，请迁移至“成员”实体事务日志类型。 有关实体事务日志类型的信息，请参阅以下主题：
* [更改实体事务日志类型 (Master Data Services)](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md)
* [成员修订历史记录](../master-data-services/member-revision-history-master-data-services.md)
  
## <a name="external-resources"></a>外部资源  
 msdn.com 上的博客文章 [Deprecated: Explicit Hierarchies and Collections](https://techcommunity.microsoft.com/t5/sql-server-integration-services/deprecated-explicit-hierarchies-and-collections/ba-p/388221)（已弃用：显式层次结构和集合）。  
  
## <a name="see-also"></a>另请参阅  
 [弃用的 Master Data Services 功能](../master-data-services/discontinued-master-data-services-features.md)  
  
  
