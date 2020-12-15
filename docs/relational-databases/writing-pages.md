---
description: 写入页
title: 写入页 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- pages
ms.assetid: 409c8753-03c4-436d-839c-6a5879971551
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3c9fc200c47ffdb82c15a86469244cd17bd75821
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97403233"
---
# <a name="writing-pages"></a>写入页
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[ssDE](../includes/ssde-md.md)] 实例的 I/O 包括逻辑写入和物理写入。 修改缓冲区缓存中页的数据时发生逻辑写入。 将页从 [缓冲区缓存](../relational-databases/memory-management-architecture-guide.md) 写入磁盘时，发生物理写入。

在缓冲区缓存中修改页后，不会将其立即写回磁盘；而是将其标记为“脏”。 也就是说在将页物理写入磁盘之前，可以将其逻辑写入多次。 对于每次逻辑写入，都会在记录修改的日志缓存中插入一条事务日志记录。 在将关联的脏页从缓冲区缓存中删除并写入磁盘之前，必须将这条些日志记录写入磁盘。 SQL Server 使用了预写日志记录技术，该技术可在关联日志记录写入到磁盘前阻止写入脏页。 这对于保证恢复管理器的正常工作至关重要。 有关详细信息，请参阅 [预写事务日志](../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)。

下图显示了写入修改后的数据页的过程。

![Writing_Pages](../relational-databases/media/writing-pages.gif)

在缓冲区管理器写入页时，它会搜索可通过一次聚集-写入操作写入的相邻脏页。 相邻页具有连续的页 ID 且属于同一文件；这些页在内存中不一定相邻。 直到发生下列事件之一，才会停止向前和向后的搜索：

 * 找到一个干净页。
 * 已找到 32 页。
 * 找到一个日志序列号 (LSN) 尚未刷新到日志中的脏页。
 * 找到一个无法立即闩锁的页。

通过这种方式，可以通过一次聚集-写入操作将整组页写入磁盘。 

仅在写入页之前，才会把数据库中指定的页保护形式添加到该页。 如果添加了残缺页保护，则必须对 I/O 专门闩锁该页。 这是因为残缺页保护会修改该页，使其不适合任何其他线程读取。 如果添加了校验和页保护，或者数据库未使用页保护，则会使用 UP（更新）闩锁对 I/O 闩锁该页。 该闩锁可防止任何其他人在写入期间修改该页，但仍然允许读取者使用它。 有关磁盘 I/O 页保护选项的详细信息，请参阅 [缓冲区管理](../relational-databases/memory-management-architecture-guide.md)。

脏页写入磁盘的方式有三种： 

* 惰性写入   
 惰性编写器是一个系统进程，它会删除缓冲区高速缓存中的不常用页，以保证存在可用的缓冲区。 首先将脏页写入磁盘。 

* 勤奋写入   
 勤奋写入进程会写入与最少记录的操作（例如大容量插入和选择插入）相关联的脏数据页。 该进程允许以并行方式创建和写入新页。 也就是说，调用操作不必等待整个操作完成，即可将页写入磁盘。

* 检查点   
 检查点进程定期在缓冲区高速缓存中扫描包含来自指定数据库的页的缓冲区，然后将所有脏页写入磁盘。 CHECKPOINT 可创建一个检查点，在该点保证全部脏页都已写入磁盘，从而在以后的恢复过程中节省时间。 用户可使用 CHECKPOINT 命令请求检查点操作，否则 [!INCLUDE[ssDE](../includes/ssde-md.md)] 会根据上次检查点之后使用的日志空间量和经过的时间生成自动检查点。 另外，在发生特定活动时会生成检查点。 例如，在数据库中添加或删除了数据或日志文件时，或在 SQL Server 实例停止时。 有关详细信息，请参阅 [检查点和日志的活动部分](../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)。

惰性写入、勤奋写入和检查点进程均不等待 I/O 操作完成。 它们始终使用异步（或重叠）I/O，并将继续执行其他工作，之后再检查 I/O 是否成功。 因此，SQL Server 在执行相应任务时可充分利用 CPU 和 I/O 资源。

## <a name="see-also"></a>另请参阅
[页和区体系结构指南](../relational-databases/pages-and-extents-architecture-guide.md)   
 [读取页](../relational-databases/reading-pages.md)
