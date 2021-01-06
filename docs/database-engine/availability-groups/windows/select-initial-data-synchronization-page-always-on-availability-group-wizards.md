---
title: “选择初始数据同步”页面（可用性组向导）
description: 介绍 SQL Server Management Studio (SSMS) 中“Always on 可用性组向导”的“选择初始数据同步”页面。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.adddatabasewizard.selectinitialdatasync.f1
- sql13.swb.newagwizard.selectinitialdatasync.f1
- sql13.swb.addreplicawizard.selectinitialdatasync.f1
ms.assetid: 457b1140-4819-4def-8f7c-54a406e6db12
author: cawrites
ms.author: chadam
ms.openlocfilehash: fd729c01993f06c9017a898e1e804dde844e10f2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642143"
---
# <a name="select-initial-data-synchronization-page-always-on-availability-group-wizards"></a>“选择初始数据同步”页（AlwaysOn 可用性组向导）
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  使用 AlwaysOn“选择初始数据同步”  页可为新的辅助数据库的初始数据同步指示你的首选项。 此页为三个向导所共有：[!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]、[!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)] 和 [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)]。  
  
 可能的选项包括“自动种子设定”、“完整的数据库和日志备份”、“仅联接”或“跳过初始数据同步”     。 选择“自动种子设定”、“完整”、或“仅联接”之前，请确保环境符合先决条件    。  
    
##  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   在初始数据同步过程中挂起主数据库的日志备份任务。  
  
