---
title: 可用性组：高可用性和灾难恢复解决方案
description: Always On 可用性组是 SQL Server 高可用性和灾难恢复解决方案，可以提供替代数据库镜像的企业级方案，具有更强大的功能。 了解此功能的基本信息和基本功能。
ms.custom: seodec18
ms.date: 04/23/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], about
- secondary replicas, see Availability Groups [SQL Server]
- availability [SQL Server]
- AlwaysOn [SQL Server], see Availability Groups [SQL Server]
- Availability Groups [SQL Server]
ms.assetid: aa427606-8422-4656-b205-c9e665ddc8c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6c6b5290db30ce42df920bac56cf38d8c130039a
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584816"
---
# <a name="always-on-availability-groups-a-high-availability-and-disaster-recovery-solution"></a>Always On 可用性组：高可用性和灾难恢复解决方案
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 功能是一个提供替代数据库镜像的企业级方案的高可用性和灾难恢复解决方案。 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 中引入了 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]功能，此功能可最大程度地提高一组用户数据库对企业的可用性。 “可用性组”  针对一组离散的用户数据库（称为“可用性数据库”  ，它们共同实现故障转移）支持故障转移环境。 一个可用性组支持一组读写主数据库以及一至八组对应的辅助数据库。 （可选）可使辅助数据库能进行只读访问和/或某些备份操作。  
  
 可用性组在可用性副本级别进行故障转移。 故障转移不是由诸如因数据文件丢失而使数据库成为可疑数据库、删除数据库或事务日志损坏等此类数据库问题导致的。  
 
 >[!NOTE]
 >此可用性功能的完整正式名称为 AlwaysOn 可用性组。 其缩写为 AG，而非 AOAG 或 AAG。 
  
##  <a name="benefits"></a><a name="Benefits"></a> 优势  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 提供了一组丰富的选项来提高数据库的可用性并改进资源使用情况。 主要组件如下：  
  
-   支持最多九个可用性副本。 “可用性副本”  是可用性组的实例化，此可用性组由特定的 SQL Server 实例承载，该实例维护属于此可用性组的每个可用性数据库的本地副本。 每个可用性组都支持一个主副本和最多八个辅助副本。 有关详细信息，请参阅： [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。  
  
    > [!IMPORTANT]  
    >  每个可用性副本都必须驻留在单个 Windows Server 故障转移群集 (WSFC) 群集的不同节点中。 有关可用性组的先决条件、限制和建议的详细信息，请参阅 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。  
  
-   支持替代可用性模式，如下所示：  
  
    -   异步提交模式  。 此可用性模式是一种灾难恢复解决方案，适合于可用性副本的分布距离较远的情况。  
  
    -   同步提交模式  。 此可用性模式相对于性能而言更强调高可用性和数据保护，为此付出的代价是事务延迟时间增加。 一个给定的可用性组可支持最多三个同步提交可用性副本（包括当前主副本）。  
  
     有关详细信息，请参阅 [可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)。 

     [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] 将同步副本的最大数目从 [!INCLUDE[ssSQL17](../../../includes/sssql17-md.md)] 中的 3 增加到了 5。 可以配置此组的 5 个副本在该组中进行自动故障转移。 有 1 个主要副本以及 4 个同步的次要副本。
  
-   支持几种形式的可用性组故障转移：自动故障转移、计划的手动故障转移（通常简称为“手动故障转移”）和强制的手动故障转移（通常简称为“强制故障转移”）。 有关详细信息，请参阅 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)概念。  
  
-   使您能够将给定的可用性副本配置为支持以下一种或两种活动辅助功能：  
  
    -   利用只读连接访问，与副本的只读连接可以在此副本作为辅助副本运行时访问和读取其数据库。 有关详细信息，请参阅 [活动辅助副本：可读辅助副本（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)概念。  
  
    -   当副本作为辅助副本运行时，对副本的数据库执行备份操作。 有关详细信息，请参阅 [活动辅助副本：辅助副本备份（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)概念。  
  
     通过使用活动辅助功能，可更好地利用辅助硬件资源，从而提高 IT 效率并降低成本。 此外，通过将读意向应用程序和备份作业转移到辅助副本，有助于提高针对主副本的性能。  
  
