---
description: MSdynamicsnapshotjobs (Transact-SQL)
title: MSdynamicsnapshotjobs (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSdynamicsnapshotjobs_TSQL
- MSdynamicsnapshotjobs
dev_langs:
- TSQL
helpviewer_keywords:
- MSdynamicsnapshotjobs system table
ms.assetid: 4f36a325-0e3c-46c4-aeeb-416346cce0bc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8a03bec5ad0e9835ca60099ed9d11583477d27fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212337"
---
# <a name="msdynamicsnapshotjobs-transact-sql"></a>MSdynamicsnapshotjobs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSdynamicsnapshotjobs** 表跟踪应用于生成筛选数据快照的参数化行筛选器信息。 该表存储在发布数据库和订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**id**|**int**|筛选数据快照作业的 ID。|  
|name|**sysname**|筛选的数据快照作业的名称。|  
|**pubid**|**uniqueidentifier**|此发布的唯一标识号。|  
|**job_id**|**uniqueidentifier**|分发服务器中 SQL Server 代理作业的 ID。|  
|**agent_id**|**int**|SQL Server 代理的 ID。|  
|**dynamic_filter_login**|**sysname**|用于计算为发布定义的参数化行筛选器中的 [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) 函数的值。|  
|**dynamic_filter_hostname**|**sysname**|用于计算为发布定义的参数化行筛选器中的 [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) 函数的值。|  
|**dynamic_snapshot_location**|**nvarchar(255)**|如果使用筛选数据快照则从中读取快照文件的文件夹的路径。|  
|**partition_id**|**int**|作业所属数据分区的 ID。|  
  
## <a name="see-also"></a>另请参阅  
 [复制表 (Transact-SQL)](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
