---
title: 配置日志传送 (SQL Server) | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中配置日志传送。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], enabling
- log shipping [SQL Server], configuring
ms.assetid: c42aa04a-4945-4417-b4c7-50589d727e9c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 71fd673e2453f49540d30567c8007f0fc5ad4c61
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813190"
---
# <a name="configure-log-shipping-sql-server"></a>配置日志传送 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中配置日志传送。  
  
> [!NOTE]  
>  [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] 及更高版本支持备份压缩。 创建日志传送配置时，可以控制日志备份的备份压缩行为。 有关详细信息，请参阅[备份压缩 (SQL Server)](../../relational-databases/backup-restore/backup-compression-sql-server.md)。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [先决条件](#Prerequisites)  
  
     [安全性](#Security)  
  
-   **若要配置日志传送，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [相关任务](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   主数据库必须使用完整恢复模式或大容量日志恢复模式，将数据库切换为简单恢复模式会导致日志传送停止工作。  
  
-   在配置日志传送之前，您必须创建共享，以便辅助服务器可以访问事务日志备份。 这是对生成事务日志备份的目录的共享。 例如，如果将事务日志备份到目录 C:\data\tlogs\\，则可以对该目录创建 \\\\*primaryserver*\tlogs 共享。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 日志传送存储过程要求 **sysadmin** 固定服务器角色中的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-configure-log-shipping"></a>配置日志传送  
  
1.  右键单击要在日志传送配置中用作主数据库的数据库，然后单击 **“属性”** 。  
  
2.  在 **“选择页”** 下，单击 **“事务日志传送”** 。  
  
3.  选中 **“将此数据库启用为日志传送配置中的主数据库”** 复选框。  
  
4.  在 **“事务日志备份”** 下，单击 **“备份设置”** 。  
  
5.  在 **“备份文件夹的网络路径”** 框中，键入为事务日志备份文件夹创建的共享的网络路径。  
  
6.  “如果备份文件夹位于主服务器上，则在备份文件夹中键入本地路径”框。 （如果备份文件夹不在主服务器上，此框可以保留为空。）  
  
    > [!IMPORTANT]  
    >  如果主服务器上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户运行在本地系统帐户下，则必须在主服务器上创建备份文件夹，并指定该文件夹的本地路径。  
  
7.  配置 **“删除文件，如果其保留时间超过”** 和 **“在以下时间内没有执行备份时报警”** 参数。  
  
8.  请注意 **“备份作业”** 下的 **“计划”** 框中列出的备份计划。 如果想要为安装自定义计划，则单击 **“计划”** 并根据需要调整 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理计划。  
  
9. [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 支持 [备份压缩](../../relational-databases/backup-restore/backup-compression-sql-server.md)。 创建日志传送配置时，可以通过选择以下选项之一来控制日志备份的备份压缩行为：“使用默认服务器设置”、“压缩备份”或“不压缩备份”  。 有关详细信息，请参阅 [Log Shipping Transaction Log Backup Settings](../../relational-databases/databases/log-shipping-transaction-log-backup-settings.md)。  
  
10. 单击“确定”。  
  
11. 在 **“辅助服务器实例和数据库”** 下，单击 **“添加”** 。  
  
12. 单击 **“连接”** ，连接到要用作辅助服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
13. 在 **“辅助数据库”** 框中，从列表中选择一个数据库或键入想要创建的数据库的名称。  
  
14. 在 **“初始化辅助数据库”** 选项卡上，选择要用于初始化辅助数据库的选项。  
  
    > [!NOTE]  
    >  如果选择让 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 从数据库备份中初始化辅助数据库，则辅助数据库的数据文件和日志文件将与 **master** 数据库的数据文件和日志文件放置在同一位置。 此位置可能不同于主数据库的数据文件和日志文件所在的位置。  
  
15. 在 **“复制文件”** 选项卡上的 **“复制文件的目标文件夹”** 框中，键入应该将事务日志备份复制到其中的文件夹的路径。 该文件夹通常位于辅助服务器上。  
  
16. 请注意 **“复制作业”** 下的 **“计划”** 框中列出的复制计划。 如果要自定义安装计划，请单击 **“计划”** ，然后根据需要调整 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理计划。 此计划应为大致的备份计划。  
  
17. 在 **“还原”** 选项卡上的 **“还原备份时的数据库状态”** 下，选择 **“无恢复模式”** 或 **“备用模式”** 选项。  
    > [!IMPORTANT]  
    > 当主服务器和辅助服务器的版本相同时，仅选择“备用模式”。 当辅助服务器的主版本高于主服务器时，仅允许“无恢复模式”
  
18. 如果选择了 **“备用模式”** 选项，请选择是否要在进行还原操作时从辅助数据库断开用户连接。  
  
19. 如果希望延迟辅助服务器上的还原进程，请在 **“延迟还原备份操作至少”** 下选择延迟时间。  
  
20. 在 **“在以下时间内没有执行还原时报警”** 下选择警报阈值。  
  
21. 请注意 **“还原作业”** 下 **“计划”** 框中列出的还原计划。 如果要自定义安装计划，请单击 **“计划”** ，然后根据需要调整 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理计划。 此计划应为大致的备份计划。  
  
22. 单击“确定”。  
  
23. 在 **“监视服务器实例”** 下，选中 **“使用监视服务器实例”** 复选框，然后单击 **“设置”** 。  
  
    > [!IMPORTANT]  
    >  若要监视此日志传送配置，必须现在添加监视服务器。 若要以后添加监视服务器，则需要先删除此日志传送配置，然后将其替换为包含监视服务器的新配置。  
  
24. 单击 **“连接”** 连接到想要用作监视服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
25. 在 **“监视器连接”** 下，选择备份、副本以及还原作业所使用的连接方法来连接到监视器服务器。  
  
26. 在 **“历史记录保持期”** 下，选择想要保留日志传送历史记录的时间长度。  
  
27. 单击“确定”。  
  
28. 在 **“数据库属性”** 对话框中，单击 **“确定”** 开始配置进程。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-configure-log-shipping"></a>配置日志传送  
  
1.  通过在辅助服务器上还原主数据库的完整备份，初始化辅助数据库。  
  
2.  在主服务器上，执行 [sp_add_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md) 以添加主数据库。 存储过程将返回备份作业 ID 和主 ID。  
  
3.  在主服务器上执行 [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) ，以为备份作业添加计划。  
  
4.  在监视服务器上，执行 [sp_add_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-add-log-shipping-alert-job-transact-sql.md) 以添加警报作业。  
  
5.  在主服务器上，启用备份作业。  
  
6.  在辅助服务器上，执行 [sp_add_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-primary-transact-sql.md) ，提供主服务器和数据库的详细信息。 此存储过程返回辅助 ID 以及复制和还原作业 ID。  
  
7.  在辅助服务器上，执行 [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) 以设置复制和还原作业的计划。  
  
8.  在辅助服务器上，执行 [sp_add_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md) 以添加辅助数据库。  
  
9. 在主服务器上，执行 [sp_add_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-secondary-transact-sql.md) 向主服务器添加有关新辅助数据库的必需信息。  
  
10. 在辅助服务器上，启用复制和还原作业。 有关详细信息，请参阅 [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md)。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [将日志传送升级至 SQL Server 2016 (Transact-SQL)](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [向日志传送配置添加辅助数据库 (SQL Server)](../../database-engine/log-shipping/add-a-secondary-database-to-a-log-shipping-configuration-sql-server.md)  
  
-   [从日志传送配置中删除辅助数据库 (SQL Server)](../../database-engine/log-shipping/remove-a-secondary-database-from-a-log-shipping-configuration-sql-server.md)  
  
-   [删除日志传送 (SQL Server)](../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   [查看日志传送报告 (SQL Server Management Studio)](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)  
  
-   [监视日志传送 (Transact-SQL)](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
-   [故障转移到日志传送辅助服务器 (SQL Server)](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [日志传送表和存储过程](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
