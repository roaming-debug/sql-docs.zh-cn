---
description: 提示 (Transact-SQL)
title: 提示 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- query optimizer [SQL Server], hints
- hints [SQL Server], about hints
- SELECT statement [SQL Server], hints
- hints [SQL Server]
- INSERT statement [SQL Server], hints
- UPDATE statement [SQL Server], hints
- DELETE statement [SQL Server], hints
ms.assetid: 99412475-b0df-4264-9938-33a0b302b41a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 0da4ac18eaf7887c772bb7906026aaab9a476ea0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207676"
---
# <a name="hints-transact-sql"></a>提示 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  提示是指定的强制选项或策略，由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询处理器针对 SELECT、INSERT、UPDATE 或 DELETE 语句执行。 提示将覆盖查询优化器可能为查询选择的任何执行计划。  
  
> [!CAUTION]  
>  由于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器通常会为查询选择最佳执行计划，因此建议仅在最后迫不得已的情况下才可由资深的开发人员和数据库管理员使用 \<join_hint>、\<query_hint> 和 \<table_hint>。
  
 本节将介绍下列提示：  
  
-   [联接提示 ](../../t-sql/queries/hints-transact-sql-join.md)  
  
-   [查询提示](../../t-sql/queries/hints-transact-sql-query.md)  
  
-   [表提示](../../t-sql/queries/hints-transact-sql-table.md)  
  
  
