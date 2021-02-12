---
description: 暂停和恢复数据迁移（Stretch Database）
title: 暂停和恢复数据迁移
ms.date: 06/14/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, pausing and resuming
- pausing Stretch Database
- resuming Stretch Database
ms.assetid: 65d6a990-b295-41b2-97f9-7b6bf3000e4d
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 5521919a31e84a9ade57bddc6a0716f00d1fc98b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031629"
---
# <a name="pause-and-resume-data-migration-stretch-database"></a>暂停和恢复数据迁移（Stretch Database）
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  要暂停或继续将数据迁移到 Azure，请在 SQL Server Management Studio 中选择某个表对应的“**延伸**”，然后选择“**暂停**”以暂停数据迁移，或选择“**恢复**”以恢复数据迁移。 你也可以使用 TRANSACT-SQL 来暂停或恢复数据迁移。  
  
 若要排查本地服务器上的问题，或最大限度地扩大可用网络带宽，请暂停各个表上的数据迁移。  

## <a name="pause-data-migration"></a>暂停数据迁移  
  
### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>使用 SQL Server Management Studio 暂停数据迁移  
  
1.  在 SQL Server Management Studio 的对象资源管理器中，选择要对其暂停数据迁移的已启用延伸的表。  
  
2.  右键单击并选择“延伸”，然后选择“暂停”。  
  
### <a name="use-transact-sql-to-pause-data-migration"></a>使用 TRANSACT-SQL 暂停数据迁移  
 运行以下命令。  
  
```sql  
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO 
```  
  
## <a name="resume-data-migration"></a>恢复数据迁移  
  
### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>使用 SQL Server Management Studio 恢复数据迁移  
  
1.  在 SQL Server Management Studio 的对象资源管理器中，选择要对其恢复数据迁移的已启用延伸的表。  
  
2.  右键单击并选择“延伸”，然后选择“继续”。  
  
### <a name="use-transact-sql-to-resume-data-migration"></a>使用 TRANSACT-SQL 恢复数据迁移  
 运行以下命令。  
  
```sql  
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```  

## <a name="check-whether-migration-is-active-or-paused"></a>检查迁移处于活动状态还是暂停状态

### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>使用 SQL Server Management Studio 检查迁移处于活动状态还是暂停状态
在 SQL Server Management Studio 中，打开“Stretch Database 监视器”并检查“迁移状态”列的值。 有关详细信息，请参阅 [数据迁移的监视与故障排除](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md)。

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>使用 Transact-SQL 检查迁移处于活动状态还是暂停状态
查询目录视图 **sys.remote_data_archive_tables** 并检查 **is_migration_paused** 列的值。 有关详细信息，请参阅 [sys.remote_data_archive_tables](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md)。

## <a name="see-also"></a>另请参阅  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
[数据迁移的监视与故障排除](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md) 
  
