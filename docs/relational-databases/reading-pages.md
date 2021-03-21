---
description: 读取页
title: 读取页 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- pages
ms.assetid: f8da760e-aacb-4661-9f3a-2578d8c11e4e
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 7eeef502690d3253d14d226a598f1d8f5078a040
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755477"
---
# <a name="reading-pages"></a>读取页
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

SQL Server [!INCLUDE[ssDE](../includes/ssde-md.md)] 实例的 I/O 包括逻辑读取和物理读取。 每次 [!INCLUDE[ssDE](../includes/ssde-md.md)] 从 [缓冲区缓存](../relational-databases/memory-management-architecture-guide.md)请求页时都会发生逻辑读取。 如果页当前不在缓冲区高速缓存中，物理读取将首先将页从磁盘复制到缓存中。

[!INCLUDE[ssDE](../includes/ssde-md.md)] 实例生成的读取请求由关系引擎控制，并由存储引擎优化。 关系引擎决定最有效的访问方法（例如，表扫描、索引扫描或键读取）；存储引擎的访问方法和缓冲区管理器组件确定要执行的读取的常规模式，并对实现访问方法所需的读取进行优化。 执行批处理的线程将安排读取。

## <a name="read-ahead"></a>预读
[!INCLUDE[ssDE](../includes/ssde-md.md)] 支持称为“预读”的性能优化机制。 预读首先预测执行查询执行计划所需的数据和索引页，然后在查询实际使用这些页之前将它们读入缓冲区高速缓存。 这样可以让计算和 I/O 重叠进行，从而充分利用 CPU 和磁盘。 

预读机制允许 [!INCLUDE[ssDE](../includes/ssde-md.md)] 从一个文件中读取最多 64 个连续页 (512KB)。 该读取作为缓冲区高速缓存中相应数量（可能是非相邻的）缓冲区的一次散播-聚集读取来执行。 如果此范围内的任何页在缓冲区高速缓存中已存在，当读取完成时，所读取的相应页将被放弃。 如果相应页在缓存中已存在，也可以从任何一端“裁剪”页的范围。

有两种类型的预读：一种用于数据页，一种用于索引页。

### <a name="reading-data-pages"></a>读取数据页
用于读取数据页的表扫描在 [!INCLUDE[ssDE](../includes/ssde-md.md)]中非常有效。 SQL Server 数据库中的索引分配映射 (IAM) 页列出了表或索引使用的区数。 存储引擎可以读取 IAM 以生成必须读取的磁盘地址的排序列表。 这使得存储引擎能够根据要读取的磁盘位置，将其 I/O 操作优化为按顺序执行的大型顺序读取。 有关 IAM 页的详细信息，请参阅 [管理对象使用的空间](../relational-databases/pages-and-extents-architecture-guide.md)。

### <a name="reading-index-pages"></a>读取索引页
存储引擎按键的顺序依次读取索引页。 例如，下图显示了一组叶级页的简化表示法，该组叶级页包含映射叶级页的键集和中间索引节点。 有关索引中页的结构的详细信息，请参阅 [聚集索引结构](../relational-databases/pages-and-extents-architecture-guide.md)。

![Reading_Pages](../relational-databases/media/reading-pages.gif)

存储引擎使用高于叶级的中间索引页上的信息为包含键的页安排序列预读。 如果请求针对的是 ABC 到 DEF 之间的所有键，则存储引擎将首先读取高于叶级页的索引页， 但它并不是仅仅按顺序读取页 504 到页 556（即指定范围内的包含键的最后一页）之间的每个数据页。 相反，存储引擎将扫描中间索引页并生成必须要读取的叶级页的列表。 然后，存储引擎会按键的顺序安排所有读取。 存储引擎还会识别出页 504/505 以及页 527/528 是相邻页，并执行一次散播读取，从而在单个操作中检索这些相邻页。 如果在一个序列操作中要检索许多页，则存储引擎将一次安排一个读取块。 完成这些读取子集后，存储引擎将安排同等数量的新读取，直到安排完所需的全部读取。

存储引擎使用预提取加快非聚集索引的基表查找。 非聚集索引的叶级行包含指针，指向含有每个特定键值的数据行。 存储引擎浏览非聚集索引的叶级页时，它也会开始计划异步读取已检索了其指针的数据行。 这可以使存储引擎在完成非聚集索引的扫描之前从基础表中检索数据行。 无论表是否有聚集索引，都会使用预提取。 SQL Server Enterprise 比 SQL Server 其他版本使用更多的预提取，可以预读更多页。 在任何版本中都无法配置预提取的级别。 有关非聚集索引的详细信息，请参阅 [非聚集索引结构](../relational-databases/pages-and-extents-architecture-guide.md)。

## <a name="advanced-scanning"></a>高级扫描
在 SQL Server Enterprise 中，高级扫描功能可以使多项任务共享完全表扫描。 如果 Transact-SQL 语句的执行计划需要扫描表中的数据页，并且 [!INCLUDE[ssDE](../includes/ssde-md.md)] 检测到其他执行计划正在扫描该表，则 [!INCLUDE[ssDE](../includes/ssde-md.md)] 会在第二个扫描的当前位置将第二个扫描加入第一个扫描。 [!INCLUDE[ssDE](../includes/ssde-md.md)] 会一次读取一页，并将每一页的行传递给这两个执行计划。 此操作将一直持续到该表的结尾处。 

此时，第一个执行计划已有完整的扫描结果，而第二个执行计划仍必须检索在它加入正在进行的扫描之前读取的数据页。 然后，第二个执行计划中的扫描将绕回到表的第一个数据页，并从这里向前扫描到它加入第一个扫描时所处的位置。 可以按这种方式组合任意数量的扫描。 [!INCLUDE[ssDE](../includes/ssde-md.md)] 将循环遍历数据页，直到完成所有扫描。 这种机制也称为“走马灯式扫描”，说明了为何在没有 ORDER BY 子句的情况下无法保证 SELECT 语句所返回结果的顺序。 

例如，假设某个表有 500,000 页。 UserA 执行了一条 Transact-SQL 语句，要求对该表进行扫描。 当扫描已处理了 100,000 页时，UserB 执行了另一条 Transact-SQL 语句，要对同一个表进行扫描。 [!INCLUDE[ssDE](../includes/ssde-md.md)] 将为页 100,001 之后的页安排一组读取请求，并将每页中的行同时传递回两个扫描。 当扫描到 200,000 页时，UserC 执行了另一条 Transact-SQL 语句，要对同一个表进行扫描。 则从页 200,001 开始， [!INCLUDE[ssDE](../includes/ssde-md.md)] 将把它读取的每一页中的行传递回所有三个扫描。 当数据库引擎读取完第 500,000 行之后，UserA 的扫描就完成了，而 UserB 和 UserC 的扫描将绕回到页 1 开始读取。 当 [!INCLUDE[ssDE](../includes/ssde-md.md)] 到达页 100,000 时，UserB 的扫描就完成了。 然后 UserC 的扫描将继续进行，直到它读取完页 200,000。 此时，所有扫描均告完成。 

在没有高级扫描的情况下，每个用户都必须要争用缓冲区空间并因此导致磁盘臂争用。 然后,会分别为每个用户读取一次相同的页，而不是一次读取并由多个用户共享，这样会降低性能并加重资源负担。

## <a name="see-also"></a>另请参阅
[页和区体系结构指南](../relational-databases/pages-and-extents-architecture-guide.md)   
 [写入页](../relational-databases/writing-pages.md)
