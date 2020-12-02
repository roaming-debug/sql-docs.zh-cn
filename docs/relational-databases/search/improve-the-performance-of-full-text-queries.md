---
description: 改进全文查询的性能
title: 改进全文查询的性能 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
ms.assetid: 0658dc74-25eb-4486-bbd6-e85c1f92c272
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 783d2a0e80728f323b5b9d25048c10189df695a3
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91868045"
---
# <a name="improve-the-performance-of-full-text-queries"></a>改进全文查询的性能
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  下面列出了有助于提高全文查询性能的建议。  
  
 硬件资源（例如内存、磁盘速度、CPU 速度和计算机体系结构）也会影响全文查询的性能。  
  
-   使用 [ALTER INDEX REORGANIZE](../../t-sql/statements/alter-index-transact-sql.md)对基表的索引进行碎片整理。  
  
-   使用 [ALTER FULLTEXT CATALOG REORGANIZE](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)重新组织全文目录。 请务必在性能测试之前执行此操作，因为它会导致该目录中全文索引的主合并。  
  
-   仅选择较小的列作为全文键列。 尽管支持 900 个字节的列，但我们建议在全文索引中使用更小的键列。 **int** 和 **bigint** 可提供最佳性能。  
  
-   使用整数型全文键可以避免与 **docid** 映射表联接。 因此，整数型全文键可以使查询性能获得数量级的提升，并改进爬网性能。 如果全文键也是聚集索引键，可能会进一步提高性能。  
  
-   将多个 [CONTAINS](../../t-sql/queries/contains-transact-sql.md) 谓词合并为一个 CONTAINS 谓词。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，您可以在 CONTAINS 查询中指定一个包含若干列的列表。  
  
-   如果只需要全文键或排名的信息，请使用 [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 或 [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) ，而不要使用分别与之对应的 CONTAINS 或 FREETEXT。  
  
-   若要限制结果数并提高性能，请使用 FREETEXTTABLE 和 CONTAINSTABLE 函数的 *top_n_by_rank* 参数。 使用 *top_n_by_rank* 可以只返回最密切相关的匹配项。 仅当商业应用场景不需要返回所有可能的匹配项（即不需要“返回全部项”）时，才应使用此参数。  
  
    > [!NOTE]  
    >  法律应用场景通常需要返回全部项，不过对于诸如电子商务等商业应用场景，性能可能更为重要。  
  
-   检查全文查询计划以确保选择了适当的联接计划。 若有必要，可使用一个联接提示或查询提示。 如果全文查询中使用了参数，则该参数的第一时间值决定查询计划。 可以使用 OPTIMIZE FOR [查询提示](../../t-sql/queries/hints-transact-sql-query.md) 强制用您指定的值编译查询。 这有助于实现确定性查询计划和更好的性能。  
  
-   如果全文索引中的全文索引碎片太多，会导致查询性能大幅下降。 若要减少碎片数，请使用 [ALTER FULLTEXT CATALOG](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的 REORGANIZE 选项重新组织全文目录。 实际上，该语句会将所有碎片合并成单个更大的碎片，并从全文索引中删除所有过时的条目。  
  
-   在  全文搜索中，在 CONTAINSTABLE (AND, OR) 中指定的逻辑运算符可以作为 SQL 联接实现或在全文执行流式表值函数 (STVF) 中实现。 通常，仅包含一种逻辑运算符的查询完全由全文执行实现，而混合多种逻辑运算符的查询还拥有 SQL 联接。 全文执行 STVF 内的逻辑运算符在实现时使用了一些特殊索引属性，使它比 SQL 联接速度快得多。 由于这个原因，我们建议在可能的情况下仅使用一种逻辑运算符来构建查询框架。  
  
-   对于包含选择性关系谓词的应用程序，如果将使用选择性关系谓词和非选择性全文谓词的查询编写成使用查询优化器，则这样的查询可能会有最佳性能。 这将允许查询优化器决定它是否可以利用谓词或范围下推来生成有效的查询计划。 与将关系数据作为全文数据建立索引相比，此方法更简单，通常也更有效。  
  
## <a name="related-resources"></a>相关资源  
 [SQL Server 2008 Full-Text Search: Internals and Enhancements（SQL Server 2008 全文搜索：内在变化与增强功能）](/previous-versions/sql/sql-server-2008/cc721269(v=sql.100))  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_fts_memory_buffers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)   
 [sys.dm_fts_memory_pools (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)  
  
