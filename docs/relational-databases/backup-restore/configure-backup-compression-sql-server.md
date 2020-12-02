---
title: 配置备份压缩 (SQL Server) | Microsoft Docs
description: 本文介绍如何在 SQL Server 中创建单个备份或计划一系列例行备份时覆盖服务器级默认设置。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 430905eb-d218-458c-bd48-aeee6fbb7446
author: cawrites
ms.author: chadam
ms.openlocfilehash: 31376d9ff735c30a0374acade23e3a8a89804baa
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127027"
---
# <a name="configure-backup-compression-sql-server"></a>配置备份压缩 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  安装时，默认情况下关闭了备份压缩。 备份压缩的默认行为是由“备份压缩默认值”选项服务器级配置选项定义的。 但是，您可以在创建单个备份或计划一系列例行备份时覆盖服务器级默认设置。 若要更改服务器级默认设置，请参阅 [查看或配置备份压缩默认值服务器配置选项](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)。  
  
## <a name="override-the-backup-compression-default"></a>覆盖备份压缩默认设置  
 您可以更改单个备份、备份作业或日志传送配置的备份压缩行为。  
  
-   **[!INCLUDE[tsql](../../includes/tsql-md.md)]**  
  
     若要在创建备份时覆盖服务器备份压缩默认值，请在 [BACKUP](../../t-sql/statements/backup-transact-sql.md) 语句中使用 WITH NO_COMPRESSION 或 WITH COMPRESSION。  
  
     对于日志传送配置，可以使用 [sp_add_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)[sp_change_log_shipping_primary_database (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-change-log-shipping-primary-database-transact-sql.md) 控制日志备份的备份压缩行为。  
  
-   **[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]**  
  
     有关如何为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例查看或配置备份压缩默认值选项的信息，请参阅[查看或配置备份压缩默认值服务器配置选项](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)。  
  
     你可以通过在以下任意对话框中指定“压缩备份”或“不压缩备份”以在创建备份时覆盖服务器备份压缩默认值：  
  
    -   [备份数据库（“选项”页）](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)  
  
         备份数据库时，可以控制单个数据库、文件或日志备份的备份压缩。  
  
    -   [使用维护计划向导](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)  
  
         通过维护计划向导，您可以控制所计划的每组类型为完全或差异的数据库备份或日志备份的备份压缩。  
  
    -   Integration Services (SSIS) [备份数据库任务](../../integration-services/control-flow/back-up-database-task.md)  
  
         您可以在创建用于备份单个数据库或多个数据库的包时控制备份压缩行为。  
  
    -   [日志传送事务日志备份设置](../../relational-databases/databases/log-shipping-transaction-log-backup-settings.md)  
  
         您可以控制日志备份的备份压缩行为。  
  
  
## <a name="see-also"></a>另请参阅  
 [备份压缩 (SQL Server)](../../relational-databases/backup-restore/backup-compression-sql-server.md)  
  
  
