---
description: 内存中数据库系统和技术
title: 内存中数据库系统的功能和技术
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- in-memory systems
- in-memory technologies
- in-memory features
- database, in-memory database
- system, in-memory system
- features, in-memory features
- in-memory
ms.assetid: 11f8017e-5bc3-4bab-8060-c16282cfbac1
author: briancarrig
ms.author: brcarrig
ms.openlocfilehash: 6625022360c93da5cbc43224572908738f6d430c
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236447"
---
# <a name="in-memory-database-systems-and-technologies"></a>内存中数据库系统和技术

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

本页面旨在用作 SQL Server 中的内存中功能和技术的参考页面。 内存中数据库系统的概念是指利用新式数据库系统中可用的更大内存容量的数据库系统。 内存中数据库本质上可以是关系数据库或非关系数据库。

通常认为，内存中数据库系统的性能优势主要是由于，其访问驻留在内存中的数据比访问甚至最快的可用磁盘子系统上的数据更快（快几个数量级）。 但是，许多 SQL Server 工作负载都可以将整个工作集放在可用内存中。 许多内存中数据库系统都可以将数据保存到磁盘中，并且可能无法始终将整个数据集放在可用内存中。

关系数据库工作负载主要使用快速易失的高速缓存，该缓存面向相当慢但持久的介质。 它需要特定方法来管理工作负载。 更快的内存传输速率、更大的容量甚至是永久内存所带来的机遇，促进了新特性和技术的开发，这些新特性和新技术可以激发新的关系数据库工作负载管理方法。

## <a name="hybrid-buffer-pool"></a>混合缓冲池

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[混合缓冲池](../database-engine/configure-windows/hybrid-buffer-pool.md)通过 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 为驻留在适用于 Windows 和 Linux 平台的可字节寻址的永久内存存储设备上的数据库文件扩展缓冲池。

## <a name="memory-optimized-tempdb-metadata"></a>内存优化 `tempdb` 元数据

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 引入了一个新功能，即[内存优化 tempdb 元数据](./databases/tempdb-database.md#memory-optimized-tempdb-metadata)，它有效地消除了某些争用瓶颈，并为 tempdb 繁重的工作负载解锁了新级别的可伸缩性。

## <a name="in-memory-oltp"></a>内存中 OLTP

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[内存中 OLTP](./in-memory-oltp/in-memory-oltp-in-memory-optimization.md) 是 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDS](../includes/sssds-md.md)] 中用于优化事务处理、数据引入、数据加载和瞬态数据方案性能的数据库技术。

## <a name="configuring-persistent-memory-support-for-linux"></a>为 Linux 配置永久内存支持

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[sqlv15](../includes/sssql19-md.md)] 介绍了如何使用 `ndctl` 实用工具[永久内存](../linux/sql-server-linux-configure-pmem.md)配置永久内存 (PMEM)。

## <a name="persisted-log-buffer"></a>永久日志缓冲区

[!INCLUDE[ssSQL16](../includes/sssql16-md.md)] Service Pack 1 引入了针对 WRITELOG 等待所约束的写入密集型工作负载的性能优化。 永久内存用于存储日志缓冲区。 该缓冲区很小（每个用户数据库 20 MB），必须刷新到磁盘上才能使写入事务日志的事务得到强化。 对于写入密集型 OLTP 工作负载，此刷新机制可能会成为瓶颈。 在永久内存中使用日志缓冲区，减少了强化日志所需的操作数量，从而缩短了总事务时间并提高了工作负载性能。 此过程称为[日志结尾缓存]( https://blogs.msdn.microsoft.com/bobsql/2016/11/08/how-it-works-it-just-runs-faster-non-volatile-memory-sql-server-tail-of-log-caching-on-nvdimm/)。 但是，与[结尾日志备份](./backup-restore/tail-log-backups-sql-server.md)存在明显的冲突，并且传统的理解是日志结尾是已强化但尚未备份的事务日志的一部分。 由于官方功能名称是永久日志缓冲区，因此，在此处使用该名称。

请参阅[向数据库添加永久日志缓冲区](./databases/add-persisted-log-buffer.md)。
