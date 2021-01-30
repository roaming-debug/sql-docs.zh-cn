---
description: MSmerge_history (Transact-SQL)
title: MSmerge_history (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_history_TSQL
- MSmerge_history
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_history system table
ms.assetid: 936195ad-ca07-41a8-a1a0-6699b6e63403
author: cawrites
ms.author: chadam
ms.openlocfilehash: 618a12dd664136e212bc6f31b9b59c1b87d413d7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203632"
---
# <a name="msmerge_history-transact-sql"></a>MSmerge_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSmerge_history** 表包含历史记录行，其中包含之前合并代理作业会话的结果的详细说明。 代理输出的每一行都在表中对应一行。 此表用在分发数据库和每个订阅数据库中。 在分发数据库中，该表包含使用分发服务器的所有合并发布和订阅的历史记录。 在每个订阅数据库中，该表包含将订阅服务器对齐进行了订阅的发布的历史记录。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|合并代理作业的 ID。|  
|**agent_id**|**int**|合并代理的 ID。|  
|**注释**|**nvarchar(255)**|消息文本。|  
|**error_id**|**int**|[MSrepl_errors](../../relational-databases/system-tables/msrepl-errors-transact-sql.md)系统表中的错误的 ID。|  
|**timestamp**|**timestamp**|该表的时间戳列。|  
|**updatable_row**|**bit**|如果可以覆盖历史记录行，则设置为 **1** 。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
