---
title: 隔离性能问题 | Microsoft Docs
description: 使用 SQL Server Profiler 和系统监视器来监视和排除 Transact-SQL 问题、与应用程序相关的问题、硬件问题和与系统相关的问题。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- isolating performance problems [SQL Server]
- monitoring performance [SQL Server], isolating problems
- database monitoring [SQL Server], isolating problems
- tuning databases [SQL Server], isolating problems
- monitoring server performance [SQL Server], isolating problems
- database performance [SQL Server], isolating problems
- server performance [SQL Server], isolating problems
ms.assetid: 2eb425cb-9166-4027-ae08-c8fc2d236f44
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 39db53bb4f198783b3da6c57d447f95467654287
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505191"
---
# <a name="isolate-performance-problems"></a>隔离性能问题
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  在一般情况下，同时使用多个 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Microsoft Windows 工具比一次只用一个工具隔离数据库性能问题更有效。 例如，图形执行计划功能（也称为“显示计划”）可以迅速识别单个查询中的死锁。 然而，如果同时使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 Windows 的监视功能，将更容易识别某些其他性能问题。  
  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 可用来监视和解决 Transact-SQL 问题以及与应用程序有关的问题。 可以使用系统监视器监视硬件问题和其他与系统有关的问题。  
  
 可以监视下列方面以解决问题：  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储过程或用户应用程序提交的批处理 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
-   用户活动（如阻塞锁或死锁）。  
  
-   硬件活动（如磁盘使用）。  
  
 问题可以包括：  
  
-   应用程序开发错误（包括错误编写 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句）。  
  
-   硬件错误（如磁盘错误或与网络有关的错误）。  
  
-   由于数据库设计不正确导致的过多阻塞。  
  
## <a name="tools-for-common-performance-problems"></a>用于解决常见性能问题的工具  
 仔细选择您希望每个工具监视或优化的性能问题同样重要。 所选的工具和实用工具取决于您要解决的性能问题的类型。  
  
 以下主题将介绍各种监视和优化工具及它们可以解决的问题。  
  
 [识别瓶颈](../../relational-databases/performance/identify-bottlenecks.md)  
  
 [监视内存用量](../../relational-databases/performance-monitor/monitor-memory-usage.md)  
  
  
