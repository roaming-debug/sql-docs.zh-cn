---
description: 增强常规复制性能
title: 增强常规复制性能 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- publications [SQL Server replication], design and performance
- designing databases [SQL Server], replication performance
- Snapshot Agent, performance
- snapshots [SQL Server replication], performance considerations
- merge replication performance [SQL Server replication]
- snapshot replication [SQL Server], performance
- subscriptions [SQL Server replication], performance considerations
- agents [SQL Server replication], performance
- performance [SQL Server replication], general considerations
- transactional replication, performance
ms.assetid: 895b1ad7-ffb9-4a5c-bda6-e1dfbd56d9bf
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 307d27f85d5643b837298418279e22dc9225ac67
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96121378"
---
# <a name="enhance-general-replication-performance"></a>增强常规复制性能
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  按照本主题介绍的指导原则，可以提高应用程序和网络上的所有复制类型的常规性能。  
  
## <a name="server-and-network"></a>服务器和网络  
  
-   设置分配给 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)] 的最小和最大内存量。  
  
     默认情况下， [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 根据可用的系统资源动态改变它的内存要求。 为避免在复制活动期间出现低内存可用性的问题，请使用 **min server memory** 选项设置最小可用内存。 为避免将操作系统页写入磁盘以节省内存，也可以使用 **max server memory** 选项设置最大内存量。 有关详细信息，请参阅[服务器内存服务器配置选项](../../../database-engine/configure-windows/server-memory-server-configuration-options.md)。  
  
-   确保正确分配数据库数据文件和日志文件。 使用单独的磁盘驱动器存放复制过程中所涉及的所有数据库的事务日志。  
  
     通过将日志文件与数据库存储在不同的磁盘驱动器上，可以减少写入事务所需的时间。 如果需要容错，可以使用独立冗余磁盘阵列 (RAID)-1 镜像该驱动器。 其他数据库文件使用 RAID 0 或 0+1（取决于容错需要）。 不管是否正在使用复制，这都是一个很好的做法。  
  
-   考虑增加复制所用的服务器的内存，尤其是分发服务器的内存。  
  
-   使用多处理器计算机。  
  
     复制代理可以利用服务器中的附加处理器。 如果运行时 CPU 使用很高，请考虑安装更快的 CPU 或多个 CPU。  
  
-   使用快速网络。  
  
     网络可能是一个重要的性能瓶颈，尤其是对于事务复制而言。 通过使用每秒 100 兆位 (Mbps) 或更快的网络，可以显著提高将更改传播到订阅服务器的速度。 如果网络速度比较慢，请指定合适的网络设置和代理参数。  
  
## <a name="database-design"></a>数据库设计  
  
-   遵循数据库设计最佳实践。  
  
     复制数据库通常能够从性能优化中获得与非复制数据库一样的好处。 但是，在订阅服务器中使用索引时应谨慎：应为订阅服务器中的主键列建立索引，但附加的索引可能影响插入、更新和删除操作的性能。  
  
-   考虑设置 READ_COMMITTED_SNAPSHOT 数据库选项。  
  
     为帮助减少用户活动与复制代理活动之间的争用，请为发布和订阅数据库设置下列选项：  
  
    ```  
    ALTER DATABASE AdventureWorks  
    SET READ_COMMITTED_SNAPSHOT ON  
    ```  
  
     有关详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../../t-sql/statements/alter-database-transact-sql.md)。  
  