-   支持每个可用性组的可用性组侦听器。 “可用性组侦听程序”  是一个服务器名称，客户端可连接到此服务器以访问 AlwaysOn 可用性组的主要副本或次要副本中的数据库。 可用性组侦听器将传入连接定向到主副本或只读辅助副本。 侦听器在可用性组故障转移后提供快速应用程序故障转移。 有关详细信息，请参阅 [可用性组侦听程序、客户端连接和应用程序故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)概念。  
  
-   支持灵活的故障转移策略以便更好地控制可用性组故障转移。 有关详细信息，请参阅 [故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)概念。  
  
-   支持用于避免页损坏的自动页修复。 有关详细信息，请参阅[自动页修复（可用性组：数据库镜像）](../../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)。  
  
-   支持加密和压缩，这提供了安全且高性能的传输方式。  
  
-   提供了一组集成的工具来简化部署和管理可用性组，这些工具包括：  
  
    -   [!INCLUDE[tsql](../../../includes/tsql-md.md)] DDL 语句。 有关详细信息，请参阅： [AlwaysOn 可用性组的 Transact-SQL 语句概述 (SQL Server)](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md)。  
  
    -   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 工具，如下所示：  
  
        -   [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] 创建和配置可用性组。 在某些环境中，此向导还可以自动准备辅助数据库并且为每个数据库启动数据同步。 有关详细信息，请参阅[使用“新建可用性组”对话框 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)。  
  
        -   [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] 向现有可用性组添加一个或多个主数据库。 在某些环境中，此向导还可以自动准备辅助数据库并且为每个数据库启动数据同步。 有关详细信息，请参阅 [使用“将数据库添加到可用性组向导”(SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)。  
  
        -   [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)] 向现有可用性组添加一个或多个辅助副本。 在某些环境中，此向导还可以自动准备辅助数据库并且为每个数据库启动数据同步。 有关详细信息，请参阅[使用“将副本添加到可用性组向导”(SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)。  
  
        -   [!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)] 启动对可用性组的手动故障转移。 根据您指定为故障转移目标的辅助副本的配置和状态，该向导可以指定计划的手动故障转移或强制手动故障转移。 有关详细信息，请参阅本主题后面的 [使用故障转移可用性组向导 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)或 PowerShell 对 AlwaysOn 可用性组执行强制故障转移（可能会丢失数据）。  
  
    -   [!INCLUDE[ssAoDash](../../../includes/ssaodash-md.md)] 监视 AlwaysOn 可用性组、可用性副本和可用性数据库，并且评估 AlwaysOn 策略的结果。 有关详细信息，请参阅[使用 AlwaysOn 面板 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)。  
  
    -   “对象资源管理器详细信息”窗格显示有关现有可用性组的基本信息。 有关详细信息，请参阅[使用对象资源管理器详细信息监视可用性组 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)。  
  
    -   PowerShell cmdlet。 有关详细信息，请参阅： [AlwaysOn 可用性组的 PowerShell Cmdlet 概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md)。  
  
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> 术语和定义  
 可用性组   
 一个容器，用于一组共同实现故障转移的数据库（“可用性数据库”  ）。  
  
 可用性数据库   
 属于可用性组的数据库。 对于每个可用性数据库，可用性组将保留一个读写副本（“主数据库”  ）和一个到八个只读副本（“辅助数据库”  ）。  
  
 主数据库   
 可用性数据库的读写副本。  
  
 辅助数据库   
 可用性数据库的只读副本。  
  
 可用性副本   
 可用性组的实例化，该可用性组由特定的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例承载，并维护属于该可用性组的每个可用性数据库的本地副本。 存在两种类型的可用性副本：一个 *主副本* 和一至八个 *辅助副本*。  
  
 主要副本   
 使主数据库可用于来自客户端的读写连接并用于将每个主数据库的事务日志记录发送到每个辅助副本的可用性副本。  
  
 次要副本   
 维护各可用性数据库的辅助副本的可用性副本，充当可用性组的潜在故障转移目标。 或者，辅助副本可以支持对辅助数据库进行只读访问，并支持对辅助数据库创建备份。  
  
 可用性组侦听器   
 一个服务器名称，客户端可连接到此服务器以访问 AlwaysOn 可用性组的主要副本或次要副本中的数据库。 可用性组侦听器将传入连接定向到主副本或只读辅助副本。  
  
