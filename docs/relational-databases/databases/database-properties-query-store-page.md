---
title: 数据库属性（“查询存储”页）| Microsoft Docs
description: 了解如何使用“数据库属性”对话框中的“查询存储”选项卡来配置查询存储模式、间隔、阈值和其他属性。
ms.custom: ''
ms.date: 11/09/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.querystore.f1
ms.assetid: da47d75e-291a-4305-acef-4b0aaf5215da
author: stevestein
ms.author: sstein
ms.openlocfilehash: a0a7612c37ff63f851e5c3a42215192f9f982bdf
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171819"
---
# <a name="database-properties-query-store-page"></a>数据库属性（查询存储页）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  从主体数据库访问此页面，并用它来配置和修改数据库查询存储的属性。 这些选项也可使用 [ALTER DATABASE SET 选项](../../t-sql/statements/alter-database-transact-sql-set-options.md)进行更改。 有关查询存储的信息，请参阅 [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)。  
  
||  
|-|  
|**适用范围**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 到 [当前版本](../../sql-server/what-s-new-in-sql-server-2016.md)）、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。|  
  
## <a name="options"></a>选项  
 操作模式  
 有效值为 OFF、READ_ONLY 和 READ_WRITE。 OFF 禁用 Query Store。 在 READ_WRITE 模式下，查询存储将收集并保留查询计划和运行时执行统计信息。 在 READ_ONLY 模式下，可以从查询存储读取信息，但不会添加新信息。 如果已用尽查询存储的最大分配空间，查询存储的操作模式将更改为 READ_ONLY。  
  
 操作模式（实际）  
 获取查询存储的实际操作模式。  
  
 操作模式（按请求）  
 获取和设置查询存储的所需操作模式。  
  
 数据刷新间隔（分钟）  
 确定写入到查询存储的数据保留到磁盘的频率。 为了优化性能，由查询存储收集的数据应以异步方式写入到磁盘。 所配置的此异步传输发生的频率。  
  
 统计信息收集间隔  
 获取和设置统计信息收集间隔值。  
  
 最大大小 (MB)  
 获取和设置分配给查询存储的总空间。  
  
 Query Store 捕获模式  
 -   None 不捕获新查询。  
  
-   All 捕获所有查询。  
  
-   Auto 根据资源消耗情况捕获查询。  
  
 过时查询阙值（天）  
 获取和设置过时查询阙值。 配置 STALE_QUERY_THRESHOLD_DAYS 参数，以指定数据在查询存储保留的天数。  
  
 清除查询数据  
 删除查询存储的内容。  
  
 饼图  
 左侧图表显示了磁盘上的数据库文件总占用量，以及哪部分的文件填充了查询存储数据。  
  
 右侧图表显示了目前已占用了哪部分的查询存储配额。 请注意，左侧图表未显示配额。 配额可能超过数据库的当前大小。  
  
## <a name="remarks"></a>备注  
 查询存储功能让 DBA 可以探查查询计划选项和性能。 它让你可以快速找到查询计划中的更改所造成的性能差异，从而简化了性能疑难解答。 这一性能会自动捕获查询、计划和运行时统计信息的历史记录，并将其保留以供你查看。 它按时间窗口将数据分割开来，使你可以查看数据库使用情况模式并了解服务器上何时发生了查询计划更改。 可使用此查询存储数据库属性页面，或者使用 [ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md) 选项来配置查询存储。 查询存储通过使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 对话框来呈现信息。 有关查询存储的详细信息，请参阅 [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)。  
  
## <a name="see-also"></a>另请参阅  
 [查询存储存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [查询存储目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)