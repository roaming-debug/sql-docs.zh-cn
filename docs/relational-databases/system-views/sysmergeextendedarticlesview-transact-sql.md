---
description: sysmergeextendedarticlesview (Transact-SQL)
title: sysmergeextendedarticlesview (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergeextendedarticlesview
- sysmergeextendedarticlesview_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeextendedarticlesview view
ms.assetid: bd5c8414-5292-41fd-80aa-b55a50ced7e2
author: stevestein
ms.author: sstein
ms.openlocfilehash: fbbf21526feee6e3340ad06e51bb2b5d7010d5bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160310"
---
# <a name="sysmergeextendedarticlesview-transact-sql"></a>sysmergeextendedarticlesview (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **Sysmergeextendedarticlesview** 视图将公开项目信息。 此视图存储在发布服务器的发布数据库以及订阅服务器的订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|项目的名称。|  
|type |**tinyint**|指示项目类型，可以为下列类型之一：<br /><br /> **10** = 表。<br /><br /> **32** = 仅处理器架构。<br /><br /> **64** = 仅查看架构或仅索引视图架构。<br /><br /> **128** = 仅函数架构。<br /><br /> **160** = 仅限同义词架构。|  
|**objid**|**int**|发布服务器对象的标识符。|  
|**sync_objid**|**int**|表示同步数据集的视图标识符。|  
|**view_type**|**tinyint**|视图类型：<br /><br /> **0** = 不是视图;使用所有基对象。<br /><br /> **1** = 永久视图。<br /><br /> **2** = 临时视图。|  
|**artid**|**uniqueidentifier**|给定项目的唯一标识号。|  
|description|**nvarchar(255)**|项目的简要说明。|  
|**pre_creation_command**|**tinyint**|在订阅数据库中创建项目时将执行的默认操作：<br /><br /> **0** = 无-如果表已存在于订阅服务器上，则不执行任何操作。<br /><br /> **1** = 删除表，然后重新创建它。<br /><br /> **2** = 删除-基于子集筛选器中的 WHERE 子句发出删除。<br /><br /> **3** = 截断-与2相同，但不删除行。 不过，不要使用 WHERE 子句。|  
|**pubid**|**uniqueidentifier**|当前项目所属发布的 ID。|  
|**昵称**|**int**|项目标识的别名映射。|  
|**column_tracking**|**int**|指示是否为项目实现列跟踪。|  
|**status**|**tinyint**|指示项目的状态，可以为下列状态之一：<br /><br /> **1** = 未同步-用于发布表的初始处理脚本将在下一次运行快照代理时运行。<br /><br /> **2** = 活动-已运行发布表的初始处理脚本。<br /><br /> **5** = 要添加 New_inactive。<br /><br /> **6** = New_active。|  
|**conflict_table**|**sysname**|包含当前项目冲突记录的本地表的名称。 该表仅用于提供信息，其内容可以由自定义冲突解决例程修改或删除，或直接由系统管理员修改或删除。|  
|**creation_script**|**nvarchar(255)**|此项目的创建脚本。|  
|**conflict_script**|**nvarchar(255)**|此项目的冲突脚本。|  
|**article_resolver**|**nvarchar(255)**|此项目的自定义行级冲突解决程序。|  
|**ins_conflict_proc**|**sysname**|用于将冲突写入 **conflict_table** 的过程。|  
|**insert_proc**|**sysname**|由默认冲突解决程序用来在同步过程中插入行的过程。|  
|**update_proc**|**sysname**|由默认冲突解决程序用来在同步过程中更新行的过程。|  
|**select_proc**|**sysname**|自动生成的存储过程的名称，合并代理使用该存储过程来完成锁定并查找项目的行和列。|  
|**schema_option**|**二进制 (8)**|有关 *schema_option* 的支持值，请参阅 [sp_addmergearticle &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)。|  
|**destination_object**|**sysname**|在订阅服务器上创建的表的名称。|  
|**resolver_clsid**|**nvarchar(50)**|自定义冲突解决程序的 ID。|  
|**subset_filterclause**|**nvarchar(1000)**|此项目的筛选子句。|  
|**missing_col_count**|**int**|缺少的列数。|  
|**missing_cols**|**varbinary(128)**|缺少的列的位图。|  
|**表列**|**varbinary(128)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**resolver_info**|**nvarchar(255)**|存储，了解自定义冲突解决程序所需的其他信息。|  
|**view_sel_proc**|**nvarchar (290)**|存储过程的名称，合并代理使用此存储过程初始填充动态筛选发布中的项目，并且枚举在任何筛选发布中的更改的行。|  
|**gen_cur**|**int**|对项目基表所做的本地更改的生成数。|  
|**excluded_cols**|**varbinary(128)**|当项目发送到订阅服务器时，从项目中排除的列的位图。|  
|**excluded_col_count**|**int**|排除的列数。|  
|**vertical_partition**|**int**|指定是否对表项目启用列筛选。 **0** 指示没有垂直筛选并发布所有列。|  
|**identity_support**|**int**|指定是否启用自动标识范围处理。 **1** 表示启用标识范围处理， **0** 表示没有标识范围支持。|  
|**destination_owner**|**sysname**|目标对象的所有者的名称。|  
|**before_image_objid**|**int**|跟踪表对象 ID。 如果将发布配置为启用分区更改优化，则跟踪表将包含某些键列值。|  
|**before_view_objid**|**int**|视图表的对象 ID。 视图所在的表用于在删除或更新行之前跟踪行是否属于特定的订阅服务器。 仅当使用 *\@ keep_partition_changes* true 创建发布时才适用  =  。|  
|**verify_resolver_signature**|**int**|指定在合并复制中使用冲突解决程序之前是否验证数字签名：<br /><br /> **0** = 不验证签名。<br /><br /> **1** = 验证签名是否来自受信任的源。|  
|**allow_interactive_resolver**|**bit**|指定是否对项目启用交互式冲突解决程序。 **1** 指定对项目使用交互式冲突解决程序。|  
|**fast_multicol_updateproc**|**bit**|指定是否已启用合并代理来使用一条 UPDATE 语句在同一行的多个列中应用更改。<br /><br /> **0** = 对每个更改的列发出单独的更新。<br /><br /> **1** = 对 UPDATE 语句发出，导致在一个语句中对多个列进行更新。|  
|**check_permissions**|**int**|在合并代理将更改应用于发布服务器时将验证的表级权限的位图。 *check_permissions* 可以具有以下值之一：<br /><br /> **0x00** = 不检查权限。<br /><br /> **0x10** = 检查发布服务器上的权限，然后才能上载在订阅服务器上进行的插入。<br /><br /> **0x20** = 检查发布服务器上的权限，然后才能上载在订阅服务器上进行的更新。<br /><br /> **0x40** = 检查发布服务器上的权限，然后才能上载在订阅服务器上执行的删除操作。|  
|**maxversion_at_cleanup**|**int**|清除了元数据的最高版本。|  
|**processing_order**|**int**|指示合并发布中项目的处理顺序;其中，值为 **0** 表示项目无序，并按从低到高的值对项目进行处理。 如果两个项目具有相同值，将对其进行并发处理。 有关详细信息，请参阅[指定合并复制属性](../../relational-databases/replication/merge/specify-merge-replication-properties.md)。|  
|**published_in_tran_pub**|**bit**|指示合并发布中的项目也将在事务发布中发布。<br /><br /> **0** = 不在事务项目中发布项目。<br /><br /> **1** = 本文还在事务项目中发布。|  
|**upload_options**|**tinyiny**|定义是否可以在订阅服务器上进行更改或从订阅服务器上载更改，可以为下列值之一：<br /><br /> **0** = 订阅服务器上所做的更新没有任何限制;所有更改都将上载到发布服务器。<br /><br /> **1** = 允许在订阅服务器上进行更改，但不会将它们上载到发布服务器。<br /><br /> **2** = 不允许在订阅服务器上进行更改。|  
|**轻质**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**delete_proc**|**sysname**|由默认冲突解决程序用来在同步过程中删除行的过程。|  
|**before_upd_view_objid**|**int**|在更新之前表视图的 ID。|  
|**delete_tracking**|**bit**|指示是否复制删除。<br /><br /> **0** = 不复制删除。<br /><br /> **1** = 复制删除，这是合并复制的默认行为。<br /><br /> 如果 *delete_tracking* 的值为 **0**，则必须在发布服务器上手动删除订阅服务器上删除的行，并且必须在订阅服务器上手动删除在发布服务器上删除的行。<br /><br /> 注意：值为 **0** 会导致非收敛。|  
|**compensate_for_errors**|**bit**|指示在同步期间遇到错误时是否采取补救措施。<br /><br /> **0** = 禁用补偿操作。<br /><br /> **1** = 不能在订阅服务器或发布服务器上应用的更改始终会导致补偿操作来撤消这些更改，这是合并复制的默认行为。<br /><br /> 注意：值为 **0** 会导致非收敛。|  
|**pub_range**|**bigint**|发布服务器标识范围大小。|  
|**range**|**bigint**|将分配到调整中订阅服务器的连续标识值的大小。|  
|**threshold**|**int**|标识范围阈值百分比。|  
|**metadata_select_proc**|**sysname**|自动生成的存储过程的名称，该存储过程用于访问合并复制系统表中的元数据。|  
|**stream_blob_columns**|**bit**|指定当复制二进制大型对象列时是否使用数据流优化。 **1** 表示将尝试进行优化。|  
|**preserve_rowguidcol**|**bit**|指示复制是否使用现有 rowguid 列。 如果值为 **1** ，则表示使用现有的 ROWGUIDCOL 列。 **0** 表示复制添加了 ROWGUIDCOL 列。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Transact-sql&#41;&#40;复制视图 ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergearticle &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_changemergearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [sp_helpmergearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)   
 [sysmergearticles &#40;Transact-sql&#41;](../../relational-databases/system-tables/sysmergearticles-transact-sql.md)  
  
  
