---
description: 启用和禁用更改跟踪 (SQL Server)
title: 启用和禁用更改跟踪
ms.custom: seo-dt-2019
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- change tracking [SQL Server], disabling
- data changes [SQL Server]
- change tracking [SQL Server], enabling
- tracking data changes [SQL Server]
- change tracking [SQL Server], configuring
- data [SQL Server], changing
ms.assetid: 1c92ec7e-ae53-4498-8bfd-c66a42a24d54
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7dda1f0aaf7d5ca1470871752a78bdd04a99fb9f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472578"
---
# <a name="enable-and-disable-change-tracking-sql-server"></a>启用和禁用更改跟踪 (SQL Server)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  本主题说明如何对数据库和表启用和禁用更改跟踪。  
  
## <a name="enable-change-tracking-for-a-database"></a>对数据库启用更改跟踪  
 您必须先在数据库级别启用更改跟踪，然后才能使用更改跟踪。 下面的示例显示了如何使用 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md)来启用更改跟踪。  
  
```sql  
ALTER DATABASE AdventureWorks2012  
SET CHANGE_TRACKING = ON  
(CHANGE_RETENTION = 2 DAYS, AUTO_CLEANUP = ON)  
```  
  
 你还可以通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 数据库属性（“ChangeTracking”页） [数据库属性（“ChangeTracking”页）](../../relational-databases/databases/database-properties-changetracking-page.md) 中启用更改跟踪。 如果数据库包含内存优化表，则无法使用 SQL Server Management Studio 启用更改跟踪。 若要启用，请使用 T-SQL。
  
 可以在启用更改跟踪时指定 CHANGE_RETENTION 和 AUTO_CLEANUP 选项，并且可以在启用更改跟踪后随时更改这些值。  
  
 更改保持期值指定了更改跟踪信息的保留时间。 早于此时间的更改跟踪信息将被定期删除。 设置该值时，应考虑应用程序与数据库中的表进行同步的频率。 指定的保持期必须至少等于最大同步时间间隔。 如果应用程序获取更改的时间间隔过长，则返回的结果可能不正确，因为某些更改信息可能已被删除。 若要避免获取错误的结果，应用程序可以使用 CHANGE_TRACKING_MIN_VALID_VERSION 系统函数来确定同步之间的时间间隔是否已太长。  
  
 可使用 AUTO_CLEANUP 选项来启用或禁用删除陈旧的更改跟踪信息的清除任务。 如果出现临时性问题使得应用程序无法同步，并且在问题解决之前必须暂停用于删除早于保持期的更改跟踪信息的进程，则该设置会很有用。  
  
 对于使用更改跟踪的任何数据库，请注意以下事项：  
  
-   若要使用更改跟踪，必须将数据库兼容级别设为 90 或更高。 如果数据库的兼容级别低于 90，则可以配置更改跟踪。 但是，用于获取更改跟踪信息的 CHANGETABLE 函数将返回错误。  
  
-   使用快照隔离是帮助确保所有更改跟踪信息保持一致的最简单方式。 因此，我们强烈建议将数据库的快照隔离设为 ON。 有关详细信息，请参阅[处理更改跟踪 (SQL Server)](../../relational-databases/track-changes/work-with-change-tracking-sql-server.md)。  
  
## <a name="enable-change-tracking-for-a-table"></a>对表启用更改跟踪  
 对于要跟踪的每个表都必须启用更改跟踪。 启用更改跟踪后，将会为表中受 DML 操作影响的所有行保留更改跟踪信息。  
  
 下面的示例显示了如何使用 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)来对表启用更改跟踪。  
  
```sql  
ALTER TABLE Person.Contact  
ENABLE CHANGE_TRACKING  
WITH (TRACK_COLUMNS_UPDATED = ON)  
```  
  
 你还可以通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 数据库属性（“ChangeTracking”页） [数据库属性（“ChangeTracking”页）](../../relational-databases/databases/database-properties-changetracking-page.md) 中启用更改跟踪。  
  
 当 TRACK_COLUMNS_UPDATED 选项设为 ON 时， [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 会将有关哪些列已更新的额外信息存储到内部更改跟踪表中。 列跟踪使应用程序可以只同步那些已更新的列。 这可以提高效率和性能。 但是，由于保留列跟踪信息增加了一些额外的存储开销，因而默认情况下此选项设为 OFF。  
  
## <a name="disable-change-tracking-for-a-database-or-table"></a>为数据库或表禁用更改跟踪  
 必须首先为所有启用了更改跟踪的表禁用更改跟踪，然后才能将数据库的更改跟踪设为 OFF。 若要确定数据库中哪些表启用了更改跟踪，请使用 [sys.change_tracking_tables](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md) 目录视图。  
  
 当数据库中没有用于跟踪更改的表时，便可以禁用数据库的更改跟踪。 下面的示例显示如何使用 [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md)对数据库禁用更改跟踪。  
  
```sql  
ALTER DATABASE AdventureWorks2012  
SET CHANGE_TRACKING = OFF  
```  
  
 下面的示例显示了如何使用 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)对表禁用更改跟踪。  
  
```sql  
ALTER TABLE Person.Contact  
DISABLE CHANGE_TRACKING;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库属性（“ChangeTracking”页）](../../relational-databases/databases/database-properties-changetracking-page.md)   
 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.change_tracking_databases (Transact-SQL)](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-databases.md)   
 [sys.change_tracking_tables (Transact-SQL)](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)   
 [跟踪数据更改 (SQL Server)](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [关于更改跟踪 (SQL Server)](../../relational-databases/track-changes/about-change-tracking-sql-server.md)   
 [处理变更数据 (SQL Server)](../../relational-databases/track-changes/work-with-change-data-sql-server.md)   
 [管理更改跟踪 (SQL Server)](../../relational-databases/track-changes/manage-change-tracking-sql-server.md)  
  
  
