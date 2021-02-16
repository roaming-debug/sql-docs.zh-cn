---
description: 迁移后验证和优化指南
title: 迁移后验证和优化指南 | Microsoft Docs
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- post-migration validation and optimization
- guide, post-migration validation and optimization
ms.assetid: 11f8017e-5bc3-4bab-8060-c16282cfbac1
author: pelopes
ms.author: harinid
ms.openlocfilehash: 397e5b7acd88f8e10dea05ce3ab2dc2f7dd105c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350651"
---
# <a name="post-migration-validation-and-optimization-guide"></a>迁移后验证和优化指南

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 迁移后步骤对于协调任何数据准确性和完整性至关重要，同时还能发现与工作负载相关的性能问题。

## <a name="common-performance-scenarios"></a>常见性能方案

以下是迁移到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 平台后会遇到的一些常见性能方案及其应对方法。 其中包括从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 迁移到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]（从较低版本迁移到较高版本），以及从外部平台（如 Oracle、DB2、MySQL 和 Sybase）迁移到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的方案。

## <a name="query-regressions-due-to-change-in-ce-version"></a><a name="CEUpgrade"></a>由于 CE 版本变更导致的查询回归

适用于：从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 迁移到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。

从较低版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 迁移到 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 或更高版本，且将[数据库兼容性级别](../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)升级到最新可用级别时，工作负载可能会面临性能回归风险。

这是因为，自 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 起，所有查询优化器更改都会绑定到最新的[数据库兼容性级别](../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)，因此计划不会在升级后立即更改，而是在用户将 `COMPATIBILITY_LEVEL` 数据库更改为最新版本后更改。 利用此功能和 Query Store，你可以在升级过程中对查询性能进行精确的控制。 

若要详细了解 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 中引入的查询优化器变更，请参阅[使用 SQL Server 2014 基数估算器优化查询计划](/previous-versions/dn673537(v=msdn.10))。

### <a name="steps-to-resolve"></a>解决步骤

将[数据库兼容性级别](../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)更改为源版本，并遵循下图中推荐的升级工作流：

![显示所建议的升级工作流的示意图。](../relational-databases/performance/media/query-store-usage-5.png "query-store-usage-5")  

有关本主题的详细信息，请参阅[在升级到更高版本 SQL Server 的过程中保持性能稳定性](../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade)。

## <a name="sensitivity-to-parameter-sniffing"></a><a name="ParameterSniffing"></a>对参数截取的敏感性

**适用于：** 从外部平台（如 Oracle、DB2、MySQL 和 Sybase）到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的迁移。

