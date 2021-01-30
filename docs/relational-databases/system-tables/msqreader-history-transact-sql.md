---
description: MSqreader_history (Transact-SQL)
title: MSqreader_history (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSqreader_history
- MSqreader_history_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSqreader_history system table
ms.assetid: c5c91d39-513c-4a77-870b-c8ef74a1cd6b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7c158b9c035f14f7904a828c7a3628494096ddb2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182605"
---
# <a name="msqreader_history-transact-sql"></a>MSqreader_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSqreader_history** 表包含与本地分发服务器关联的队列读取器代理的历史记录行。 此表存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**agent_id**|**int**|队列读取器代理的 ID。|  
|**publication_id**|**int**|发布 ID。|  
|**runstatus**|**int**|代理的运行状态：<br /><br /> **1** = 启动。<br /><br /> **2** = 成功。<br /><br /> **3** = 正在进行。<br /><br /> **4** = 空闲。<br /><br /> **5** = 重试。<br /><br /> **6** = 失败。|  
|**start_time**|**datetime**|代理会话开始的日期和时间。|  
|**time**|**datetime**|最后记录消息的日期和时间。|  
|**duration**|**int**|记录的会话活动的占用时间（秒）。|  
|**注释**|**nvarchar(255)**|说明文本。|  
|**transaction_id**|**nvarchar(40)**|存储在消息中的事务 ID（如果适用）。|  
|**transaction_status**|**int**|事务的状态。|  
|**transactions_processed**|**int**|会话中处理的事务的累计数量。|  
|**commands_processed**|**int**|会话中处理的命令的累计数量。|  
|**delivery_rate**|**float(53)**|每秒传送的平均命令数。|  
|**transaction_rate**|**float(53)**|事务处理速率。|  
|**订阅服务器**|**sysname**|订阅服务器的名称。|  
|**subscriberdb**|**sysname**|订阅数据库的名称。|  
|**error_id**|**int**|如果不是零，则数字表示 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误消息。|  
|**timestamp**|**timestamp**|表的时间戳列。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
