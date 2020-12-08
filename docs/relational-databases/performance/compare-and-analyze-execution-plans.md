---
title: 比较和分析执行计划 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 比较和分析执行计划。 执行计划显示查询优化器的数据检索方法。
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Showplan results
- execution plans [SQL Server]
- queries [SQL Server], tuning
- execution plans [SQL Server], how-to topics
- SQL Server Management Studio [SQL Server], execution plans
- tuning queries [SQL Server]
ms.assetid: bcd6f094-c613-4835-ae19-4caaadb4bb17
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 9a969277322f24861e2dcd4c85e92df9e4ebb4f0
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505361"
---
# <a name="compare-and-analyze-execution-plans"></a>比较和分析执行计划
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
本节介绍如何使用 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 比较和分析执行计划。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v17.4 及更高版本支持此功能。  
  
执行计划以图形方式显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器选择的数据检索方法。 执行计划使用图标表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中特定语句和查询的执行开销，而不是使用 [SET SHOWPLAN_ALL](../../t-sql/statements/set-showplan-all-transact-sql.md) 或 [SET SHOWPLAN_TEXT](../../t-sql/statements/set-showplan-text-transact-sql.md) 语句生成的表格表示形式。 这种图形表示法对了解查询的性能特征非常有用。 

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 包括允许用户比较两个执行计划的功能，例如在同一查询的好的计划和坏的计划之间比较，并执行根本原因分析。 还包括执行单个查询计划分析的功能，通过分析其执行计划，可以深入了解可能影响查询性能的情况。

有关查询执行计划的详细信息，请参阅[估计执行计划](../../relational-databases/performance/display-the-estimated-execution-plan.md)、[实际执行计划](../../relational-databases/performance/display-an-actual-execution-plan.md)和[查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md)。
  
## <a name="in-this-section"></a>本节内容  
[比较执行计划](../../relational-databases/performance/display-the-estimated-execution-plan.md)     
[分析实际执行计划](../../relational-databases/performance/display-an-actual-execution-plan.md)      
  
