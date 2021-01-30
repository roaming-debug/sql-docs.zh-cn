---
title: sys.remote_data_archive_tables (Transact-sql) |Microsoft Docs
description: 了解对于存储从启用延伸的本地表中的数据的每个远程表，sys.remote_data_archive_tables 各占一行。
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.remote_data_archive_tables
- sys.remote_data_archive_tables_TSQL
- remote_data_archive_tables
- remote_data_archive_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.remote_data_archive_tables catalog view
ms.assetid: 765069b7-60fd-414c-875f-3455460b75cd
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: cd0c62c78c93557c011c299c7cf0e65c7285fba8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209040"
---
# <a name="stretch-database-catalog-views---sysremote_data_archive_tables"></a>Stretch Database 目录视图-sys.remote_data_archive_tables
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  每个从启用 Stretch 的本地表存储数据的远程表在表中各占一行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|已启用延伸的本地表的对象 ID。|  
|**remote_database_id**|**int**|自动生成的远程数据库本地标识符。|  
|**remote_table_name**|**sysname**|远程数据库中与已启用延伸的本地表对应的表的名称。|  
|**filter_predicate**|**nvarchar(max)**|标识要迁移的表中的行的筛选器谓词（如果有）。 如果值为 null，则整个表符合迁移条件。<br /><br /> 有关详细信息，请参阅为 [表启用 Stretch Database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) 和 [使用筛选器谓词选择要迁移的行](~/sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md)。|  
|**migration_direction**|**tinyint**|当前迁移数据的方向。 可用值如下。<br/>1 (出站) <br/>2 (入站) |  
|**migration_direction_desc**|**nvarchar(60)**|当前迁移数据的方向的说明。 可用值如下。<br/>出站 (1) <br/>入站 (2) |  
|**is_migration_paused**|**bit**|指示迁移当前是否已暂停。|  
|**is_reconciled**|**bit**| 指示远程表与 SQL Server 表是否同步。<br/><br/>如果 **is_reconciled** 的值为 1 (为 true) ，则远程表和 SQL Server 表将同步，您可以运行包含远程数据的查询。<br/><br/>如果 **is_reconciled** 的值为 0 (false) ，则远程表和 SQL Server 表将不同步。最近迁移的行必须再次迁移。 当你还原远程 Azure 数据库时，或者从远程表中手动删除行时，会发生这种情况。 在对表进行协调之前，无法运行包含远程数据的查询。 若要协调表，请运行 [sys.sp_rda_reconcile_batch](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-batch-transact-sql.md)。 |  
  
## <a name="see-also"></a>另请参阅  
 [Stretch 数据库](../../sql-server/stretch-database/stretch-database.md)  
  
  

