---
title: 将日志传送升级到 SQL Server 2016 及更高版本
description: 了解在从更低版本升级到 SQL Server 2016 及更高版本时保留日志传送灾难恢复解决方案的适当顺序。
ms.custom: seo-lt-2019
ms.date: 02/01/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], upgrading
ms.assetid: b1289cc3-f5be-40bb-8801-0e3eed40336e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8682cc0d93a279c9a14a04d767a52af7b66ed955
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346922"
---
# <a name="upgrading-log-shipping-to-sql-server-2016-transact-sql"></a>将日志传送升级至 SQL Server 2016 (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

若要保留日志传送灾难恢复解决方案，请升级或按适当的顺序应用服务更新。 服务更新包括服务包或累积更新。  
  
> [!NOTE]  
> [中引入了](../../relational-databases/backup-restore/backup-compression-sql-server.md) 备份压缩 [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)]。 升级后的日志传送配置使用“备份压缩默认值”服务器级配置选项控制是否对事务日志备份文件使用备份压缩。 可以为每个日志传送配置指定日志备份的备份压缩行为。 有关详细信息，请参阅[配置日志传送 (SQL Server)](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)。  
  
 **本主题内容：**  
  
-   [先决条件](#Prerequisites)  
  
-   [在升级前保护好您的数据](#ProtectData)  
  
-   [升级监视服务器实例](#UpgradeMonitor)  
  
-   [升级辅助服务器实例](#UpgradeSecondaries)  
  
-   [升级主实例](#UpgradePrimary)  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 开始之前，请仔细阅读以下重要信息：  
  
-   [支持的版本和版本升级](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)：验证是否可以从你的 Windows 操作系统版本和 SQL Server 版本升级到 SQL Server 2016。 例如，不能直接从 SQL Server 2005 实例升级到 [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)]。  
  
-   [选择数据库引擎升级方法](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md)：检查支持的版本和版本升级以及环境中安装的其他组件，并据此选择适当的升级方法和步骤，按正确顺序升级组件。  
  
-   [计划并测试数据库引擎升级计划](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md)：查看发行说明和已知升级问题、预升级清单，并制定和测试升级计划。  
  
-   [安装 SQL Server 2016 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)：查看安装 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 的软件要求。 如果需要其他软件，则应在升级过程开始之前在每个节点上安装该软件，从而最大程度减少故障时间。  
  
##  <a name="protect-your-data-before-the-upgrade"></a><a name="ProtectData"></a> 在升级前保护好您的数据  
 建议您最好在日志传送升级之前保护好您的数据。  
  
 **保护数据**  
  
1.  对各个主数据库执行完整数据库备份。  
  
     有关详细信息，请参阅 [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)中创建差异数据库备份。  
  
2.  对各个主数据库运行 [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) 命令。  
  
> [!IMPORTANT]  
>  确保在主服务器上有足够的空间，以便按预计升级辅助副本时保存日志备份的副本。  如果要故障转移到辅助服务器，相同的考虑事项也适用于辅助服务器（新的主服务器）。  
  
##  <a name="upgrading-the-optional-monitor-server-instance"></a><a name="UpgradeMonitor"></a> 升级（可选）监视服务器实例  
 监视服务器实例（如果存在）可随时升级。 但是，升级主服务器和辅助服务器时，不需要升级可选的监视服务器。  
  
 升级监视服务器时，日志传送配置仍将有效，但其状态不会记录在监视器上的表中。 监视服务器正在升级期间，已配置的任何警报都不会触发。 升级完毕后，可以通过执行 [sp_refresh_log_shipping_monitor](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md) 系统存储过程来更新监视器表中的信息。   有关监视服务器的详细信息，请参阅[关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)。  
  
##  <a name="upgrading-the-secondary-server-instances"></a><a name="UpgradeSecondaries"></a> 升级辅助服务器实例  

升级过程包括在升级主服务器实例之前升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 辅助服务器实例。 始终首先升级辅助 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 日志传送将在整个升级过程中继续进行，因为升级后的辅助服务器实例将继续从主服务器实例还原日志备份。 由于在较新版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中创建的备份无法在较旧版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中还原，因此如果主服务器实例先于辅助服务器实例升级，将导致日志传送失败。 你可以同时或按顺序升级辅助实例，但必须在升级主实例前升级所有辅助实例，以避免日志传送失败。  
  
辅助服务器实例正在升级期间，日志传送复制和还原作业不运行。 这意味着未还原的事务日志备份将在主服务器上累积，因此需确保空间充足以便保存这些未还原的备份。 累积的数量取决于主服务器实例上进行计划备份的频率，以及升级辅助实例的序列。 此外，如果还配置了单独的监视服务器，则若未执行还原的时间超过了所配置的时间间隔，就会引发警报。  
  
 辅助服务器实例升级完毕后，日志传送代理作业即开始继续将日志备份从主服务器实例复制并还原到辅助服务器实例。 辅助服务器实例使辅助数据库达到最新所需的时间会有所不同，具体情况取决于升级辅助服务器实例占用的时间和主服务器上进行备份的频率。  
  
> [!NOTE]  
> 在服务器升级过程中，辅助数据库本身不会升级到新版本。 仅当启动日志传送数据库的故障转移使得辅助数据库联机时，它才将进行升级。 从理论上讲，这种情况可能会无限期地继续下去。 启动故障转移时用于升级数据库元数据的时间量很小。  
  
> [!IMPORTANT]  
>  对于要求升级的数据库，不支持 RESTORE WITH STANDBY 选项。 如果已使用 RESTORE WITH STANDBY 配置了升级的辅助数据库，则在升级后可能不再能够还原事务日志。 要对该辅助数据库恢复日志传送，您需要再次对该备用服务器设置日志传送。 有关 STANDBY 选项的详细信息，请参阅[还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)。  
  
##  <a name="upgrading-the-primary-server-instance"></a><a name="UpgradePrimary"></a> 升级主服务器实例  
 由于日志传送是主要的灾难恢复解决方案，因此，最简单也是最常见的方案就是就地升级主实例，只是在此升级期间将无法使用数据库。 服务器升级完毕后，数据库即自动回到联机状态，随即进行升级。 数据库升级完毕后，日志传送作业将继续进行。  
  
> [!NOTE]  
>  日志传送还支持[故障转移到日志传送辅助服务器 (SQL Server)](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) 的选项以及选择性地支持[交换主日志传送服务器和辅助日志传送服务器的角色 (SQL Server)](../../database-engine/log-shipping/change-roles-between-primary-and-secondary-log-shipping-servers-sql-server.md)的选项。 但是，自从很少再将日志传送配置为高可用性解决方案（较新的选项更为可靠）以后，由于不会同步系统数据库对象，并且客户端也难以定位并连接到已升级的辅助数据库，因此故障转移通常不会最小化停机时间。  
  
## <a name="see-also"></a>另请参阅  
 [使用安装向导（安装程序）升级到 SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)   
 [从命令提示符安装 SQL Server 2016](../install-windows/install-sql-server-from-the-command-prompt.md)   
 [配置日志传送 (SQL Server)](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)   
 [监视日志传送 (Transact-SQL)](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)   
 [日志传送表和存储过程](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
