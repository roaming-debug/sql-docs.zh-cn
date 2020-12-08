---
title: 显示和保存执行计划 | Microsoft Docs
description: 了解如何显示执行计划以及如何使用 SQL Server Management Studio 将执行计划保存到 XML 格式的文件中。
ms.custom: ''
ms.date: 08/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
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
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f06128a425040fbd6542b12d0275276efa66d480
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505256"
---
# <a name="display-and-save-execution-plans"></a>显示和保存执行计划
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
本节说明如何显示执行计划以及如何使用 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]将执行计划保存到 XML 格式的文件中。  
  
执行计划以图形方式显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器选择的数据检索方法。 执行计划使用图标表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中特定语句和查询的执行开销，而不是使用 [SET SHOWPLAN_ALL](../../t-sql/statements/set-showplan-all-transact-sql.md) 或 [SET SHOWPLAN_TEXT](../../t-sql/statements/set-showplan-text-transact-sql.md) 语句生成的表格表示形式。 这种图形表示法对了解查询的性能特征非常有用。  

虽然 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器只生成一个执行计划，但存在估计的执行计划和实际的执行计划的概念 。
-  [估计的执行计划](../../relational-databases/performance/display-the-estimated-execution-plan.md)将返回由查询优化器在编译时生成的执行计划。 生成估计的执行计划不会确实执行查询或批处理，因此不包含任何运行时信息，如实际资源使用量度量值或运行时警告。 
-  [实际的执行计划](../../relational-databases/performance/display-an-actual-execution-plan.md)将在查询或批处理完成执行后返回查询优化器生成的执行计划。 这包括资源使用量度量值和任何运行时警告的相关信息。  

有关查询执行计划的详细信息，请参阅[查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md)。
  
## <a name="in-this-section"></a>本节内容  
  
-   [显示估计的执行计划](../../relational-databases/performance/display-the-estimated-execution-plan.md)  
  
-   [显示实际执行计划](../../relational-databases/performance/display-an-actual-execution-plan.md)  
  
-   [以 XML 格式保存执行计划](../../relational-databases/performance/save-an-execution-plan-in-xml-format.md)  
  
  
