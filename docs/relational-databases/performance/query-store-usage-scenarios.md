---
title: Query Store 使用方案 | Microsoft Docs
description: 了解如何使用查询存储来跟踪和确保可预测的工作负载性能。 请考虑 SQL Server 中的几个示例。
ms.custom: ''
ms.date: 11/29/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store, usage scenarios
ms.assetid: f5309285-ce93-472c-944b-9014dc8f001d
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a5c182e7425e51a06b170178ee2716c42c58e115
ms.sourcegitcommit: 36fe62a3ccf34979bfde3e192cfa778505add465
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94521214"
---
# <a name="query-store-usage-scenarios"></a>Query Store 使用方案
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  在需要跟踪工作负荷并确保其性能可预测的很多情况下，都可以使用 Query Store。 下面是可以考虑使用 Query Store 的一些示例：  
  
-   找出并解决使用计划选择回归的查询  
-   确定和优化排名靠前的资源占用查询  
-   A/B 测试  
-   升级到新版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 期间保持性能稳定  
-   识别并改进临时工作负载  
  
## <a name="pinpoint-and-fix-queries-with-plan-choice-regressions"></a>找出并解决使用计划选择回归的查询  
 在常规查询执行过程中，查询优化器可以决定是否因下述重要输入变得不同而选择不同计划：数据基数已更改，索引已创建、更改或删除，统计信息已更新，等等。大多数情况下，新计划要优于以前使用的计划，或二者的效果差不多。 但有时候，新计划的效果要差很多 - 这种情况称为计划选择更改回归。 在查询存储出现之前，这是一个很难确定和解决的问题，因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 没有针对使用过一段时间的执行计划为用户提供可供查看的内置数据的存储。  
  
 使用查询存储，可快速执行以下操作：  
  
-   确定你关注的时间段内（过去一小时、昨天、上周等）执行指标已降级的所有查询。 在 **中使用** 回归查询 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 加快分析速度。  
  
-   在回归查询中，很容易找到那些有多个计划的查询，以及由于计划选择错误而降级的查询。 使用“回归查询”中的“计划摘要”窗格来显示回归查询的所有计划及其在某个时间段的查询性能。  
  
-   强制实施历史记录中的旧计划（如果该计划经证明效果更好）。 使用“回归查询”中的“强制计划”按钮，强制实施针对查询选择的计划。  
  
 ![查询数据存储的屏幕截图，其中显示了计划摘要。](../../relational-databases/performance/media/query-store-usage-1.png "query-store-usage-1")  
  
 有关方案的详细说明，请参阅 [Query Store:A flight data recorder for your database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)（查询存储：适用于数据库的飞行数据记录器）博客。  
  
