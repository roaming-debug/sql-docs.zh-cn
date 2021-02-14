---
description: 数据迁移的监视与故障排除 (Stretch Database)
title: 数据迁移的监视与故障排除
ms.date: 06/14/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, monitoring
- monitoring Stretch Database
ms.assetid: 06950858-8c02-4ec6-9c59-42b787316a2d
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: f4d46ccfca9642a1be2f9e60d7f3382b4c49e90d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079898"
---
# <a name="monitor-and-troubleshoot-data-migration-stretch-database"></a>数据迁移的监视与故障排除 (Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  在 SQL Server Management Studio 中选择数据库的“任务 | Stretch | 监视”以监视 Stretch Database 监视器中的数据迁移。  
  
## <a name="check-the-status-of-data-migration-in-the-stretch-database-monitor"></a>在延伸数据库监视器中检查数据迁移状态  
 在 SQL Server Management Studio 中选择数据库的“任务 | Stretch | 监视”以打开 Stretch Database 监视器并监视数据迁移。  
  
-   此监视器的上半部分显示有关已启用拉伸的 SQL Server 数据库和远程 Azure 数据库的常规信息。  
  
-   监视器的下半部分显示数据库中每个已启用拉伸的表的数据迁移状态。  
  
 ![Stretch Database 监视器](../../sql-server/stretch-database/media/stretch-monitor.PNG "Stretch Database 监视器")  
  
##  <a name="check-the-status-of-data-migration-in-a-dynamic-management-view"></a><a name="Migration"></a> 检查动态管理视图中数据迁移的状态  
 打开动态管理视图 **sys.dm_db_rda_migration_status** 以查看有多少批数据和数据行已迁移。 有关详细信息，请参阅 [sys.dm_db_rda_migration_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/stretch-database-sys-dm-db-rda-migration-status.md)。  
  
##  <a name="troubleshoot-data-migration"></a><a name="Firewall"></a> 数据迁移故障排除  
 **我的已启用拉伸的表中的行未迁移到 Azure。这是什么问题？**  
 有几个问题可能会影响迁移。 请检查以下事项。  
  
-   检查 SQL Server 计算机的网络连接。  
  
-   检查 Azure 防火墙，确保其未阻止 SQL 服务器连接到远程终结点。  
  
-   检查动态管理视图 **sys.dm_db_rda_migration_status** ，了解最新批处理的状态。 如果出现错误，请检查此批处理的 error_number、error_state 和 error_severity 值。  
  
    -   有关视图的详细信息，请参阅 [sys.dm_db_rda_migration_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/stretch-database-sys-dm-db-rda-migration-status.md)。  
  
    -   有关 SQL Server 错误消息内容的详细信息，请参阅 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)。  
  
 **Azure 防火墙阻止了来自我的本地服务器的连接。**  
 可能必须在 Azure 服务器的 Azure 防火墙设置中添加一条规则，以使 SQL Server 可与远程 Azure 服务器进行通信。  
  
## <a name="see-also"></a>另请参阅  
 [延伸数据库的管理和故障排除](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)  
  
  
