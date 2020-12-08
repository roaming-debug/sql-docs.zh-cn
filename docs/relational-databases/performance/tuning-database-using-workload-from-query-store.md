---
title: 使用 Query Store 中的工作负荷优化数据库 | Microsoft Docs
description: 数据库引擎优化顾问支持使用 Query Store 自动选择用于优化的适当工作负载的选项。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor, Query Store
ms.assetid: 17107549-5073-4fa2-8ee7-5ed33b38821e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 64c730f4559173f1ac8dcf1817c688dcb11e3ce4
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504941"
---
# <a name="tuning-database-using-workload-from-query-store"></a>使用 Query Store 中的工作负荷优化数据库
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 [查询存储](../../relational-databases/performance/how-query-store-collects-data.md) 功能可自动捕获查询、计划和运行时统计信息的历史记录，并将此信息保存在数据库中。 [数据库引擎优化顾问 (DTA)](../../relational-databases/performance/database-engine-tuning-advisor.md) 支持使用一个新选项来利用 Query Store 自动选择用于优化的适当工作负荷。 对于许多用户而言，使用此功能便不需要显式收集用于优化的工作负荷。 仅当数据库已启用 Query Store 功能时，此功能才可用。 
  
[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v16.4 或更高版本支持此功能。 
  
## <a name="how-to-tune-a-workload-from-query-store-in-database-engine-tuning-advisor-gui"></a>如何在数据库引擎优化顾问 GUI 中优化 Query Store 的工作负荷
在 DTA GUI 中的“常规”窗格内选中“Query Store”单选按钮以启用此功能（参阅下图）。 

![查询存储中的 DTA 工作负载](../../relational-databases/performance/media/dta-workload-from-query-store.gif)
 
## <a name="how-to-tune-a-workload-from-query-store-in-dtaexe-command-line-utility"></a>如何在 dta.exe 命令行实用工具中优化 Query Store 的工作负荷
在命令行 (dta.exe) 中，选择 **-iq** 选项来选择 Query Store 中的工作负荷。 

选择 Query Store 中的工作负荷时，可通过命令行调用其他两个选项来帮助优化 DTA 的行为。 无法通过 GUI 调用这些选项：
  1. **要优化的工作负荷事件数**：此选项使用 -n 命令行参数进行指定，可让用户控制查询存储中要优化的事件数。 默认情况下，DTA 对此选项使用值 1000。 DTA 始终根据总持续时间选择开销最大的事件。 
  
  2. **要优化的事件时限**：由于查询存储包含的查询可能是很久以前执行的，因此，此选项可让用户指定过去的某个时限（以小时为单位），DTA 只考虑优化已执行了该时限的查询。 此选项是使用 **-I** 命令行参数指定的。 

有关详细信息，请参阅 [dta Utility](../../tools/dta/dta-utility.md)。

## <a name="difference-between-using-workload-from-query-store-and-plan-cache"></a>使用 Query Store 和计划缓存中的工作负荷的差别 
Query Store 与计划缓存选项之间的差别在于，前者包含已针对数据库执行的、每次重新启动服务器都会保存的查询的更长历史记录， 而计划缓存只包含最近执行的、其计划缓存在内存中的查询的子集。 重新启动服务器时，将会丢弃计划缓存中的项。

## <a name="see-also"></a>另请参阅  
[数据库引擎优化顾问](../../relational-databases/performance/database-engine-tuning-advisor.md)     
[教程：数据库引擎优化顾问](../../tools/dta/tutorial-database-engine-tuning-advisor.md)        
[查询存储的数据收集方式](../../relational-databases/performance/how-query-store-collects-data.md)     
[Query Store 最佳做法](../../relational-databases/performance/best-practice-with-the-query-store.md)
