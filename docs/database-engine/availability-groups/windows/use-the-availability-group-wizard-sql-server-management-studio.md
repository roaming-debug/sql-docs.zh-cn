---
title: 在 SSMS 中配置可用性组
description: 使用 SQL Server Management Studio (SSMS) 中的“新建可用性组向导”创建和配置 Always On 可用性组。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.newagwizard.f1
- sql13.swb.newavgroupwiz.f1
helpviewer_keywords:
- New Availability Group Wizard, availability replicas
- Availability Groups [SQL Server], wizards
- Availability Groups [SQL Server], creating
ms.assetid: e1f1dccc-9e65-471d-8fd1-b45085c9484a
author: cawrites
ms.author: chadam
ms.openlocfilehash: f4361019a3f65dbe1fc4a1b65fe54e388175a84d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641767"
---
# <a name="use-the-availability-group-wizard-sql-server-management-studio"></a>使用可用性组向导 (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 **中的“新建可用性组向导”在** 中创建和配置 AlwaysOn 可用性组[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)][!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]。 “可用性组”  定义一组用户数据库，这些用户数据库将以支持故障转移的单个单元和一组故障转移伙伴（称作“可用性副本”  ）的形式进行故障转移。  
  
> [!NOTE]  
>  有关可用性组的简介，请参阅 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)中通过 PowerShell 创建和配置 AlwaysOn 可用性组。  
    
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
 我们强烈建议您首先阅读此部分，再尝试创建您的第一个可用性组。  
  
###  <a name="prerequisites-restrictions-and-recommendations"></a><a name="Prerequisites"></a> 先决条件、限制和建议  

在大多数情况下，可以使用新建可用性组向导来完成创建和配置可用性组所需的所有任务。 但是，您可能需要手动完成一些任务。  
  
- 如果使用 Windows Server 故障转移群集 (WSFC) 群集类型托管可用性组，请先验证托管可用性副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例是否驻留在同一 WSFC 内的不同群集服务器（或节点）上。 此外，还请验证每个服务器实例是否都满足所有其他 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 先决条件。 有关详细信息，我们强烈建议你参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。  
  
