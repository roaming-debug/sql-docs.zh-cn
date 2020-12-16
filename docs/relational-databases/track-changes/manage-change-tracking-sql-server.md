---
description: 管理更改跟踪 (SQL Server)
title: 管理更改跟踪
ms.custom: seo-dt-2019
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- tracking data changes [SQL Server]
- change tracking [SQL Server], overhead
- change tracking [SQL Server]
- change tracking [SQL Server], managing
ms.assetid: 94a8d361-e897-4d6d-9a8f-1bb652e7a850
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1513249d7c071d35a11423d405ee4b7d528d829b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472568"
---
# <a name="manage-change-tracking-sql-server"></a>管理更改跟踪 (SQL Server)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  本主题说明如何管理更改跟踪。 本主题还说明如何配置安全性以及如何确定使用更改跟踪时对存储和性能的影响。  
  
## <a name="managing-change-tracking"></a>管理更改跟踪  
 以下各节列出了与管理更改跟踪相关的目录视图、权限和设置。  
  
### <a name="catalog-views"></a>目录视图  
 若要确定哪些表和数据库启用了更改跟踪，可以使用以下目录视图：  
  
-   [sys.change_tracking_databases (Transact-SQL)](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-databases.md)  
  
-   [sys.change_tracking_tables (Transact-SQL)](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)  
  
 此外， [sys.internal_tables](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md) 目录视图还列出了对用户表启用更改跟踪时所创建的内部表。  
  
### <a name="security"></a>安全性  
 若要使用 [更改跟踪函数](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)访问更改跟踪信息，主体必须拥有以下权限：  
  
-   至少针对主键列（已启用更改跟踪的表针对被查询表的主键列）拥有 SELECT 权限。  
  
-   对于要获取其更改的表拥有 VIEW CHANGE TRACKING 权限。 要求拥有 VIEW CHANGE TRACKING 权限的原因如下：  
  
    -   更改跟踪记录包含有关已删除行的信息，具体而言，就是已删除行的主键值。 在删除了某些敏感数据之后，某个主体可能已被授予针对启用了更改跟踪的表的 SELECT 权限。 在这种情况下，您不会希望该主体能够使用更改跟踪来访问那些已删除的信息。  
  
    -   更改跟踪信息可以存储有关更新操作所更改的列的信息。 某个主体可能无权访问包含敏感信息的列。 但是，由于有更改跟踪信息，因此主体可以确定某列的值是否已更新，但是该主体无法确定该列的值。  
  
## <a name="understanding-change-tracking-overhead"></a>了解更改跟踪开销  
 启用表的更改跟踪后，会影响某些管理操作。 下表列出了应当注意的操作和影响。  
  
|Operation|启用更改跟踪后|  
|---------------|-------------------------------------|  
|DROP TABLE|会删除已删除表的所有更改跟踪信息。|  
|ALTER TABLE DROP CONSTRAINT|删除 PRIMARY KEY 约束的尝试将失败。 必须先禁用更改跟踪，然后才能删除 PRIMARY KEY 约束。|  
|ALTER TABLE DROP COLUMN|如果要删除的列是主键的一部分，则不允许删除该列，而不管是否启用了更改跟踪。<br /><br /> 如果要删除的列不是主键的一部分，则可以成功删除该列。 但是，首先应了解此操作对同步此数据的任何应用程序的影响。 如果为该表启用了列更改跟踪，则可能仍会将已删除的列作为更改跟踪信息的一部分返回。 已删除列的处理由应用程序负责。|  
|ALTER TABLE ADD COLUMN|如果将新列添加到启用了更改跟踪的表中，则不会跟踪该列的添加。 只会跟踪对新列所做的更新和更改。|  
|ALTER TABLE ALTER COLUMN|不会跟踪非主键列的数据类型更改。|  
|ALTER TABLE SWITCH|如果其中一个表或两个表都启用了更改跟踪，则切换分区将失败。|  
|DROP INDEX 或 ALTER INDEX DISABLE|不能删除或禁用强制使用主键的索引。|  
|TRUNCATE TABLE|可以对启用了更改跟踪的表执行截断表操作。 但是，不会跟踪由该操作删除的行，并且会更新最低有效版本。 当应用程序检查其版本时，检查结果会表明该版本太陈旧，需要进行重新初始化。 这与禁用后又重新启用表的更改跟踪的效果相同。|  
  
 由于在操作过程中会存储更改跟踪信息，因此使用更改跟踪会增加 DML 操作的一些开销。  
  
### <a name="effects-on-dml"></a>对 DML 的影响  
 更改跟踪已经过优化，以尽可能减小对 DML 操作的性能影响。 对表使用更改跟踪所导致的性能开销增加类似于为表创建了一个索引并需要维护该索引时而导致的开销。  
  
 对于由 DML 操作更改的每一行，都会向内部更改跟踪表中添加一行。 这种与 DML 操作相关的影响取决于各种因素，例如：  
  
-   主键列数  
  
-   用户表行中所更改的数据量  
  
-   事务中所执行的操作数  
  
 如果使用了快照隔离，则它也会影响所有 DML 操作的性能，而不管是否启用了更改跟踪。  
  
### <a name="effects-on-storage"></a>对存储的影响  
 更改跟踪数据存储在以下类型的内部表中：  
  
-   内部更改表  
  
     启用了更改跟踪的每个用户表都有一个内部更改表。  
  
-   内部事务表  
  
     数据库有一个内部事务表。  
  
 这些内部表对存储要求有下列影响：  
  
-   对于用户表中每行的每个更改，都会向内部更改表中添加一行。 该行有一个较小的固定开销，外加一个大小等于主键列大小的可变开销。 该行可以包含由应用程序设置的可选上下文信息。 此外，如果启用了列跟踪，则每个发生更改的列还需要在跟踪表中占用 4 字节。  
  
-   对于每个已提交的事务，都会向内部事务表中添加一行。  
  
 对于其他内部表，可以使用 [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) 存储过程来确定用于更改跟踪表的空间。 可以使用 [sys.internal_tables](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md) 目录视图来获取这些内部表的名称，如下例所示。  
  
```sql  
sp_spaceused 'sys.change_tracking_309576141'  
sp_spaceused 'sys.syscommittab'  
```  
  
## <a name="see-also"></a>另请参阅  
 [跟踪数据更改 (SQL Server)](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [数据库属性（“ChangeTracking”页）](../../relational-databases/databases/database-properties-changetracking-page.md)   
 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.change_tracking_databases (Transact-SQL)](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-databases.md)   
 [sys.change_tracking_tables (Transact-SQL)](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)   
 [跟踪数据更改 (SQL Server)](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [关于更改跟踪 (SQL Server)](../../relational-databases/track-changes/about-change-tracking-sql-server.md)   
 [处理变更数据 (SQL Server)](../../relational-databases/track-changes/work-with-change-data-sql-server.md)  
  
  
