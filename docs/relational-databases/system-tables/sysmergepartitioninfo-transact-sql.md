---
description: sysmergepartitioninfo (Transact-SQL)
title: sysmergepartitioninfo (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergepartitioninfo_TSQL
- sysmergepartitioninfo
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepartitioninfo system table
ms.assetid: 7429ad2c-dd33-4f7d-89cc-700e083af518
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8a94f14212f2603639c1d272da4d35e9bea9fb29
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100484"
---
# <a name="sysmergepartitioninfo-transact-sql"></a>sysmergepartitioninfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  提供有关每个项目的分区的信息。 本地数据库中定义的每个合并项目都在表中占一行。 该表存储在发布数据库和订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**artid**|**uniqueidentifier**|给定项目的唯一标识号。|  
|**pubid**|**uniqueidentifier**|此发布的唯一标识号；在添加发布时生成。|  
|**partition_view_id**|**int**|该表的分区视图 ID。 该视图显示项目中每行到它所属的不同分区 ID 的映射。|  
|**repl_view_id**|**int**|内容待定。|  
|**partition_deleted_view_rule**|**nvarchar(4000)**|在合并复制触发器内部使用的 SQL 语句，用于根据其旧列值检索每个已删除或已更新行的分区 ID。|  
|**partition_inserted_view_rule**|**nvarchar(4000)**|在合并复制触发器内部使用的 SQL 语句，用于根据其新列值检索每个已插入或已更新行的分区 ID。|  
|**membership_eval_proc_name**|**sysname**|计算 **MSmerge_contents** 中的行的当前分区 id 的过程的名称。|  
|column_list|**nvarchar(4000)**|在项目中复制的列的逗号分隔列表。|  
|**column_list_blob**|**nvarchar(4000)**|在项目中复制的列（包括二进制大型对象列）的逗号分隔列表。|  
|**expand_proc**|**sysname**|过程名称，该过程重新计算新插入的父行的所有子行的分区 ID，以及经历了分区更改或已被删除的父行的分区 ID。|  
|**logical_record_parent_nickname**|**int**|逻辑记录中指定项目的顶级父项目的别名。|  
|**logical_record_view**|**int**|一个视图，用于输出与各子项目 rowguid 相对应的顶级父项目 rowguid。|  
|**logical_record_deleted_view_rule**|**nvarchar(4000)**|与 **logical_record_view** 类似，不同之处在于，它在 update 和 delete 触发器的 "deleted" 表中显示子行。|  
|**logical_record_level_conflict_detection**|**bit**|指示应在逻辑记录级还是行级或列级检测冲突。<br /><br /> **0** = 使用行级或列级冲突检测。<br /><br /> **1** = 使用逻辑记录冲突检测，在这种情况下，发布服务器上的行更改与订阅服务器上的同一逻辑记录在单独的行中更改时，会被视为冲突。<br /><br /> 如果该值为 **1**，则只能使用逻辑记录级别的冲突解决方法。|  
|**logical_record_level_conflict_resolution**|**bit**|指示应在逻辑记录级别还是行或列级别解决冲突。<br /><br /> **0** = 使用行级或列级的解析。<br /><br /> **1** = 如果发生冲突，来自入选方的整个逻辑记录将覆盖失去一方的整个逻辑记录。<br /><br /> 值 **1** 既可用于逻辑记录级别的检测，也可用于行级或列级检测。|  
|**partition_options**|**tinyint**|定义项目数据的分区方式，当所有行只属于一个分区或只属于一个订阅时，这将可以实现性能优化。 *partition_options* 可以是下列值之一。<br /><br /> **0** = 项目的筛选是静态的，或者不为每个分区生成唯一的数据子集，即 "重叠" 分区。<br /><br /> **1** = 分区重叠，在订阅服务器上所做的 DML 更新不能更改行所属的分区。<br /><br /> **2** = 对项目的筛选将生成不重叠分区，但多个订阅服务器可以接收相同的分区。<br /><br /> **3** = 对项目的筛选将生成对每个订阅唯一的非重叠分区。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
