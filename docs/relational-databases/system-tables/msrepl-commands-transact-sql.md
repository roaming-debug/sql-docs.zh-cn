---
description: MSrepl_commands (Transact-SQL)
title: MSrepl_commands (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSrepl_commands
- MSrepl_commands_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_commands system table
ms.assetid: 53b9f9cd-9429-47a0-aba2-908fc60e7036
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9a64f06d234c5fe9bd037ad1a3aa1d142e688fd7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188741"
---
# <a name="msrepl_commands-transact-sql"></a>MSrepl_commands (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSrepl_commands** 表包含复制的命令行。 此表存储在分发数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**publisher_database_id**|**int**|发布服务器数据库的 ID。|  
|**xact_seqno**|**varbinary(16)**|事务序列号。|  
|type |**int**|命令类型。|  
|**article_id**|**int**|项目的 ID。|  
|**originator_id**|**int**|发起方的 ID。|  
|**command_id**|**int**|命令的 ID。|  
|**partial_command**|**bit**|指示这是否为部分命令。|  
|**command**|**varbinary (1024)**|命令值。|  
|**hashkey**|**int**|仅供内部使用。|  
|**originator_lsn**|**varbinary(16)**|标识原始发布中命令的 LSN。 这在对等事务复制中使用。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Transact-sql&#41;&#40;复制视图 ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_replcmds (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)  
  
  
