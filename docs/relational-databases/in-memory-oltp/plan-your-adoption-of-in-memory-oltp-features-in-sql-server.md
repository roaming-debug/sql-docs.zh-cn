---
title: '内存中 OLTP 的计划采用 '
description: 了解 SQL Server 中的内存中 OLTP 功能的采用如何影响企业系统的其他方面。
ms.custom: seo-dt-2019
ms.date: 01/28/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 041b428f-781d-4628-9f34-4d697894e61e
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 078dc8fd820e5b93830964933c2641e677134f4a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485249"
---
# <a name="plan-your-adoption-of-in-memory-oltp-features-in-sql-server"></a>在 SQL Server 中计划内存中 OLTP 功能的应用
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


本文章介绍了内存中功能的采用如何影响企业系统的其他方面。



## <a name="a-adoption-of-in-memory-oltp-features"></a>A. 内存中 OLTP 功能的采用


以下小节介绍在采用和实现计划内存中功能时必须考虑的因素。 若要了解详细的说明性信息，请访问：

- [使用内存中 OLTP 改善 Azure SQL 数据库中的应用程序性能](/azure/azure-sql/in-memory-oltp-configure)



### <a name="a1-prerequisites"></a>A.1 先决条件

使用内存中功能的一个先决条件涉及 SQL 产品的版本或服务层。 有关此先决条件和其他先决条件的信息，请参阅：

- [使用内存优化表的要求](../../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)
    - [SQL Server 2016 的版本和组件](../../sql-server/editions-and-components-of-sql-server-2016.md)
    - [SQL 数据库定价层建议](/azure/azure-sql/database/service-tiers-vcore)


### <a name="a2-forecast-the-amount-of-active-memory"></a>A.2 预测活动内存量

系统是否有足够的活动内存来支持新的内存优化表？

#### <a name="microsoft-sql-server"></a>Microsoft SQL Server

包含 200 GB 数据的内存优化表需要超过 200 GB 的活动内存专用于支持。 实现包含大量数据的内存优化表之前，必须预测可能需要添加到服务器计算机的其他活动内存量。 有关估算指导，请参阅：

- [估算内存优化表的内存需求](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md)

#### <a name="azure-sql-database"></a>Azure SQL Database

对于 Azure SQL 数据库云服务中托管的数据库，所选的服务层会影响允许数据库使用的活动内存量。 应计划使用警报监视数据库的内存使用。 有关详细信息，请参阅：

- 查看[定价层](/azure/sql-database/sql-database-purchase-models)的内存中 OLTP 存储限制
- [监视内存中 OLTP 存储](/azure/azure-sql/in-memory-oltp-monitor-space)

#### <a name="memory-optimized-table-variables"></a>内存优化表变量

声明为内存优化的表变量有时要优于驻留在 **tempdb** 数据库中的传统 #TempTable。 此类表变量可显著提高性能，而无需使用大量活动内存。

### <a name="a3-table-must-be-offline-to-convert-to-memory-optimized"></a>A.3 表必须处于脱机状态以转换为内存优化

某些 ALTER TABLE 功能可用于内存优化表。 但无法发出 ALTER TABLE 语句将基于磁盘的表转换为内存优化表。 相反，必须使用一组手动程度更高的步骤。 下面是可将基于磁盘的表转换为内存优化表的各种方法。

#### <a name="manual-scripting"></a>手动编写脚本

将基于磁盘的表转换为内存优化表的一种方法是自己编写 Transact-SQL 步骤。


1. 挂起应用程序活动。

2. 执行完整备份。

3. 对基于磁盘的表进行重命名。

4. 发出 CREATE TABLE 语句以创建新的内存优化表。

5. 从基于磁盘的表使用嵌套 SELECT 插入 (INSERT INTO) 内存优化表。

6. 删除 (DROP) 基于磁盘的表。

7. 执行另一个完整备份。

8. 继续应用程序活动。


#### <a name="memory-optimization-advisor"></a>内存优化顾问

内存优化顾问工具可生成脚本来帮助实现将基于磁盘的表转换为内存优化表。 作为 SQL Server Data Tools (SSDT) 的一部分，对此工具进行安装。

- [内存优化顾问](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md)
- [下载 SQL Server Data Tools (SSDT)](../../ssdt/download-sql-server-data-tools-ssdt.md)


#### <a name="dacpac-file"></a>.dacpac 文件

可通过使用.dacpac 文件就地更新由 SSDT 管理的数据库。 在 SSDT 中，可指定在 .dacpac 文件中编码的架构进行的更改。

