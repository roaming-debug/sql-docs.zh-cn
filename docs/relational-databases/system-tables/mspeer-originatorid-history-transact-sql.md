---
description: MSpeer_originatorid_history (Transact-SQL)
title: MSpeer_originatorid_history (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSpeer_originatorid_history_TSQL
- MSpeer_originatorid_history
dev_langs:
- TSQL
helpviewer_keywords:
- MSpeer_originatorid_history
ms.assetid: c1f53d0f-4080-43ff-be38-2b10395c68c9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0080e3af9f2072e4a86da350a80c633117550301
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200311"
---
# <a name="mspeer_originatorid_history-transact-sql"></a>MSpeer_originatorid_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  拓扑中定义的每个发起方 ID 在表中占一行。 这包括不再处于活动状态的节点的 ID。 为冲突检测配置新节点时，将使用该表来确保指定的发起方 ID 尚未使用。 该表存储在发布数据库中。 有关冲突检测的详细信息，请参阅 [对等复制中的冲突检测](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|originator_publication|**sysname**|指定了发起方 ID 的发布。|  
|originator_id|**int**|标识拓扑中的每个节点以进行冲突检测的编号。|  
|originator_node|**sysname**|指定了发起方 ID 的服务器实例。|  
|originator_db|**sysname**|指定了发起方 ID 的发布数据库。|  
|originator_db_version|**int**|标识发起方数据库的版本号。|  
|originator_version|**int**|指定发布服务器的版本号。|  
|inserted_date|**datetime**|将发起方 ID 行插入此表中的日期和时间。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
