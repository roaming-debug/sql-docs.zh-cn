---
description: restorefilegroup (Transact-SQL)
title: restorefilegroup (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- restorefilegroup_TSQL
- restorefilegroup
dev_langs:
- TSQL
helpviewer_keywords:
- filegroups [SQL Server], restorefilegroup system table
- restorefilegroup system table
ms.assetid: 3aa15c55-6b72-4f76-97d7-bd88391d105c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 97ecbc7998f3a0d7be6ca38c2dbb09d856893290
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209235"
---
# <a name="restorefilegroup-transact-sql"></a>restorefilegroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  每个已还原的文件组在表中占一行。 该表存储在 **msdb** 数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**restore_history_id**|**int**|标识相应还原操作的唯一标识号。 引用 **restorehistory (restore_history_id)**。|  
|filegroup_name|**nvarchar(128)**|被还原的文件组的名称 可以为 NULL。<br /><br /> 将数据库恢复到数据库快照时，此值的填充方式与完全还原时的填充方式相同。|  
  
## <a name="remarks"></a>备注  
 若要减少此表以及其他备份和历史记录表中的行数，请执行 [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) 存储过程。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;备份和还原表 ](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [restorefile &#40;Transact-sql&#41;](../../relational-databases/system-tables/restorefile-transact-sql.md)   
 [restorehistory &#40;Transact-sql&#41;](../../relational-databases/system-tables/restorehistory-transact-sql.md)   
 [系统表 (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
