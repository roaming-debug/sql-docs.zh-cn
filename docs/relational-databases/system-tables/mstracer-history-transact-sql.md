---
description: MStracer_history (Transact-SQL)
title: MStracer_history (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MStracer_history
- MStracer_history_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MStracer_history system table
ms.assetid: 97237a0c-d574-4b17-8a94-1a8730b31d98
author: cawrites
ms.author: chadam
ms.openlocfilehash: c419865662e3a88bf327035376ddc8a383dbd591
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211565"
---
# <a name="mstracer_history-transact-sql"></a>MStracer_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MStracer_history** 表维护已在订阅服务器上收到的所有跟踪令牌的记录。 此表存储在分发数据库中，复制过程使用此表来监视性能。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**parent_tracer_id**|**int**|唯一地标识跟踪令牌。|  
|**agent_id**|**int**|标识处理跟踪令牌记录的代理。|  
|**subscriber_commit**|**datetime**|向订阅服务器提交跟踪令牌记录的日期和时间。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