在类型为数据库的 Visual Studio 项目的上下文中，处理 .dacpac 文件。

- [数据层应用程序](../../relational-databases/data-tier-applications/data-tier-applications.md)和 .dacpac 文件



### <a name="a4-guidance-for-whether-in-memory-oltp-features-are-right-for-your-application"></a>A.4 有关内存中 OLTP 功能是否适合应用程序的指南

有关内存中 OLTP 功能是否可以改善特定应用程序的性能的指南，请参阅：

- [内存中 OLTP（内存中优化）](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)



## <a name="b-unsupported-features"></a>B. 不支持的功能

有关在某些内存中 OLTP 方案中不支持的功能，请参阅：

- [内存中 OLTP 不支持的 SQL Server 功能](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md)


以下各小节突出显示了一些更重要的不受支持的功能。


### <a name="b1-snapshot-of-a-database"></a>B.1 数据库的快照

第一次在给定数据库中创建任何内存优化表或模块之后，千万不要拍摄数据库 [快照](../../relational-databases/databases/database-snapshots-sql-server.md) 。 具体原因有以下几点：

- 第一个内存优化项使我们无法从内存优化的文件组删除最后一个文件；以及
- 任何在内存优化的文件组中具有文件的数据库均不支持快照。

通常情况下快照可以非常方便地快速测试迭代。


### <a name="b2-cross-database-queries"></a>B.2 跨数据库查询

内存优化表不支持 [跨数据库](../../relational-databases/in-memory-oltp/cross-database-queries.md) 事务。 不能从也访问某一内存优化表的相同事务或相同查询访问其他数据库。

表变量不是事务性的。 因此，可将 [内存优化表变量](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md) 用于跨数据库查询。


### <a name="b3-readpast-table-hint"></a>B.3 READPAST 表提示

任何查询均不可将 READPAST [表提示](../../t-sql/queries/hints-transact-sql-table.md) 应用于任何内存优化表。

在多个会话正在逐个访问和修改一小组相同的行时（例如，在处理队列时），READPAST 提示可提供帮助。


### <a name="b4-rowversion-sequence"></a>B.4 RowVersion、SEQUENCE

- 不可将内存优化表上的任何列标记为 [RowVersion](../../t-sql/data-types/rowversion-transact-sql.md) 。


- 不能将 [SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md) 与内存优化表中的约束配合使用。 例如，不能使用 NEXT VALUE FOR 子句创建 DEFAULT 约束。 可以将 SEQUENCE 与 INSERT 和 UPDATE 语句配合使用。


## <a name="c-administrative-maintenance"></a>C. 管理维护


本部分介绍使用内存优化表的数据库管理方面的差异。


### <a name="c1-identity-seed-reset-increment--1"></a>C.1 标识种子重置，增量 > 1

为重设种子 IDENTITY 列，不能将[DBCC CHECKIDENT](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)用于内存优化表。

对于内存优化表上的 IDENTITY 列，将增量值限定为 1。


### <a name="c2-dbcc-checkdb-cannot-validate-memory-optimized-tables"></a>C.2 DBCC CHECKDB 无法验证内存优化表

其目标为内存优化表时， [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) 命令不执行任何操作。 以下步骤可解决此问题：


1. [备份事务日志](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)。

2. 将内存优化文件组 (FILEGROUP)中的文件备份到空设备中。 备份过程将调用校验和验证。

    如果发现损坏，继续执行后续步骤。

3. 将数据从内存优化表复制到基于磁盘的表，用于临时存储。

4. 还原内存优化文件组 (FILEGROUP) 的文件。

5. 将临时存储在基于磁盘的表中的数据插入 (INSERT INTO) 内存优化表。

6. 删除 (DROP) 临时存放数据的基于磁盘的表。



## <a name="d-performance"></a>D. 性能

本部分介绍内存优化表在完全利用潜力时达到卓越性能的情况。


### <a name="d1-index-considerations"></a>D.1 索引注意事项

由与表相关的语句 CREATE TABLE 和 ALTER TABLE 创建和管理内存优化表上的所有索引。 无法使用 CREATE INDEX 语句将内存优化表作为目标。

首次实现内存优化表时，传统的 B 树的非聚集索引通常是合理、简单的选择。 此后，看到应用程序的执行方式后，可考虑换用另一种索引类型。

需要在内存优化表的上下文中讨论以下两种特殊类型的索引：哈希索引和列存储索引。

有关内存优化表上索引的概述，请参阅：

- [内存优化表的索引](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)


#### <a name="hash-indexes"></a>哈希索引

在使用“ **=** ”运算符按其确切的主键值访问特定行时，哈希索引是速度最快的格式。