## <a name="identify-and-tune-top-resource-consuming-queries"></a>确定和优化排名靠前的资源占用查询  
 虽然你的工作负荷可能会生成数千个查询，但通常情况下，使用大部分系统资源的实际上只是其中一部分查询，因此你只需要注意这部分查询。 通常情况下，在资源使用排名靠前的查询中，你会发现有些查询是回归性查询，有些查询则可在进一步优化后获得性能改善。  
  
 开始浏览时，最方便的方式是打开 **中的“资源使用排名靠前的查询”。** [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 用户界面分成三个窗格：一个直方图，代表资源使用排名靠前的查询（左）；一个针对所选查询的计划摘要（右）；一个针对所选计划的可视化查询计划（底部）。 单击“配置”按钮即可控制要分析的查询个数，以及要设置的时间间隔。  此外，还可以在不同的资源消耗维度（持续时间、CPU、内存、IO、执行数）和基线（平均、最小、最大、总计、标准偏差）之间进行选择。  
  
 ![查询数据存储的屏幕截图，其中显示用户可以识别和优化资源消耗较大的查询。](../../relational-databases/performance/media/query-store-usage-2.png "query-store-usage-2")  
  
 查看右侧的计划摘要，以便分析执行历史记录并了解各种不同的计划及其运行时统计信息。 使用底部窗格检查各种不同的计划，或者用肉眼对这些并排呈现的计划进行比较（使用“比较”按钮）。  
  
如果确定某个查询的性能不够理想，则可根据问题性质进行操作：  
  
1.  如果所执行的查询具有多个计划，而最后一个计划明显不如前面的计划，则可通过计划强制机制来确保 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在今后执行查询时使用最佳计划  
  
2.  查看优化器是否建议了 XML 计划中缺失的索引。 如果答案为是，则请创建该缺失的索引，并在创建完索引后使用 Query Store 来评估查询性能。  
  
3.  确保查询使用的基础表的统计信息是最新的。  
  
4.  确保查询所使用的索引已进行碎片整理。  
  
5.  考虑重新编写成本高的查询。 例如，可以充分利用查询参数化，减少动态 SQL 的使用。 在读取数据时实施最佳逻辑（在数据库端而非应用程序端应用数据筛选）。  

## <a name="ab-testing"></a>A/B 测试  
 在计划引入应用程序更改之前和之后，使用 Query Store 来比较工作负荷性能。 在下表包含的多个示例中，你可以使用 Query Store 来评估环境或应用程序更改对工作负荷性能的影响：  
  
-   推出新应用程序版本。  
  
-   向服务器添加新硬件。  
  
-   在消耗大量资源的查询引用的表上创建缺失的索引。  
  
-   应用筛选策略以确保行级别安全性。 有关详细信息，请参阅 [Optimizing Row Level Security with Query Store](/archive/blogs/sqlsecurity/optimizing-rls-performance-with-the-query-store)（使用查询存储优化行级别安全性）。  
  
-   将临时系统版本控制添加到由 OLTP 应用程序频繁修改的表。  
  
任何此类方案都可应用以下工作流：  
  
1.  在进行计划的更改之前，使用 Query Store 运行工作负荷，以便生成性能基线。  
  
2.  在控制的时间点应用应用程序更改。  
  
3.  继续运行工作负荷，直至生成更改后的系统性能图。  
  
4.  对 #1 和 #3 的结果进行比较。  
  
    1.  打开“数据库总体使用情况”以确定对整个数据库的影响。  
  
    2.  打开“资源使用排名靠前的查询”（或使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 运行你自己的分析），以便分析所做的更改对最重要查询的影响。  
  
5.  决定是保留所做的更改，还是在无法接受新性能的情况下进行回退。  
  
下图显示了如何在创建缺失索引的情况下进行 Query Store 分析（步骤 4）。 打开“资源使用排名靠前的查询”/“计划摘要”窗格，此时将显示会受索引创建操作影响的查询的该视图：  
  
![屏幕截图显示缺少索引创建的查询数据存储分析（步骤 4）。](../../relational-databases/performance/media/query-store-usage-3.png "query-store-usage-3")  
  
此外，你还可以在索引创建前后对计划进行比较，只需将这些计划并排呈现即可。 （“在单独的窗口中比较选定查询的计划”工具栏选项，此选项已在工具栏中使用红色正方形进行标记。）  
  
![屏幕截图显示查询数据存储，以及在单独窗口工具栏选项中显示的“比较所选查询的计划”。](../../relational-databases/performance/media/query-store-usage-4.png "query-store-usage-4")  
  
在创建索引之前的计划（plan_id = 1，见上）提示索引缺失，你可以通过检查发现 Clustered Index Scan 是查询中成本最高的运算符（红色矩形）。  
  
在创建缺失索引之后的计划（plan_id = 15，见下）现在可以使用 Index Seek (Nonclustered) 来减少查询的总体成本并改进其性能（绿色矩形）。  
  
根据分析，查询性能获得了提升，因此你会保留索引。  
  
## <a name="keep-performance-stability-during-the-upgrade-to-newer-ssnoversion"></a><a name="CEUpgrade"></a>升级到新版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 期间保持性能稳定  
在 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]之前，用户在升级到最新的平台版本时要冒性能下降的风险。 之所以会出现这种情况，是因为最新版查询优化器会在新版本安装之后即时启用。  
  
自 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 起，所有查询优化器更改都会绑定到最新的[数据库兼容性级别](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)，因此计划不会在升级后立即更改，而是在用户将 `COMPATIBILITY_LEVEL` 数据库更改为最新版本后更改。 利用此功能和 Query Store，你可以在升级过程中对查询性能进行精确的控制。 建议的升级工作流如下图所示：  
  
