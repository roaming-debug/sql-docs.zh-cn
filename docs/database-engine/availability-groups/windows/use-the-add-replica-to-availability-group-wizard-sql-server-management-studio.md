---
title: 将副本添加到可用性组 (SSMS)
description: 使用 SQL Server Management Studio 中的向导将副本添加到 AlwaysOn 可用性组。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
f1_keywords:
- sql13.swb.addreplicawizard.f1
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], wizards
ms.assetid: 60d962b6-2af4-4394-9190-61939a102bc0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 99cfe654cf9540d2b031353f0487d783045dd0c9
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94583669"
---
# <a name="add-a-replica-to-your-always-on-availability-group-using-the-availability-group-wizard-in-sql-server-management"></a>使用 SQL Server Management 中的“可用性组向导”将副本添加到 AlwaysOn 可用性组
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  使用“将副本添加到可用性组向导”可帮助你将新的次要副本添加到现有 AlwaysOn 可用性组。  
  
> [!NOTE]  
>  有关使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 或 PowerShell 将辅助副本添加到可用性组的详细信息，请参阅 [将辅助副本添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)。  
    
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
 如果你从未向可用性组添加过任何可用性副本，请参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)中的“服务器实例”与“可用性组和副本”部分。  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   您必须连接到承载当前主副本的服务器实例。  
  
-   在添加次要副本前，请验证 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的主机实例与现有副本位于相同的 Windows Server 故障转移群集 (WSFC) 中，但驻留在不同的群集节点上。 此外，还请验证此服务器实例满足所有其他 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 先决条件。 有关详细信息，请参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)配置服务器实例时遇到的典型问题。  
  
