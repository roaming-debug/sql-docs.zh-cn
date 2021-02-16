---
title: 数据库镜像系统对象参考 |Microsoft Docs
description: 查看有关数据库镜像系统对象（系统目录视图、系统动态管理视图和系统表）的信息。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a0bf290bb61cd5e6eb5cbd3715618b5949df87f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349206"
---
# <a name="database-mirroring-system-object-reference"></a>数据库镜像系统对象参考
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="system-catalog-views"></a>系统目录视图

| 系统目录视图 | 说明|
| :------ | :----------------------------- |
| [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   | 服务器在数据库镜像合作关系中充当的每个见证服务器角色在表中都占用一行。 |
| &nbsp; | &nbsp; |

## <a name="system-dynamic-management-views"></a>系统动态管理视图

| 系统动态管理视图 | 说明|
| :------ | :----------------------------- |
| [sys.dm_db_mirroring_auto_page_repair](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-auto-page-repair.md)   | 对服务器实例上所有镜像数据库的每个自动页修复尝试返回一行。  |
| [sys.dm_db_mirroring_connections](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md)    | 针对为每个数据库镜像建立的连接返回一行。 |
| &nbsp; | &nbsp; |

## <a name="system-tables"></a>系统表

| 系统表 | 说明|
| :------ | :----------------------------- |
| [sysdbmaintplan_databases](../../relational-databases/system-tables/sysdbmaintplan-databases-transact-sql.md)   | 返回有关数据库镜像维护计划的信息。 |
| [sysdbmaintplan_history](../../relational-databases/system-tables/sysdbmaintplan-history-transact-sql.md)    | 返回有关数据库镜像维护计划的历史记录信息。 |
| [sysdbmaintplan_jobs](../../relational-databases/system-tables/sysdbmaintplan-jobs-transact-sql.md)    |返回有关数据库镜像维护计划作业的信息。  |
| [sysdbmaintplans](../../relational-databases/system-tables/sysdbmaintplans-transact-sql.md)    | 返回有关数据库镜像计划的信息。  |
| &nbsp; | &nbsp; |


## <a name="see-also"></a>另请参阅  
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   

  
  
