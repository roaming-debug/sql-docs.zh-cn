---
description: MSrepl_errors (Transact-SQL)
title: MSrepl_errors (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSrepl_errors
- MSrepl_errors_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_errors system table
ms.assetid: c6e023c1-2c32-4269-8d76-e442ea309e4b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0899b22bbd4c31676dd5520eaa8d6dfe4a0830a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194355"
---
# <a name="msrepl_errors-transact-sql"></a>MSrepl_errors (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSrepl_errors** 表包含具有扩展分发代理和合并代理失败信息的行。 此表存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**id**|**int**|错误的 ID。|  
|**time**|**datetime**|出现错误的时间。|  
|**error_type_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**source_type_id**|**int**|错误源类型 ID。|  
|**source_name**|**nvarchar (100)**|错误源名称。|  
|**error_code**|**sysname**|错误代码。|  
|**error_text**|**ntext**|错误消息。|  
|**xact_seqno**|**varbinary(16)**|执行失败的批次的起始事务日志序列号。 仅由分发代理使用，是失败的执行批处理中第一个事务的日志序列号。|  
|**command_id**|**int**|执行失败的批次的命令 ID。 仅由分发代理使用，是执行失败的批次中的第一个命令的 ID。|  
|**session_id**|**int**|出现错误的代理会话 ID。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