-   对于大型数据库，完整备份和还原操作可能占用大量的时间和资源。 在这种情况下，我们建议您自行准备辅助数据库。 有关详细信息，请参阅本主题后面的 [手动准备辅助数据库](#PrepareSecondaryDbs)。  
  
-   完全初始数据同步要求您指定网络共享。 在使用向导执行完整初始数据同步之前，建议您对网络共享文件夹的访问权限实施安全计划。 此预防措施很重要，因为备份文件中潜在敏感的数据可由对该文件夹具有“读”权限的任何人访问。 此外，若要保护备份操作和还原操作，建议您对每个承载可用性副本的服务器实例和网络共享文件夹之间的网络通道提供适当的保护。  
  
     如果必须高度保护您的备份操作和还原操作，建议您选择 **“仅加入”** 或 **“跳过初始数据同步”** 选项。  
  
## <a name="automatic-seeding"></a><a name="Auto"></a>自动种子设定
 
 SQL Server 自动为此组中的每个数据库创建次要副本。 自动种子设定要求数据和日志文件路径在参与此组的每个 SQL Server 实例上均相同。 可在 [!INCLUDE[sssql15-md.md](../../../includes/sssql15-md.md)] 及更高版本中使用。 请参阅[自动初始化 AlwaysOn 可用性组](automatically-initialize-always-on-availability-group.md)。

##  <a name="full-database-and-log-backup"></a><a name="Full"></a>完整的数据库和日志备份 
 对于每个主数据库，“完整的数据库和日志备份”选项将在一个工作流中执行以下若干操作：创建主数据库的完整备份和日志备份、通过在托管次要副本的每个服务器实例上还原这些备份来创建对应的辅助数据库，以及将每个辅助数据库联接到可用性组  。  
  
 仅当您的环境符合使用完全初始数据同步的以下先决条件且您希望该向导自动启动数据同步时，才选择此选项。  
  
 **使用完整的数据库和日志备份初始数据同步的先决条件**  
  
-   在承载可用性组的副本的每个服务器实例上，所有数据库文件路径都必须完全相同。  
  
    > [!NOTE]  
    >  如果您运行该向导的服务器实例和要承载辅助副本的任何服务器实例之间的备份和还原文件路径不同， 必须使用 WITH MOVE 选项手动执行备份和还原操作。 有关详细信息，请参阅本主题后面的 [手动准备辅助数据库](#PrepareSecondaryDbs)。  
  
-   没有任何主数据库名称可存在于承载辅助副本的任何服务器实例上。 这意味着尚没有任何新的辅助数据库可以存在。  
  
-   为了使该向导创建并访问备份，需要指定网络共享。 对于主副本，用于启动 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 的帐户必须对网络共享具有读写文件系统权限。 对于辅助副本，该帐户必须具有对网络共享区的读权限。  
  
    > [!IMPORTANT]  
    >  日志备份将是您的日志备份链的一部分。 适当地存储日志备份文件。  
  
 **如果未满足先决条件**  
  
 向导不能为此可用性组创建辅助数据库。 有关如何准备辅助数据库的详细信息，请参阅本主题后面的 [手动准备辅助数据库](#PrepareSecondaryDbs)。  
  
 **如果满足先决条件**  
  
 如果完全满足这些先决条件并且需要向导执行完整初始数据同步，请选择“完整的数据库和日志备份”选项并指定网络共享  。 这将导致向导创建每个所选数据库的完整的数据库和日志备份，并将这些备份放置于您指定的网络共享上。 然后，在承载新的辅助副本之一的每个服务器实例上，该向导将通过使用 RESTORE WITH NORECOVERY 还原备份以创建辅助数据库。 创建每个辅助数据库之后，该向导将新的辅助数据库加入可用性组中。 加入辅助数据库后，将在该数据库上启动数据同步。  
  
 **指定所有副本可访问的共享网络位置**  
 若要创建和还原备份，该向导要求您指定一个网络共享。 用于在承载可用性副本的每个服务器实例上启动 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 的帐户必须对网络共享具有读写文件系统权限。  
  
> [!IMPORTANT]  
>  日志备份将是您的日志备份链的一部分。 适当地存储其备份文件。  
  
##  <a name="join-only"></a><a name="Joinonly"></a> “仅加入”  
 仅当每个承载可用性组的辅助副本的服务器实例上已存在新的辅助数据库时，才选择此选项。 有关准备辅助数据库的信息，请参阅本主题后面的 [手动准备辅助数据库](#PrepareSecondaryDbs)。  
  
 如果您选择 **“仅加入”** ，则该向导将尝试将每个现有辅助数据库加入可用性组中。  
  
## <a name="skip-initial-data-synchronization"></a><a name="Skip"></a> “跳过初始数据同步”  
 如果您希望自行执行每个主数据库的数据库备份和日志备份，并将它们还原到每个承载辅助副本的服务器实例，则选择此选项。 退出向导后，您需要加入每个辅助副本上的每个辅助数据库。  
  
> [!NOTE]  
>  有关详细信息，请参阅本主题后面的 [启动 AlwaysOn 辅助数据库的数据移动 (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)。  
  
##  <a name="to-prepare-secondary-databases-manually"></a><a name="PrepareSecondaryDbs"></a> 手动准备辅助数据库  
 若要独立于任何 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 向导准备辅助数据库，可以使用下列方法之一：  
  
-   使用 RESTORE WITH NORECOVERY 手动还原主数据库的最新数据库备份，然后使用 RESTORE WITH NORECOVERY 还原各个后续日志备份。 如果主数据库和辅助数据库具有不同的文件路径，则必须使用 WITH MOVE 选项。 在每个承载可用性组的辅助副本的服务器实例上执行此还原序列。  您可以使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 或 PowerShell 执行这些备份和还原操作。  
  
     **详细信息：**  
  
     [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   如果您在将一个或多个日志传送主数据库添加到可用性组，则可能能够将一个或多个相应的辅助数据库从日志传送迁移到 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]。 有关详细信息，请参阅本主题后面的 [从日志传送迁移到 AlwaysOn 可用性组的先决条件 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)。  
  
    > [!NOTE]  
    >  在您为可用性组创建了所有辅助数据库后，如果您想要在辅助副本上执行备份，将需要重新配置该可用性组的自动备份首选项。  
  
     **详细信息：**  
  
     [从日志传送迁移到 AlwaysOn 可用性组的先决条件 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)  
  
     [配置可用性副本备份 (SQL Server)](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
 创建辅助数据库后，将所有当前日志备份应用于新的辅助数据库。  
  
 或者，您也可以在运行向导前准备所有辅助数据库。 然后，在向导的 **“指定初始数据同步”** 页上，选择 **“仅加入”** 选项以便自动将新的辅助数据库加入该可用性组。  
  
##  <a name="related-tasks"></a><a name="LaunchWiz"></a> 相关任务  
  
-   [使用“新建可用性组”对话框 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [使用“将副本添加到可用性组向导”(SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [使用“将数据库添加到可用性组向导”(SQL Server Management Studio)](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
-   [使用故障转移可用性组向导 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)  
  
-   [启动 AlwaysOn 辅助数据库的数据移动 (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)  
  
-   [将辅助数据库联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [使用“新建可用性组”对话框 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
