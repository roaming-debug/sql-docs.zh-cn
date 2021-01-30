---
title: '扩展存储过程 (Transact-sql) '
description: 了解在使用启用 Stretch 的数据库时可以使用的扩展存储过程。 请参阅如何协调列并执行其他任务。
titleSuffix: SQL Server Stretch Database
ms.custom: seo-dt-2019
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Stretch Database, stored procedures
ms.assetid: bda29952-4b8b-4295-ab78-f24dcb0b03c6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 556e74812ca328a42f1332f20b2576b4cc1b9359
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178068"
---
# <a name="stretch-database-extended-stored-procedures-transact-sql"></a> (Transact-sql 的扩展存储过程 Stretch Database) 
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

 本部分介绍与 Stretch Database 相关的扩展存储过程。  
  
## <a name="in-this-section"></a>本节内容  
[sys.sp_rda_deauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md) 删除启用了本地 Stretch 的数据库和远程 Azure 数据库之间经过身份验证的连接。

[sys.sp_rda_get_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-get-rpo-duration-transact-sql.md) 获取 SQL Server 保留在临时表中的已迁移数据的小时数，以帮助确保远程 Azure 数据库完全还原（如果需要还原）。
  
 [sys.sp_rda_reauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) 还原已启用延伸的本地数据库与远程数据库之间经过身份验证的连接。
  
 [sys.sp_rda_reconcile_batch](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-batch-transact-sql.md)  
 通过存储在远程 Azure 表中的批 ID，协调已启用 Stretch 的 SQL Server 表中存储的批 ID。 
 
[sys.sp_rda_reconcile_columns](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-columns-transact-sql.md) 将远程 Azure 表中的列与启用 Stretch SQL Server 表中的列协调。
 
 [sys.sp_rda_reconcile_indexes](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-indexes-transact-sql.md) 将架构任务排队以协调远程表上的索引。
 
 [sys.sp_rda_set_query_mode](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md) 指定是否对当前启用 Stretch 的数据库及其表进行查询同时返回默认)  (本地和远程数据，或仅返回本地数据。
 
 [sys.sp_rda_set_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-set-rpo-duration-transact-sql.md) 设置 SQL Server 保留在临时表中的已迁移数据的小时数，以帮助确保远程 Azure 数据库完整还原（如果需要还原）。
 
 [sys.sp_rda_test_connection](../../relational-databases/system-stored-procedures/sys-sp-rda-test-connection-transact-sql.md) 测试从 SQL Server 到远程 Azure 服务器的连接，并报告可能阻止数据迁移的问题。
 
## <a name="see-also"></a>另请参阅  
 [Stretch 数据库](../../sql-server/stretch-database/stretch-database.md)  
  
  
