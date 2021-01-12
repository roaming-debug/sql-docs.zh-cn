---
title: 选择数据库引擎升级方法 | Microsoft Docs
description: 本文介绍 SQL Server 中数据库引擎的升级途径，包括就地升级、迁移到新安装及滚动升级。
ms.custom: ''
ms.date: 07/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 5e57a427-2e88-4ef6-b142-4ccad97bcecc
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c36d6718455f85314f98219b860a801984804d9d
ms.sourcegitcommit: 3ec49252e82590de0fe559a8574606ae213f6f3b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/07/2021
ms.locfileid: "97975435"
---
# <a name="choose-a-database-engine-upgrade-method"></a>选择数据库引擎升级方法
[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

当为了最小化停机时间和风险而计划将 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 从 SQL Server 的先前版本进行升级时，有几种方法可以考虑。 你可以执行就地升级、迁移到新安装或者执行滚动升级。 下面的图表将帮助你在这些方法中进行选择。 图表中的每个方法也会在下面进行讨论。 为了有助于你了解图表中的决策点，也请查阅 [计划并测试数据库引擎升级计划](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md)。  
  
 ![数据库引擎升级方法决策树](../../database-engine/install-windows/media/database-engine-upgrade-method-decision-tree.png "数据库引擎升级方法决策树")  
  
 **下载**  
  
-   若要下载 [!INCLUDE[SSnoversion](../../includes/ssnoversion-md.md)]，请转到  **[评估中心](https://www.microsoft.com/evalcenter/evaluate-sql-server)** 。  
  
-   已经拥有 Azure 帐户？  然后转到 **[此处](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=overview)** 启动装有 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 开发人员版的虚拟机。  
  
> [!NOTE]  
>  你也可以考虑升级 Azure SQL 数据库或虚拟化 SQL Server 环境作为你升级计划的一部分。 这些文章已超出本文的范围，但这里有一些链接：
>   - [Azure 虚拟机上 SQL Server 的概述](https://azure.microsoft.com/services/virtual-machines/sql-server/#overview)
>   - [Azure SQL 数据库](https://azure.microsoft.com/services/sql-database/) 
>   - [选择 Azure 中的 SQL Server 选项](/azure/azure-sql/azure-sql-iaas-vs-paas-what-is-overview)。  
  
## <a name="upgrade-in-place"></a>就地升级  
 使用此方法时，SQL Server 安装程序会通过将现有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 位替换为新的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 位来升级现有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装，然后升级每个系统和用户数据库。  就地升级方法是最简单的，需要一定的停机时间，如果需要进行回退的话，则会花费更长时间进行回退操作，且并非所有方案都支持这一方法。 有关支持和不支持就地升级方法的方案详细信息，请参阅 [支持的版本升级](../../database-engine/install-windows/supported-version-and-edition-upgrades-version-15.md)。  
  
 这种方法经常用于以下方案：  
  
-   不需要高可用性 (HA) 配置的开发环境。  
  
-   能够容忍停机时间且在最新的硬件和软件上运行的非任务关键生产的环境。 停机时间量取决于你的数据库大小和 I/O 子系统速度。 在内存优化表处于使用中时升级 SQL Server 2014 会耗费一些额外时间。 有关详细信息，请参阅 [计划并测试数据库引擎升级计划](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md)。  

> [!NOTE]
> 有关 Microsoft 如何使用此策略的示例，请参阅以下博客文章：[是否升级](https://azure.microsoft.com/blog/azure-sql-server-to-upgrade-or-not-to-upgrade-that-is-the-question/)。 


> [!WARNING]  
>  在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序时，作为运行预升级检查的一部分，将停止并重启 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
> [!CAUTION]  
>  在升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]后，早期的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例将被覆盖，在计算机中不再存在。 因此在升级前，请备份 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库以及与早期的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例相关的其他对象。  
  
 以下图表提供了 [!INCLUDE[ssDE](../../includes/ssde-md.md)]就地升级所需步骤的高级概述。  
  
 ![数据库引擎升级非 HA 就地升级](../../database-engine/install-windows/media/database-engine-upgrade-non-ha-in-place-upgrade.png "数据库引擎升级非 HA 就地升级")  
  
 有关详细步骤，请参阅[使用安装向导（安装程序）升级 SQL Server](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)。  
  
## <a name="migrate-to-a-new-installation"></a>迁移到新安装  
 使用此方法时，你需要在建立新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 环境的同时，在新硬盘上使用新版操作系统频繁地维护当前环境。 在新环境中安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 后，执行若干步骤来准备新环境，以便你能够从现有环境将现有的用户数据库迁移到新环境并且最小化停机时间。 这些步骤包括迁移下列对象：  
  
-   **系统对象：** 某些应用程序依赖于单个用户数据库范围之外的信息、实体和/或对象。 通常，应用程序具有对 master 和 msdb 数据库的依赖关系，并且还具有对用户数据库的依赖关系。 用户数据库正确运行所需的存储在该数据库外部的任何内容必须在目标服务器实例上可用。 例如，应用程序的登录名作为元数据存储在 master 数据库中，你必须在目标服务器上重新创建这些登录名。 如果应用程序或数据库维护计划依赖于 SQL Server 代理作业（其元数据存储在 msdb 数据库中），则必须在目标服务器实例上重新创建这些作业。 同样，服务器级触发器的元数据存储在 master 中。  
 
   将应用程序的数据库移动到其他服务器实例时，必须在目标服务器实例的 master 和 msdb 中重新创建依赖实体和依赖对象的所有元数据。 例如，如果数据库应用程序使用服务器级触发器，则仅在新系统上附加或还原数据库是不够的。 如果不手动在 master 数据库中重新创建这些触发器的元数据，则数据库不能按预期方式工作。 有关详细信息，请参阅[当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)  
  
-   **存储在 MSDB 中的 Integration Services 包：** 如果要在 MSDB 中存储包，需要使用 [dtutil Utility](../../integration-services/dtutil-utility.md) 编写这些包的脚本或者将其重新部署到新服务器。 在新服务器上使用包之前，需要将包升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 有关详细信息，请参阅 [升级 Integration Services 包](../../integration-services/install-windows/upgrade-integration-services-packages.md)。  
  
-   **Reporting Services 加密密钥：** 报表服务器配置的一个重要部分是为用于加密敏感信息的对称密钥创建备份副本。 该密钥的备份副本对许多例程操作来说是必需的，通过使用备份副本，您可以在新的安装中重用现有报表服务器数据库。 有关详细信息，请参阅 [备份和还原 Reporting Services 加密密钥](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md) 和 [升级和迁移 Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)  
  
 新的   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 环境有了与现有环境相同的系统对象后，则可立即以最小化现有系统的停机时间的方式从现有系统将用户数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 使用备份和还原完成数据库迁移，或者如果你处于 SAN 环境下，则通过重构 LUN 来完成数据库迁移。 两种方法的步骤如以下图表中所述。  
  
> [!CAUTION]  
>  停机时间量取决于你的数据库大小和 I/O 子系统速度。 在内存优化表处于使用中时升级 SQL Server 2014 会耗费一些额外时间。 有关详细信息，请参阅 [计划并测试数据库引擎升级计划](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md)。  
  
 迁移用户数据库之后，可使用多种方法中的一种将新用户指向到新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例（如重命名服务器、使用 DNS 条目、修改连接字符串）。  与就地升级相比，新的安装方法可以降低风险和停机时间，并可有助于同时完成操作系统的升级和升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
> [!NOTE]  
>  如果已有到位的高可用性 (HA) 解决方案或其他的多 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例环境，请转到 [滚动升级](#rolling-upgrade)。 如果没有到位的高可用性解决方案，则可以考虑临时配置 [数据库镜像](../database-mirroring/setting-up-database-mirroring-sql-server.md) 以进一步最小化停机时间从而简化此升级，或者利用这个机会配置 [AlwaysOn 可用性组](../availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md) 作为永久的 HA 解决方案。  
  
 例如，可以使用这种方法来升级：  
  
-   在不受支持的操作系统上安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。    
-   由于 [!INCLUDE[ss2016](../../includes/sssql15-md.md)] 及更高版本不支持 x86 安装，请安装 x86 的 SQL Server。   
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 到新硬件和/或操作系统新版本。    
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 结合服务器合并。   
-   由于 [!INCLUDE[ss2016](../../includes/sssql15-md.md)] 及更高版本不支持就地升级 SQL Server 2005，请使用 SQL Server 2005。 有关详细信息，请参阅[是否正在从 SQL Server 2005 进行升级](../../sql-server/end-of-support/sql-server-end-of-life-overview.md)。

  
新的安装升级所需的步骤根据你是否在使用连接存储或 SAN 存储而存在少许差异。  
  
-   **连接存储环境：** 如果具有使用连接存储的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 环境，可通过以下图表和图表内的链接完成 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的新安装升级所需执行的步骤。  
  
     ![使用备份和还原附加存储的新安装升级方法](../../database-engine/install-windows/media/new-installation-upgrade-method-using-backup-and-restore-for-attached-storage.png "使用备份和还原附加存储的新安装升级方法")  
  
-   **SAN 存储环境：** 如果具有使用 SAN 存储的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 环境，可通过以下图表和图表内的链接完成 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的新安装升级所需执行的步骤。  
  
     ![使用拆离和附加 SAN 存储的新安装升级方法](../../database-engine/install-windows/media/new-installation-upgrade-method-using-detach-and-attach-for-san-storage.png "使用拆离和附加 SAN 存储的新安装升级方法")  
  
## <a name="rolling-upgrade"></a>滚动升级  
 在涉及多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例（这些实例必须以特定顺序进行升级以最大化运行时间、最小化风险和保留功能）的 SQL Server 解决方案环境中，需要执行滚动升级。 滚动升级实质上是多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例按特定顺序进行的升级，此方法在每个现有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上执行就地升级，或者执行新安装升级作为升级项目的一部分来简化硬件和/或操作系统的升级。 在很多方案中你都需要使用滚动升级方法。 以下文章中记录了这些方案：  
  
-   AlwaysOn 可用性组：有关在此环境中执行滚动升级的详细步骤，请参阅 [升级 AlwaysOn 可用性组副本实例](../../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md)。    
-   故障转移群集实例：有关在此环境中执行滚动升级的详细步骤，请参阅[升级 SQL Server 故障转移群集实例](../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)    
-   镜像实例：有关在此环境中执行滚动升级的详细步骤，请参阅[升级镜像实例](../../database-engine/database-mirroring/upgrading-mirrored-instances.md)。    
-   日志传送实例：有关在此环境中执行滚动升级的详细步骤，请参阅[为 SQL Server 升级日志传送 &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)。    
-   复制环境：要了解在此环境中执行滚动升级的详细步骤，请参阅[升级复制的数据库](../../database-engine/install-windows/upgrade-replicated-databases.md)。  
-   SQL Server Reporting Services 横向扩展部署：有关在此环境中执行滚动升级的详细步骤，请参阅 [升级和迁移 Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)。  
  
## <a name="next-steps"></a>后续步骤
 [计划并测试数据库引擎升级计划](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md)   
 [完成数据库引擎升级](../../database-engine/install-windows/complete-the-database-engine-upgrade.md)