-   如果您选择承载可用性副本的服务器实例正在以域用户帐户运行并且尚不具有数据库镜像端点，则此向导可以创建该端点并将 CONNECT 权限授予服务器实例的服务帐户。 但是，如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务正在以内置帐户（例如 Local System、Local Service 或 Network Service）或非域帐户运行，您必须使用证书来进行端点身份验证，并且该向导将无法在服务器实例上创建数据库镜像端点。 在此情况下，我们建议您首先手动创建数据库镜像端点，然后启动新建可用性组向导。  
  
     **使用数据库镜像端点证书：**  
  
     [CREATE ENDPOINT (Transact-SQL)](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
     [使用数据库镜像终结点证书 (Transact-SQL)](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   SQL Server 故障转移群集实例 (FCI) 不支持通过可用性组来自动进行故障转移，因此，只能为手动故障转移配置任何由 FCI 承载的可用性副本。  
  
-   **向导执行完全初始数据同步的先决条件**  
  
    -   在承载可用性组的副本的每个服务器实例上，所有数据库文件路径都必须完全相同。  
  
    -   没有任何主数据库名称可存在于承载辅助副本的任何服务器实例上。 这意味着尚没有任何新的辅助数据库可以存在。  
  
    -   为了使该向导创建并访问备份，需要指定网络共享。 对于主副本，用于启动 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 的帐户必须对网络共享具有读写文件系统权限。 对于辅助副本，该帐户必须具有对网络共享区的读权限。  
  
        > [!IMPORTANT]  
        >  日志备份将是您的日志备份链的一部分。 适当地存储日志备份文件。  
  
     如果您无法使用该向导执行完全初始数据同步，则需要手动准备您的辅助数据库。 您可以在运行该向导之前或之后进行准备。 有关详细信息，请参阅 [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)或 PowerShell 将辅助数据库联接到 Always On 可用性组。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要 **sysadmin** 固定服务器角色的成员资格，以及 CREATE AVAILABILITY GROUP 服务器权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
 如果要允许可用性组向导管理数据库镜像端点，还需要 CONTROL ON ENDPOINT 权限。  
  
##  <a name="using-the-new-availability-group-wizard"></a><a name="RunAGwiz"></a> 使用新建可用性组向导  
  
1.  在对象资源管理器中，连接到承载主副本的服务器实例。  
  
2.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
3.  若要启动新建可用性组向导，请选择 **“新建可用性组向导”** 命令。  
  
4.  首次运行该向导时， **“简介”** 页将出现。 若要在将来跳过此页，可单击 **“不再显示此页”** 。 在阅读了此页后，单击 **“下一步”** 。  
  
5.  在“指定可用性组选项”页上的“可用性组名称”字段中，输入新可用性组的名称   。 此名称必须是在群集和域中唯一有效的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 标识符。 可用性组名称的最大长度为 128 个字符。

6. 接下来，指定群集类型。 可能的群集类型取决于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本和操作系统。 选择“WSFC”、“EXTERNAL”或“NONE”    。 有关详细信息，请参阅[“指定可用性组名称”页](specify-availability-group-name-page.md)
 
6.  在 **“选择数据库”** 页上，网格中列出所连接的服务器实例上有资格成为“可用性数据库”  的用户数据库。 选择一个或多个列出的数据库以参与新的可用性组。 这些数据库最初将成为初始“主数据库”  。  
  
     对于每个列出的数据库， **“大小”** 列显示数据库大小（如果已知）。 “状态”  列指示给定的数据库是否满足可用性数据库的[先决条件](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。 如果未满足这些先决条件，会有简短的状态说明指出该数据库不合格的原因；例如，可能是因为它不使用完整恢复模式。 有关详细信息，请单击该状态说明。  
  
     如果数据库经过更改已经合格，请单击 **“刷新”** 以更新数据库网格。  
  
     如果数据库包含数据库主密钥，则请在“密码”列中输入数据库主密钥的密码。   
  
7.  在 **“指定副本”** 页上，为新的可用性组指定和配置一个或多个副本。 此页包含四个选项卡。 下表介绍了这些选项卡。 有关详细信息，请参阅[“指定副本”页（新建可用性组向导：添加副本向导）](../../../database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard.md)主题。  
  
    |选项卡|简要说明|  
    |---------|-----------------------|  
    |**副本**|使用此选项卡可以指定将承载辅助副本的每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 请注意，您当前连接的服务器实例必须承载主副本。|  
    |**端点**|使用此选项卡可以验证任何现有数据库镜像端点，此外，如果在其服务帐户使用 Windows 身份验证的服务器实例上缺少该端点，则会自动创建该端点。<br /><br /> 注意：如果任何服务器实例基于非域用户帐户运行，则你需要首先对你的服务器实例进行手动更改，然后才能在向导中继续执行。 有关详细信息，请参阅本主题前面的 [先决条件](#Prerequisites)。|  
    |**备份首选项**|使用此选项卡可以整体为可用性组指定您的备份首选项，并为各个可用性副本指定备份优先级。|  
    |**侦听器**|使用此选项卡可以创建可用性组侦听器。 默认情况下，该向导不创建侦听器。|  
  
8.  在 **“选择初始数据同步”** 页上，选择如何创建新的辅助数据库并将其联接到可用性组。 选择以下选项之一：  
  
    -   **自动种子设定**  
  
         SQL Server 自动为此组中的每个数据库创建次要副本。 自动种子设定要求数据和日志文件路径在参与此组的每个 SQL Server 实例上均相同。 可在 [!INCLUDE[sssql15-md.md](../../../includes/sssql15-md.md)] 及更高版本中使用。 请参阅[自动初始化 AlwaysOn 可用性组](automatically-initialize-always-on-availability-group.md)。
    
    -   **完整的数据库和日志备份**  
  
         如果你的环境满足自动启动初始数据同步的要求，则选择此选项（有关详细信息，请参阅本主题前面的 [先决条件、限制和建议](#Prerequisites)）。  
  
         如果选择 **“完全”** ，则在创建可用性组后，向导会将每个主数据库及其事务日志备份到网络共享，并在每个承载辅助副本的服务器实例上还原备份。 然后，该向导将每个辅助数据库联接到可用性组。  
  
         在“指定可由所有副本访问的共享网络位置”  字段中，指定承载副本的所有服务器都具有读写访问权限的备份共享。 有关详细信息，请参阅本主题前面的 [先决条件](#Prerequisites)。  在验证步骤中，向导将执行测试，确保所提供的网络位置有效，测试将在名为“BackupLocDb_”加 Guid 的主要副本上创建数据库，并对所提供的网络位置执行备份，然后在次要副本上进行还原。 在向导未能删除此数据库及其备份历史记录和备份文件情况下，将之删除是安全的操作。
  
    -   **仅联接**  
  
         如果在将承载辅助副本的服务器实例上手动准备了辅助数据库，则可以选择此选项。 该向导将每个现有辅助数据库联接到可用性组。  
  
    -   **跳过初始数据同步**  
  
         如果要使用您自己的数据库和主数据库的日志备份，请选择此选项。 有关详细信息，请参阅本主题后面的 [启动 AlwaysOn 辅助数据库的数据移动 (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)。  
  
9. **“验证”** 页验证在此向导中指定的值是否满足新建可用性组向导的要求。 若要进行更改，请单击 **“上一页”** 以返回前面的向导页，更改一个或多个值。 单击 **“下一步”** 返回到 **“验证”** 页，然后单击 **“重新运行验证”** 。  
  
10. 在 **“摘要”** 页上，查看您为新的可用性组进行的选择。 若要进行更改，请单击 **“上一步”** 以返回到相应页。 在进行更改后，单击 **“下一步”** 以返回到 **“摘要”** 页。  
  
    > [!IMPORTANT]  
    >  如果将要承载新的可用性副本的服务器实例的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户未作为登录名存在，则新建可用性组向导需要创建一个登录名。 在 **“摘要”** 页上，该向导将显示要创建的登录名的信息。 如果单击 **“完成”** ，则该向导将为 SQL Server 服务帐户创建该登录名，并授予该登录名 CONNECT 权限。  
  
     如果您满意所做的选择，可以选择单击 **“脚本”** 以创建向导将执行的步骤的脚本。 然后，若要创建和配置新的可用性组，请单击 **“完成”** 。  
  
11. **“进度”** 页将显示创建可用性组的各步骤（配置端点、创建可用性组和将辅助副本联接到该组）的进度。  
  
12. 在这些步骤完成后， **“结果”** 页将显示各步骤的结果。 如果所有这些步骤都成功，则新的可用性组得到了完全配置。 如果任何步骤导致错误，您可能需要手动完成配置或对失败的步骤使用向导。 有关给定错误的原因的信息，请单击 **“结果”** 列中关联的“错误”链接。  
  
     完成向导后，单击 **“关闭”** 以退出安装向导。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **完成可用性组配置**  
  
-   [将辅助副本联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [将辅助数据库联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
 **用于创建可用性组的其他方法**  
  
-   [使用“新建可用性组”对话框 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [创建可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [创建可用性组 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
 **启用 AlwaysOn 可用性组**  
  
-   [启用和禁用 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
 **配置数据库镜像端点**  
  
-   [为 AlwaysOn 可用性组创建数据库镜像终结点 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [为 Windows 身份验证创建数据库镜像终结点 (Transact-SQL)](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [使用数据库镜像终结点证书 (Transact-SQL)](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [在添加或修改可用性副本时指定终结点 URL (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **解决 AlwaysOn 可用性组配置问题**  
  
-   [AlwaysOn 可用性组配置故障排除 (SQL Server)](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
-   [添加文件操作失败的故障排除（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   **博客：**  
  
     [AlwaysOn - HADRON 学习系列：启用了 HADRON 的数据库的工作线程池用法](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
     [SQL Server AlwaysOn 团队博客：SQL Server AlwayOn 团队官方博客](/archive/blogs/sqlalwayson/)  
  
     [CSS SQL Server 工程师博客](/archive/blogs/psssql/)  
  
-   **视频：**  
  
     [Microsoft SQL Server Code-Named "Denali" AlwaysOn 系列，第一部分：介绍下一代高可用性解决方案](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)  
  
     [Microsoft SQL Server Code-Named "Denali" AlwaysOn 系列，第二部分：使用 AlwaysOn 生成关键任务高可用性解决方案](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **白皮书：**  
  
     [用于高可用性和灾难恢复的 Microsoft SQL Server AlwaysOn 解决方案指南](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [针对 SQL Server 2012 的 Microsoft 白皮书](https://msdn.microsoft.com/library/hh403491.aspx)  
  
     [SQL Server 客户咨询团队白皮书](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="alternate-ways-to-create-availability-groups"></a>用于创建可用性组的其他方法

除了使用新建可用性组向导之外，您还可以使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell cmdlet。 有关详细信息，请参阅 [创建可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md) 或 [创建可用性组 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)中创建和配置 AlwaysOn 可用性组。  


## <a name="see-also"></a>另请参阅  
 [数据库镜像终结点 (SQL Server)](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
