---
description: sysmergepartitioninfoview (Transact-SQL)
title: sysmergepartitioninfoview (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergepartitioninfoview
- sysmergepartitioninfoview_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepartitioninfoview view
ms.assetid: 714e2935-1bc7-4901-aea2-64b1bbda03d6
author: stevestein
ms.author: sstein
ms.openlocfilehash: f9a5063691cb0d8ed77fdc19ce3bba1adebf8858
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181577"
---
# <a name="sysmergepartitioninfoview-transact-sql"></a>sysmergepartitioninfoview (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **Sysmergepartitioninfoview** 视图显示表项目的分区信息。 此视图存储在发布服务器的发布数据库以及订阅服务器的订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|项目的名称。|  
|type |**tinyint**|指示项目类型，可以为下列类型之一：<br /><br /> **0x0a** = Table。<br /><br /> **0x20** = 仅过程架构。<br /><br /> **0x40** = 仅查看架构或仅索引视图架构。<br /><br /> **0x80** = 仅函数架构。|  
|**objid**|**int**|已发布对象的标识符。|  
|**sync_objid**|**int**|表示同步数据集的视图的对象 ID。|  
|**view_type**|**tinyint**|视图类型：<br /><br /> **0** = 不是视图;使用所有基对象。<br /><br /> **1** = 永久视图。<br /><br /> **2** = 临时视图。|  
|**artid**|**uniqueidentifier**|给定项目的唯一标识号。|  
|description|**nvarchar(255)**|项目的简要说明。|  
|**pre_creation_command**|**tinyint**|在订阅数据库中创建项目时将执行的默认操作：<br /><br /> **0** = 无-如果表已存在于订阅服务器上，则不执行任何操作。<br /><br /> **1** = 删除表后再重新创建它。<br /><br /> **2** = 删除-基于子集筛选器中的 WHERE 子句发出删除。<br /><br /> **3** = 截断-与2相同，但不删除行。 不过，不要使用 WHERE 子句。|  
|**pubid**|**uniqueidentifier**|当前项目所属发布的 ID。|  
|**昵称**|**int**|项目标识的别名映射。|  
|**column_tracking**|**int**|指示是否为项目执行列跟踪。|  
|**status**|**tinyint**|指示项目的状态，可以为下列状态之一：<br /><br /> **1** = 未同步-用于发布表的初始处理脚本将在下一次运行快照代理时运行。<br /><br /> **2** = 活动-已运行发布表的初始处理脚本。|  
|**conflict_table**|**sysname**|包含当前项目冲突记录的本地表的名称。 该表仅用于提供信息，其内容可以由自定义冲突解决例程修改或删除，或直接由系统管理员修改或删除。|  
|**creation_script**|**nvarchar(255)**|此项目的创建脚本。|  
|**conflict_script**|**nvarchar(255)**|此项目的冲突脚本。|  
|**article_resolver**|**nvarchar(255)**|该项目的冲突解决程序。|  
|**ins_conflict_proc**|**sysname**|用于向冲突表写入冲突信息的过程。|  
|**insert_proc**|**sysname**|用于在同步过程中插入行的过程。|  
|**update_proc**|**sysname**|用于在同步过程中更新行的过程。|  
|**select_proc**|**sysname**|自动生成的存储过程的名称，合并代理使用该存储过程完成锁定并查找项目的行和列。|  
|**metadata_select_proc**|**sysname**|自动生成的存储过程的名称，该存储过程用于访问合并复制系统表中的元数据。|  
|**delete_proc**|**sysname**|用于在同步过程中删除行的过程。|  
|**schema_option**|**二进制 (8)**|给定项目的架构生成选项的位图。 有关支持的 *schema_option* 值的信息，请参阅 [sp_addmergearticle &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)。|  
|**destination_object**|**sysname**|在订阅服务器上创建的表的名称。|  
|**destination_owner**|**sysname**|目标对象的所有者的名称。|  
|**resolver_clsid**|**nvarchar(50)**|自定义冲突解决程序的 ID。 对于业务逻辑处理程序，该值为 NULL。|  
|**subset_filterclause**|**nvarchar(1000)**|此项目的筛选子句。|  
|**missing_col_count**|**int**|项目中缺少的已发布列数。|  
|**missing_cols**|**varbinary(128)**|用于说明项目中的缺少列的位图。|  
|**excluded_cols**|**varbinary(128)**|已从项目中排除的列的位图。|  
|**excluded_col_count**|**int**|从项目中排除的列数。|  
|**表列**|**varbinary(128)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**deleted_cols**|**varbinary(128)**|用于说明从项目中删除的列的位图。|  
|**resolver_info**|**nvarchar(255)**|存储自定义冲突解决程序所需的其他信息。|  
|**view_sel_proc**|**nvarchar (290)**|存储过程的名称，合并代理使用该存储过程初始填充动态筛选发布中的项目，并枚举在任何筛选发布中的已更改行。|  
|**gen_cur**|**bigint**|生成项目基表的本地更改数。|  
|**vertical_partition**|**int**|指定是否对表项目启用列筛选。 **0** 指示没有垂直筛选并发布所有列。|  
|**identity_support**|**int**|指定是否启用自动标识范围处理。 **1** 表示启用标识范围处理， **0** 表示没有标识范围支持。|  
|**before_image_objid**|**int**|跟踪表对象 ID。 当已对发布启用分区更改优化时，跟踪表将包含某些键列值。|  
|**before_view_objid**|**int**|视图表的对象 ID。 视图所在的表用于在删除或更新行之前跟踪行是否属于特定的订阅服务器。 仅当对发布启用了分区更改优化时才适用。|  
|**verify_resolver_signature**|**int**|指定在合并复制中使用冲突解决程序之前是否验证数字签名：<br /><br /> **0** = 不验证签名。<br /><br /> **1** = 验证签名是否来自受信任的源。|  
|**allow_interactive_resolver**|**bit**|指定是否对项目启用交互式冲突解决程序。 **1** 表示可对项目使用交互式冲突解决程序。|  
|**fast_multicol_updateproc**|**bit**|指定是否已启用合并代理来使用一条 UPDATE 语句在同一行的多个列中应用更改。<br /><br /> **0** = 对每个更改的列发出单独的更新。<br /><br /> **1** = 对 UPDATE 语句发出，导致在一个语句中对多个列进行更新。|  
|**check_permissions**|**int**|在合并代理将更改应用于发布服务器时将验证的表级权限的位图。 *check_permissions* 可以具有以下值之一：<br /><br /> **0x00** = 不检查权限。<br /><br /> **0x10** = 在订阅服务器上执行插入之前检查发布服务器上的权限。<br /><br /> **0x20** = 检查发布服务器上的权限，然后才能上载在订阅服务器上进行的更新。<br /><br /> **0x40** = 检查发布服务器上的权限，然后才能上载在订阅服务器上执行的删除操作。|  
|**maxversion_at_cleanup**|**int**|合并代理下次运行时清除的最大生成。|  
|**processing_order**|**int**|指示合并发布中项目的处理顺序;其中，值为 **0** 指示项目未排序，并按从低到高的值对项目进行处理。 如果两个项目具有相同值，将对其进行并发处理。 有关详细信息，请参阅[指定合并复制属性](../../relational-databases/replication/merge/specify-merge-replication-properties.md)。|  
|**upload_options**|**tinyint**|定义是否可以在订阅服务器上进行更改或从订阅服务器上载更改，可以为下列值之一：<br /><br /> **0** = 订阅服务器上所做的更新没有任何限制;所有更改都将上载到发布服务器。<br /><br /> **1** = 允许在订阅服务器上进行更改，但不会将它们上载到发布服务器。<br /><br /> **2** = 不允许在订阅服务器上进行更改。|  
|**published_in_tran_pub**|**bit**|指示合并发布中的项目也将在事务发布中发布。<br /><br /> **0** = 项目未在事务项目中发布。<br /><br /> **1** = 本文还在事务项目中发布。|  
|**轻质**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**procname_postfix**|**nchar(32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**well_partitioned_lightweight**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**before_upd_view_objid**|**int**|更新前表视图的 ID。|  
|**delete_tracking**|**bit**|指示是否复制删除。<br /><br /> **0** = 不复制删除。<br /><br /> **1** = 复制删除，这是合并复制的默认行为。<br /><br /> 如果 *delete_tracking* 的值为 **0**，则必须在发布服务器上手动删除订阅服务器上删除的行，并且必须在订阅服务器上手动删除在发布服务器上删除的行。<br /><br /> 注意：值为 **0** 会导致非收敛。|  
|**compensate_for_errors**|**bit**|指示在同步期间遇到错误时是否采取补救措施。<br /><br /> **0** = 禁用补偿操作。<br /><br /> **1** = 不能在订阅服务器或发布服务器上应用的更改始终会导致补偿操作来撤消这些更改，这是合并复制的默认行为。<br /><br /> 注意：值为 **0** 会导致非收敛。|  
|**pub_range**|**bigint**|发布服务器标识范围大小。|  
|**range**|**bigint**|将分配到调整中订阅服务器的连续标识值的大小。|  
|**threshold**|**int**|标识范围阈值百分比。|  
|**stream_blob_columns**|**bit**|指示是否使用针对二进制大型对象列的流式优化。 **1** 表示尝试进行优化。|  
|**preserve_rowguidcol**|**bit**|指示复制是否使用现有 rowguid 列。 如果值为 **1** ，则表示使用现有的 ROWGUIDCOL 列。 **0** 表示复制添加了 ROWGUIDCOL 列。|  
|**partition_view_id**|**int**|标识定义订阅服务器分区的视图。|  
|**repl_view_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**partition_deleted_view_rule**|**sysname**|在合并复制触发器内部使用的语句，用于根据其旧列值检索每个已删除或已更新行的分区 ID。|  
|**partition_inserted_view_rule**|**Sysname**|在合并复制触发器内部使用的语句，用于根据其新列值检索每个已插入或已更新行的分区 ID。|  
|**membership_eval_proc_name**|**sysname**|计算 [MSmerge_contents &#40;transact-sql&#41;](../../relational-databases/system-tables/msmerge-contents-transact-sql.md)中的行的当前分区 id 的过程名称。|  
|column_list|**sysname**|在项目中发布的列的逗号分隔列表。|  
|**column_list_blob**|**sysname**|在项目中发布的列（包括二进制大型对象列）的逗号分隔列表。|  
|**expand_proc**|**sysname**|为新插入的父行的所有子行以及已完成分区更改或已删除的父行重新评估分区 Id 的过程的名称。|  
|**logical_record_parent_nickname**|**int**|逻辑记录中指定项目的顶级父项目的别名。|  
|**logical_record_view**|**int**|一个视图，用于输出与各子项目 rowguid 相对应的顶级父项目 rowguid。|  
|**logical_record_deleted_view_rule**|**sysname**|与 **logical_record_view** 类似，不同之处在于，它在 update 和 delete 触发器的 "deleted" 表中显示子行。|  
|**logical_record_level_conflict_detection**|**bit**|指示应在逻辑记录级还是行级或列级检测冲突。<br /><br /> **0** = 使用行级或列级冲突检测。<br /><br /> **1** = 使用逻辑记录冲突检测，在这种情况下，发布服务器上的行更改与订阅服务器上的同一逻辑记录在单独的行中更改时，会被视为冲突。<br /><br /> 当该值为 1 时，只能使用逻辑记录级别的冲突解决。|  
|**logical_record_level_conflict_resolution**|**bit**|指示是否应在逻辑记录级或行级或列级解决冲突。<br /><br /> **0** = 使用行级或列级的解析。<br /><br /> **1** = 如果发生冲突，来自入选方的整个逻辑记录将覆盖失去一方的整个逻辑记录。<br /><br /> 值 1 既可用于逻辑记录级别的检测，也可用于行或列级别的检测。|  
|**partition_options**|**tinyint**|定义项目数据的分区方式，当所有行只属于一个分区或只属于一个订阅时，这将可以实现性能优化。 *Partition_options* 可以是下列值之一。<br /><br /> **0** = 项目的筛选是静态的，或者不为每个分区生成唯一的数据子集，即 "重叠" 分区。<br /><br /> **1** = 分区重叠，在订阅服务器上所做的 DML 更新不能更改行所属的分区。<br /><br /> **2** = 对项目的筛选将生成不重叠分区，但多个订阅服务器可以接收相同的分区。<br /><br /> **3** = 对项目的筛选将生成对每个订阅唯一的非重叠分区。|  
|name|**sysname**|分区的名称。|  
  
## <a name="see-also"></a>另请参阅  
 [使用参数化筛选器为合并发布管理分区](../../relational-databases/replication/publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)   
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Transact-sql&#41;&#40;复制视图 ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergepartition &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md)   
 [sp_helpmergepartition &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepartition-transact-sql.md)  
  
  
