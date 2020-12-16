---
title: 内存优化表的统计信息 | Microsoft Docs
description: 了解查询优化器如何使用内存优化表中的有关列的统计信息来创建可提高内存中 OLTP 查询性能的查询计划。
ms.custom: ''
ms.date: 10/23/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: e644766d-1d1c-43d7-83ff-8ccfe4f3af9f
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 119d3702dd315e424c56a3e1431b42a79ca636d4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485159"
---
# <a name="statistics-for-memory-optimized-tables"></a>内存优化表的统计信息
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  查询优化器使用有关列的统计信息来创建可提高查询性能的查询计划。 从数据库中的表收集统计信息并将它保存在数据库元数据中。  
  
 统计信息自动创建，但也可以手动创建。 例如，在创建索引时自动对索引键列创建统计信息。 有关创建统计信息的详细信息，请参阅 [Statistics](../../relational-databases/statistics/statistics.md)。  
  
 随着行的插入、更新和删除，表数据通常在一段时间后发生变化。 这意味着需要定期更新统计信息。 默认情况下，在查询优化器确定表中的统计信息可能过期时，会自动更新它们。  
  
 有关内存优化表的统计信息的注意事项：  
  
-   在 SQL Server 2016 和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]中启动时，如果使用至少为 130 的数据库兼容级别，则内存优化表支持自动更新统计信息。 请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。 如果数据库中有先前使用较低兼容级别创建的表，则需要手动更新统计信息一次，以便统计信息能继续自动更新。
  
-   对于本机编译的存储过程，在编译该过程时对过程中查询的执行计划进行优化，该优化发生于创建时期。 当统计信息更新时，它们不会自动重新编译。 因此在创建过程之前，这些表中应包含有代表性的数据集。  
  
-   可以使用 [sp_recompile (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md)手动重新编译本机编译的存储过程，如果数据库脱机并重新联机或者数据库故障转移或服务器重新启动，则可以进行自动重新编译。  
  
## <a name="enabling-automatic-update-of-statistics-in-existing-tables"></a>在现有表中启用自动更新统计信息

在一个兼容级别至少为 130 的数据库中创建表时，此表中的所有统计信息都将启用自动更新，且不需要额外操作。

如果数据库具有在早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中创建的内存优化表或该数据库的兼容级别低于 130，则需要手动更新一次统计信息，以便统计信息能继续自动更新。

若要启用自动更新在旧的兼容级别下创建的内存优化表统计信息，请按照以下步骤操作：

1. 更新数据库兼容级别： `ALTER DATABASE CURRENT SET COMPATIBILITY_LEVEL=130`

2. 手动更新内存优化表的统计信息。 下面是执行相同任务的示例脚本。

3. 手动重新编译本机编译的存储过程以从更新的统计信息中受益。

统计信息的一次性脚本：对于在较低兼容级别下创建的内存优化表，可以运行以下 Transact-SQL 脚本一次，以更新所有内存优化表的统计信息，并实现在以后自动更新统计信息（假定对数据库启用了 AUTO_UPDATE_STATISTICS）：

```
-- Assuming AUTO_UPDATE_STATISTICS is already ON for your database:
-- ALTER DATABASE CURRENT SET AUTO_UPDATE_STATISTICS ON;

ALTER DATABASE CURRENT SET COMPATIBILITY_LEVEL = 130;
GO
DECLARE @sql NVARCHAR(MAX) = N'';
SELECT
      @sql += N'UPDATE STATISTICS '
         + quotename(schema_name(t.schema_id))
         + N'.'
         + quotename(t.name)
         + ';' + CHAR(13) + CHAR(10)
   FROM sys.tables AS t
   WHERE t.is_memory_optimized = 1 AND 
        t.object_id IN (SELECT object_id FROM sys.stats WHERE no_recompute=1)
;
EXECUTE sp_executesql @sql;
GO
-- Each row appended to @sql looks roughly like:
-- UPDATE STATISTICS [dbo].[MyMemoryOptimizedTable];
```

验证是否已启用自动更新：以下脚本将验证在内存优化表中的统计信息是否启用了自动更新。 在运行前面的脚本后，它将在所有统计信息对象的 `1` 列中返回 `auto-update enabled` 。

```
SELECT 
    quotename(schema_name(o.schema_id)) + N'.' + quotename(o.name) AS [table],
    s.name AS [statistics object],
    1-s.no_recompute AS [auto-update enabled]
FROM sys.stats s JOIN sys.tables o ON s.object_id=o.object_id
WHERE o.is_memory_optimized=1
```

## <a name="guidelines-for-deploying-tables-and-procedures"></a>部署表和过程的指南  
 要确保查询优化器在创建查询计划时具有最新统计信息，请执行以下四个步骤部署内存优化表以及访问这些表的本机编译存储过程：  
  
1.  确保数据库的兼容级别至少为 130。 请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。

2.  创建表和索引。 在 **CREATE TABLE** 语句中应该内联指定索引。  
  
3.  将数据加载到表。  
  
4.  创建访问表的存储过程。  
  
 在加载数据后创建本机编译的存储过程可确保优化器为内存优化的表提供统计信息。 这将确保在编译过程时生成高效的查询计划。  

## <a name="see-also"></a>另请参阅  
 [Memory-Optimized Tables](./sample-database-for-in-memory-oltp.md)  
  