-   如果您选择承载可用性副本的服务器实例正在以域用户帐户运行并且尚不具有数据库镜像端点，则此向导可以创建该端点并将 CONNECT 权限授予服务器实例的服务帐户。 但是，如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务正在以内置帐户（例如 Local System、Local Service 或 Network Service）或非域帐户运行，您必须使用证书来进行端点身份验证，并且该向导将无法在服务器实例上创建数据库镜像端点。 在此情况下，我们建议您首先手动创建数据库镜像端点，然后启动“将副本添加到可用性组向导”。  
  
     **使用数据库镜像端点证书：**  
  
     [CREATE ENDPOINT (Transact-SQL)](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
     [使用数据库镜像终结点证书 (Transact-SQL)](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   **使用完全初始数据同步的先决条件**  
  
    -   在承载可用性组的副本的每个服务器实例上，所有数据库文件路径都必须完全相同。  
  
    -   没有任何主数据库名称可存在于承载辅助副本的任何服务器实例上。 这意味着尚没有任何新的辅助数据库可以存在。  
  
    -   为了使该向导创建并访问备份，需要指定网络共享。 对于主副本，用于启动 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 的帐户必须对网络共享具有读写文件系统权限。 对于辅助副本，该帐户必须具有对网络共享区的读权限。  
  
     如果您无法使用该向导执行完全初始数据同步，则需要手动准备您的辅助数据库。 您可以在运行该向导之前或之后进行准备。 有关详细信息，请参阅 [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)或 PowerShell 将辅助数据库联接到 Always On 可用性组。  
   
## <a name="permissions"></a><a name="Permissions"></a> 权限  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
 如果要允许“将副本添加到可用性组向导”管理数据库镜像端点，还需要 CONTROL ON ENDPOINT 权限。  
  
##  <a name="using-the-add-replica-to-availability-group-wizard-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用“将副本添加到可用性组向导”(SQL Server Management Studio)  
 **使用“将副本添加到可用性组向导”**  
  
1.  在对象资源管理器中，连接到承载可用性组的主副本的服务器实例，然后展开服务器树。  
  
2.  依次展开“Always On 高可用性”节点和“可用性组”节点。  
  
3.  右键单击要向其添加辅助副本的可用性组，然后选择“添加副本”命令。 这将启动“将副本添加到可用性组向导”。  
  
4.  在 **“连接到现有的辅助副本”** 页上，连接到可用性组中的每个辅助副本。 有关详细信息，请参阅[“连接到现有次要副本”页（添加副本向导：添加数据库向导）](../../../database-engine/availability-groups/windows/connect-to-existing-secondary-replicas-page.md)。  
  
5.  在 **“指定副本”** 页上，为可用性组指定和配置一个或多个新的辅助副本。 此页包含三个选项卡。 下表介绍了这些选项卡。 有关详细信息，请参阅[“指定副本”页（新建可用性组向导：添加副本向导）](../../../database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard.md)。  
  
    |选项卡|简要说明|  
    |---------|-----------------------|  
    |**副本**|使用此选项卡可以指定将承载新的辅助副本的每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。|  
    |**端点**|使用此选项卡可验证每个新的辅助副本的现有数据库镜像端点（如果有）。 如果在其服务帐户使用 Windows 身份验证的服务器实例上缺少该端点，则该向导会自动创建该端点。<br /><br /> <br /><br /> 注意：如果任何服务器实例基于非域用户帐户运行，则你需要首先对你的服务器实例进行手动更改，然后才能在向导中继续执行。 有关详细信息，请参阅本主题前面的 [先决条件](#Prerequisites)。|  
    |**备份首选项**|使用此选项卡可以整体为可用性组指定您的备份首选项；如果您想要修改当前设置，还可为各个可用性副本指定备份优先级。|  
  
6.  如果所选的副本包含具有数据库主密钥的数据库，请在“密码”列中输入数据库主密钥的密码。  “状态”列指示具有数据库主密钥的数据库  “所需的密码”。 在“密码”列中输入正确的密码之前，“下一步”将显示为灰色 。 输入密码后，请单击 “刷新”。 如果正确输入密码，则“状态”列会指示“密码已输入”，然后便可进行“下一步”操作 。  
  
7.  在 **“选择初始数据同步”** 页上，选择如何创建新的辅助数据库并将其联接到可用性组。 选择以下选项之一：  
  
    -   **完整**  
  
         如果你的环境满足自动启动初始数据同步的要求，则选择此选项（有关详细信息，请参阅本主题前面的 [先决条件、限制和建议](#Prerequisites)）。  
  
         如果选择 **“完全”** ，则在创建可用性组后，向导会将每个主数据库及其事务日志备份到网络共享，并在每个承载新的辅助副本的服务器实例上还原备份。 然后，该向导将每个新的辅助数据库联接到可用性组。  
  
         在“指定可由所有副本访问的共享网络位置”字段中，指定承载副本的所有服务器都具有读写访问权限的备份共享。 日志备份将是您的日志备份链的一部分。 适当地存储日志备份文件。  
  
        > [!IMPORTANT]  
        >  有关所需文件系统权限的详细信息，请参阅本主题中前面的 [先决条件](#Prerequisites)部分。  
  
    -   **仅联接**  
  
         如果在将承载新的辅助副本的服务器实例上手动准备了辅助数据库，则可以选择此选项。 该向导将这些新的现有辅助数据库联接到可用性组。  
  
    -   **跳过初始数据同步**  
  
         如果要使用您自己的数据库和主数据库的日志备份，请选择此选项。 有关详细信息，请参阅本主题后面的 [启动 AlwaysOn 辅助数据库的数据移动 (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)。  
  
8.  **“验证”** 页验证在此向导中指定的值是否满足“将副本添加到可用性组向导”的要求。 若要进行更改，请单击 **“上一页”** 以返回前面的向导页，更改一个或多个值。 单击 **“下一步”** 返回到 **“验证”** 页，然后单击 **“重新运行验证”** 。  
  
9. 在 **“摘要”** 页上，查看您为新的可用性组进行的选择。 若要进行更改，请单击 **“上一步”** 以返回到相应页。 在进行更改后，单击 **“下一步”** 以返回到 **“摘要”** 页。  
  
     如果您满意所做的选择，可以选择单击“脚本”以创建向导将执行的步骤的脚本。 然后，若要创建和配置新的可用性组，请单击 **“完成”** 。  
  
10. **“进度”** 页将显示创建可用性组的各步骤（配置端点、创建可用性组和将辅助副本联接到该组）的进度。  
  
11. 在这些步骤完成后， **“结果”** 页将显示各步骤的结果。 如果所有这些步骤都成功，则新的可用性组得到了完全配置。 如果任何步骤导致错误，您可能需要手动完成配置。 有关给定错误的原因的信息，请单击 **“结果”** 列中关联的“错误”链接。  
  
     完成向导后，单击 **“关闭”** 以退出安装向导。  
  
> [!IMPORTANT]  
>  添加副本后，请参阅[将次要副本添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md) 中的“跟进：添加副本后”部分。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [将辅助副本添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [将辅助副本添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
  
