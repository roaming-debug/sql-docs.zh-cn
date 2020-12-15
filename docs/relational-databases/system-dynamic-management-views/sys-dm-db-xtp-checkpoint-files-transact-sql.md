---
title: sys.dm_db_xtp_checkpoint_files (Transact-sql) |Microsoft Docs
description: 显示有关检查点文件的信息，包括文件大小、物理位置和事务 ID。 了解此视图与 SQL Server 版本的不同之处。
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: in-memory-oltp
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_checkpoint_files
- sys.dm_db_xtp_checkpoint_files_TSQL
- dm_db_xtp_checkpoint_files_TSQL
- sys.dm_db_xtp_checkpoint_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_files dynamic management view
ms.assetid: ac8e6333-7a9f-478a-b446-5602283e81c9
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8efd90743e84db69dfddef113942a02e51a5386f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474978"
---
# <a name="sysdm_db_xtp_checkpoint_files-transact-sql"></a>sys.dm_db_xtp_checkpoint_files (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  显示有关检查点文件的信息，包括文件大小、物理位置和事务 ID。  
  
> **注意：** 对于尚未关闭的当前检查点，的 "状态" 列 `ys.dm_db_xtp_checkpoint_files` 将针对新文件进行构造。 自最后一个检查点后，如果存在足够的事务日志增长，或 `CHECKPOINT` [&#40;transact-sql&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)) 发出命令 (检查点，则检查点将自动关闭。  
  
 内存优化文件组在内部使用仅限追加的文件存储内存中表的插入行和已删除行。 有两种类型的文件。 数据文件包含插入的行，而差异文件包含对已删除行的引用。 
  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 与更新的版本有很大差异，在 [SQL Server 2014](#bkmk_2014)的主题中对此进行了深入讨论。  
  
 有关详细信息，请参阅 [创建和管理 Memory-Optimized 对象的存储](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)。  
  
##  <a name="sssql15-and-later"></a><a name="bkmk_2016"></a> [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本  
 下表描述了 `sys.dm_db_xtp_checkpoint_files` 从开始的列 **[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]** 。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|container_id|**int**|数据或差异文件所属的容器 ID（在 sys.database_files 中表示为类型为 FILESTREAM 的文件）。 与 [sys.database_files &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)中的 file_id 的联接。|  
|container_guid|**uniqueidentifier**|根、数据或差异文件所属的容器的 GUID。 与 sys.database_files 表中 file_guid 的联接。|  
|checkpoint_file_id|**uniqueidentifier**|检查点文件的 GUID。|  
|relative_file_path|**nvarchar(256)**|相对于它映射到的容器的文件路径。|  
|file_type|**smallint**|-1 表示免费<br /><br /> 对于数据文件，为0。<br /><br /> 1表示增量文件。<br /><br /> 2对于根文件<br /><br /> 3对于大数据文件|  
|file_type_desc|**nvarchar(60)**|FREE-所有免费维护的文件均可供分配。 可用文件的大小可能不同，具体取决于系统的预期需求。 最大大小为1GB。<br /><br /> 数据数据文件包含已插入到内存优化表中的行。<br /><br /> 增量增量文件包含对已删除数据文件中的行的引用。<br /><br /> 根根文件包含内存优化对象和本机编译的对象的系统元数据。<br /><br /> 大数据-大型数据文件包含插入 (n) varchar (max) 和 varbinary 的值 (最大) 列以及内存优化表的列存储索引中的列段。|  
|internal_storage_slot|**int**|内部存储数组中的文件的索引。 如果根为 NULL 或为1，则为 NULL。|  
|checkpoint_pair_file_id|**uniqueidentifier**|对应的数据或差异文件。 对于根为 NULL。|  
|file_size_in_bytes|**bigint**|磁盘上文件的大小。|  
|file_size_used_in_bytes|**bigint**|对于仍在填充的检查点文件对，此列将在下一个检查点的后面更新。|  
|logical_row_count|**bigint**|对于 "数据"，插入的行数。<br /><br /> 对于增量，为删除表在记帐后删除的行数。<br /><br /> 对于 Root，为 NULL。|  
|state|**smallint**|0-预创建<br /><br /> 1-正在构造<br /><br /> 2 - ACTIVE<br /><br /> 3-合并目标<br /><br /> 8-等待日志截断|  
|state_desc|**nvarchar(60)**|预创建-预分配多个检查点文件，以最小化或消除任何在执行事务时分配新文件的等待。 这些预创建文件的大小可能不同，具体取决于工作负荷的预计需求，但不包含任何数据。 这是具有 MEMORY_OPTIMIZED_DATA 文件组的数据库中的存储开销。<br /><br /> 正在构造-这些检查点文件正在构造中，这意味着它们将根据数据库生成的日志记录进行填充，并且尚未包含在检查点中。<br /><br /> 活动-包含先前已关闭检查点中插入/删除的行。 它们包含在数据库重新启动时应用事务日志的活动部分之前，区域读入内存的表的内容。 我们希望这些检查点文件的大小大约为内存优化表的内存中大小的2倍，假设合并操作与事务工作负荷保持一致。<br /><br /> 合并目标-合并操作的目标-这些检查点文件存储合并策略标识的源文件中的合并数据行。 合并已安装之后，MERGE TARGET 转换为 ACTIVE 状态。<br /><br /> 等待日志截断-一旦安装了合并，并且合并目标 CFP 是持久检查点的一部分，合并源检查点文件将转换为此状态。 具有内存优化表的数据库的操作正确性需要此状态的文件。  例如，用于从持久检查点恢复以便及时返回。|  
|lower_bound_tsn|**bigint**|文件中事务的下限;如果状态不在 (1，3) ，则为 null。|  
|upper_bound_tsn|**bigint**|文件中事务的上限;如果状态不在 (1，3) ，则为 null。|  
|begin_checkpoint_id|**bigint**|开始检查点的 ID。|  
|end_checkpoint_id|**bigint**|结束检查点的 ID。|  
|last_updated_checkpoint_id|**bigint**|更新此文件的最后一个检查点的 ID。|  
|encryption_status|**smallint**|0、1、2|  
|encryption_status_desc|**nvarchar(60)**|0 = 不加密><br /><br /> 1 => 用密钥1加密<br /><br /> 2 => 密钥2加密。 仅对活动文件有效。|  
  
##  <a name="sssql14"></a><a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 下表对的列进行了说明 `sys.dm_db_xtp_checkpoint_files` **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]** 。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|container_id|**int**|数据或差异文件所属的容器 ID（在 sys.database_files 中表示为类型为 FILESTREAM 的文件）。 与 [sys.database_files &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)中的 file_id 的联接。|  
|container_guid|**uniqueidentifier**|数据或差异文件所属的容器的 GUID。|  
|checkpoint_file_id|**GUID**|数据或差异文件的 ID。|  
|relative_file_path|**nvarchar(256)**|数据或差异文件的路径（相对于容器的位置）。|  
|file_type|**tinyint**|0 表示数据文件。<br /><br /> 1 表示差异文件。<br /><br /> 如果状态列设置为 7，则为 NULL。|  
|file_type_desc|**nvarchar(60)**|如果 "状态" 列设置为7，则为文件类型： "DATA_FILE"、"DELTA_FILE" 或 "NULL"。|  
|internal_storage_slot|**int**|内部存储数组中的文件的索引。 如果状态列不是 2 或 3，则为 NULL。|  
|checkpoint_pair_file_id|**uniqueidentifier**|对应的数据或差异文件。|  
|file_size_in_bytes|**bigint**|所用文件的大小。 如果状态列设置为 5、6 或 7，则为 NULL。|  
|file_size_used_in_bytes|**bigint**|所用文件的已用大小。 如果状态列设置为 5、6 或 7，则为 NULL。<br /><br /> 对于仍在填充的检查点文件对，此列将在下一个检查点的后面更新。|  
|inserted_row_count|**bigint**|数据文件中的行数。|  
|deleted_row_count|**bigint**|差异文件中删除的行数。|  
|drop_table_deleted_row_count|**bigint**|删除表影响的数据文件中的行数。 当状态列等于 1 时，应用于数据文件。<br /><br /> 显示从已删除表中删除的行计数。 在对已删除表中的行完成内存垃圾回收并且实施了检查点之后，汇总 drop_table_deleted_row_count 统计信息。 如果您在此列中反映删除表统计信息之前重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，则统计信息会在恢复过程中更新。 恢复过程不会从已删除表中加载行。 已删除表的统计信息会在加载阶段中进行汇总并在此列中进行报告（恢复完成时）。|  
|state|**int**|0-预创建<br /><br /> 1-正在构造<br /><br /> 2 - ACTIVE<br /><br /> 3-合并目标<br /><br /> 4-合并源<br /><br /> 5-备份/HA 必需的<br /><br /> 6-正在转换为 TOMBSTONE<br /><br /> 7-逻辑删除|  
|state_desc|**nvarchar(60)**|预创建-一组较小的数据和差异文件对（也称为检查点文件对 (Cfp) 将保留预分配，以最小化或消除任何在执行事务时分配新文件的等待。 针对数据文件的预分配 CFP 的完整大小是 128MB，而针对差异文件的预分配 CFP 的完整大小是 8 MB，但不包含任何数据。 CFP 的数目计算为逻辑处理器或计划程序的数目（每个核心一个，无最大值），最小值为 8。 这是具有内存优化表的数据库中的固定存储开销。<br /><br /> 在 "构造-Cfp" 下，存储自上一个检查点以来新插入和可能删除的数据行。<br /><br /> ACTIVE - 这些包含来自以前关闭的检查点的已插入和已删除行。 这些 CFP 包含数据库重新启动时在应用事务日志的活动部分前所需的所有已插入和已删除行。 这些 CFP 的大小大约是内存优化表在内存中的大小的 2 倍（假定合并操作是针对事务工作负荷的当前操作）。<br /><br /> 合并目标-CFP 存储合并策略标识的 CFP () 中的合并数据行。 合并已安装之后，MERGE TARGET 转换为 ACTIVE 状态。<br /><br /> 合并的源-一旦安装了 merge 操作，源 Cfp 将标记为 "合并源"。 请注意，合并策略计算器可能标识多个合并，但是一个 CFP 只能参与一个合并操作。<br /><br /> 备份/HA 必需-一旦安装了合并，并且合并目标 CFP 是持久检查点的一部分，则合并源 Cfp 会转换为此状态。 为保证具有内存优化表的数据库的运行正确性，需要处于此状态的 CFP。  例如，用于从持久检查点恢复以便及时返回。 在日志截断点移出其事务范围后，可以将 CFP 标为进行垃圾回收。<br /><br /> 转换为 TOMBSTONE 时，In-Memory OLTP 引擎不需要这些 Cfp，可以对它们进行垃圾回收。 此状态指示这些 CFP 在等待后台线程将它们转换为下一个状态（即 TOMBSTONE）。<br /><br /> TOMBSTONE-这些 Cfp 正在等待 filestream 垃圾回收器对其进行垃圾回收。  ([sp_filestream_force_garbage_collection &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md)) |  
|lower_bound_tsn|**bigint**|文件中包含事务的下限。 如果状态列不是 2、3 或 4，则为 Null。|  
|upper_bound_tsn|**bigint**|文件中包含事务的上限。 如果状态列不是 2、3 或 4，则为 Null。|  
|last_backup_page_count|**int**|上次备份时确定的逻辑页计数。 状态列设置为 2、3、4 或 5 时应用。 如果页计数未知，则为 NULL。|  
|delta_watermark_tsn|**int**|写入此差异文件的上个检查点的事务。 这是差异文件的水印。|  
|last_checkpoint_recovery_lsn|**nvarchar (23)**|仍需要文件的上个检查点的恢复日志序列号。|  
|tombstone_operation_lsn|**nvarchar (23)**|tombstone_operation_lsn 落后于日志截断日志序列号之后，文件将删除。|  
|logical_deletion_log_block_id|**bigint**|仅适用于状态 5。|  
  
## <a name="permissions"></a>权限  
 要求具有对服务器的 `VIEW DATABASE STATE` 权限。  
  
## <a name="use-cases"></a>用例  
 可以按如下所示估算 In-Memory OLTP 使用的存储：  
  
```  
-- total storage used by In-Memory OLTP  
SELECT SUM (file_size_in_bytes)/(1024*1024) as file_size_in_MB  
FROM sys.dm_db_xtp_checkpoint_files  
```  
  
  
若要查看按状态和文件类型分类的存储利用率，请运行以下查询：
  
```  
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(file_size_in_bytes) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  

  
## <a name="see-also"></a>另请参阅  
 [内存优化表动态管理视图 &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
