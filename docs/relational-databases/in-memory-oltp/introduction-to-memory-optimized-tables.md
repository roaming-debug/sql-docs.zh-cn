---
title: 内存优化表简介 | Microsoft Docs
description: 了解内存优化表，这些表具有持久性并支持具有原子性、一致性、隔离性和持久性的事务。
ms.custom: ''
ms.date: 12/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: ef1cc7de-63be-4fa3-a622-6d93b440e3ac
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e55bab58f73521570a7d0a57ced6dd72cad9511f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473898"
---
# <a name="introduction-to-memory-optimized-tables"></a>内存优化表简介
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  内存优化表是使用 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md) 创建而成的表。  
  
 默认情况下，内存优化表具有完全持久性。与（传统）基于磁盘的表上的事务一样，内存优化表上的事务具有完全原子性、一致性、隔离性和持久性 (ACID)。 内存优化表和本机编译的存储过程仅支持一部分 [!INCLUDE[tsql](../../includes/tsql-md.md)] 功能。
 
自 SQL Server 2016 起以及在 Azure SQL 数据库中，内存中 OLTP 特定的 [排序规则或代码页](../../relational-databases/collations/collation-and-unicode-support.md) 没有任何限制。
  
 内存优化表的主存储器是主要内存。 从内存读取表中的行和将这些行写入内存。 表数据的另一个副本维护在磁盘上，但仅用于持续性目的。 有关持久表的详细信息，请参阅 [创建和管理用于内存优化的对象的存储](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md) 。 在数据库恢复期间（例如， 在服务器重启后），内存优化表中的数据只能从磁盘读取。  
  
 为了获得更大的性能提升，内存中 OLTP 支持事务持续性延迟的持久表。 延迟的持久事务在提交事务并将控制权归还客户端后不久即保存到磁盘。 作为提高性能的代价，在服务器崩溃或故障转移过程中将丢失已提交但未保存到磁盘的事务。  
  
 除了默认持久的内存优化表之外， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还支持非持久的内存优化表，不记录这些表的日志且不在磁盘上保存它们的数据。 这意味着这些表上的事务不需要任何磁盘 IO，但如果服务器崩溃或进行故障转移，则无法恢复数据。  
  
 内存中 OLTP 与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 集成，以便在所有方面（如开发、部署、可管理性和可支持性）提供无缝体验。 数据库可包含内存中对象以及基于磁盘的对象。  
  
 内存优化表中的行是版本化的。 这意味着表中的每行都可能有多个版本。 所有行版本均维护在同一个表数据结构中。 行版本控制用于实现对同一行的并发读取和写入。 有关对同一行的并发读取和写入的更多信息，请参阅 [内存优化表中的事物](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)。  
  
 下图展示多版本控制。 该图显示了一个包含三行的表，其中，每行都有不同的版本。  
  
![多版本控制。](../../relational-databases/in-memory-oltp/media/hekaton-tables-1.gif "多版本控制。")  
  
 该表有三行：r1、r2 和 r3。 r1 有三个版本，r2 有两个版本，r3 有四个版本。 注意，同一行的不同版本不必占用连续的内存位置。 不同的行版本可分散到整个表数据结构中。  
  
 可将内存优化的表数据结构视为一个行版本集合。 基于磁盘的表中的行以页和区形式组织，各个行借助页码和页偏移量进行寻址，而内存优化表中的行版本则借助 8 字节的内存指针进行寻址。  
  
 可通过以下两种方式访问内存优化表中的数据：  
  
-   通过本机编译的存储过程。  
  
-   通过本机编译的存储过程之外的解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)]。 这些 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句可位于解释型存储过程内，也可以是临时 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
## <a name="accessing-data-in-memory-optimized-tables"></a>访问内存优化表中的数据  

可以最高效地从本机编译的存储过程访问内存优化表（[本机编译的存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)）。 还可以使用（传统的）解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)]访问内存优化表。 解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 是指在不使用本机编译的存储过程的情况下访问内存优化表。 解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问的一些示例包括从 DML 触发器、即席 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理、视图和表值函数访问内存优化表。  
  
 下表总结了对各种对象的本机和解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问。  
  
|Feature|使用本机编译的存储过程访问|解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问|CLR 访问|  
|-------------|-------------------------------------------------------|-------------------------------------------|----------------|  
|内存优化的表|是|是|否*|  
|内存优化的表类型|是|是|否|  
|本机编译的存储过程|现在支持嵌套本机编译存储过程。 只要引用的过程也是本机编译过程，则可以在存储过程中使用 EXECUTE 语法。|是|否*|  
  
 *无法从上下文连接（执行 CLR 模块时与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的连接）访问内存优化表或本机编译存储过程。 但是，可以创建和打开能够访问内存优化的表和本机编译的存储过程的其他连接。  
  
## <a name="performance-and-scalability"></a>性能和可伸缩性  

以下因素会影响可以通过内存中 OLTP 实现的性能提升：  
  
通信：与包含较少调用且每个存储过程中实现的功能较多的应用程序相比，具有许多短存储过程调用的应用程序的性能提升可能较小。  
  
[!INCLUDE[tsql](../../includes/tsql-md.md)]执行：在使用本机编译的存储过程而不是解释的存储过程或查询执行时，内存中 OLTP 实现最佳性能。 从此类存储过程访问内存优化表可能有好处。  
  
范围扫描与点查找：内存优化的非聚集索引支持范围扫描和有序扫描。 对于点查找，内存优化的哈希索引具有比内存优化的非聚集索引更好的性能。 内存优化的非聚集索引具有比基于磁盘的索引更好的性能。

