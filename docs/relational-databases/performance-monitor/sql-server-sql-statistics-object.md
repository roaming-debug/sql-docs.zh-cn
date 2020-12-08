---
title: SQL Server - SQL Statistics 对象 | Microsoft Docs
description: 了解 SQLServer:SQL Statistics 对象，该对象提供计数器来监视编译和发送到 SQL Server 实例的请求类型。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:SQL Statistics
- SQL Statistics object
ms.assetid: da7dbb4b-f632-45a0-b1ab-c35cc2695c86
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e37516249bf251a95d5c03ff5fad1bdeeab7fbf8
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505570"
---
# <a name="sql-server-sql-statistics-object"></a>SQL Server:SQL Statistics 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  **中的** SQLServer:SQL Statistics [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象提供计数器来监视编译和发送到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的请求类型。 通过监视查询编译和重新编译的次数以及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例收到的批数，可了解 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 处理用户查询的速度，以及查询优化器处理查询的效率。  
  
 编译时间在查询总时间中占到很大一部分。 为节省编译开销， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将编译过的查询计划保存在一个查询缓存中。 缓存的目标是通过存储编译过的查询以便再次使用来减少编译，从而在将来执行查询时，不需重新编译它。 但是，每个唯一的查询至少需要编译一次。 以下因素可造成重新编译查询：  
  
-   架构更改，包括基本架构更改（例如在表中添加列或索引）和统计架构更改（例如在表中插入或删除很多行）。  
  
-   环境（SET 语句）改变。 在 ANSI_PADDING 或 ANSI_NULLS 等会话设置中的更改，可能会导致查询的重新编译。  
  
 有关简单参数化和强制参数化的详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)。  
  
 下面列出了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Statistics 计数器。  
  
|SQL Server SQL Statistics 计数器|说明|  
|----------------------------------------|-----------------|  
|**Auto-Param Attempts/sec**|每秒的自动参数化尝试数。 其总数应为失败的、安全的和不安全的自动参数化尝试之和。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例尝试通过将某些文字替换为参数来参数化 [!INCLUDE[tsql](../../includes/tsql-md.md)] 请求时，就会进行自动参数化，这样可以对多个相似的请求再次使用保存在缓存中的执行计划。 请注意，在更新版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，自动参数化也称为简单参数化。 此计数器不包括强制参数化。|  
|**Batch Requests/sec**|每秒收到的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 命令批数。 这一统计信息受所有约束（如 I/O、用户数、高速缓存大小、请求的复杂程度等）影响。 批处理请求数值高意味着吞吐量很好。|  
|**Failed Auto-Params/sec**|每秒自动参数化尝试失败次数。 该值应很小。 请注意，在更高版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，自动参数化也称为简单参数化。|  
|**Forced Parameterizations/sec**|每秒成功执行的强制参数化次数。|  
|**Guided Plan Executions/sec**|每秒执行的计划数，其中的查询计划是通过使用计划指南生成的。|  
|**Misguided Plan Executions/sec**|每秒执行的计划数，其中的查询计划无法使用计划指南生成。 系统将忽略计划指南并使用正常的编译过程生成执行计划。|  
|**Safe Auto-Params/sec**|每秒安全自动参数化尝试次数。 安全指确定保存在缓存中的执行计划可以在不同的相似 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句之间共享。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进行多次自动参数化尝试，某些成为安全的，某些就失败了。 请注意，在更高版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，自动参数化也称为简单参数化。 它不包括强制参数化。|  
|**SQL Attention rate**|每秒发出关注信号的数目。 发出一次关注信号就是终止当前运行请求的一次客户端请求。|  
|**SQL Compilations/sec**|每秒的 SQL 编译数。 表示编译代码路径被进入的次数。 包括 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中语句级重新编译导致的编译。 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户活动稳定后，该值将达到稳定状态。|  
|**SQL Re-Compilations/sec**|每秒语句重新编译的次数。 计算触发语句重新编译的次数。 一般来说，这个数值最好较小。|  
|**Unsafe Auto-Params/sec**|每秒不安全的自动参数化尝试次数。 例如，查询有某些特性会防止保存在缓存中的计划被共享。 它们将被指定为不安全的。 此计数器不计算强制参数化次数。|  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Plan Cache 对象](../../relational-databases/performance-monitor/sql-server-plan-cache-object.md)   
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
