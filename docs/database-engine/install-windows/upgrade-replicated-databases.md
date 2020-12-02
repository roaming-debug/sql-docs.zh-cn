---
title: 升级或修补复制的数据库 | Microsoft Docs
description: SQL Server 支持从以前版本的 SQL Server 升级复制的数据库，无需停止其他节点上的活动。
ms.custom: ''
ms.date: 07/24/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- merge replication database upgrades [SQL Server replication]
- replication [SQL Server], upgrading
- transactional replication, upgrading databases
- snapshot replication [SQL Server], upgrading databases
- upgrading replicated databases
ms.assetid: 9926a4f7-bcd8-4b9b-9dcf-5426a5857116
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 55220291257cf90f7345acb63ce2369b9308f435
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125743"
---
# <a name="upgrade-or-patch-replicated-databases"></a>升级或修补复制的数据库

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] 支持从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本升级复制数据库；在升级某一节点时，不需要停止其他节点的活动。 请务必遵守有关拓扑中支持哪些版本的规则：  
  
-   分发服务器的版本可以是高于或等于发布服务器版本的任何版本（在许多情况下，分发服务器与发布服务器是同一个实例）。    
-   发布服务器的版本可以是低于或等于分发服务器版本的任何版本。    
-   订阅服务器版本取决于发布的类型：    
    - 用于事务发布的订阅服务器版本可以是两个发布服务器版本中的任何一个版本。 例如：SQL Server 2012 (11.x) 发布服务器可以包含 SQL Server 2014 (12.x) 和 SQL Server 2016 (13.x) 订阅服务器；SQL Server 2016 (13.x) 发布服务器可以包含 SQL Server 2014 (12.x) 和 SQL Server 2012 (11.x) 订阅服务器。     
    - 合并发布的订阅服务器可以是等于或低于发布服务器版本的所有版本，而发布服务器版本是生命周期支持周期支持的版本。  
 
SQL Server 的升级路径因部署模式而异。 SQL Server 一般情况下提供两个升级路径：
- 并行升级：部署并行环境，将数据库随关联的实例级别对象（例如登录名、作业等）一起迁移到新环境。 
- 就地升级：替换 SQL Server 位并升级数据库对象，使 SQL Server 安装介质升级现有的 SQL Server 安装。 对于运行 Always On 可用性组或故障转移群集实例的环境，将就地升级与[滚动升级](choose-a-database-engine-upgrade-method.md#rolling-upgrade)结合，尽量减少停机时间。 

并行升级复制拓扑的常用方法是将发布服务器-订阅服务器对分批迁移到新的并行环境，而不是迁移整个拓扑。 这种分阶段方法有助于控制停机时间，并在某种程度上把对依赖复制的业务的影响降至最低。  

本文的大部分内容都适用于升级 SQL Server 的版本。 但是，在使用服务包或累积更新修补 SQL Server 时，也应使用就地升级过程。 

 >[!WARNING]
 > 升级复制拓扑涉及多个步骤。 建议在于实际生产环境中运行升级之前，尝试在测试环境中升级复制拓扑的副本。 通过这种方式，在实际升级过程中不需要使用任何操作文档，便能顺利升级，且不会发生成本高昂的长时间停机。 我们已了解到，客户在升级复制拓扑时，为生产环境使用 Always On 可用性组和/或 SQL Server 故障转移群集实例可显著减少停机时间。 此外，我们建议在尝试升级之前，对所有数据库都进行备份（包括 MSDB、Master、分发数据库和参与复制的用户数据库）。


## <a name="replication-matrix"></a>复制矩阵

[!INCLUDE[repl matrix](../../includes/replication-compat-matrix.md)]
  
## <a name="run-the-log-reader-agent-for-transactional-replication-before-upgrade"></a>在升级前先运行用于事务复制的日志读取器代理  
 升级 [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] 之前，必须确保已发布表中的所有已提交事务已由日志读取器代理进行了处理。 若要确保所有事务均得到处理，请针对包含事务发布的每个数据库执行以下步骤：  
  
1.  确保针对数据库运行了日志读取器代理。 默认情况下，该代理会连续运行。    
2.  停止已发布表上的用户活动。  
3.  日志读取器代理和分发代理支持事务读取和提交操作的批大小。  
4.  执行 [sp_replcmds](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md) 以验证所有事务是否得到了处理。 此过程的结果集应为空。   
5.  执行 [sp_replflush](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md) 以关闭与 sp_replcmds 的连接。   
6.  将服务器升级为最新版 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]。   
7.  如果在升级后 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理和日志读取器代理没有自动启动，请重新启动它们。  
  
