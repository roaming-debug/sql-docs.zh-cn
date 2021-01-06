---
title: 使用“可用性组向导”将数据库添加到可用性组
description: 使用 SQL Server Management Studio 中的“可用性组向导”将数据库添加到 Always On 可用性组。
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
f1_keywords:
- sql13.swb.adddatabasewizard.f1
helpviewer_keywords:
- Availability Groups [SQL Server], wizards
- Availability Groups [SQL Server], databases
ms.assetid: 81e5e36d-735d-4731-8017-2654673abb88
author: cawrites
ms.author: chadam
ms.openlocfilehash: e26d1dbf4b6599403beed4262eedc89eba2b0dd2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641241"
---
# <a name="add-a-database-to-an-always-on-availability-group-with-the-availability-group-wizard"></a>使用“可用性组向导”将数据库添加到 Always On 可用性组
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  使用“将数据库添加到可用性组向导”可帮助你将一个或多个数据库添加到现有的 Always On 可用性组。  
  
> [!NOTE]  
>  有关使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 或 PowerShell 将次要副本添加到数据库的信息，请参阅 [将数据库添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)。  
  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
 如果你从未向可用性组添加过数据库，请参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。  
  
##  <a name="prerequisites-restrictions-and-recommendations"></a><a name="Prerequisites"></a> 先决条件、限制和建议  
  
-   您必须连接到承载当前主副本的服务器实例。  
  
-   **使用完全初始数据同步的先决条件**  
  
    -   在承载可用性组的副本的每个服务器实例上，所有数据库文件路径都必须完全相同。  
  
    -   没有任何主数据库名称可存在于承载辅助副本的任何服务器实例上。 这意味着尚没有任何新的辅助数据库可以存在。  
  
    -   为了使该向导创建并访问备份，需要指定网络共享。 对于主副本，用于启动 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 的帐户必须对网络共享具有读写文件系统权限。 对于辅助副本，该帐户必须具有对网络共享区的读权限。  
  
     如果您无法使用该向导执行完全初始数据同步，则需要手动准备您的辅助数据库。 您可以在运行该向导之前或之后进行准备。 有关详细信息，请参阅 [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)或 PowerShell 将辅助数据库联接到 Always On 可用性组。  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。  
  
##  <a name="use-the-new-availability-group-wizard"></a>使用“新建可用性组”向导
  
1.  在对象资源管理器中，连接到承载可用性组的主副本的服务器实例，然后展开服务器树。  
  
2.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
3.  右键单击要向其添加数据库的可用性组，然后选择“添加数据库”  命令。 该命令将启动“将数据库添加到可用性组向导”。  
  
4.  在 **“选择数据库”** 页上，选择一个或多个数据库。 有关详细信息，请参阅[“选择数据库”页（新建可用性组向导和添加数据库向导）](../../../database-engine/availability-groups/windows/select-databases-page-new-availability-group-wizard-and-add-database-wizard.md)。  
  
     如果数据库包含数据库主密钥，则请在“密码”列中输入数据库主密钥的密码。   
  