> [!NOTE]
> 对于从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的迁移，如果 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 源中已存在此问题，则按原样迁移到较新版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将无法应对此方案。 

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 可针对存储过程编译查询计划，方法是在首次编译时截取输入参数，生成参数化且可重复使用的计划，然后对该输入数据分发进行优化。 即使未使用存储过程，生成普通计划的大多数据语句仍将被参数化。 缓存计划后，任何后期执行都会映射到之前缓存的计划中。
如果首次编译未使用适用于常用工作负载的最常用参数集，可能会出现潜在问题。 对不同的参数使用相同的执行计划可能导致效率低下。 有关本主题的详细信息，请参阅[参数探查](../relational-databases/query-processing-architecture-guide.md#ParamSniffing)。

### <a name="steps-to-resolve"></a>解决步骤

1.  使用 `RECOMPILE` 提示。 每次基于一个参数值对计划进行调整时，都会对计划进行计算。
2.  重写存储过程以使用 `(OPTIMIZE FOR(<input parameter> = <value>))` 选项。 确定并使用最适合相关工作负载的值，同时创建并维护可基于参数化值变得更高效的计划。
3.  使用过程中的本地变量重写存储过程。 现在，优化器可使用密度向量进行预估，无论使用什么参数值都将生成相同计划。
4.  重写存储过程以使用 `(OPTIMIZE FOR UNKNOWN)` 选项。 与使用本地变量方法的效果相同。
5.  使用 `DISABLE_PARAMETER_SNIFFING` 提示重写查询。 完全禁用参数截取与使用本地变量方法的效果相同（除非使用 `OPTION(RECOMPILE)`、`WITH RECOMPILE` 或 `OPTIMIZE FOR <value>`）。

> [!TIP] 
> 利用 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 计划分析功能快速识别这是否是一个问题。 请访问[此处](/archive/blogs/sql_server_team/new-in-ssms-query-performance-troubleshooting-made-easier)了解更多可用信息。

## <a name="missing-indexes"></a><a name="MissingIndexes"></a>缺失索引

**适用于：** 从外部平台（如 Oracle、DB2、MySQL 和 Sybase）和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的迁移。

不正确的索引或缺失索引会导致额外的 I/O，从而产生额外内存并浪费 CPU。 原因可能是工作负载配置文件已更改（例如使用了其他谓词），进而导致现有索引设计无效。 索引策略不佳或工作负载配置文件发生更改的证据包括：
-   查找重复、冗余、很少使用及完全未使用过的索引。
-   特别注意有更新但未使用过的索引。

### <a name="steps-to-resolve"></a>解决步骤

1.  对任何缺失索引的引用使用图形执行计划。
2.  [数据库引擎优化顾问](../tools/dta/tutorial-database-engine-tuning-advisor.md)生成的索引建议。
3.  利用[缺失索引 DMV](../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)或通过 [SQL Server 性能仪表板](https://www.microsoft.com/download/details.aspx?id=29063)。
4.  利用可使用现有 DMV 的预先存在的脚本深入了解任何缺失、重复、冗余、较少使用和完全未使用过的索引，还可以了解数据库中是否有可以编写提示/硬编码为现有过程或函数的任何索引引用。 

> [!TIP] 
> 此类预先存在的脚本示例包括 [Index Creation](https://github.com/Microsoft/tigertoolbox/tree/master/Index-Creation) 和 [Index Information](https://github.com/Microsoft/tigertoolbox/tree/master/Index-Information)。 

## <a name="inability-to-use-predicates-to-filter-data"></a><a name="InabilityPredicates"></a>无法使用谓词筛选数据

**适用于：** 从外部平台（如 Oracle、DB2、MySQL 和 Sybase）和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的迁移。

> [!NOTE]
> 对于从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的迁移，如果 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 源中已存在此问题，则按原样迁移到较新版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将无法应对此方案。

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 查询优化器仅适用于编译时已知的信息。 如果工作负荷依赖于仅可在执行时已知的谓词，则选择不适合的计划的可能性会增加。 若要获得质量更佳的计划，谓词必须是 SARGable 或 Search Argumentable。

非 SARGable 谓词的一些示例：
-   隐式数据转换，例如从 VARCHAR 转换到 NVARCHAR，或从 INT 转换到 NVARCHAR。 查找实际执行计划中的运行时 CONVERT_IMPLICIT 警告。 从一种类型转换到另一种类型还会导致精度损失。
-   不确定性的复杂表达式，如 `WHERE UnitPrice + 1 < 3.975`，而不是 `WHERE UnitPrice < 320 * 200 * 32`。
-   使用函数的表达式，如 `WHERE ABS(ProductID) = 771` 或 `WHERE UPPER(LastName) = 'Smith'`
-   以通配符开头的字符串，如 `WHERE LastName LIKE '%Smith'`，而不是 `WHERE LastName LIKE 'Smith%'`。

### <a name="steps-to-resolve"></a>解决步骤

1. 始终将变量/参数声明为所需目标[数据类型](../t-sql/data-types/data-types-transact-sql.md)。 
  -   这可能涉及将数据库中存储的任何用户定义的代码构造（如存储过程、用户定义函数或视图）与托管基础表中使用的数据类型信息的系统表（例如 [sys.columns](../relational-databases/system-catalog-views/sys-columns-transact-sql.md)）进行比较。
2. 如果无法将所有代码遍历到前一个点，则出于相同的目的，请更改表中的数据类型，使其匹配任何变量/参数声明。
3. 了解以下结构的用途：

  -   用作谓词的函数；
  -   通配符搜索；
  -   基于列数据的复杂表达式 - 评估是否需要改为创建可以索引的持久化计算列；

> [!NOTE] 
> 可以编程方式完成上述所有操作。

## <a name="use-of-table-valued-functions-multi-statement-vs-inline"></a><a name="TableValuedFunctions"></a>使用表值函数（多语句表值函数和内联表值函数）

**适用于：** 从外部平台（如 Oracle、DB2、MySQL 和 Sybase）和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的迁移。

> [!NOTE]
> 对于从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的迁移，如果 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 源中已存在此问题，则按原样迁移到较新版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将无法应对此方案。

表值函数返回可作为视图替代项的表数据类型。 视图仅限为单个 `SELECT` 语句，而用户定义函数可包含更多语句，与视图相比，这些语句支持更多逻辑。

> [!IMPORTANT] 
> 由于 MSTVF（多语句表值函数）不在编译时创建，因此 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 查询优化器需依赖于试探方法（而非实际统计信息）来确定行预估数。 即使向基表中添加索引也不会有任何帮助。 对于 MSTVF，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 对于MSTVF 预期返回的行数使用固定预估值 1（从 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] 开始，该固定预估值为 100 行）。

### <a name="steps-to-resolve"></a>解决步骤

1.  如果多语句 TVF 仅为单个语句，请将其转换为内联 TVF。

    ```sql
    CREATE FUNCTION dbo.tfnGetRecentAddress(@ID int)
    RETURNS @tblAddress TABLE
    ([Address] VARCHAR(60) NOT NULL)
    AS
    BEGIN
      INSERT INTO @tblAddress ([Address])
      SELECT TOP 1 [AddressLine1]
      FROM [Person].[Address]
      WHERE  AddressID = @ID
      ORDER BY [ModifiedDate] DESC
    RETURN
    END
    ```

    接下来显示内联格式示例。

    ```sql
    CREATE FUNCTION dbo.tfnGetRecentAddress_inline(@ID int)
    RETURNS TABLE
    AS
    RETURN (
      SELECT TOP 1 [AddressLine1] AS [Address]
      FROM [Person].[Address]
      WHERE  AddressID = @ID
      ORDER BY [ModifiedDate] DESC
    )
    ```

2.  如果问题更复杂，请考虑使用内存优化表或临时表中存储的中间结果。

##  <a name="additional-reading"></a><a name="Additional_Reading"></a> 其他阅读主题

 [Query Store 最佳实践](../relational-databases/performance/best-practice-with-the-query-store.md)  
[Memory-Optimized Tables](./in-memory-oltp/sample-database-for-in-memory-oltp.md)  
[用户定义函数](../relational-databases/user-defined-functions/user-defined-functions.md)  
[表变量和行预估 - 第 1 部分](/archive/blogs/blogdoezequiel/table-variables-and-row-estimations-part-1)  
[表变量和行预估 - 第 2 部分](/archive/blogs/blogdoezequiel/table-variables-and-row-estimations-part-2)  
[执行计划的缓存和重新使用](../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse)