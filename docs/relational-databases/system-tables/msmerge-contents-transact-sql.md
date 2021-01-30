---
description: MSmerge_contents (Transact-SQL)
title: MSmerge_contents (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_contents
- MSmerge_contents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_contents system table
ms.assetid: 8d68a61a-683f-4b20-92f9-c0a8d9ba0ad1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 78d7fb9732013970956554e932b3d4804d461df9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205055"
---
# <a name="msmerge_contents-transact-sql"></a>MSmerge_contents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  自发布以来，当前数据库中修改的每行在 **MSmerge_contents** 表中各占一行。 合并过程使用此表来确定已更改的行。 该表存储在发布数据库和订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|已发布表的别名。|  
|**rowguid**|**uniqueidentifier**|给定行的行标识符。|  
|**产生**|**bigint**|由 **tablenick** 和 **rowguid** 标识的行的生成。|  
|**partchangegen**|**bigint**|与上次数据更改相关联的代（不论行是否属于已筛选发布，这些更改都可能已经发生）。|  
|**衍生**|**varbinary (501)**|用于维护此行的更改历史记录的订阅服务器的别名和版本号对。|  
|**colvl**|**varbinary (7489)**|列版本信息。|  
|**'**|**uniqueidentifier**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**logical_record_parent_rowguid**|**uniqueidentifier**|通过 **rowguid**) 为逻辑记录中的每个对应子行标识 **MSmerge_contents** (中的顶级父行。|  
|**logical_record_lineage**|**varbinary (501)**|用于维护逻辑记录中的顶级父行的更改的历史记录的订阅服务器的别名、版本号对。 对于逻辑记录中的所有子行，此值为 NULL。|  
|**logical_relation_change_gen**|**bigint**|与导致逻辑记录重新调整的上次更改相关联的代（即将现有行移入或移出逻辑记录）。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
