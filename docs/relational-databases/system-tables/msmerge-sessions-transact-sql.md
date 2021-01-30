---
description: MSmerge_sessions (Transact-SQL)
title: MSmerge_sessions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_sessions
- MSmerge_sessions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_sessions system table
ms.assetid: 09ada8fc-c148-4379-9524-7826b1b0216c
author: cawrites
ms.author: chadam
ms.openlocfilehash: af57f9ae760c09fa14bffe42ceba987734b7c5ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200290"
---
# <a name="msmerge_sessions-transact-sql"></a>MSmerge_sessions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSmerge_sessions** 表包含历史记录行，其中包含之前合并代理作业会话的结果。 每运行一次合并代理，都会在表中添加一个新行。 此表存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|合并代理作业会话的 ID。|  
|**agent_id**|**int**|合并代理的 ID。|  
|**start_time**|**datetime**|作业开始执行的时间。|  
|**end_time**|**datetime**|结束执行作业的时间。|  
|**duration**|**int**|此作业会话的累计持续时间（以秒为单位）。|  
|**delivery_time**|**int**|应用一批更改所花费的秒数。|  
|**upload_time**|**int**|向发布服务器上载更改所花费的秒数。|  
|**download_time**|**int**|将更改下载到订阅服务器所花费的秒数。|  
|**delivery_rate**|**float**|每秒传递的平均命令数。|  
|**time_remaining**|**int**|活动会话中估计的剩余秒数。|  
|**percent_complete**|**decimal**|活动的会话中已传递的全部更改的估计百分比。|  
|**upload_inserts**|**int**|发布服务器上应用的插入数。|  
|**upload_updates**|**int**|发布服务器上应用的更新数。|  
|**upload_deletes**|**int**|发布服务器上应用的删除数。|  
|**upload_conflicts**|**int**|在发布服务器上应用更改时发生的冲突数。|  
|**upload_conflicts_resolved**|**int**|在发布服务器上应用更改时发生并得到解决的冲突数。|  
|**upload_rows_retried**|**int**|上载到发布服务器并被重试的行数。|  
|**download_inserts**|**int**|订阅服务器上应用的插入数。|  
|**download_updates**|**int**|订阅服务器上应用的更新数。|  
|**download_deletes**|**int**|订阅服务器上应用的删除数。|  
|**download_conflicts**|**int**|在订阅服务器上应用更改时发生的冲突数。|  
|**download_conflicts_resolved**|**int**|在订阅服务器上应用更改时发生并得到解决的冲突数。|  
|**download_rows_retried**|**int**|下载到订阅服务器并被重试的行数。|  
|**schema_changes**|**int**|会话过程中应用的架构更改数。|  
|**metadata_rows_cleanedup**|**int**|会话过程中清除的元数据行数。|  
|**runstatus**|**int**|运行状态：<br /><br /> **1** = 启动。<br /><br /> **2** = 成功。<br /><br /> **3** = 正在进行。<br /><br /> **4** = 空闲。<br /><br /> **5** = 重试。<br /><br /> **6** = 失败。|  
|**estimated_upload_changes**|**int**|需要在发布服务器上应用的估计更改数。|  
|**estimated_download_changes**|**int**|需要在订阅服务器上应用的估计更改数。|  
|**connection_type**|**int**|上载过程中使用的连接：<br /><br /> **1** = 局域网 (LAN) 。<br /><br /> **2** = 拨号网络连接。<br /><br /> **3** = Web 同步。|  
|**timestamp**|**timestamp**|该表的时间戳列。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