## <a name="run-agents-for-merge-replication-after-upgrade"></a>升级后运行用于合并复制的代理  
 升级后，请为每一个合并发布运行快照代理，并为每一个订阅运行合并代理，以更新复制元数据。 因为不必重新初始化订阅，所以不需要应用新快照。 升级后首次运行合并代理时，将更新订阅元数据。 这表明发布服务器升级时订阅数据库可以保持在线状态和活动状态。  
  
 合并复制将发布和订阅元数据存储在发布和订阅数据库中的若干系统表中。 运行快照代理将更新发布元数据，而运行合并代理将更新订阅元数据。 只需生成发布快照。 如果合并发布使用参数化筛选器，则每一个分区也都有快照。 不必更新这些分区快照。  
  
 从 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、复制监视器或命令行运行代理。 有关运行快照代理的详细信息，请参阅下列文章：  
  
-   [创建并应用初始快照](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)
-   [启动和停止复制代理 (SQL Server Management Studio)](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)
-   [创建并应用初始快照](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)
-   [Replication Agent Executables Concepts](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  

有关运行合并代理的详细信息，请参阅下列文章：
-   [同步请求订阅](../../relational-databases/replication/synchronize-a-pull-subscription.md)
-   [同步推送订阅](../../relational-databases/replication/synchronize-a-push-subscription.md)  
  

在使用合并复制的拓扑中升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之后，如果要使用新功能，请更改所有发布的发布兼容级别。  
  
## <a name="upgrading-to-standard-workgroup-or-express-editions"></a>升级至 Standard Edition、Workgroup Edition 或 Express Edition  
在从 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 的某一版本升级到另一版本之前，请验证要升级到的版本是否支持当前使用的功能。 有关详细信息，请参阅 [SQL Server 版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)中有关复制的部分。  

## <a name="steps-to-upgrade-a-replication-topology"></a>升级复制拓扑的步骤
这些步骤体现了升级复制拓扑中的服务器的顺序。 无论是运行事务复制还是合并复制，以上步骤都适用。 但是，这些步骤不包括对等复制、排队更新订阅或立即更新订阅。 

### <a name="in-place-upgrade"></a>就地升级 
1. 升级分发服务器。 
2. 升级发布服务器和订阅服务器。 可以按任意顺序升级这二者。 

 >[!NOTE]
 > 对于 SQL 2008 和 2008 R2，必须在同时完成发布服务器和订阅服务器的升级，以与复制拓扑矩阵一致。 SQL 2008 或 2008R2 发布服务器或订阅服务器不能使用 SQL 2016（或更高版本）发布服务器或订阅服务器。 如果不能同时升级，请使用中间升级将 SQL 实例升级到 SQL 2014，然后再升级到 SQL 2016（或更高版本）。  

### <a name="side-by-side-upgrade"></a>并行升级
1. 升级分发服务器。
1. 在新的 SQL Server 实例上，重新配置[分发](../../relational-databases/replication/configure-distribution.md)。
1. 升级发布服务器。
1. 升级订阅服务器。
1. 重新配置所有“发布服务器-订阅服务器”对，包括订阅服务器的重新初始化。 


## <a name="steps-for-side-by-side-migration-of-the-distributor-to-windows-server-2012-r2"></a>分发服务器并行迁移到 Windows Server 2012 R2 的步骤
如果计划将 SQL Server 实例升级到 SQL Server 2016（或更高版本），且当前的 OS 是 Windows 2008（或 2008 R2），则需要将 OS 并行升级到 Windows Server R2 或更高版本。 该中间 OS 升级的原因是 SQL Server 2016 不能安装在 Windows Server 2008/2008 R2 上，并且 Windows Server 2008/2008 R2 不允许直接就地升级到 Windows Server 2016。 尽管可以从 Windows Server 2008/2008 R2 就地升级到 Windows Server 2012，然后再升级到 Windows Server 2016，但通常不建议这样做，因为停机和增加的复杂性会阻止简单的回滚路径。 并行升级是可用于参与故障转移群集的 SQL Server 实例的唯一升级路径。  可以在一个独立的 SQL Server 实例上或 Always On 故障转移群集实例 (FCI) 中的某个 SQL Server 实例上执行以下步骤。

1. 将新的 SQL Server 实例（独立版或 Always On 故障转移群集）、版次和版本设置为 Windows Server 2012 R2/2016 上具有不同 Windows 群集和 SQL Server FCI 名称或独立主机名的分发服务器。 你需要使目录结构与旧分发服务器保持一致，以确保可在新环境的同一路径下找到复制代理可执行文件、复制文件夹和数据库文件路径。 这会减少迁移/升级后所需的步骤。
1. 确保同步了复制，然后关闭所有复制代理。 
1. 关闭当前的 SQL Server 分发服务器实例。 如果这是独立实例，则关闭服务器。 如果这是 SQL FCI，则在群集管理器中使整个 SQL Server 角色脱机，包括网络名称。 
1. 删除旧（当前分发服务器实例）环境的 DNS 和 AD 计算机对象条目。 
1. 更改新服务器的主机名，以匹配旧服务器的主机名。
    1. 如果这是 SQL FCI，将新 SQL FCI 重命名为旧实例所用的虚拟服务器名称。 
1. 使用 SAN 重定向、存储副本或文件复制从前一个实例复制数据库文件。 
1. 使新 SQL Server 实例联机。 
1. 重启所有复制代理，并验证代理是否成功运行。
1. 验证复制是否正在按预期的方式工作。 
1. 使用 SQL Server 安装介质，将 SQL Server 实例就地升级为新版 SQL Server。 


  >[!NOTE]
  > 为了减少停机时间，我们建议将分发服务器的“并行迁移”作为一个活动执行，将“就地升级到 SQL Server 2016”作为另一个活动执行 。 这样，可通过分阶段方法来降低风险和尽量减少停机时间。

## <a name="web-synchronization-for-merge-replication"></a>合并复制的 Web 同步  
 合并复制的 Web 同步选项要求将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 复制侦听器 (replisapi.dll) 复制到用于同步的 Internet Information Services (IIS) 服务器上的虚拟目录中。 配置 Web 同步时，该文件被配置 Web 同步向导复制到虚拟目录中。 若要升级安装在 IIS 服务器上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件，必须将 replisapi.dll 从 COM 目录手动复制到 IIS 服务器上的虚拟目录。 有关配置 Web 同步的详细信息，请参阅 [配置 Web 同步](../../relational-databases/replication/configure-web-synchronization.md)。  
  
## <a name="restoring-a-replicated-database-from-an-earlier-version"></a>从早期版本还原复制的数据库  
 在从早期版本还原复制数据库的备份时，若要确保保留复制设置：请还原到与创建备份的服务器和数据库同名的服务器和数据库。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 复制](../../relational-databases/replication/sql-server-replication.md)  
 [复制管理常见问题解答](../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)   
 [副本后向兼容性](../../relational-databases/replication/replication-backward-compatibility.md)   
 [支持的版本升级](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)   
 [升级 SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
 [将复制拓扑升级到 SQL Server 2016](/archive/blogs/sql_server_team/upgrading-a-replication-topology-to-sql-server-2016)