- 从 SQL Server 2016 开始，内存优化表的查询计划可以并行方式扫描表。 这提高了分析查询的性能。
  - 在 SQL Server 2016 中，哈希索引也变为可以并行方式进行扫描。
  - 在 SQL Server 2016 中，非聚集索引也变为可以并行方式进行扫描。
  - 从一开始在 SQL Server 2014 中，列存储索引就可以并行方式进行扫描。
  
索引操作：索引操作不记入日志并且只存在于内存中。  
  
并发：在性能受引擎级别并发（如闩锁争用或阻塞）影响的应用程序迁移到内存中 OLTP 时，其性能会显著提高。  
  
下表列出了关系数据库中常见的性能和可伸缩性问题以及内存中 OLTP 提高性能的方式。  
  
|问题|内存中 OLTP 影响|  
|-----------|----------------------------|  
|性能<br /><br /> 资源（CPU、I/O、网络或内存）使用率较高。|CPU<br /> 本机编译的存储过程可大幅降低 CPU 使用率，因为此类存储过程执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句所需的指令比解释型存储过程少得多。<br /><br /> 内存中 OLTP 可以帮助减少扩展工作负荷时的硬件投资，因为一台服务器可提供五到十台服务器的吞吐量。<br /><br /> I/O<br /> 如果在处理数据或索引页方面遇到 I/O 瓶颈，则内存中 OLTP 可缓解瓶颈现象。 此外，内存中 OLTP 对象的检查点编号是连续的，不会导致 I/O 操作突然增多。 但是，如果对性能至关重要的表的工作集不能容纳于内存中，则内存中 OLTP 不会提高性能，因为它需要数据驻留在内存中。 如果在日志记录方面遇到 I/O 瓶颈，则内存中 OLTP 可缓解瓶颈现象，因为它进行的日志记录操作较少。 如果将一个或多个内存优化的表配置为非持久的表，您可以消除数据的日志记录。<br /><br /> 内存<br /> 内存中 OLTP 不会提供任何性能优势。 内存中 OLTP 可能会对内存产生额外的压力，因为这些对象需要驻留在内存中。<br /><br /> 网络<br /> 内存中 OLTP 不会提供任何性能优势。 数据需要从数据层传输到应用层。|  
|可伸缩性<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应用程序中的大多数伸缩问题是由并发问题（如锁、闩锁和旋转锁中的争用）引起的。|闩锁争用<br /> 典型情况是按键顺序并发插入行时索引的最后一页上的争用。 由于内存中 OLTP 在访问数据时不采用闩锁，因此可完全消除与闩锁争用相关的可伸缩性问题。<br /><br /> 旋转锁争用<br /> 由于内存中 OLTP 在访问数据时不采用闩锁，因此可完全消除与旋转锁争用相关的可伸缩性问题。<br /><br /> 与锁定相关的争用<br /> 如果数据库应用程序遇到读操作与写操作之间的阻塞问题，则内存中 OLTP 可消除这些阻塞问题，因为它使用新的乐观并发控制形式来实现所有事务隔离级别。 内存中 OLTP 不使用 TempDB 来存储行版本。<br /><br /> 如果伸缩问题是由两个写操作之间的冲突（例如，两个并发事务尝试更新相同的行）引起的，则内存中 OLTP 会让其中一个事务成功，而让另一个事务失败。 必须显式或隐式重新提交失败的事务，从而重试该事物。 在任一情况下，您都需要对应用程序进行更改。<br /><br /> 如果应用程序遇到两个写操作之间的频繁冲突，则会减小乐观锁定的值。 该应用程序不适用于内存中 OLTP。 除非冲突是由锁升级引起的，否则大多数 OLTP 应用程序没有写冲突。|  
  
##  <a name="row-level-security-in-memory-optimized-tables"></a><a name="rls"></a> 内存优化表中的行级安全性  

内存优化表支持[行级安全性](../../relational-databases/security/row-level-security.md) 。 向内存优化表应用行级安全策略实际上与基于磁盘的表一样，只是作为安全谓词使用的内联表值函数必须是本机编译函数（使用 WITH NATIVE_COMPILATION 选项创建）。 有关详细信息，请参阅 [行级安全性](../../relational-databases/security/row-level-security.md#Limitations) 主题中的 [跨功能兼容性](../../relational-databases/security/row-level-security.md) 。  
  
 已为内存中的表启用了各种内置安全函数，这些函数对行级安全至关重要。 有关详情，请参阅 [本机编译的模块中的内置函数](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md#bfncsp)。  
  
 **EXECUTE AS CALLER** - 所有本机模块现在默认支持并使用 EXECUTE AS CALLER，即使未指定提示也是如此。 这是因为所有行级安全谓词都会使用 EXECUTE AS CALLER，因此将会在调用用户的上下文中计算该函数（以及其中使用的任何内置函数）。   
EXECUTE AS CALLER 会对调用方产生由权限检查造成的微小（约为 10%）性能下降。 如果该模块显式指定 EXECUTE AS OWNER 或 EXECUTE AS SELF，则将避免这些权限检查和与其相关的性能损失。 但是，与上述内置函数一起使用任一选项会进行必要的上下文切换，从而极大地降低性能。  
  
## <a name="scenarios"></a>方案

有关 [!INCLUDE[hek_1](../../includes/hek-1-md.md)] 可提高性能的典型方案的浅谈，请参阅 [内存中 OLTP](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。  
  
## <a name="see-also"></a>另请参阅

[内存中 OLTP（内存中优化）](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