![显示所建议的升级工作流的示意图。](../../relational-databases/performance/media/query-store-usage-5.png "query-store-usage-5")  
  
1.  升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 而不更改数据库兼容性级别。 它不会公开最新的查询优化器更改，但仍会提供包括查询存储在内的新版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。  
  
2.  启用查询存储。 有关本主题的详细信息，请参阅[使查询存储适应工作负荷](../../relational-databases/performance/best-practice-with-the-query-store.md#Configure)。

3.  允许查询存储捕获查询和计划，并建立包含源/以前的数据库兼容性级别的性能基线。 在此步骤停留足够长的时间，确保捕获所有计划并获取稳定的基线。 这可以是生产工作负荷常用业务周期的持续时间。  
  
4.  转到最新兼容性级别：向工作负荷显示最新的查询优化器更改，让其创建可能的新计划。  
  
5.  使用查询存储进行分析并解决回归问题：大多数情况下，新查询优化器更改会生成更好的计划。 不过，查询存储可以让你轻松识别计划选择回归并使用计划强制机制对其进行修复。 从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始，使用[自动计划更正](../../relational-databases/automatic-tuning/automatic-tuning.md#automatic-plan-correction)功能时，此步骤可自动进行。  

    a.  对于出现回归的情况，请在查询存储中强制执行之前已知的有效计划。  
  
    b.  如果存在未能强制执行的查询计划，或者如果性能仍不足，请考虑将[数据库兼容级别](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)还原到之前的设置，然后寻求 Microsoft 客户支持。  
    
> [!TIP]
> 使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 升级数据库任务，升级数据库的[数据库兼容性级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#compatibility-levels-and-database-engine-upgrades)。 有关详细信息，请参阅[使用查询优化助手升级数据库](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)。
  
## <a name="identify-and-improve-ad-hoc-workloads"></a>识别并改进临时工作负载  
某些工作负载没有可通过优化来提高应用程序整体性能的主查询。 通常情况下，这些工作负荷的特点是有相对较大的不同查询，每个查询都会消耗一部分系统资源。 这些查询在性质上很独特，执行次数很少（通常仅执行一次，因此才称为即席查询），因此其运行时消耗并不重要。 另一方面，由于应用程序总是在生成全新的查询，因此大部分系统资源都消耗在没有进行优化的查询编译上。 这对于 Query Store 来说并不是一种理想的情形，因为大量的查询和计划会占据你所保留的空间，这意味着 Query Store 可能很快就会进入只读模式。 如果你激活了“基于大小的清除策略”（[强烈建议](best-practice-with-the-query-store.md)使用它来让 Query Store 始终处于启动和运行状态），则大部分时间会由后台进程清理 Query Store 结构，这也会消耗大量系统资源。  
  
 你可以通过“资源使用排名靠前的查询”视图，率先了解工作负荷的即席性质：  
  
![屏幕截图显示“资源消耗较大的查询”视图，其中显示了大多数资源消耗较大的查询只执行了一次。](../../relational-databases/performance/media/query-store-usage-6.png "query-store-usage-6")  
  
可以通过“执行计数”度量值来分析排名靠前的查询是否为即席查询（这需要使用 `QUERY_CAPTURE_MODE = ALL` 运行 Query Store）。 从上图可以看出，90% 的“资源使用排名靠前的查询”仅执行一次。  
  
此外，还可以通过运行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本来获取系统中查询文本、查询和计划的总数，并可通过比较 query_hash 和 plan_hash 来确定其差异：  
  
```sql  
--Do cardinality analysis when suspect on ad hoc workloads
SELECT COUNT(*) AS CountQueryTextRows FROM sys.query_store_query_text;  
SELECT COUNT(*) AS CountQueryRows FROM sys.query_store_query;  
SELECT COUNT(DISTINCT query_hash) AS CountDifferentQueryRows FROM  sys.query_store_query;  
SELECT COUNT(*) AS CountPlanRows FROM sys.query_store_plan;  
SELECT COUNT(DISTINCT query_plan_hash) AS  CountDifferentPlanRows FROM  sys.query_store_plan;  
```  
  
在工作负荷包含即席查询的情况下，你可能会获得这种结果：  
  
![屏幕截图显示在工作负荷包含即席查询的情况下可能得到的结果。](../../relational-databases/performance/media/query-store-usage-7.png "query-store-usage-7")  
  
查询结果显示，尽管查询存储中查询和计划的数量很大，其 query_hash 和 query_plan_hash 并没有什么不同。 唯一查询文本和唯一 query hash 的比率远远大于 1，这表明工作负荷适合进行参数化，因为这些查询之间的唯一差异就是作为查询文本一部分提供的文本常数（参数）。  
  
通常，这种情况发生的条件是你的应用程序生成了查询（而不是调用存储过程或参数化查询），或者该应用程序依赖于会默认生成查询的对象关系映射框架。  
  
如果你可以控制应用程序代码，则可以考虑重新编写数据访问层，以便利用存储过程或参数化查询。 不过，也可以在不更改应用程序的情况下显著改善这种状况，方法是针对整个数据库强制实施查询参数化（所有查询）或者使用同一 query_hash 针对单个查询模板来进行。  
  
使用单个查询模板进行操作时，需要创建计划指南：  
  
```sql  
--Apply plan guide for the selected query template 
DECLARE @stmt nvarchar(max);  
DECLARE @params nvarchar(max);  
EXEC sp_get_query_template   
    N'<your query text goes here>',  
    @stmt OUTPUT,   
    @params OUTPUT;  
  
EXEC sp_create_plan_guide   
    N'TemplateGuide1',   
    @stmt,   
    N'TEMPLATE',   
    NULL,   
    @params,   
    N'OPTION (PARAMETERIZATION FORCED)';  
```  
  
使用计划指南的解决方案操作起来更精确，但需要完成更多的工作。  
  
如果所有查询（或大部分查询）都可以进行自动参数化，则针对整个数据库更改 `FORCED PARAMETERIZATION` 可能是一个更好的选项：  
  
```sql  
--Apply forced parameterization for entire database  
ALTER DATABASE <database name> SET PARAMETERIZATION FORCED;  
```  

> [!NOTE]
> 有关本主题的详细信息，请参阅[强制参数化使用指南](../../relational-databases/query-processing-architecture-guide.md#ForcedParamGuide)。

应用任何此类步骤之后，即可通过“资源使用排名靠前的查询”从另一个角度来了解你的工作负荷。   
  
![屏幕截图显示“资源消耗较大的查询”视图，其中以另一种方式显示工作负荷。](../../relational-databases/performance/media/query-store-usage-8.png "query-store-usage-8")  
  
某些情况下，你的应用程序可能会生成大量不同的查询，而这些查询并不适合进行自动参数化。 在这种情况下，你会看到系统中存在大量查询，但唯一查询和唯一 `query_hash` 之间的比率可能接近 1。  
  
在这种情况下，建议启用“[针对即席工作负荷进行优化](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md)”服务器选项，防止将缓存内存浪费在不大可能再次执行的查询上。 若要防止在 Query Store 中捕获这些查询，可将 `QUERY_CAPTURE_MODE` 设置为 `AUTO`。  
  
```sql  
EXEC sys.sp_configure N'show advanced options', N'1' RECONFIGURE WITH OVERRIDE
GO
EXEC sys.sp_configure N'optimize for ad hoc workloads', N'1'
GO
RECONFIGURE WITH OVERRIDE
GO 
  
ALTER DATABASE [QueryStoreTest] SET QUERY_STORE CLEAR;  
ALTER DATABASE [QueryStoreTest] SET QUERY_STORE = ON   
    (OPERATION_MODE = READ_WRITE, QUERY_CAPTURE_MODE = AUTO);  
```  
  
## <a name="see-also"></a>另请参阅  
 [相关视图、函数和过程](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [查询存储最佳实践](../../relational-databases/performance/best-practice-with-the-query-store.md)         
 [使用查询优化助手升级数据库](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)           
