---
title: 不支持的功能 - 内存中 OLTP
description: 了解内存优化对象不支持的 SQL Server 功能。 查看已受支持的内存中 OLTP 的功能。
ms.custom: ''
ms.date: 02/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: c39f03a7-e223-4fd7-bd30-142e28f51654
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 608419ef77ab11f18274bb3c00ebb66b365448e4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480088"
---
# <a name="unsupported-sql-server-features-for-in-memory-oltp"></a>内存中 OLTP 不支持的 SQL Server 功能
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

本主题讨论不支持用于内存优化对象的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。 此外，最后一节列出了内存中 OLTP 不支持的功能，但后来添加了对这些功能的支持。
  
## <a name="ssnoversion-features-not-supported-for-in-memory-oltp"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存中 OLTP 不支持的功能  

具有内存优化对象（包括内存优化数据文件组）的数据库不支持以下 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。  

  
|不支持的功能|功能说明|  
|-------------------------|-------------------------|  
|对内存优化表进行数据压缩。|您可以使用数据压缩功能帮助压缩数据库中的数据并帮助减小数据库的大小。 有关详细信息，请参阅 [Data Compression](../../relational-databases/data-compression/data-compression.md)。|  
|对内存优化表和 HASH 索引以及非聚集索引进行分区。|已分区表和已分区索引的数据划分为分布于一个数据库中多个文件组的单元。 有关详细信息，请参阅 [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md)。|  
| 复制 | 对订阅服务器上内存优化表进行的事务复制之外的其他复制配置与引用内存优化表的表或视图不兼容。<br /><br />如果存在内存优化文件组，则不支持使用 sync_mode=’database snapshot’ 的复制。<br /><br />有关详细信息，请参阅 [复制到内存优化表订阅服务器](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md)。|
|镜像|具有 MEMORY_OPTIMIZED_DATA 文件组的数据库不支持数据库镜像。 有关镜像的详细信息，请参阅[数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)。|  
|重新生成日志|具有 MEMORY_OPTIMIZED_DATA 文件组的数据库不支持通过附加或 ALTER DATABASE 重新生成日志。|  
|链接服务器|不能在与内存优化表相同的查询或事务中访问链接服务器。 有关详细信息，请参阅 [链接服务器（数据库引擎）](../../relational-databases/linked-servers/linked-servers-database-engine.md)。|  
|大容量日志记录|无论数据库处于什么恢复模式，都将始终完整记录针对持久内存优化表的所有操作的日志。|  
|最小日志记录|内存优化表不支持最小日志记录。 有关最小日志记录的详细信息，请参阅[事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md) 和[在批量导入中按最小方式记录日志的前提条件](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md)。|  
|更改跟踪|内存优化表不支持更改跟踪。 |
| DDL 触发器 | 内存中 OLTP 表或本机编译的模块不支持数据库级别和服务器级别的 DDL 触发器。 |  
| 变更数据捕获 (CDC) | SQL Server 2017 CU15 及更高版本支持在具有内存优化表的数据库上启用 CDC。 这仅适用于数据库和数据库中的任何磁盘上表。 在更低版本的 SQL Server 中，CDC 不能与具有内存优化表的数据库一起使用，因为它在内部使用 DROP TABLE 的 DDL 触发器。 |  
| 纤程模式 | 内存优化表不支持纤程模式：<br /><br />如果启用纤程模式，则不能创建具有内存优化文件组的数据库，也不能向现有数据库添加内存优化文件组。<br /><br />如果有包含内存优化文件组的数据库，可以启用纤程模式。 不过，启用纤程模式需要重新启动服务器。 在这种情况下，具有内存优化文件组的数据库将无法恢复。 随后将看到一条错误消息，建议禁用纤程模式，以使用具有内存优化文件组的数据库。<br /><br />如果启用纤程模式，附加和还原具有内存优化文件组的数据库会失败。 数据库将标记为可疑。<br /><br />有关详细信息，请参阅 [lightweight pooling 服务器配置选项](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)。 |  
|Service Broker 的限制|无法访问本机编译存储过程中的队列。<br /><br /> 无法在访问内存优化表的事务中访问远程数据库中的队列。|  
|在订阅服务器上复制|支持对订阅服务器上的内存优化表进行事务复制，不过有一些限制。 有关详细信息，请参阅 [复制到内存优化表订阅服务器](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md)|  
|||

#### <a name="cross-database-queries-and-transcations"></a>跨数据库查询和事务

除若干例外情况，一般不支持跨数据库事务。 下表介绍支持的情况和相应的限制。 （另请参阅 [跨数据库查询](../../relational-databases/in-memory-oltp/cross-database-queries.md)。）  


|数据库|允许|说明|  
|---------------|-------------|-----------------|  
| 用户数据库、模型和 msdb 。 | 否 | 多数情况下，不支持跨数据库查询和事务。<br /><br />如果查询使用内存优化表或本机编译存储过程，则此查询无法访问其他数据库。 此限制适用于事务以及查询。<br /><br />tempdb 和 master 系统数据库除外 。 此时，master 数据库可进行只读访问。 |
| 资源数据库和 tempdb  | 是 | 在涉及内存中 OLTP 对象的事务中，可以使用资源和 tempdb 系统数据库，而无需添加限制 。
||||

## <a name="scenarios-not-supported"></a>不支持的方案  
  
- 使用上下文连接从 CLR 存储过程内部访问内存优化表。  
  
- 访问内存优化表的查询上的键集和动态游标。 这些游标将降级为静态和只读的。  
  
- 不支持使用 MERGE INTO target，其中 target 是内存优化表。
    - 内存优化表支持 MERGE USING source。  
  
- 不支持 ROWVERSION (TIMESTAMP) 数据类型。 有关详细信息，请参阅 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)。
  
- 具有 MEMORY_OPTIMIZED_DATA 文件组的数据库不支持自动关闭。  

- 用户事务内部不支持内存中 OLTP 对象的 CREATE/ALTER/DROP 等事务性 DDL。  
  
- 事件通知。  
  
- 基于策略的管理 (PBM)。
    - 不支持 PBM 的仅阻止并记录模式。 当服务器上存在此类策略时，可能会使内存中 OLTP DDL 无法成功执行。 支持“按需”和“按计划”模式。  

- 内存中 OLTP 不支持数据库包含（[包含的数据库](../../relational-databases/databases/contained-databases.md)）。
    - 支持 contained database authentication。 但是，在动态管理视图 (DMV) dm_db_uncontained_entities 中，所有内存中 OLTP 对象都被标记为“breaking containment”。

## <a name="recently-added-supports"></a>最近添加的支持

有时，较新版本的 SQL Server 增加了对以前不支持的功能的支持。 本节列出了内存中 OLTP 过去不支持的功能，但后来内存中 OLTP 添加了对这些功能的支持。

在下表中，version 值（如 `(15.x)`）是指 Transact-SQL 语句 `SELECT @@Version;` 返回的值。

| 功能名称 | SQL Server 的版本 | 注释 |
| :----------- | :-------------------- | :------- |
| 数据库快照 | 2019 (15.x) | 具有 MEMORY_OPTIMIZED_DATA 文件组的数据库现支持数据库快照。 |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="see-also"></a>另请参阅

- [SQL Server 对内存中 OLTP 的支持](./transact-sql-support-for-in-memory-oltp.md)