> [!NOTE]  
>  有关详细信息，请参阅： [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。  
  
##  <a name="interoperability-and-coexistence-with-other-database-engine-features"></a><a name="Interoperability"></a> 与其他数据库引擎功能的互操作性和共存  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 可与以下 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]功能和组件一起使用：  
  
-   [关于变更数据捕获 (SQL Server)](../../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
-   [关于更改跟踪 (SQL Server)](../../../relational-databases/track-changes/about-change-tracking-sql-server.md)  
  
-   [包含的数据库](../../../relational-databases/databases/contained-databases.md)  
  
-   [数据库加密](../../../relational-databases/security/encryption/transparent-data-encryption.md)  
  
-   [数据库快照](../../../database-engine/availability-groups/windows/database-snapshots-with-always-on-availability-groups-sql-server.md)  
  
-   [FILESTREAM](../../../relational-databases/blob/filestream-sql-server.md)  
  
-   [FileTable](../../../relational-databases/blob/filetables-sql-server.md)  
  
-   [日志传送](../../../database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
-   [远程 Blob 存储区 (RBS)](../../../relational-databases/blob/remote-blob-store-rbs-sql-server.md)  
  
-   [复制](../../../relational-databases/replication/sql-server-replication.md)  
  
-   [Service Broker](../../../database-engine/configure-windows/sql-server-service-broker.md)  
  
-   [SQL Server 代理](../../../ssms/agent/sql-server-agent.md)  
  
-   [Reporting Services](../../../database-engine/availability-groups/windows/reporting-services-with-always-on-availability-groups-sql-server.md)  
  
> [!WARNING]  
>  有关通过 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 使用其他功能的限制和局限性的详细信息，请参阅 [AlwaysOn 可用性组：互操作性 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [AlwaysOn 可用性组入门 (SQL Server)](../../../database-engine/availability-groups/windows/getting-started-with-always-on-availability-groups-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   **博客：**  
  
     [SQL Server AlwaysOn 团队博客：SQL Server AlwayOn 团队官方博客](/archive/blogs/sqlalwayson/)  
  
     [CSS SQL Server 工程师博客](/archive/blogs/psssql/)  
  
-   **视频：**  
  
     [Microsoft SQL Server Code-Named "Denali" AlwaysOn 系列，第一部分：介绍下一代高可用性解决方案](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)  
  
     [Microsoft SQL Server Code-Named "Denali" AlwaysOn 系列，第二部分：使用 AlwaysOn 生成关键任务高可用性解决方案](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **白皮书：**  
  
     [用于高可用性和灾难恢复的 Microsoft SQL Server AlwaysOn 解决方案指南](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [针对 SQL Server 2012 的 Microsoft 白皮书](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [SQL Server 客户咨询团队白皮书](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [为 AlwaysOn 可用性组配置服务器实例 (SQL Server)](../../../database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md)   
 [创建和配置可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [管理可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server.md)   
 [监视可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [AlwaysOn 可用性组的 Transact-SQL 语句概述 (SQL Server)](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md)   
 [AlwaysOn 可用性组的 PowerShell Cmdlet 概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md)  
  
