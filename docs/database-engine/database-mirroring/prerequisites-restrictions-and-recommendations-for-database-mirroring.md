---
title: 数据库镜像：先决条件、限制和建议
description: 了解有关使用 SQL Server 配置数据库镜像的先决条件、限制和建议。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- partners [SQL Server]
- database mirroring [SQL Server], prerequisites
- database mirroring [SQL Server], recommendations
- database mirroring [SQL Server], restrictions
- database mirroring [SQL Server], planning
- database mirroring [SQL Server], about database mirroring
ms.assetid: fdcf2251-9895-44c6-b81e-768fef32e732
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 0aca4b4b5250232f065b02ca21be7187ab42c119
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641468"
---
# <a name="prerequisites-restrictions-and-recommendations-for-database-mirroring"></a>数据库镜像的前提条件、限制和建议
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 改为使用 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]。  
  
 本主题说明了设置数据库镜像的前提条件和建议。 有关数据库镜像的介绍，请参阅 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)。  
  
  
##  <a name="support-for-database-mirroring"></a><a name="DbmSupport"></a> 数据库镜像支持  
 有关 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中数据库镜像支持的信息，请参阅 [SQL Server 2016 的各版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)。
  
 请注意，数据库镜像可使用任意支持的数据库兼容级别。 有关支持的兼容级别的信息，请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。  
  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   若要建立镜像会话，伙伴双方和见证服务器（如果有）必须在相同版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]上运行。  
  
-   确保两个伙伴（即主体服务器和镜像服务器）必须运行相同版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 见证服务器（如果有）在任意支持数据库镜像的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本上运行。  
  
    > [!NOTE]  
    >  您可以将作为镜像会话中的伙伴的服务器实例升级到较新版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 有关详细信息，请参阅 [Upgrading Mirrored Instances](../../database-engine/database-mirroring/upgrading-mirrored-instances.md)。  
  
