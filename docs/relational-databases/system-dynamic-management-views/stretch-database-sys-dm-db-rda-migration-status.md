---
title: sys.dm_db_rda_migration_status (Transact-sql) |Microsoft Docs
description: 了解对于 SQL Server 的本地实例上每个启用 Stretch 的表中的每批已迁移数据，sys.dm_db_rda_migration_status 如何包含一行。
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.dm_db_rda_migration_status
- sys.dm_db_rda_migration_status_TSQL
- dm_db_rda_migration_status
- dm_db_rda_migration_status_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_rda_migration_status dynamic management view
ms.assetid: faf3901c-a0e0-4e0c-8b1b-86d9f15f34dd
author: pmasl
ms.author: pelopes
ms.openlocfilehash: fc0253df9c1f9c593ef2169f03cb3ff98382cdad
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236839"
---
# <a name="stretch-database---sysdm_db_rda_migration_status"></a>Stretch Database-sys.dm_db_rda_migration_status
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  本地实例上每个启用了 Stretch 的表中的已迁移数据的每批占一行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 批处理由其开始时间和结束时间标识。  
  
 **sys.dm_db_rda_migration_status** 的作用域限定为当前数据库上下文。 请确保在要查看其迁移状态的 Stretch enable 表的数据库上下文中输入。  
  
 在中 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] ， **sys.dm_db_rda_migration_status** 的输出限制为200行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|table_id|**int**|从中迁移行的表的 ID。|  
|database_id|**int**|从中迁移行的数据库的 ID。|  
|**migrated_rows**|**bigint**|在此批处理中迁移的行数。|  
|**start_time_utc**|**datetime**|批处理开始时的 UTC 时间。|  
|**end_time_utc**|**datetime**|批处理完成时的 UTC 时间。|  
|error_number|**int**|如果批失败，则出现错误的错误号;否则为 null。|  
|**error_severity**|**int**|如果批失败，则为发生的错误的严重级别;否则为 null。|  
|**error_state**|**int**|如果批失败，则为发生的错误的状态;否则为 null。<br /><br /> **Error_state** 指示发生错误的条件或位置。|  
  
## <a name="see-also"></a>另请参阅  
 [Stretch 数据库](../../sql-server/stretch-database/stretch-database.md)  
  
  