- 如果用于哈希索引，则不精确的运算符（例如，“!=”、“>”或“BETWEEN”）将损害性能  。

- 如果键值重复率变得过高，则哈希索引可能不是最佳选择。

- 请勿低估哈希索引可能需要的 bucket 的数量，以避免在单个 bucket 内出现长链。 有关详细信息，请参阅：
    - [内存优化表的哈希索引](../../relational-databases/in-memory-oltp/hash-indexes-for-memory-optimized-tables.md)


#### <a name="nonclustered-columnstore-indexes"></a>非聚集列存储索引

内存优化表提供高吞吐量的典型业务事务数据，在范例中，调用联机事务处理或 OLTP。 列存储索引提供高吞吐量的聚合和称为 Analytics 的类似处理。 过去几年，可用于满足 OLTP 和 Analytics 需求的最好的方法是提供具有大量数据移动和一定程度数据重复的单独的表。 现可采用更简单的 **混合解决方案** ：提供基于内存优化表的列存储索引。


- 可在基于磁盘的表中生成 [列存储索引](../../relational-databases/indexes/columnstore-indexes-overview.md) ，甚至是聚集索引。 但不能在内存优化表上聚集列存储索引。


- 内存优化表的 LOB 或行外列阻止在表上创建列存储索引。


- 表上存在列存储索引时，不可对内存优化表执行任何 ALTER TABLE 语句。
    - 截至 2016 年 8 月，Microsoft 已有提高重新创建列存储索引的性能的近期计划。



### <a name="d2-lob-and-off-row-columns"></a>D.2 LOB 和行外列

大型对象 (LOB) 是 varchar (**max**) 等类型的列。 内存优化表中有几列 LOB 列并不会严重损害性能。 但请避免 LOB 列的数目超出数据需要。 此建议同样适用于行外列。 如果 varchar(512) 已足够，则不要将某列定义为 nvarchar(3072)。


有关 LOB 和行外列的详细信息，请参阅：

- [内存优化表中的表和行大小](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md)
- [内存中 OLTP 支持的数据类型](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md)



## <a name="e-limitations-of-native-procs"></a>E. 本机过程的限制


本机编译的 T-SQL 模块（包括存储过程）中不支持 Transact-SQL 的特定元素。 有关支持的功能的详细信息，请参阅：

- [本机编译的 T-SQL 模块支持的功能](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)

有关将使用不支持的功能的 Transact-SQL 模块迁移到本机编译的模块时的注意事项，请参阅：

- [本机编译存储过程的迁移问题](./a-guide-to-query-processing-for-memory-optimized-tables.md)

除存在对 Transact-SQL 特定元素的限制外，对本机编译的 T-SQL 模块中支持的查询运算符也存在限制。 由于这些限制，本机编译的存储过程不适用于可处理大型数据集的分析查询。

#### <a name="no-parallel-processing-in-a-native-proc"></a>本机过程中不进行并行处理

并行处理不属于本机过程的任何查询计划。 本机过程始终采用单线程的方式执行。


#### <a name="join-types"></a>联接类型


哈希联接和合并联接都不属于本机过程的任何查询计划。 使用嵌套的循环联接。


#### <a name="no-hash-aggregation"></a>无哈希聚合

本机过程的查询计划需要聚合阶段时，仅可用流聚合。 本机过程的查询计划不支持哈希聚合。

- 必须聚合很多行的数据时，哈希聚合是更好的选择。



## <a name="f-application-design-transactions-and-retry-logic"></a>F. 应用程序设计：事务和重试逻辑

涉及内存优化表的事务可能会依赖于涉及同一个表的另一个事务。 如果依赖事务的数量超过允许的最大值，则所有依赖事务都失败。

SQL Server 2016 中：

- 允许的最大值是 8 个依赖事务。 8 也是最大的任何给定事务可以依赖的事务的极限数量。
- 错误号为 41839。 （SQL Server 2014 中的错误号是 41301。）


通过将重试逻辑添加到脚本中，可使 Transact-SQL 脚本更可靠（针对可能的事务错误）。 频繁调用 UPDATE 和 DELETE 时，或者另一个表的外键引用内存优化表时，重试逻辑很有可能会有帮助。 有关详细信息，请参阅：

- [具有内存优化表的事务](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)
- [内存优化表的事务依赖限制 - 错误 41839](/archive/blogs/sqlcat/transaction-dependency-limits-with-memory-optimized-tables-error-41839)



## <a name="related-links"></a>相关链接

- [内存中 OLTP（内存中优化）](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)