5.  在 **“选择初始数据同步”** 页上，选择如何创建新的辅助数据库并将其联接到可用性组。 选择以下选项之一：  

    - **自动种子设定**
      
      选择此选项可使用自动种子设定。 自动种子设定使用日志流传输将使用 VDI 的备份流式传输到使用所配置终结点的可用性组的每个数据库的次要副本。 无需手动执行此操作即可还原次要副本上的数据库备份。 有关自动种子设定的详细信息，请参阅[自动种子设定 ](automatic-seeding-secondary-replicas.md)。
  
    -   **完整**  
  
         如果你的环境满足自动启动初始数据同步的要求，则选择此选项（有关详细信息，请参阅本主题前面的 [先决条件、限制和建议](#Prerequisites)）。  
  
         如果选择 **“完全”** ，则在创建可用性组后，向导会尝试将每个主数据库及其事务日志备份到网络共享，并在每个承载辅助副本的服务器实例上还原备份。 然后，该向导将每个辅助数据库联接到可用性组。  
  
         在“指定可由所有副本访问的共享网络位置”  字段中，指定承载副本的所有服务器都具有读写访问权限的备份共享。 日志备份将是您的日志备份链的一部分。 适当地存储日志备份文件。  
  
        > [!IMPORTANT]  
        >  有关所需文件系统权限的详细信息，请参阅本主题中前面的 [先决条件](#Prerequisites)部分。  
  
    -   **仅联接**  
  
         如果在将承载辅助副本的服务器实例上手动准备了辅助数据库，则可以选择此选项。 该向导将每个现有辅助数据库联接到可用性组。  
  
    -   **跳过初始数据同步**  
  
         如果要使用您自己的数据库和主数据库的日志备份，请选择此选项。 有关详细信息，请参阅本主题后面的 [启动 AlwaysOn 辅助数据库的数据移动 (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)。  
  
     有关详细信息，请参阅[“选择初始数据同步”页（Always On 可用性组向导）](../../../database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards.md)。  
  
6.  在 **“连接到现有的辅助副本”** 页上，如果承载该可用性组的可用性副本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例全部作为相同用户帐户中的某个服务运行，则单击 **“全部连接”** 。 如果任何服务器实例作为不同帐户下的某个服务运行，则单击每个服务器实例名称右侧的各个 **“连接”** 按钮。  
  
     有关详细信息，请参阅[连接到现有的辅助副本页（添加副本向导：添加数据库向导）](../../../database-engine/availability-groups/windows/connect-to-existing-secondary-replicas-page.md)。  
  
7.  **“验证”** 页验证在此向导中指定的值是否满足新建可用性组向导的要求。 若要进行更改，可以单击 **“上一页”** 以返回前面的向导页，更改一个或多个值。 单击 **“下一步”** 返回到 **“验证”** 页，然后单击 **“重新运行验证”** 。  
  
     有关详细信息，请参阅[“验证”页（Always On 可用性组向导）](../../../database-engine/availability-groups/windows/validation-page-always-on-availability-group-wizards.md)。  
  
8.  在 **“摘要”** 页上，查看您为新的可用性组进行的选择。 若要进行更改，请单击 **“上一步”** 以返回到相应页。 在进行更改后，单击 **“下一步”** 以返回到 **“摘要”** 页。  
  
     有关详细信息，请参阅[“综述”页（Always On 可用性组向导）](../../../database-engine/availability-groups/windows/summary-page-always-on-availability-group-wizards.md)。  
  
     如果您满意所做的选择，可以选择单击“脚本”以创建向导将执行的步骤的脚本。 然后，若要创建和配置新的可用性组，请单击 **“完成”** 。  
  
9. **“进度”** 页将显示创建可用性组的各步骤（配置端点、创建可用性组和将辅助副本联接到该组）的进度。  
  
     有关详细信息，请参阅[“进度”页（Always On 可用性组向导）](../../../database-engine/availability-groups/windows/progress-page-always-on-availability-group-wizards.md)。  
  
10. 在这些步骤完成后， **“结果”** 页将显示各步骤的结果。 如果所有这些步骤都成功，则新的可用性组得到了完全配置。 如果任何步骤导致错误，您可能需要手动完成配置。 有关给定错误的原因的信息，请单击 **“结果”** 列中关联的“错误”链接。  
  
     完成向导后，单击 **“关闭”** 以退出安装向导。  
  
     有关详细信息，请参阅 [“结果”页（AlwaysOn 可用性组向导）](../../../database-engine/availability-groups/windows/results-page-always-on-availability-group-wizards.md)或 PowerShell 来对 AlwaysOn 可用性组执行计划的手动故障转移或强制的手动故障转移（强制故障转移）。  
  
11. 如果在所有辅助数据库上未自动启动初始数据同步，则需要配置任何尚未加入的辅助数据库。 有关详细信息，请参阅本主题后面的 [启动 AlwaysOn 辅助数据库的数据移动 (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [将辅助数据库联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [将数据库添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)   
 [启动 AlwaysOn 辅助数据库的数据移动 (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)   
 [将数据库添加到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)  
  
  
