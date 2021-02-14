---
description: 'sys.fn_hadr_backup_is_preferred_replica (Transact-sql) '
title: sys.fn_hadr_backup_is_preferred_replica (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_hadr_backup_is_preferred_replica_TSQL
- sys.fn_hadr_backup_is_preferred_replica
- fn_hadr_backup_is_preferred_replica_TSQL
- fn_hadr_backup_is_preferred_replica
dev_langs:
- TSQL
helpviewer_keywords:
- backup on secondary replicas
- active secondary replicas [SQL Server], backup on secondary replicas
- sys.fn_hadr_backup_is_preferred_replica function
ms.assetid: 61b9be77-e2f6-4da1-b2ae-a62cbe226145
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2b0cf6856f06b4afe5f73915be85d4d4e6163413
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338117"
---
# <a name="sysfn_hadr_backup_is_preferred_replica--transact-sql"></a>sys.fn_hadr_backup_is_preferred_replica (Transact-sql) 
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  用于确定当前副本是否为首选备份副本。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.fn_hadr_backup_is_preferred_replica ( 'dbname' )  
```  
  
## <a name="arguments"></a>参数  
 "*dbname*"  
 要备份的数据库的名称。 *dbname* 的类型为 sysname。  
  
## <a name="returns"></a>返回  
 如果当前实例上的数据库是首选副本，则返回数据类型 **bool**：1，否则返回0。  
  
## <a name="remarks"></a>备注  
 在备份脚本中使用此函数来确定当前数据库是否位于用于备份的首选副本上。 您可以在每个可用性副本上运行脚本。 其中的每个作业都将查看相同的数据来确定应运行的作业，因此，只有一个计划的作业实际上会继续到备份阶段。 示例代码可与以下代码相似。  
  
```  
If sys.fn_hadr_backup_is_preferred_replica( @dbname ) <> 1   
BEGIN  
-- If this is not the preferred replica, exit (probably without error).  
END  
-- If this is the preferred replica, continue to do the backup.  
  
```  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-sysfn_hadr_backup_is_preferred_replica"></a>A. 使用 sys.fn_hadr_backup_is_preferred_replica  
 如果当前数据库是首选备份副本，则下面的示例将返回 1。  
  
```  
SELECT sys.fn_hadr_backup_is_preferred_replica ('TestDB');  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [配置可用性副本备份 (SQL Server)](../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [Always On 可用性组函数 &#40;Transact-sql&#41;](../../relational-databases/system-functions/always-on-availability-groups-functions-transact-sql.md)   
 [AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [CREATE AVAILABILITY GROUP (Transact-SQL)](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP (Transact-SQL)](../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [活动辅助副本：辅助副本上的备份 &#40;Always On 可用性组&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md) [Always On 可用性组目录视图 &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)      
  
  
