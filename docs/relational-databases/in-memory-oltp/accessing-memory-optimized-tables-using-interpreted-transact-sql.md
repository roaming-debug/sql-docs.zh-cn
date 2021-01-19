---
title: 使用解释型 T-SQL 的内存优化表
description: 了解如何使用解释型 Transact-SQL（SQL Server 中的 Transact-SQL 批处理或存储过程）访问内存优化表。
ms.custom: seo-dt-2019
ms.date: 05/31/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 92a44d4d-0e53-4fb0-b890-de264c65c95a
author: MightyPen
ms.author: genemi
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c88fd07e9af86a3c12561935364514b23bc2ea91
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172279"
---
# <a name="accessing-memory-optimized-tables-using-interpreted-transact-sql"></a>使用解释型 Transact-SQL 访问内存优化表
[!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)]

 除了少数例外情况，可以使用任何 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询或 DML 操作（选择、插入、更新或删除）、即席批处理和 SQL 模块（如存储过程、表值函数、触发器和视图）来访问内存优化表。  
  
解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 指 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理或是非本机编译的存储过程的存储过程。 对内存优化表的解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问称为互操作访问。  

从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]开始，经解释的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中的查询能够以并行模式而不只是串行模式扫描内存优化表。

内存优化表还可使用本机编译的存储过程访问。 建议将本机编译的存储过程用于对性能至关重要的 OLTP 操作。  
  
建议将解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问用于以下情况：  
  
- 即席查询和管理任务。  
  
- 报表查询，通常使用本机编译的存储过程中不可用的构造（如 *window* 函数，有时称为 [OVER](../../t-sql/queries/select-over-clause-transact-sql.md) 函数）。  
  
- 要将应用程序中对性能至关重要的部分迁移到内存优化表，只需极少的应用程序代码更改（或无需更改）。 通过迁移表可能会提高性能。 如果随后将存储过程迁移到本机编译的存储过程，则可以进一步提高性能。  
  
- 当 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句不可用于本机编译的存储过程时。  
  
不过，访问内存优化表中的数据的解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程中不支持以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 构造。  
  
|区域|不支持|  
|----------|-----------------|  
|访问表|TRUNCATE TABLE<br /><br /> MERGE（使用内存优化的表作为目标）<br /><br /> 动态和键集游标（这些会自动降级为静态）。<br /><br /> 使用上下文连接从 CLR 模块访问。<br /><br /> 从索引视图引用内存优化表。|  
|跨数据库|跨数据库查询<br /><br /> 跨数据库事务<br /><br /> 链接服务器|  
  
## <a name="table-hints"></a>表提示

有关表提示的详细信息，请参阅。 [表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。 添加了 SNAPSHOT 以支持 [!INCLUDE[hek_2](../../includes/hek-2-md.md)]。  
  
使用解释型 [!INCLUDE[tsql](../../includes/tsql-md.md)]访问内存优化表时，不支持以下表提示。  

:::row:::
    :::column:::
        HOLDLOCK

        PAGLOCK

        READUNCOMMITTED

        TABLOCKXX
    :::column-end:::
    :::column:::
        IGNORE_CONSTRAINTS

        READCOMMITTED

        ROWLOCK

        UPDLOCK
    :::column-end:::
    :::column:::
        IGNORE_TRIGGERS

        READCOMMITTEDLOCK

        SPATIAL_WINDOW_MAX_CELLS = *integer*

        XLOCK
    :::column-end:::
    :::column:::
        NOWAIT

        READPAST

        TABLOCK
    :::column-end:::
:::row-end:::

使用经解释的 [!INCLUDE[tsql](../../includes/tsql-md.md)]从显式或隐式事务中访问内存优化表时，你必须至少执行下列操作之一：  
  
- 指定 [隔离级别表提示](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md) ，例如 SNAPSHOT、REPEATABLEREAD 或 SERIALIZABLE。  
  
- 将数据库选项 [MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT](../../t-sql/statements/alter-database-transact-sql-set-options.md) 设置为 ON。  
  
由在 [自动提交模式](../../odbc/reference/develop-app/auto-commit-mode.md)下运行的查询访问的内存优化表不需要隔离级别表提示。  
  
## <a name="see-also"></a>另请参阅

[对内存中 OLTP 的 Transact-SQL 支持](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)   

[迁移到内存中 OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)