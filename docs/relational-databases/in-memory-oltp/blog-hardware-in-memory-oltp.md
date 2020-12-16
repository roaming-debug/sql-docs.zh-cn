---
title: SQL 内存中 OLTP 的硬件 | Microsoft Docs
description: 了解 SQL Server 中的内存中 OLTP 性能的硬件注意事项。 内存中 OLTP 使用内存和磁盘的方式与基于磁盘的表不同。
ms.custom: ''
ms.date: 03/28/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 09483122764de51b565693d85c3ae8efa9a8c570
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465348"
---
# <a name="hardware-considerations-for-in-memory-oltp-in-sql-server"></a>SQL Server 中的内存中 OLTP 的硬件注意事项

内存中 OLTP 使用内存和磁盘的方式与传统基于磁盘的表不同。 你将看到的内存中 OLTP 的性能改进取决于你所使用的硬件。 在这篇博客文章中我们讨论了一些常规硬件注意事项，并提供了用于内存中 OLTP 的硬件的一般准则。

> [!NOTE]
> 本文是由 Microsoft SQL Server 2014 团队于 2013 年 8 月 1 日发布的博客文章。 博客网页即将停用。
>
> [SQL Server 内存中 OLTP](./in-memory-oltp-in-memory-optimization.md)

<!--
    Here was the link to the blog. This blog was captured into this new article on 2018/11/30, by GeneMi (MightyPen).
    https://cloudblogs.microsoft.com/sqlserver/2013/08/01/hardware-considerations-for-in-memory-oltp-in-sql-server-2014/
    At least one pre-existing article that contained the obsolete blog link was:
        relational-databases\in-memory-oltp\sample-database-for-in-memory-oltp.md
-->

## <a name="cpu"></a>CPU

内存中 OLTP 不需要高端服务器来支持高吞吐量的 OLTP 工作负载。 建议使用带有 2 个 CPU 插槽的中型服务器。 由于内存中 OLTP 提高了吞吐量，因此 2 个插槽可能足以满足你的业务需求。

建议使用内存中 OLTP 打开超线程。 对于一些 OLTP 工作负载，我们在使用超线程时可以看到性能提升高达 40%。

## <a name="memory"></a>内存

所有内存优化表都完全驻留在内存中。 因此，必须具有足够的物理内存用于表本身并维持针对数据库运行的工作负载 - 所需的内存大小实际上取决于工作负载，但作为起点，可能需要足够的可用内存，大约为数据大小的 2 倍。 如果工作负载也在传统的基于磁盘的表上运行，则还需要足够的内存用于缓冲池。

若要确定给定内存优化表使用的内存量，请运行以下查询：

```sql
select object_name(object_id), * from sys.dm_db_xtp_table_memory_stats;
```

结果将显示用于内存优化表及其索引的内存。 表数据包括用户数据，以及运行事务仍需要或尚未被系统清理的所有旧行版本。 哈希索引使用的内存是常量，并且不依赖于表中的行数。

请记住，使用内存中 OLTP 时整个数据库不需要容纳于内存中。 只要热数据（即内存优化表）的大小不超过 256GB，就可以拥有一个多 TB 的数据库并仍然可以从内存中 OLTP 中受益。 SQL Server 可以为单个数据库管理的最大检查点数据文件数为 4000，每个文件为 128MB。 虽然理论上最大值为 512GB，但为了保证 SQL Server 能够与合并检查点文件保持同步并且不会达到 4000 个文件的限制，我们最多支持 256GB。 请注意，此限制仅适用于内存优化表；在同一个 SQL Server 数据库中的传统基于磁盘的表上没有此大小限制。

非持久内存优化表 (NDT)，即具有 DURABILITY = SCHEMA_ONLY 的内存优化表不会保留在磁盘上。 尽管 NDT 不受检查点文件数量的限制，但仅支持 256GB。 本文其余部分中对日志和数据驱动器的注意事项不适用于非持久表，因为数据仅存在于内存中。

## <a name="log-drive"></a>日志驱动器

与内存优化表相关的日志记录将与其他 SQL Server 日志记录一起写入数据库事务日志。

务必始终将日志文件放在具有低延迟的驱动器上，这样事务不需要等待太长时间，并且可以防止对日志 IO 的争用。 系统与最慢的组件的运行速度相同（Amdahl 定律）。 需要确保在运行内存中 OLTP 时，日志 IO 设备不会成为瓶颈。 建议使用低延迟的存储设备，至少使用 SSD。

请注意，内存优化表使用的日志带宽比基于磁盘的表少，因为它们不记录索引操作，也不记录撤消记录。 这有助于减轻日志 IO 争用。

## <a name="data-drive"></a>数据驱动器

使用检查点文件的内存优化表的持久性使用流式处理的 IO。 因此，这些文件不需要具有低延迟或快速随机 IO 的驱动器。 相反，这些驱动器的主要因素是顺序性的 IO 速度和主机总线适配器 (HBA) 的带宽。 因此，不需要 SSD 用于检查点文件；可以将它们放置在高性能主轴（例如 SAS）上，只要其顺序 IO 速度满足要求即可。

确定速度要求的最大因素是服务器重启时的 RTO [恢复时间目标]。 在数据库恢复期间，需要将内存优化表中的所有数据从磁盘读取到内存。 数据库恢复以 IO 子系统的顺序读取速度发生；磁盘是瓶颈。

为了满足严格的 RTO 要求，建议通过向 MEMORY_OPTIMIZED_DATA 文件组添加多个容器，将检查点文件分布在多个磁盘上。 SQL Server 支持从多个驱动器并行加载检查点文件 - 以驱动器的聚合速度进行恢复。

在磁盘容量方面，建议可用内存优化表大小的 2-3 倍。

## <a name="see-also"></a>另请参阅

[内存中 OLTP 的示例数据库](sample-database-for-in-memory-oltp.md)