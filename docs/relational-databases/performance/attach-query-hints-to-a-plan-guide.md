---
title: 将查询提示附加到计划指南 | Microsoft Docs
description: 计划指南中可以使用有效查询提示的任何组合。 了解有关在 SQL Server 中将提示附加到计划指南的信息。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: 2131f796-6359-4f9e-9047-da0b3d4dedaf
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6d2980803fb7a3361d74ccd86c0bcef4da734317
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505383"
---
# <a name="attach-query-hints-to-a-plan-guide"></a>将查询提示附加到计划指南
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  计划指南中可以使用有效查询提示的任何组合。 当计划指南与某一查询匹配时，计划指南提示子句中指定的 OPTION 子句将先添加到该查询中，然后该查询才会进行编译和优化。 如果与计划指南匹配的查询已包含 OPTION 子句，则计划指南中指定的查询提示将替换查询中已存在的查询提示。 但是，对于要与已包含 OPTION 子句的查询匹配的计划指南，在 sp_create_plan_guide 语句中指定要匹配的查询文本时，必须包含查询的 OPTION 子句。 若要将计划指南中指定的提示添加到查询中已存在的提示，而不是替换已存在的提示，则必须在计划指南的 OPTION 子句中同时指定原始提示和附加提示。  
  
> [!CAUTION]  
>  计划指南错用查询提示会导致出现编译、执行或性能问题。 因此计划指南应仅由经验丰富的开发人员和数据库管理员使用。  
  
## <a name="common-query-hints-used-in-plan-guides"></a>计划指南中常用的查询提示  
 能够从计划指南中受益的查询通常是基于参数的，并且性能较差，因为它们使用的是参数值不表示最差情况或最具代表性方案的缓存查询计划。 OPTIMIZE FOR 查询提示和 RECOMPILE 查询提示可用于解决这一问题。 优化查询时，OPTIMIZE FOR 会指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用参数的特定值。 执行后，RECOMPILE 指示服务器放弃查询计划，并在下次执行相同的查询时强制查询优化器重新编译新的查询计划。 有关示例，请参阅 [计划指南](../../relational-databases/performance/plan-guides.md)。  
  
 此外，您可以指定表提示 INDEX、FORCESCAN 和 FORCESEEK 作为查询提示。 指定为查询提示时，这些提示的行为类似于内联表提示或视图提示。 INDEX 提示强制查询优化器仅使用指定的索引来访问被引用表或视图中的数据。 FORCESEEK 提示强制优化器仅使用索引查找操作来访问被引用表或视图中的数据。 这些提示提供了附加的计划指南功能并允许用户更多地干预使用计划指南的查询的优化。  
  
  
