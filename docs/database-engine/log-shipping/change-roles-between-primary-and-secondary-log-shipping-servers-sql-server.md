---
title: 更改主数据库和辅助数据库日志传送角色
description: 了解如何将辅助数据库配置为充当 SQL Server 日志传送解决方案的主数据库。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], role changes
- secondary data files [SQL Server], roles changed between
- primary databases [SQL Server]
- initial role changes [SQL Server]
- log shipping [SQL Server], failover
- failover [SQL Server], log shipping
ms.assetid: 2d7cc40a-47e8-4419-9b2b-7c69f700e806
author: cawrites
ms.author: chadam
ms.openlocfilehash: 27d97745e5965024a824828009889e368d800b75
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353766"
---
# <a name="change-roles-between-primary-and-secondary-log-shipping-servers-sql-server"></a>交换主日志传送服务器和辅助日志传送服务器的角色 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  在将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志传送配置故障转移到辅助服务器后，可以将辅助数据库配置为主数据库。 然后，就可以根据需要交换主数据库和辅助数据库。  
  
## <a name="performing-the-initial-role-change"></a>执行初始角色交换  
 当初次将故障转移到辅助数据库并将其用作新的主数据库时，必须执行一系列步骤。 按照这些初始步骤操作后，就可以轻松地交换主数据库和辅助数据库的角色。  
  
1.  手动从主数据库故障转移到辅助数据库。 请确保用 NORECOVERY 备份主服务器上的活动事务日志。 有关详细信息，请参阅 [故障转移到日志传送辅助服务器 (SQL Server)](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)。  
  
2.  禁用原始主服务器上的日志传送备份作业以及原始辅助服务器上的复制和还原作业。  
  
3.  使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在辅助数据库（要用作新的主数据库的数据库）上配置日志传送。 有关详细信息，请参阅[配置日志传送 (SQL Server)](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)。 包括下列步骤：  
  
    1.  使用同一个共享来创建为原来的主服务器所创建的备份。  
  
    2.  添加辅助数据库时，在 **“辅助数据库设置”** 对话框的 **“辅助数据库”** 框中输入原来的主数据库的名称。  
  
    3.  在 **“辅助数据库设置”** 对话框中，选中 **“否，辅助数据库已初始化”** 。  
  
4.  如果对于您之前的日志传送配置启用了日志传送监视，则重新配置日志传送监视以便监视新的日志传送配置。  将 threshold_alert_enabled 设置为 1 指定在超过 restore_threshold 时将引发警报。 执行以下命令，将 *database_name* 替换为你的数据库的名称：  
  
    1.  **在新的主服务器上**  
  
         执行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
        ```sql  
        -- Statement to execute on the new primary server  
        USE msdb  
        GO  
        EXEC master.dbo.sp_change_log_shipping_secondary_database @secondary_database = N'database_name', @threshold_alert_enabled = 1;  
        GO  
        ```  
  
    2.  **在新的辅助服务器上**  
  
         执行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句：  
  
        ```sql  
        -- Statement to execute on the new secondary server  
        USE msdb  
        GO  
        EXEC master.dbo.sp_change_log_shipping_primary_database @database=N'database_name', @threshold_alert_enabled = 1;  
        GO  
        ```  
  
## <a name="swapping-roles"></a>交换角色  
 完成以上步骤执行初始角色交换后，就可以按照本节的下列步骤交换主数据库和辅助数据库的角色。 若要执行角色交换，请执行下列常规步骤：  
  
1.  使辅助数据库联机，用 NORECOVERY 备份主服务器上的事务日志。  
  
2.  禁用原始主服务器上的日志传送备份作业以及原始辅助服务器上的复制和还原作业。  
  
3.  在辅助服务器（新的主服务器）上启用日志传送备份作业，在主服务器（新的辅助服务器）上启用复制和还原作业。  
  
> [!IMPORTANT]  
>  将辅助数据库更改为主数据库时，为了给用户和应用程序提供一致的体验，您可能需要在新的主服务器实例中为数据库重新创建部分或全部元数据（例如登录和作业）。 有关详细信息，请参阅 [当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [故障转移到日志传送辅助服务器 (SQL Server)](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
-   [角色切换后登录名和作业的管理 (SQL Server)](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [日志传送表和存储过程](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
