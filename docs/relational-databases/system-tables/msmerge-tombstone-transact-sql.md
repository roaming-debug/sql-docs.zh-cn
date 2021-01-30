---
description: MSmerge_tombstone (Transact-SQL)
title: MSmerge_tombstone (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_tombstone_TSQL
- MSmerge_tombstone
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_tombstone system table
ms.assetid: 8b3fc7bf-729b-40f2-8a26-e7dfbe8ddb38
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6cd4fb0d58f0e400d23834090f31bf9043f547d5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205896"
---
# <a name="msmerge_tombstone-transact-sql"></a>MSmerge_tombstone (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSmerge_tombstone** 表包含有关已删除行的信息，并允许将删除传播到其他订阅服务器。 该表存储在发布数据库和订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**rowguid**|**uniqueidentifier**|行标识符。|  
|**tablenick**|**int**|表的别名。|  
|type |**tinyint**|删除的类型：<br /><br /> 1 = 用户删除。<br /><br /> 5 = 行不再属于筛选分区。<br /><br /> 6 = 系统删除。|  
|**衍生**|**varbinary (249)**|指示被删除的记录的版本，以及删除此记录时已知的更新。 出现两台订阅服务器在同一时间分别对一个行执行更新和删除操作的情况时，允许规则执行一致的冲突解决。|  
|**产生**|**int**|在删除行时赋值。 如果订阅服务器请求第 N 代，则只发送代 >= N 的逻辑删除。|  
|**logical_record_parent_rowguid**|**uniqueidentifier**|标识被删除的行所属的逻辑记录。|  
|**logical_record_lineage**|**Varbinary (501)**|订阅服务器别名和版本号对，用于维护此行所属的逻辑记录的删除历史记录。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
