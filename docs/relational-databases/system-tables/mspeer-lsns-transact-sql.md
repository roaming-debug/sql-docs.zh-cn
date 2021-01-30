---
description: MSpeer_lsns (Transact-SQL)
title: MSpeer_lsns (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSpeer_lsns
- MSpeer_lsns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_lsns system table
ms.assetid: 0ba33907-601b-4c3d-8099-2663f680a161
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8316d023cb030e2eba09aff82d4e941cf551d1a5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205853"
---
# <a name="mspeer_lsns-transact-sql"></a>MSpeer_lsns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **Mspeer_lsns** 表用于将每个事务映射到对等复制拓扑中的订阅。 此表存储在采用对等复制拓扑的每个发布数据库中，以及对等发布的所有订阅服务器的订阅数据库中。 有关这种类型的事务复制拓扑的详细信息，请参阅 [对等事务复制](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)。 该表存储在发布数据库中。  
  
## <a name="definition"></a>定义  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**id**|**int**|标识对等 LSN。|  
|**last_updated**|**datetime**|上次更新行的 **日期时间** 。|  
|**发出**|**sysname**|发起事务的发布服务器的名称。|  
|**originator_db**|**sysname**|从中产生事务的数据库的名称。|  
|**originator_publication**|**sysname**|发起事务的发布的名称。|  
|**originator_publication_id**|**int**|发起事务的发布的标识符。|  
|**originator_db_version**|**int**|标识发起方数据库的版本号。|  
|**originator_lsn**|**int**|标识发起发布中的 LSN。|  
|**originator_version**|**int**|指定发布服务器的版本号。|  
|**originator_id**|**smallint**|标识拓扑中的每个节点以进行冲突检测。 有关详细信息，请参阅 [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
