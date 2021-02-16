---
title: 故障转移到日志传送辅助数据库
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 故障转移到 SQL Server 日志传送辅助服务器。
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- primary databases [SQL Server]
- secondary data files [SQL Server], manual fail over
- log shipping [SQL Server], failover
- failover [SQL Server], log shipping
ms.assetid: edfe5d59-4287-49c1-96c9-dd56212027bc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2c6a999ee4c91842025f3f5030339a48a4a5756d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353738"
---
# <a name="fail-over-to-a-log-shipping-secondary-sql-server"></a>故障转移到日志传送辅助服务器 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  如果主服务器实例失败或需要维护，则在出现故障时转移到日志传送辅助服务器将十分有用。  
  
## <a name="preparing-for-a-controlled-failover"></a>为受控故障转移做准备  
 通常，主数据库与辅助数据库不同步，因为主数据库在其最新的备份作业后会继续更新。 此外，在某些情况下，最新的事务日志备份尚未复制到辅助服务器实例中，或者某些已复制的日志备份可能尚未应用到辅助数据库中。 建议如有可能，首先将所有辅助数据库与主数据库同步。  
  
 有关日志传送作业的信息，请参阅 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)。  
  
## <a name="failing-over"></a>故障转移  
 在出现故障时转移到辅助数据库：  
  
1.  将所有未复制的备份文件从备份共享复制到每台辅助服务器的复制目标文件夹中。  
  
2.  将所有未应用的事务日志备份按顺序应用到每个辅助数据库中。 有关详细信息，请参阅[应用事务日志备份 (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)。  
  
3.  如果可以访问主数据库，则请备份活动的事务日志，并将日志备份应用到辅助数据库。 可能需要在发出 restore 命令之前将数据库设置为[单用户模式](../../relational-databases/databases/set-a-database-to-single-user-mode.md)以获得独占访问权限，然后在还原完成后将其切换回多用户模式。  
  
     如果原始主服务器实例没有损坏，则请使用 WITH NORECOVERY 备份主数据库的事务日志尾部。 这将使数据库处于还原状态，因此用户无法使用。 最终，您将能够通过应用替换主数据库中的事务日志备份前滚此数据库。  
  
     有关详细信息，请参阅[事务日志备份 (SQL Server)](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)。   
  
4.  同步辅助服务器之后，可以根据您的首选，通过恢复任一辅助数据库并将客户端重定向到该服务器实例来故障转移该辅助服务器。 恢复操作将使数据库处于一致的状态并使其联机。  
  
    > [!NOTE]  
    >  辅助数据库可用时，应确保其元数据与原始主数据库的元数据一致。 有关详细信息，请参阅 [当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)。  
  
5.  恢复辅助数据库之后，可以将其重新配置为其他辅助数据库的主数据库。  
  
     如果其他辅助数据库不可用，请参阅[配置日志传送 (SQL Server)](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [交换主日志传送服务器和辅助日志传送服务器的角色 (SQL Server)](../../database-engine/log-shipping/change-roles-between-primary-and-secondary-log-shipping-servers-sql-server.md)  
  
-   [角色切换后登录名和作业的管理 (SQL Server)](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [日志传送表和存储过程](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)   
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [结尾日志备份 (SQL Server)](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)  
  
  