-   数据库必须使用完整恢复模式。 简单恢复模式和大容量日志恢复模式不支持数据库镜像。 因此，镜像数据库的大容量操作始终被完整地记入日志。 有关恢复模式的信息，请参阅[恢复模式 (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)。  
  
-   验证镜像服务器是否能为镜像数据库提供足够的磁盘空间。  
  
    > [!NOTE]  
    >  有关如何对复制数据库使用数据库镜像的信息，请参阅[数据库镜像和复制 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)。  
  
-   在镜像服务器上创建镜像数据库时，请确保指定相同数据库名称 WITH NORECOVERY 来还原主体数据库备份。 另外，还必须通过 WITH NORECOVERY 应用在该备份执行后创建的所有日志备份。  
  
    > [!IMPORTANT]  
    >  如果数据库镜像已经停止，则必须将对主体数据库执行的所有后续日志备份应用到镜像数据库中，然后才可以重新启动镜像。  
  
  
##  <a name="restrictions"></a><a name="Restrictions"></a> 限制  
  
-   只能镜像用户数据库。 不能镜像 **master**、 **msdb**、 **tempdb** 或 **model** 数据库。  
  
-   镜像的数据库在数据库镜像会话过程中不能重命名。  
  
-   数据库镜像不支持 FILESTREAM。 不能在主体服务器上创建 FILESTREAM 文件组。 不能为包含 FILESTREAM 文件组的数据库配置数据库镜像。  
  
-   跨数据库事务和分布式事务均不支持数据库镜像。 有关详细信息，请参阅[用于 AlwaysOn 可用性组和数据库镜像的跨数据库事务和分布式事务 (SQL Server)](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)。  
  
  
##  <a name="recommendations-for-configuring-partner-servers"></a><a name="RecommendationsForPartners"></a> 配置伙伴服务器的建议  
  
-   应该在可以处理相同工作负荷的类似系统上运行伙伴。  
  
    > [!NOTE]  
    >  如果计划使用具有自动故障转移功能的高安全性模式，则每个故障转移伙伴上的正常负载应小于 50％ 的 CPU。 如果工作负荷超过 CPU 负荷，则故障转移伙伴可能无法对镜像会话中的其他服务器实例使用 ping 命令。 这将导致不必要的故障转移。 如果无法保持 CPU 使用率始终在 50％ 以下，则建议使用不带自动故障转移功能的高安全性模式或使用高性能模式。  
  
-   如有可能，镜像数据库的路径（包括驱动器号）应该与主体数据库的路径相同。 如果文件布局必须有所不同，则必须在 RESTORE 语句中包括 MOVE 选项。 例如，如果主体数据库位于“F:”驱动器上，但镜像系统没有“F:”驱动器。  
  
    > [!IMPORTANT]  
    >  如果在创建镜像数据库时移动了数据库文件，则可能导致以后不挂起镜像就无法向数据库添加文件。  
  
-   镜像会话中的所有服务器实例都应该使用相同的主代码页和排序规则。 如果使用不同的主代码页和排序规则，则在镜像设置期间可能会出现问题。  
  
-   （可选）估计故障转移数据库的时间，以确保系统配置能提供所需性能。 有关详细信息，请参阅 [估计在角色切换期间服务的中断（数据库镜像）](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md)。  
  
-   为获得最佳性能，请为镜像使用专用网络适配器（网络接口卡）。  
  
-   关于广域网 (WAN) 对高安全性模式下的数据库镜像是否足够可靠，我们没有任何建议。 如果您决定在 WAN 上使用高安全性模式，则应注意将见证服务器添加至会话的方式，因为可能会发生意外的自动故障转移。 有关详细信息，请参阅本主题稍后部分中的 [部署数据库镜像的建议](#RecommendationsForDeploying)。  
  
  
##  <a name="recommendations-for-deploying-database-mirroring"></a><a name="RecommendationsForDeploying"></a> 部署数据库镜像的建议  
 使用异步操作可获得最佳的数据库镜像性能。 如果镜像会话使用同步操作，则当该会话的工作负荷生成大量事务日志数据时，可能会降低性能。  
  
 在测试环境中，应当研究所有运行模式以评估数据库镜像的执行效率。 但是，在生产环境中部署镜像之前，务必要了解网络在实际环境中的运行方式。  
  
 具有自动故障转移功能的高安全性模式专用于具有专用连接或非常简单的网络配置的高级服务网络，它可最大程度地减少可能的网络故障源。 这种高质量的网络环境对于具有自动故障转移功能的高安全性模式而言是必需的，同时也建议将其用于所有数据库镜像会话。 但是，网络可靠性对高性能模式和不带自动故障转移功能的高安全性模式具有较小的影响。  
  
 因此，对于生产环境，建议遵守下列部署指南：  
  
1.  在异步、高性能模式下启动运行。 此模式受网络环境的影响最小，并为研究镜像的工作方式提供了最佳配置。 除非您确信自己的带宽支持镜像，并且熟知镜像设置以及异步模式在您环境中的性能，否则，建议异步运行系统。 有关详细信息，请参阅 [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)。  
  
    > [!IMPORTANT]  
    >  建议您在测试过程中监视会话，以便发现导致数据库镜像失败的网络错误。 有关潜在的故障源的详细信息，请参阅 [Possible Failures During Database Mirroring](../../database-engine/database-mirroring/possible-failures-during-database-mirroring.md)。 有关如何监视数据库镜像的信息，请参阅[监视数据库镜像 (SQL Server)](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)。  
  
2.  在确信异步操作符合您的业务需要之后，您可能希望尝试同步操作来提高数据保护能力。 当测试同步镜像在您环境中的工作方式时，建议首先测试不带自动故障转移功能的高安全性模式。 此测试的主要目的是了解同步操作如何影响数据库的性能。 有关详细信息，请参阅 [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)。  
  
3.  等到确信不带自动故障转移功能的高安全性模式满足您的业务需要并且网络错误不会导致故障时再启用自动故障转移。 有关详细信息，请参阅 [数据库镜像会话期间的角色切换 (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)的各版本中均未提供见证服务器实例。  
  
  
## <a name="see-also"></a>另请参阅  
 [设置数据库镜像 (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)   
 [针对数据库镜像和 AlwaysOn 可用性组的传输安全性 (SQL Server)](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [数据库镜像配置故障排除 (SQL Server)](../../database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
