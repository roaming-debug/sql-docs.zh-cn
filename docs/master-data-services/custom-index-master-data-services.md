---
description: 自定义索引 (Master Data Services)
title: 自定义索引
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: c57bf8b8-55a6-4b6c-9adb-91b5f4f1ee3c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4431155c44db9a76f8ebf94f9a6be228e9fd358a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339081"
---
# <a name="custom-index-master-data-services"></a>自定义索引 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  自定义索引在实体中 (单个索引) 或 (复合索引) 的特性列表中创建非聚集索引。 通常索引可提高查询过程的性能。 有关 SQL Server 索引的详细信息，请参阅 [索引](../relational-databases/indexes/indexes.md)。  
  
## <a name="type-of-indexes"></a>索引类型  
 可以为每个实体创建以下类型的多个自定义索引。  
  
-   唯一索引  
  
-   非唯一索引  
  
 唯一索引确保索引的列中不包含重复值。 对于组合唯一索引，索引确保所选特性列表中每个值的组合都是唯一的。 如果所选特性存在重复值，则不能创建唯一索引。  
  
## <a name="rules"></a>规则  
 以下规则适用于自定义索引，唯一和非唯一均可。  
  
-   若要创建自定义索引，请确保至少选择一个特性。  
  
-   如果尝试保存的索引与另一索引具有相同的特性列表和唯一性标志，则无法保存该索引。 会显示错误。  
  
    > [!NOTE]  
    >  MDS 自动为某些特性创建索引（如 DBA 和代码）。 这意味着不能创建包含这些特性之一且不包含其他特性的另一个索引。  
  
-   只要其他索引中至少有一个不同的特性，特性就可以包含在多个自定义索引中。 否则，这些索引是相同的。  
  
-   如果创建包含很多或大型特性的索引，并且所选特性的总大小超过索引键的最大大小（900 个字节），则该索引将无法保存。  
  
-   可以对叶成员特性创建自定义索引，文件特性除外。  
  
-   如果想要删除自定义索引中包含的特性，以下内容适用。  
  
    -   如果仅对某一特性（单个索引）创建索引，则会将该特性和索引都删除。  
  
    -   如果对多个特性（组合索引）创建索引，在编辑该索引前不能删除该特性。  
  
-   不能更改自定义索引中包含的特性类型。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|任务说明|主题|  
|----------------------|-----------|  
|创建索引|[创建索引 (Master Data Services)](../master-data-services/create-an-index-master-data-services.md)|  
|编辑和删除索引|[编辑和删除索引 (Master Data Services)](../master-data-services/edit-and-delete-an-index-master-data-services.md)|  
  
  
