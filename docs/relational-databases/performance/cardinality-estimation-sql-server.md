---
title: 基数估计 (SQL Server) | Microsoft Docs
description: SQL Server 查询优化器选择估计的处理成本最低的查询计划，该计划将根据已处理的行和成本模型来确定。
ms.custom: ''
ms.date: 02/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- cardinality estimator
- CE (cardinality estimator)
- estimating cardinality
ms.assetid: baa8a304-5713-4cfe-a699-345e819ce6df
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f2d814c604ac742723fc1fb9c7e3c12dc375884f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97417976"
---
# <a name="cardinality-estimation-sql-server"></a>基数估计 (SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器是基于成本的查询优化器。 也就是说，它选择估计处理成本最低的查询计划。 查询优化器基于以下两个主要因素来确定执行查询计划的开销：

- 查询计划每个级别上处理的总行数，称为该计划的基数。
- 由查询中所使用的运算符规定的算法的开销模式。

第一个因素（基数）用作第二个因素（开销模式）的输入参数。 因此，增大基数将减少估计开销，从而加快执行计划。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的基数估计 (CE)主 要派生自创建索引或统计信息时所创建的直方图（以手动或自动方式）。 有时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还使用查询的约束信息和逻辑重写来确定基数。

在下列情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法精确计算基数。 这会导致成本计算不准确，进而可能导致查询计划欠佳。 避免在查询中使用这些构造可以提高查询性能。 有时，使用查询表达式或其他措施也可以提高查询性能，如下所述：

- 带谓词的查询，这些查询在同一表的不同列之间使用比较运算符。
- 带谓词的查询，这些查询使用运算符且下列任何一种情况为 True：
  - 运算符两侧所涉及的列中没有统计信息。
  - 统计信息中值的分布不均匀，但查询将查找高选择性的值集。 特别是，当运算符是除相等 (=) 运算符以外的任何其他运算符时，这种情况可能为 True。
  - 谓词使用不等于 (!=) 比较运算符或 `NOT` 逻辑运算符。
- 使用任意 SQL Server 内置函数或标量值用户定义函数（其参数不是常量值）的查询。
- 包含通过算术或字符串串联运算符联接的列的查询。
- 比较在编译或优化查询时其值未知的变量的查询。

本文将阐释如何评估和选择系统的最佳 CE 配置。 大多数系统受益于最新的 CE，因为它最准确。 CE 将预测查询可能返回的行数。 查询优化器使用基数预测来生成最佳查询计划。 通过更准确的估计，查询优化器通常可以更好地生成更优查询计划。  
  
你的应用程序系统可能具有重要的查询，其计划由于 CE 在各版本中发生而更改为较慢计划。 你可以使用技术和工具来识别因 CE 问题而执行变慢的查询。 你也可以选择如何解决后续性能问题。
  
## <a name="versions-of-the-ce"></a>CE 的版本

在 1998 年，CE 的重大更新是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 的一部分，其兼容级别为 70。 此版本的 CE 模型建立在四个基本假设之上：

