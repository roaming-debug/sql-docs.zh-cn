---
description: 系统版本控制临时表与内存优化表
title: 系统版本控制临时表与内存优化表 | Microsoft Docs
ms.custom: ''
ms.date: 07/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 23274522-e5cf-4095-bed8-bf986d6342e0
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 05ed5ec0d34d2a1a612b76c0117d0bf66675ad7a
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171889"
---
# <a name="system-versioned-temporal-tables-with-memory-optimized-tables"></a>系统版本控制临时表与内存优化表


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


用于 [内存优化表](../in-memory-oltp/sample-database-for-in-memory-oltp.md) 的系统版本控制临时表旨在为以下情形提供经济高效的解决方案：需要基于使用内存中 OLTP 工作负荷收集的数据进行 [数据审核和时间点分析](./temporal-table-usage-scenarios.md)。 它们不仅提供高事务吞吐量和无锁并发，还能存储大量可轻松查询的历史记录数据。

## <a name="overview"></a>概述

系统版本控制临时表自动保留完整的数据更改历史记录，并公开了一些实用的 Transact-SQL 扩展以用于时间点分析。 在典型方案中，即使是不定期查询的数据历史记录，也会保留很长时间（数月甚至数年）。

可以在不同的环境中要求进行数据审核和基于时间的分析，尤其是在处理极其大量的请求并且使用内存中 OLTP 技术的 OLTP 系统中。 但是，在临时方案中使用内存优化表极具挑战性，因为所生成的大量历史数据通常会超出可用 RAM 内存的限制。 与此同时，使用 RAM 来存储因变旧而较少访问的只读历史数据并不是最佳的解决方案。

用于内存优化表的系统版本控制时态表不仅提供高事务吞吐量和无锁并发，还能使用内存中表来存储当前数据（时态表）以及使用基于磁盘的表来存储历史数据，从而存储大量历史记录数据。 通过使用自动生成的内部内存优化临时表来存储最新历史记录并支持从本机编译的代码执行 DML，可以最大程度地降低对 DML 操作的影响。

下图对这种体系结构进行了说明。![临时内存中体系结构](../../relational-databases/tables/media/temporal-in-memory-architecture.png "临时内存中体系结构")

## <a name="implementation-details"></a>实现详细信息

在创建由系统控制版本的内存优化表时，需要注意有关由系统控制版本的时态表和内存优化表的以下事实。 有关语法选项和示例，请参阅 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)。

- 只有持久内存优化表才能进行系统版本控制 (**DURABILITY = SCHEMA_AND_DATA**)。
- 无论创建者是最终用户还是系统，用于内存优化系统版本控制表的历史记录表都必须基于磁盘。
- 仅影响当前表（内存中）的查询可用于 [本机编译的 T-SQL 模块](../in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md)。 本机编译的模块中不支持使用 FOR SYSTEM TIME 子句的临时查询。 支持在即席查询和非本机模块中将 FOR SYSTEM TIME 子句与内存优化表一起使用。
- 当 **SYSTEM_VERSIONING = ON** 时，系统将自动创建内部内存优化临时表，以接受对内存优化当前表执行更新和删除操作时所产生的最新系统版本控制更改。
- 内部内存优化临时表中的数据将通过异步数据刷新任务定期移动到基于磁盘的历史记录表。 此数据刷新机制的目标之一是将内部内存缓冲区中父对象的内存消耗控制在 10% 以下。 你可以通过查询 [sys.dm_db_xtp_memory_consumers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-memory-consumers-transact-sql.md) 并汇总内部内存优化临时表和当前临时表的数据，来跟踪内存优化系统版本控制临时表的内存总消耗。
- 可通过调用 [sp_xtp_flush_temporal_history](../../relational-databases/system-stored-procedures/temporal-table-sp-xtp-flush-temporal-history.md)强制进行数据刷新。
- 当 **SYSTEM_VERSIONING = OFF** 时，或者当通过添加、删除或更改列来修改系统版本控制表的架构时，内部临时缓冲区的全部内容将移动到基于磁盘的历史记录表中。
- 历史数据查询在快照隔离级别下非常有效，并且始终返回内存中临时缓冲区与基于磁盘的表之间的并集（无重复项）。
- 从内部更改表架构的 **ALTER TABLE** 操作必须执行数据刷新，这可能会延长该操作的持续时间。

## <a name="the-internal-memory-optimized-staging-table"></a>内部内存优化临时表

内部内存优化临时表是一个由系统创建用来优化 DML 操作的内部对象。

- 采用以下格式生成表名：生成的表名采用以下格式：Memory_Optimized_History_Table_<object_id>，其中 <object_id> 是当前时态表的标识符。
- 该表在当前临时表的架构上另加一个 BIGINT 列。 此附加列保证了移动到内部历史记录缓冲区的行的唯一性。
- 此附加列采用以下名称格式：Change_ID[_< suffix>]，其中在表已有 Change_ID 列的情况下可选择性地添加 _\<suffix> 。
- 系统版本控制的内存优化表的最大行大小因临时表中的附加 BIGINT 列而减去 8 个字节。 新的最大值现在为 8052 字节。
- 内部内存优化临时表不显示在 SQL Server Management Studio 的对象资源管理器中。
- 你可以在 [sys.internal_tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md) 中找到此表的相关元数据及其与当前临时表的关联。

## <a name="the-data-flush-task"></a>数据刷新任务

数据刷新是一项定期激活的任务，用于检查是否有任何内存优化表满足数据移动的基于内存大小的条件。 当内部临时表的内存消耗达到当前临时表的内存消耗的 8% 时，将启动数据移动。

数据刷新任务定期激活，其计划因现有工作负荷而异。 工作负荷较重时，将以每 5 秒一次的频率频繁刷新，而工作负荷较轻时，刷新频率将减少到每 1 分钟一次。 每个需要清理的内部内存优化临时表将生成一个线程。

数据刷新会从内存中内部缓冲区删除早于当前运行的最早事务的所有记录，以便将这些记录移动到基于磁盘的历史记录表。

可通过调用 [sp_xtp_flush_temporal_history](../../relational-databases/system-stored-procedures/temporal-table-sp-xtp-flush-temporal-history.md) 并指定架构和表名来强制进行数据刷新：sys.sp_xtp_flush_temporal_history \@schema_name、\@object_name。 利用这个由用户执行的命令，将像在系统按内部计划调用数据刷新任务时一样调用相同的数据移动进程。

## <a name="see-also"></a>另请参阅

- [临时表](../../relational-databases/tables/temporal-tables.md)
- [系统版本控制临时表入门](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [临时表使用方案](../../relational-databases/tables/temporal-table-usage-scenarios.md)
- [临时表系统一致性检查](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [临时表分区](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [临时表注意事项和限制](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [临时表安全性](../../relational-databases/tables/temporal-table-security.md)
- [管理版本由系统控制的临时表中历史数据的保留期](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [临时表元数据视图和函数](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