-   在触发器中慎用应用程序逻辑。  
  
     在订阅服务器中的用户定义触发器中使用业务逻辑可能会降低将更改复制到订阅服务器的速度：  
  
    -   对于事务复制，将这种逻辑包含在用来应用复制命令的自定义存储过程中会更有效。 有关详细信息，请参阅[指定如何传播事务项目的更改](../../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
    -   对于合并复制，使用业务逻辑处理程序会更有效。 有关详细信息，请参阅[合并同步期间执行业务逻辑](../../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md)。  
  
     如果在为合并复制发布的表中使用触发器来维护引用完整性，请指定表的处理顺序，以减少合并代理所需的重试次数。 有关详细信息，请参阅[指定合并复制选项](../../../relational-databases/replication/merge/specify-merge-replication-properties.md)。  
  
-   限制使用大型对象 (LOB) 数据类型。  
  
     LOB 比其他列数据类型需要更多的存储空间和处理。 除非应用程序需要，否则不要在项目中包括这些列。 不推荐使用数据类型 **text**、 **ntext** 和 **image** 。 如果确实要包括 LOB，建议分别使用数据类型 **varchar(max)**、 **nvarchar(max)** 和 **varbinary(max)**。  
  
     对于事务复制，请考虑使用名为“用于 OLEDB 流式处理的分发配置文件” 的分发代理配置文件。 有关详细信息，请参阅 [Replication Agent Profiles](../../../relational-databases/replication/agents/replication-agent-profiles.md)。  
  
## <a name="publication-design"></a>发布设计  
  
-   仅发布所需的数据。  
  
     由于复制易于设置，所以有一种趋势是发布比实际需要量更多的数据。 这会在分发数据库和快照文件中耗费额外资源，并降低所需数据的吞吐量。 应避免发布不必要的表，并考虑不要过于频繁地更新发布。  
  
-   通过发布设计和应用程序行为最大限度地减少冲突。  
  
     以下类型的复制允许在订阅服务器中更改数据：合并复制、具有可更新订阅的事务复制和对等事务复制。 如果在同步之间的多个节点上更新给定的行，合并复制以及具有可更新订阅的事务复制支持数据冲突。 对等复制不支持数据冲突，必须对数据更改进行分区。 不管使用哪种类型的复制，我们都建议您尽可能地对更改进行分区，因为这可以减少冲突检测和解决所需的处理操作。  
  
     通过将数据子集发布到每个订阅服务器或者让应用程序将给定行的更改定向到给定的节点，可以对更改进行分区：  
  
    -   合并复制支持对单个发布使用参数化筛选器发布数据子集。 有关详细信息，请参阅 [参数化行筛选器](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)。  
  
    -   事务复制支持对多个发布使用静态筛选器发布数据子集。 有关详细信息，请参阅[筛选已发布数据](../../../relational-databases/replication/publish/filter-published-data.md)。  
  
-   灵活地使用行筛选器。  
  
     当事务发布包括一个或多个使用行筛选器的项目时，日志读取器代理在扫描事务日志时，必须将该筛选器应用到受表更新影响的每一行。 日志读取器代理的吞吐量因此受到影响。  
  
     同样，合并复制必须计算已更改或删除的行，以确定哪些订阅服务器应接收这些行。 当利用行筛选器减少订阅服务器中要求的数据时，此处理更加复杂，并且比发布表中的所有行时更慢。 应仔细考虑降低每个订阅服务器的存储要求与获得最大吞吐量的需求之间的平衡点。 有关筛选的详细信息，请参阅[筛选已发布数据](../../../relational-databases/replication/publish/filter-published-data.md)。  
  
## <a name="subscription-considerations"></a>订阅注意事项  
  
-   当存在大量订阅服务器时，应使用请求订阅。  
  
     分发代理和合并代理在分发服务器中运行以实现推送订阅，在订阅服务器中运行以实现请求订阅。 使用请求订阅可以提高性能，因为它将代理处理从分发服务器移到了订阅服务器中。 有关详细信息，请参阅[订阅发布](../../../relational-databases/replication/subscribe-to-publications.md)。  
  
-   如果订阅服务器滞后太多，请考虑重新初始化订阅。  
  
     当需要将大量更改发送到订阅服务器时，用新快照重新初始化这些更改可能比使用复制分别移动每个更改要快。 有关详细信息，请参阅 [重新初始化订阅](../../../relational-databases/replication/reinitialize-subscriptions.md)。  
  
     对于事务复制，复制监视器在 **“未分发的命令”** 选项卡上显示下列信息：分发数据库中尚未分发到订阅服务器的事务数，以及预计分发这些事务所需的时间。 有关详细信息，请参阅[使用复制监视器查看信息和执行任务](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)。  
  
## <a name="snapshot-considerations"></a>快照注意事项  
  
-   仅在必要时和非高峰时段运行快照代理。  
  
     快照代理将发布服务器中已发布表中的数据大容量复制到分发服务器中的快照文件夹的一个文件中。 生成快照可能是一个占用大量资源的过程，最好安排在非高峰时段进行。  
  
-   除非要求使用字符模式快照，否则请使用本机模式快照。  
  
     除了非[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 订阅服务器和运行 [!INCLUDE[ssEW](../../../includes/ssew-md.md)]的订阅服务器（它们要求使用字符模式快照）外，请为其他所有订阅服务器使用默认的本机模式快照。  
  
-   为一个发布使用一个快照文件夹。  
  
     当指定与快照位置有关的发布属性时，可选择在默认的快照文件夹、备用快照文件夹或同时在这两个文件夹中生成快照文件。 当快照代理运行时，同时在这两个位置生成快照需要额外的磁盘空间和更多的处理。  
  
-   将快照文件夹放在分发服务器上未用于存储数据库或日志文件的本地驱动器中。  
  
     快照代理将按顺序将数据写入快照文件夹。 将快照文件夹放在与数据库或日志文件分开的驱动器上可以减少磁盘间的争用，并有助于快照进程更快地完成。  
  
-   在订阅服务器上创建订阅数据库时，考虑指定简单恢复模式或大容量日志恢复模式。 这允许在订阅服务器中应用快照时执行最少的大容量插入日志记录。 将快照应用于订阅数据库后，如有必要，可以更改为不同的恢复模式（复制的数据库可以使用任何恢复模式）。 有关选择恢复模式的详细信息，请参阅[还原和恢复概述 (SQL Server)](../../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)。  
  
-   对于低带宽网络，考虑在可移动介质上使用备用快照文件夹和压缩的快照。  
  
     压缩备用快照文件夹中的快照文件可以减少快照的磁盘存储空间要求，使快照文件更容易在可移动介质上传输。  
  
     在某些情况下，压缩的快照可以提高通过网络传输快照文件的性能。 但是，如果压缩快照，则在生成快照文件时需要通过快照代理进行额外处理，而在应用快照文件时需要通过分发代理或合并代理进行额外处理。 在某些情况下，这可能会降低生成快照的速度，并增加应用快照所需的时间。 此外，如果发生网络故障，压缩的快照将无法恢复，因此不适用于不可靠的网络。 在网络中使用压缩的快照时，应仔细权衡这些利弊。 有关详细信息，请参阅[修改快照选项](../../../relational-databases/replication/snapshot-options.md)。 
-   考虑手动初始化订阅。  
  
     在某些方案中，如涉及大型初始数据集的方案，使用快照以外的其他方法来初始化订阅更可取。 有关详细信息，请参阅 [初始化事务订阅（不使用快照）](../../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)中手动初始化订阅。  
  
## <a name="agent-parameters"></a>代理参数  
  
-   降低复制代理的详细级别，在初始测试、监视或调试期间除外。  
  
     降低分发代理或合并代理的 **–HistoryVerboseLevel** 参数和 **–OutputVerboseLevel** 参数。 这样可以减少为跟踪代理历史记录和输出而插入的新行数。 相反，具有相同状态的以前的历史记录消息将更新为新的历史记录信息。 提高测试、监视和调试的详细级别，这样就可以获得有关代理活动的尽可能多的信息。  
  
-   使用快照代理、合并代理和分发代理的 **–MaxBCPThreads** 参数（指定的线程数不应超过计算机上的处理器数）。 此参数指定创建和应用快照时可以并行执行的大容量复制操作的数目。  
  
-   使用分发代理和合并代理的 **–UseInprocLoader** 参数（如果已发布表中包含 XML 列，则无法使用此参数）。 此参数使代理在应用快照时使用 BULK INSERT 命令。  
  
 代理参数可以在代理配置文件和命令行中指定。 有关详细信息，请参阅：  
  
-   [处理复制代理配置文件](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
  
-   [查看和修改复制代理命令提示符参数 (SQL Server Management Studio)](../../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
-   [Replication Agent Executables Concepts](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)的最小和最大内存量。  
  
  