-  **独立性：** 假设不同列上的数据分布是独立于彼此的，除非可获取相关性信息且信息可用。
-  **一致性：** 不同值均匀分布且具有相同的频率。 更确切地说，是在每个[直方图](../../relational-databases/statistics/statistics.md#histogram)步骤中，不同值均匀分布，并且每个值都具有相同的频率。 
-  **包容（简单）：** 用户查询已存在的数据。 例如，对于两个表之间的等值联接，在联接直方图以评估联接选择性之前，考虑每个输入直方图中的谓词选择性<sup>1</sup>。 
-  **包含：** 对于 `Column = Constant` 的筛选器谓词，通常假定事实上关联列存在常数。 如果相应的直方图步骤非空，则假定该步骤的其中一个不同值匹配谓词的值。

  <sup>1</sup>满足谓词的行计数。

后续更新从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 开始，意味着兼容性级别为 120 及以上。 级别 120 及以上的 CE 更新中引入了已更新的假设和算法，非常适用于现代数据仓库和 OLTP 工作负荷。 从 CE 70 假设开始，以下模型假设已自 CE 120 起更改：

-  独立性转变为相关性   ：不同列值的组合不一定独立。 这可能类似于更真实的数据查询。
-  简单包含转变为基本包含   ：用户可能查询不存在的数据。 例如，对于两个表之间的等值联接，我们使用基本表直方图来评估联接选择性，然后考虑谓词选择性因素。
  
**兼容性级别：** 通过使用以下 [COMPATIBILITY_LEVEL](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) 的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码，可以确保数据库位于特定级别。  

```sql  
SELECT ServerProperty('ProductVersion');  
GO  
  
ALTER DATABASE <yourDatabase>  
SET COMPATIBILITY_LEVEL = 130;  
GO  
  
SELECT d.name, d.compatibility_level  
FROM sys.databases AS d  
WHERE d.name = 'yourDatabase';  
GO  
```  
  
对于在兼容级别 120 及以上设置的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库，激活[跟踪标志 9481](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 会强制系统使用 CE 版本 70。  
  
**旧版 CE：** 对于在兼容级别 120 及以上设置的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库，CE 版本 70 可通过在数据库级别使用 [ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) 来激活。
  
```sql  
ALTER DATABASE SCOPED CONFIGURATION 
SET LEGACY_CARDINALITY_ESTIMATION = ON;  
GO  
  
SELECT name, value  
FROM sys.database_scoped_configurations  
WHERE name = 'LEGACY_CARDINALITY_ESTIMATION';  
GO
```  
 
或者从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1（[查询提示](../../t-sql/queries/hints-transact-sql-query.md#use_hint) `USE HINT ('FORCE_LEGACY_CARDINALITY_ESTIMATION')`）开始。
 
 ```sql  
SELECT CustomerId, OrderAddedDate  
FROM OrderTable  
WHERE OrderAddedDate >= '2016-05-01'
OPTION (USE HINT ('FORCE_LEGACY_CARDINALITY_ESTIMATION'));  
```
 
**查询存储：** 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，查询存储是用于检查查询性能的一种方便的工具。 在 [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] 中的数据库节点下的对象资源管理器中，当查询存储启用时，显示“查询存储”节点   。  
  
```sql  
ALTER DATABASE <yourDatabase>  
SET QUERY_STORE = ON;  
GO  
  
SELECT q.actual_state_desc AS [actual_state_desc_of_QueryStore],  
        q.desired_state_desc,  
        q.query_capture_mode_desc  
FROM sys.database_query_store_options AS q;  
GO  
  
ALTER DATABASE <yourDatabase>  
SET QUERY_STORE CLEAR;  
```  
  
> [!TIP] 
> 建议安装最新版本的 [Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) 并且经常更新。  

> [!IMPORTANT] 
> 确保为数据库和工作负载正确配置查询存储。 有关详细信息，请参阅[查询存储最佳做法](../../relational-databases/performance/best-practice-with-the-query-store.md)。 
  
跟踪基数估计过程的另一种方法是使用名为 query_optimizer_estimate_cardinality 的扩展事件  。 以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码示例在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上运行。 它将 .xel 文件写入 `C:\Temp\`（尽管可以更改路径）。 在 [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] 中打开此 .xel 文件时，其详细信息将以用户友好的方式显示。  
  
```sql  
DROP EVENT SESSION Test_the_CE_qoec_1 ON SERVER;  
go  
  
CREATE EVENT SESSION Test_the_CE_qoec_1  
ON SERVER  
ADD EVENT sqlserver.query_optimizer_estimate_cardinality  
    (  
        ACTION (sqlserver.sql_text)  
            WHERE (  
                sql_text LIKE '%yourTable%'  
                and sql_text LIKE '%SUM(%'  
            )  
    )  
ADD TARGET package0.asynchronous_file_target   
        (SET  
            filename = 'c:\temp\xe_qoec_1.xel',  
            metadatafile = 'c:\temp\xe_qoec_1.xem'  
        );  
GO  
  
ALTER EVENT SESSION Test_the_CE_qoec_1  
ON SERVER  
STATE = START;  --STOP;  
GO  
```  
  
有关为 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 定制的扩展事件的信息，请参阅 [SQL 数据库中的扩展事件](/azure/azure-sql/database/xevent-db-diff-from-svr)。  
  
## <a name="steps-to-assess-the-ce-version"></a>评估 CE 版本的步骤  
  
以下步骤用于评估当使用最新 CE 时最重要的查询的执行是否不佳。 其中一些步骤通过运行上一节中提供的代码示例来执行。  
  
1.  打开 [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]。 确保将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库设为最高可用兼容性级别。  
  
2.  执行以下初始步骤：  
  
    1.  打开 [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]。  
  
    2.  运行 T-SQL，确保将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库设为最高可用兼容性级别。  
  
    3.  确保数据库已关闭其 `LEGACY_CARDINALITY_ESTIMATION` 配置。  
  
    4.  清除查询存储。 确保查询存储已打开。  
  
    5.  运行语句：`SET NOCOUNT OFF;`  
  
3.  运行语句：`SET STATISTICS XML ON;`  
  
4.  运行重要的查询。  
  
5.  在结果窗格的“消息”  选项卡上，记下实际受影响的行数。  
  
6.  在结果窗格的“结果”  选项卡上，双击包含 XML 格式的统计信息的单元格。 将显示图形查询计划。  
  
7.  在图形查询计划的第一个框中右键单击，然后单击“属性”  。  
  
8.  针对后面的与不同配置的比较，请记下以下属性的值：  
  
    -   **CardinalityEstimationModelVersion**。  
  
    -   **估计的行数**。  
  
    -   **估计的 I/O 成本**，以及一些涉及实际性能而不是行数预测的类似的估计  属性。  
  
    -   **逻辑操作** 和 **物理操作**。 “并行”是一个不错的选择。  
  
    -   **实际执行模式**。 “批处理”是一个不错的选择，优于“行”  。  
  
9. 将估计的行数与实际行数进行比较。 CE 的不准确率偏高或偏低 1% 还是 10%？  
  
10. 运行：`SET STATISTICS XML OFF;`  
  
11. 运行 T-SQL 以便将数据库的兼容性级别降低一个级别（如从 130 到 120）。  
  
12. 重新运行所有非初始步骤。  
  
13. 比较这两次运行的 CE 属性值。  
  
    - 最新 CE 下的不准确率是不是小于较旧 CE 下的不准确率？  
  
14. 最后，比较这两次运行中的各个性能属性值。  
  
    -   在这两个不同的 CE 估计下你的查询是否使用了不同的计划？  
  
    -   在最新 CE 下你的查询是否运行较缓慢？  
  
    -   除非查询在较旧 CE 下运行地更好，并且使用不同的计划，否则几乎可以肯定地想要最新的 CE。  
  
    -   但是，如果在较旧的 CE 下查询使用较快的计划运行，则将考虑强制系统使用较快计划而忽略 CE。 这种方式可以让你在任何情况下拥有最新 CE，同时在一种独特的情况下保持使用较快计划。  
  
## <a name="how-to-activate-the-best-query-plan"></a>如何激活最佳查询计划  
  
假设在使用 CE 120 的或更高版本情况下，针对查询生成了效率较低的查询计划。 以下是一些可以用来激活更好计划的方法：  
  
1. 可以将整个数据库的兼容性级别设置为低于最新级别的值。  
  
   - 例如，将兼容性级别设置为 110 或更低会激活 CE 70，但这会使所有查询都受制于以前的 CE 模型。  
  
   - 此外，设置较低的兼容性级别也会遗漏最新版本中查询优化器的大量改进。  
  
2. 可以使用 `LEGACY_CARDINALITY_ESTIMATION` 数据库选项使整个数据库使用较旧 CE，同时保留查询优化器中的其他改进。   

3. 可以使用 `LEGACY_CARDINALITY_ESTIMATION` 查询提示，让单个查询使用较旧 CE，同时保留查询优化器中的其他改进。  
  
为了实现更好的控制，可以强制系统在测试期间使用通过 CE 70 生成的计划  。 固定首选计划后，可以将整个数据库设置为使用最新兼容性级别和 CE。 该方法将在后面详细说明。  
  
### <a name="how-to-force-a-particular-query-plan"></a>如何强制使用特定的查询计划  
  
查询存储提供了不同方式来强制系统使用特定的查询计划：  
  
- 执行 **sp_query_store_force_plan**。  
  
- 在 [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] 中，展开“查询存储”节点，右键单击“资源使用排名靠前的节点”，然后单击“查看资源使用排名靠前的节点”    。 此时将显示“强制使用计划”和“取消强制使用计划”按钮。  
  
有关查询存储的详细信息，请参阅 [《Monitoring Performance By Using the Query Store》](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)（使用查询存储监控性能）。  
  
## <a name="examples-of-ce-improvements"></a>CE 改进示例  
  
本节介绍了从最新版本的 CE 中实施的改进中获益的示例查询。 这是背景信息，不需要你的具体操作。  
  
### <a name="example-a-ce-understands-maximum-value-might-be-higher-than-when-statistics-were-last-gathered"></a>示例 A. CE 认为最大值可能大于最近收集统计信息时的值  
  
当 `OrderAddedDate` 的最大值为 `2016-04-30` 时，假定上次在 `2016-04-30` 收集 `OrderTable` 的统计信息。 CE 120（和更高版本）认为数据按升序排序的 `OrderTable` 中的列的值可能大于由统计信息记录的最大值  。 这种假设改进了 [!INCLUDE[tsql](../../includes/tsql-md.md)] SELECT 语句的查询计划，如下所示。  
  
```sql  
SELECT CustomerId, OrderAddedDate  
FROM OrderTable  
WHERE OrderAddedDate >= '2016-05-01';  
```  
  
### <a name="example-b-ce-understands-that-filtered-predicates-on-the-same-table-are-often-correlated"></a>示例 B. CE 认为同一个表的筛选谓词通常是相关的  
  
在下面的 SELECT 语句中我们看到 `Model` 和 `ModelVariant` 的筛选谓词。 我们直观地了解到，当 `Model` 是“Xbox”时，`ModelVariant` 有可能是“One”，因为 Xbox 有一个名为 One 的变体。  
  
从 CE 120 开始，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 认为同一表中 `Model` 和 `ModelVariant` 两个列之间存在相关性。 CE 对于查询将返回多少行进行更准确的估计，并且[查询优化器](../../relational-databases/query-processing-architecture-guide.md#optimizing-select-statements)将生成更优的计划。  
  
```sql  
SELECT Model, Purchase_Price  
FROM dbo.Hardware  
WHERE Model = 'Xbox' AND  
      ModelVariant = 'One';  
```  
  
### <a name="example-c-ce-no-longer-assumes-any-correlation-between-filtered-predicates-from-different-tables"></a>示例 C. CE 不再假设不同表的筛选谓词之间存在任何相关性 
对现代工作负荷和实际业务数据的延伸性的新研究表明，从不同表中筛选的谓词通常没有相互关联性。 在下面的查询中，CE 假设 `s.type` 与 `r.date` 之间没有相关性。 因此，CE 对于返回的行数有一个偏低的估计值。  
  
```sql  
SELECT s.ticket, s.customer, r.store  
FROM dbo.Sales AS s  
CROSS JOIN dbo.Returns AS r  
WHERE s.ticket = r.ticket AND  
      s.type = 'toy' AND  
      r.date = '2016-05-11';  
```  
  
## <a name="see-also"></a>另请参阅  
 [监视和优化性能](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [使用 SQL Server 2014 基数估算器优化查询计划](/previous-versions/dn673537(v=msdn.10))  
 [查询提示](../../t-sql/queries/hints-transact-sql-query.md)     
 [USE HINT 查询提示](../../t-sql/queries/hints-transact-sql-query.md#use_hint)       
 [使用查询优化助手升级数据库](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)           
 [相关视图、函数和过程](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)    
 [查询处理体系结构指南](../../relational-databases/query-processing-architecture-guide.md)