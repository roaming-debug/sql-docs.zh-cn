---
description: sys.database_recovery_status (Transact-SQL)
title: sys.database_recovery_status (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- database_recovery_status_TSQL
- database_recovery_status
- sys.database_recovery_status
- sys.database_recovery_status_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_recovery_status catalog view
ms.assetid: 46fab234-1542-49be-8edf-aa101e728acf
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5e9c968ff33c47f81eb84b2f5cd73e2483b7af56
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098034"
---
# <a name="sysdatabase_recovery_status-transact-sql"></a>sys.database_recovery_status (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  每个数据库占一行。 如果数据库未打开，则 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]尝试启动它。  
  
 若要查看 **master** 或 **tempdb** 以外的数据库行，必须满足以下条件之一：  
  
-   是数据库的所有者。  
  
-   拥有 ALTER ANY DATABASE 或 VIEW ANY DATABASE 服务器级别的权限。  
  
-   在 **master** 数据库中具有 CREATE DATABASE 权限。    
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**int**|数据库 ID（在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中唯一）。|  
|**database_guid**|**uniqueidentifier**|用于将数据库的所有数据库文件关联在一起。 所有文件必须在其标题页中包含此 GUID，才能使数据库按预期方式启动。 仅有一个数据库拥有此 GUID，但可以通过复制和附加数据库来创建副本。 RESTORE 总是在您还原某个尚不存在的数据库时生成一个新的 GUID。<br /><br /> NULL 表示数据库脱机，或将不启动数据库。|  
|**family_guid**|**uniqueidentifier**|数据库“备份家族”的标识符，用于检测匹配的还原状态。<br /><br /> NULL = 数据库处于脱机状态，或者数据库将无法启动。|  
|**last_log_backup_lsn**|**numeric(25,0)**|下一次日志备份的起始日志序列号。<br /><br /> 如果为 NULL，则不能执行事务日志备份，因为数据库处于简单恢复状态或当前没有数据库备份。|  
|**recovery_fork_guid**|**uniqueidentifier**|标识数据库当前在其上处于活动状态的当前恢复分叉。<br /><br /> NULL 表示数据库脱机，或将不启动数据库。|  
|**first_recovery_fork_guid**|**uniqueidentifier**|恢复分叉开始的标识符。<br /><br /> NULL 表示数据库脱机，或将不启动数据库。|  
|**fork_point_lsn**|**numeric(25,0)**|如果 **first_recovery_fork_guid** 不等于 (！ =) **recovery_fork_guid**，则 **fork_point_lsn** 为当前分叉点的日志序列号。 否则，该值为 NULL。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [数据库和文件目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
