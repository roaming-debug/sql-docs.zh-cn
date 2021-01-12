---
description: restorehistory (Transact-SQL)
title: restorehistory (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- restorehistory
- restorehistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- restorehistory system table
ms.assetid: 9140ecc1-d912-4d76-ae70-e2a857da6d44
author: cawrites
ms.author: chadam
ms.openlocfilehash: c4d164bdceaea91650a0d1bd8226133b22692912
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096154"
---
# <a name="restorehistory-transact-sql"></a>restorehistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  每个还原操作在表中占一行。 该表存储在 **msdb** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**restore_history_id**|**int**|标识每个还原操作的唯一标识号。 标识，主键。|  
|**restore_date**|**datetime**|还原操作开始的日期和时间。 可以为 NULL。|  
|**destination_database_name**|**nvarchar(128)**|还原操作的目标数据库的名称。 可以为 NULL。|  
|user_name|**nvarchar(128)**|执行还原操作的用户的名称。 可以为 NULL。|  
|**backup_set_id**|**int**|标识被还原的备份集的唯一标识号。 引用 **backupset (backup_set_id)**。|  
|**restore_type**|**char(1)**|还原操作的类型：<br /><br /> D = 数据库<br /><br /> F = 文件<br /><br /> G = 文件组<br /><br /> I = 差异<br /><br /> L = 日志<br /><br /> V = 仅验证<br /><br /> 可以为 NULL。|  
|**replace**|**bit**|指示还原操作是否指定了 REPLACE 选项：<br /><br /> 1 = 已指定<br /><br /> 0 = 未指定<br /><br /> 可以为 NULL。<br /><br /> 将数据库恢复到数据库快照时，0 是唯一的选项。|  
|**recovery**|**bit**|指示还原操作指定的是 RECOVERY 选项还是 NORECOVERY 选项：<br /><br /> 1 = RECOVERY<br /><br /> 可以为 NULL。<br /><br /> 数据库恢复到数据库快照时，只有1个选项。<br /><br /> 0 = NORECOVERY|  
|**restart**|**bit**|指示还原操作是否指定了 RESTART 选项：<br /><br /> 1 = 已指定<br /><br /> 0 = 未指定<br /><br /> 可以为 NULL。<br /><br /> 将数据库恢复到数据库快照时，0 是唯一的选项。|  
|**stop_at**|**datetime**|数据库要恢复到的时间点。 可以为 NULL。|  
|**device_count**|**tinyint**|还原操作涉及的设备数。 此数目可以小于备份使用的介质簇数。 可以为 NULL。<br /><br /> 将数据库恢复到数据库快照时，此数目将始终为 1。|  
|**stop_at_mark_name**|**nvarchar(128)**|指示恢复到包含命名标记的事务。 可以为 NULL。<br /><br /> 将数据库恢复到数据库快照时，此值为 NULL。|  
|**stop_before**|**bit**|指示恢复中是否包含命名标记的事务：<br /><br /> 0 = 在标记的事务前停止恢复。<br /><br /> 1 = 恢复包括标记的事务。<br /><br /> 可以为 NULL。<br /><br /> 将数据库恢复到数据库快照时，此值为 NULL。|  
  
## <a name="remarks"></a>备注  
 若要减少此表以及其他备份和历史记录表中的行数，请执行 [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) 存储过程。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;备份和还原表 ](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [restorefile &#40;Transact-sql&#41;](../../relational-databases/system-tables/restorefile-transact-sql.md)   
 [restorefilegroup &#40;Transact-sql&#41;](../../relational-databases/system-tables/restorefilegroup-transact-sql.md)   
 [系统表 (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
