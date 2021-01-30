---
description: MSsync_states (Transact-SQL)
title: MSsync_states (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsync_states
- MSsync_states_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsync_states system table
ms.assetid: b25e17e1-7718-432e-a442-c4946741d474
author: cawrites
ms.author: chadam
ms.openlocfilehash: 338700f614cc853b2b275d135e7ef466825e8019
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211576"
---
# <a name="mssync_states-transact-sql"></a>MSsync_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSsync_states** 表将跟踪哪个发布仍处于并发快照模式。 此表存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|发布服务器 ID。|  
|**publisher_db**|**sysname**|发布数据库的名称。|  
|**publication_id**|**int**|发布 ID。|  
  
## <a name="see-also"></a>另请参阅  
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [&#40;Transact-sql&#41;的 Integration Services 表 ](../../relational-databases/system-tables/integration-services-tables-transact-sql.md)   
 [&#40;Transact-sql&#41;备份和还原表 ](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [日志传送表 (Transact-SQL)](../../relational-databases/system-tables/log-shipping-tables-transact-sql.md)  
  
  
