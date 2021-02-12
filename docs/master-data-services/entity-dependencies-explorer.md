---
description: 实体依赖关系资源管理器
title: 实体依赖关系资源管理器
ms.custom: ''
ms.date: 04/06/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
keywords:
- master data services
ms.assetid: 9d922118-1412-4a9d-9c02-70d6c48d6c0d
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 46fceca874105ee036815d2149638be75a53b687
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272568"
---
# <a name="entity-dependencies-explorer"></a>实体依赖关系资源管理器

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  
[!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] 2016 将添加一个新的资源管理器页，即“实体依赖关系”，它提供了另一种方法，可在模型中按其基于域的属性 (DBA) 值将实体成员的关系可视化，而不必首先定义一个派生层次结构。   
  
它有助于回答以下问题：“谁在使用我的实体，如何使用？”。 此视图类似于派生层次结构资源管理器页面，但它包含的内容更多。 它显示了所有 DBA 关系，而不仅仅是那些定义为某个特定层次结构的一部分的关系。 由于显示的层次结构只是从现有的 DBA 推断，因此层次结构定义不是必需的。  
  
在资源管理器页面菜单中，实体依赖关系菜单项列出了模型中至少被一个实体所依赖的所有实体（即，至少一个实体具有引用列出的实体的 DBA）。 实体名称旁边显示依赖项（直接和间接）的数量，并按此数字对列表以及顶部大量引用的实体进行排序。 以下来自 [sample data（数据采样）](./sql-server-samples-model-deployment-packages-mds.md)的 Customer 模型的屏幕截图显示，BigArea 实体由 7 个实体（直接或间接）引用：  
  
![MDS_EntityDependencies_Menu.jpg](../master-data-services/media/mds-entitydependencies-menu-jpg.jpg)  
    
单击此菜单项将打开 BigArea 实体的新实体依赖关系资源管理器页面，其中介绍了使用实体成员的方法。 它显示了一个层次结构视图，其中 BigArea 成员位于树的顶部，下面嵌套了其 7 个正在使用的实体成员：  
  
![MDS_EntityDependencies_Tree.jpg](../master-data-services/media/mds-entitydependencies-tree-jpg.jpg)  
    
一个实体可能被多个实体直接使用。 在上面的示例中，SubRegion 和 RegionClimate 实体（具有 DBA 引用）都使用 Region 实体。 使用的每个实体的成员都在该实体名称的父节点下组合在一起：   
  
![MDS_EntityDependencies_Entity_Node.jpg](../master-data-services/media/mds-entitydependencies-entity-node-jpg.jpg)  
  
这些容器树节点在实体名称左侧有一个类似网格的图标，并按层次结构级别深度分色显示文本。 上例表明，“CDSR {Canada}”SubRegion 具有对“CDR {Canada}” Region 的 DBA 引用，该区域引用了“CDA {Canada}” Area，而它又引用了“NAm {N. America}”BigArea。  
  
该视图是完全可编辑的，就像层次结构资源管理器页面一样。 通过剪切和粘贴或拖放子成员，将其从一个父级复制到另一个父级，可以修改树中的父 - 子关系。 可以在树右侧的详细信息面板中修改其他成员属性值。   
  
  
  
