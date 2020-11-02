本文概述 SQL Server 中高可用性和灾难恢复的业务连续性解决方案。 

每个人在部署 SQL Server 时都需执行一项常见任务，即确保所有任务关键型 SQL Server 实例以及其中的数据库在业务和最终用户需要时（无论是朝九晚五还是全天候）可用。 其目标是尽量减少或杜绝中断，保持业务正常运行。 此概念也称为业务连续性。

SQL Server 2017 在现有功能基础上引入了许多新功能或增强功能，其中部分功能用于提高可用性。 SQL Server 2017 的最大变化在于在 Linux 发行版上增加了对 SQL Server 的支持。 有关 SQL Server 2017 中新功能的完整列表，请参阅主题 [SQL Server 的新增功能](../sql-server/what-s-new-in-sql-server-2017.md)。

本文重点介绍 SQL Server 2017 中的可用性方案以及 SQL Server 2017 中新增和增强的可用性功能。 这些方案包括能跨 Windows Server 和 Linux 上的 SQL Server 部署的混合方案，以及可增加数据库可读副本数量的方案。 虽然本文并未介绍 SQL Server 外的可用性选项（例如由虚拟化提供的可用性选项），但文中讨论的所有内容都适用于来宾虚拟机中的 SQL Server 安装，无论该虚拟机是位于公有云中还是由本地虚拟机监控程序服务器托管。

## <a name="sql-server-2017-scenarios-using-the-availability-features"></a>使用可用性功能的 SQL Server 2017 方案

可用性组、FCI 和日志传送可通过多种方式使用，而非仅用于可用性。 可用性功能的使用方式主要有以下四种：

* 高可用性
* 灾难恢复
* 迁移和升级
* 扩大一个或多个数据库的可读副本

每节内容都会介绍可用于该特定情况的相关功能。 [SQL Server 复制](../relational-databases/replication/sql-server-replication.md)功能未涵盖在内。 虽然未正式指定为“AlwaysOn”下的可用性功能，但在某些情况下，它常用于使数据冗余。 在未来版本中，Linux 上的 SQL Server 将添加复制功能。

> [!IMPORTANT] 
> SQL Server 可用性功能不能替换对经过充分测试的可靠备份和还原策略的需求，后者是所有可用性解决方案最基本的构建基块。

### <a name="high-availability"></a>高可用性

请务必确保在数据中心或云区域中的单个区域存在局部问题的情况下，SQL Server 实例或数据库可用。 本部分介绍 SQL Server 可用性功能如何帮助完成该任务。 Windows Server 和 Linux 上都提供了此处描述的所有功能。 

#### <a name="always-on-availability-groups"></a>AlwaysOn 可用性组

SQL Server 2012 中引入的 AlwaysOn 可用性组将数据库的每个事务发送到另一个实例，从而提供数据库级别的保护，该实例称为副本，其中包含处于特定状态的数据库副本。 可用性组可部署在 Standard 版本或 Enterprise 版本上。  参与可用性组的实例可以是独立实例，也可以是 AlwaysOn 故障转移群集实例（即下一节中介绍的 FCI）。 由于在事务发生时将它发送到副本，建议在需要较低恢复点目标和恢复时间目标的情况下使用可用性组。 副本之间的数据移动可以是同步的或异步的，Enterprise 版本允许同步多达三个副本（包括主要副本）。 可用性组具有一个数据库的完全读/写副本且位于主要副本上，而所有次要副本都不能直接从最终用户或应用程序接收事务。 

> [!NOTE] 
> AlwaysOn 是 SQL Server 中可用性功能的总称，涵盖可用性组和 FCI。 AlwaysOn 不是可用性组功能的名称。

因为可用性组只提供数据库级保护，而非实例级保护，所以需要为每个次要副本手动同步事务日志中未捕获的或数据库中未配置的任何内容。 必须手动同步的对象的一些示例为实例级登录、链接服务器和 SQL Server 代理作业。

可用性组还有一个名为侦听器的组件，该组件允许应用程序和最终用户在不知道哪个 SQL Server 实例承载着主要副本的情况下进行连接。 每个可用性组都有自己的侦听器。 虽然侦听器的实现在 Windows Server 与 Linux 上略有不同，但它提供的功能和使用方法是相同的。 下图显示了使用 Windows Server 故障转移群集 (WSFC) 的基于 Windows Server 的可用性组。 无论是在 Linux 还是 Windows Server 上，实现可用性必须具备 OS 层的基础群集。 该示例显示了以 WSFC 为基础群集的两个简单服务器、节点或配置。 

![简单可用性组](media/sql-server-ha-story/image1.png)
 
就副本而言，Standard 版本和 Enterprise 版本具有不同的最大值。 Standard 版本中的可用性组（称为 Basic 可用性组）支持两个副本（一个主要副本和一个次要副本），且可用性组中只有一个数据库。 Enterprise 版本不仅允许在一个可用性组中配置多个数据库，而且拥有的副本总数可多达 9 个（1 个主要副本，8 个次要副本）。 Enterprise 版本还提供了其他可选权益，如可读次要副本和备份次要副本的能力等。

>[!NOTE]
> SQL Server 2012 中已弃用的数据库镜像在 Linux 版 SQL Server 上不可用，以后也不会添加该功能。 仍在使用数据库镜像的客户应开始计划迁移到替代数据库镜像的可用性组。

在可用性方面，可用性组可提供自动或手动故障转移。 如果配置了同步数据移动，并且主要副本和次要副本上的数据库处于同步状态，则会发生自动故障转移。 只要使用侦听器，且应用程序使用较高版本的 .NET（3.5 更新版本，或 4.0 及更高版本），则应能利用侦听器，在尽量减小甚至不影响最终用户的情况下进行故障转移。 可将使次要副本成为新的主要副本的故障转移配置为自动或手动，且测量的单位通常为秒。

下面的列表突出显示了 Windows Server 与 Linux 上的可用性组之间存在的一些差异：
* 由于基础群集在 Linux 和 Windows Server 上的工作方式不同，因此，在 Linux 上，可用性组的所有故障转移（手动或自动）都是通过群集完成的。 而在基于 Windows Server 的可用性组部署中，手动故障转移必须通过 SQL Server 完成。 自动故障转移则由 Windows Server 和 Linux 上的基础群集处理。 
* 在 SQL Server 2017 中，建议将 Linux 上的可用性组配置为至少三个副本。 这是因为基础群集的工作方式。 发布后，会提供两个副本配置的改进解决方案。
* 在 Linux 上，每个侦听器使用的公用名都在 DNS 中定义，而不是像 Windows Server 上那样在群集中定义。

在 SQL Server 2017 中，可用性组有一些新功能和增强功能：

* 群集类型
* REQUIRED_SECONDARIES_TO_COMMIT
* 增强的 Microsoft 分布式事务处理协调器 (DTC) 支持基于 Windows Server 的配置
* 只读数据库的其他横向扩展方案（本文后面进行了介绍）

##### <a name="always-on-availability-group-cluster-types"></a>AlwaysOn 可用性组群集类型

通过名为故障转移群集的功能启用 Windows Server 中群集的内置可用性形式。 通过它，用户可生成与可用性组或 FCI 一起使用的 WSFC。 由 SQL Server 提供的群集感知资源 DLL 进行可用性组和 FCI 的集成。 

每个受支持的 Linux 分发都拥有自己的 Pacemaker 群集解决方案。 Linux 上的 SQL Server 2017 支持使用 Pacemaker。 Pacemaker 是一个开放堆栈解决方案，每个分发都可与其堆栈集成。 虽然分发提供 Pacemaker，但并不像 Windows Server 中的故障转移群集功能一样集成。

相较于差异，WSFC 和 Pacemaker 存在更多的相似之处。 两者都提供这样一种方式：使用多个单独的服务器，在配置中将它们合并，从而提供可用性；此外两者都具有资源、约束（尽管实施方式不同）、故障转移等概念。 为支持 Pacemaker 的可用性组和 FCI 配置（包括自动故障转移等），Microsoft 为 Pacemaker 提供了 mssql-server-ha 包，它与 WSFC 中的资源 DLL 类似但不完全相同。 WSFC 和 Pacemaker 之间的区别之一是 Pacemaker 中没有网络名称资源，该组件有助于提取 WSFC 上的侦听器名称（或 FCI 名称）。 DNS 在 Linux 上提供名称解析。

由于群集堆栈有所不同，SQL Server 必须处理一些由 WSFC 本机处理的元数据，因此需要对可用性组进行一些更改。 最[!IMPORTANT]的更改是为可用性组引入了群集类型。 这存储在 cluster_type 和 cluster_type_desc 列的 sys.availability_groups 中。 有以下三种群集类型：

* WSFC 
* 外部
* 无

要求可用性的所有可用性组都必须使用基础群集，在 SQL Server 2017 中则为 WSFC 或 Pacemaker。 对于使用基础 WSFC 的基于 Windows Server 的可用性组，默认群集类型为 WSFC 且无需设置。 对于基于 Linux 的可用性组，创建可用性组时，群集类型必须设置为“外部”。 在创建可用性组后配置与 Pacemaker 的集成，而在 WSFC 上，需在创建时进行集成。

Windows Server 和 Linux 可用性组都可使用“无”群集类型。 将群集类型设置为“无”，表示可用性组不需要基础群集。 这意味着 SQL Server 2017 是首个支持无群集可用性组的 SQL Server 版本，但其不利的一面是高可用性解决方案不支持此配置。 

> [!IMPORTANT] 
> SQL Server 2017 不允许在可用性组创建完成后，更改其群集类型。 这意味着可用性组不能从“无”切换为“外部”或“WSFC”，反之亦然。 

有的用户想只添加额外的数据库只读副本，或者喜欢可用性组为迁移/升级提供的内容，却不希望基础群集甚至复制带来额外的复杂性，对于这样的用户，群集类型为“无”的可用性组是最佳解决方案。 有关详细信息，请参阅[迁移和升级](#Migrations)和[读取缩放](#ReadScaleOut)部分。 

下面的屏幕截图显示了对 SSMS 中各种群集类型的支持。 必须运行 17.1 版或更高版本。 下面的屏幕截图取自 17.2 版。

![SSMS AG 选项](media/sql-server-ha-story/image2.png)
 
##### <a name="required_synchronized_secondaries_to_commit"></a>REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT

SQL Server 2016 将 Enterprise 版本中支持的同步副本数从两个增加到了三个。 然而，如果其中一个次要副本已同步，但另一个副本遇到问题，则无法控制告知主要副本等待运行异常的副本或允许它继续运行的行为。 这表示即使次要副本未处于同步状态，在某种情况下主要副本仍会继续接收写入流量，这意味着次要副本上存在数据丢失。
现在，SQL Server 2017 中提供了一个选项，可控制在出现名为 REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT 的同步副本时执行何种操作的行为。 该选项的工作原理如下所示：
* 有三个可能的值：0、1 和 2
* 值是必须同步的次要副本数，它对数据丢失、可用性组可用性和故障转移都有影响
* 对于 WSFC 和群集类型为“无”的情况，默认值为 0，可手动设置为 1 或 2
* 对于群集类型为“外部”的情况，该值默认由群集机制设置，并可手动重写。 对于三个同步副本，默认值为 1。
在 Linux 上，REQUIRED SYNCHRONIZED SECONDARIES_TO_COMMIT 的值在群集中的可用性组资源上配置。 在 Windows 上，则通过 Transact-SQL 设置。

大于 0 的值可确保更高的数据保护程度，因为如果无法获得所需的次要副本数，那么在该问题解决之前，主要副本不可用。 REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT 还影响故障转移行为，因为如果适当数量的次要副本未处于正确状态，则不会发生自动故障转移。 在 Linux 上，若该值为 0 则不允许自动故障转移，因此在 Linux 上结合使用同步与自动故障转移时，该值必须设置为大于 0 才能实现自动故障转移。 在 Windows Server 上将该值设置为 0 是 SQL Server 2016 和更早版本的行为。

##### <a name="enhanced-microsoft-distributed-transaction-coordinator-support"></a>增强的 Microsoft 分布式事务处理协调器支持

在 SQL Server 2016 之前，对于需要分布式事务（在后台使用 DTC）的应用程序而言，在 SQL Server 中获取可用性的唯一方法是部署 FCI。 可通过以下两种方式之一执行分布式事务：
* 在同一 SQL Server 实例中跨越多个数据库的事务
* 跨越多个 SQL Server 实例或可能涉及非 SQL Server 数据源的事务

SQL Server 2016 引入了部分 DTC 支持，适用于涵盖后一种情况的可用性组。 SQL Server 2017 对此进行了完善，对以上两种情况都提供 DTC 支持。

可用性组 DTC 支持的另一增强之处在于，在 SQL Server 2016 中，只有在创建可用性组时才能启用对可用性组的 DTC 支持，之后无法添加这种支持。 而在 SQL Server 2017 中，也可在可用性组创建后向它添加 DTC 支持。

>[!NOTE]
> 只能为基于 Windows Server 的 SQL Server 实例中的数据库配置 DTC 支持。 如果应用程序要求 DTC，则必须使用 Windows Server 作为 SQL Server 部署的 OS，不能使用 Linux。 

#### <a name="always-on-failover-cluster-instances"></a>AlwaysOn 故障转移群集实例
自 6.5 版以来，群集安装一直是 SQL Server 的一项功能。 FCI 称为实例，是一种行之有效的方法，可为整个 SQL Server 安装提供可用性。 这意味着如果基础服务器遇到问题，则实例内的所有内容（包括数据库、SQL Server 代理作业、链接服务器等）都将移到另一台服务器。 所有 FCI 都要求某种形式的共享存储，即使通过网络提供。 FCI 的资源只能在任何给定时间由一个节点运行和拥有。 下图中，由群集的第一个节点拥有 FCI，这也意味着它拥有与之相关联的共享存储资源，这些资源用与存储相连的实线表示。

![故障转移群集实例](media/sql-server-ha-story/image3.png)
 
故障转移后，所有权发生更改，如下图所示。

![故障转移后](media/sql-server-ha-story/image4.png)
 
FCI 未出现数据丢失，但因为有一个数据副本，所以基础共享存储是单一故障点。 FCI 通常与其他可用性方法（如可用性组或日志传送）结合使用，具有数据库的冗余副本。 部署的其他方法应使用与 FCI 物理上分离的存储。 FCI 故障转移到另一个节点时，它会在一个节点上停止，并在另一个节点上启动，这类似于关闭服务器然后再打开。 FCI 遍历常规恢复过程，这意味着将回滚需要前滚的任何事务以及任何不完整的事务。 所以，数据库在从数据点到故障或手动故障转移的时间这一期间始终如一，没有数据丢失。 数据库仅在恢复完成后才可用，因此恢复时间取决于诸多因素，且通常比可用性组的故障转移时间更长。 但不利的一面是，对可用性组进行故障转移时，可能需要执行额外的任务才能使数据库可用，例如启用 SQL Server 代理作业。

如同可用性组一样，FCI 提取承载它的基础群集节点。 FCI 始终保留相同的名称。 应用程序和最终用户绝不会连接到节点；使用分配给 FCI 的唯一名称。 FCI 可作为一个承载主要副本或次要副本的实例加入可用性组。

下面的列表突出显示了 Windows Server 与 Linux 上的 FCI 之间存在的一些差异：

* 在 Windows Server 上，FCI 属于安装过程。 而 Linux 上的 FCI 则是在 SQL Server 安装完成后配置。
* Linux 仅支持每个主机安装一个 SQL Server，因此所有 FCI 都是默认实例。 Windows Server 支持每个 WSFC 具有最多 25 个 FCI。
* Linux 中 FCI 使用的公用名在 DNS 中定义，并且名称应与为 FCI 创建的资源相同。

#### <a name="log-shipping"></a>日志传送
如果恢复点和恢复时间目标更灵活，或者数据库中的任务并不是极为关键，则日志传送是 SQL Server 中另一个可靠的可用性功能。 基于 SQL Server 的本机备份，日志传送过程自动生成事务日志备份，并将其复制到一个或多个称为热备用状态的实例，然后自动将事务日志备份应用于该备用实例。 日志传送使用 SQL Server 代理作业自动执行备份、复制和应用事务日志备份的过程。

![日志传送](media/sql-server-ha-story/image5.png)
 
可以说，在某种程度上使用日志传送的最大优点在于它会考虑人为错误。 事务日志的应用可能会延迟。 因此，如果有人在没有 WHERE 子句的情况下发出类似 UPDATE 的内容，则备用可能尚未更改，如此便可在修复主系统时切换到该备用实例。 虽然日志传送易于配置，但始终需要手动从主系统切换到热备用状态的实例（称为角色更改）。 角色更改通过 Transact-SQL 启动，并且像可用性组一样，必须手动同步事务日志中未捕获的所有对象。 还需配置每个数据库的日志传送，而单个可用性组可包含多个数据库。 与可用性组或 FCI 不同，日志传送不提取角色更改。 应用程序必须能够处理这种情况。 可以使用 DNS 别名 (CNAME) 等技术，但存在利弊，例如切换后 DNS 刷新需要一定的时间。

## <a name="disaster-recovery"></a>灾难恢复

主要可用性位置遭遇地震或洪水等灾难事件时，企业必须做好准备，在其他地方将系统联机。 本部分介绍 SQL Server 可用性功能如何帮助实现业务连续性。

### <a name="always-on-availability-groups"></a>AlwaysOn 可用性组

可用性组的优点之一是可使用单个功能配置高可用性和灾难恢复。 由于不需要确保共享存储也具有高可用性，可以更轻松地实现在一个数据中心内具有用于高可用性的本地副本，在其他数据中心内具有用于灾难恢复的远程备份，且每个备份都有单独的存储。 确保冗余的代价是具有额外的数据库副本。 下面的示例为跨越多个数据中心的可用性组。 一个主要副本负责确保所有次要副本保持同步。

![可用性组](media/sql-server-ha-story/image6.png)
 
可用性组（群集类型为“无”的可用性组除外）要求所有副本都属于同一基础群集（无论是 WSFC 还是 Pacemaker）。 这意味着在上图中，WSFC 延伸到两个不同的数据中心，增加了复杂性。 无论使用什么平台（Windows Server 或 Linux）。 跨距离延伸群集会增加复杂性。 SQL Server 2016 中引入了分布式可用性组，它允许可用性组跨越在不同的群集上配置的可用性组。 这降低了节点必须全部位于同一个群集中这一要求，使配置灾难恢复更加容易。 有关分布式可用性组的详细信息，请参阅[分布式可用性组](../database-engine/availability-groups/windows/distributed-availability-groups.md)。

![分布式可用性组关系图。](media/sql-server-ha-story/image11.png)
 
### <a name="always-on-failover-cluster-instances"></a>AlwaysOn 故障转移群集实例

FCI 可用于灾难恢复。 与一般可用性组一样，基础群集机制也必须扩展到所有位置，这会增加复杂性。 FCI 还有一个注意事项：共享存储。 必须在主站点和辅助站点中使用相同的磁盘，因此需要借助外部方法，例如存储供应商在硬件层提供的功能或使用 Windows Server 中的存储副本，确保 FCI 使用的磁盘存在于其他地方。 

![AlwaysOn FCI](media/sql-server-ha-story/image8.png)
 
### <a name="log-shipping"></a>日志传送
日志传送是为 SQL Server 数据库提供灾难恢复最古老的方法之一。 日志传送通常与可用性组和 FCI 结合使用，在其他选项可能由于环境、管理技能或预算而可能不太适用的情况下，提供经济高效且更简单的灾难恢复。 与日志传送的高可用性情况类似，许多环境会延迟加载事务日志，以便解决人为错误。

## <a name="migrations-and-upgrades"></a><a name = "Migrations"></a>迁移和升级

部署新实例或升级旧实例时，业务不容许出现长时间的中断。 本部分介绍如何使用 SQL Server 的可用性功能，最大限度地减少执行计划内体系结构更改、服务器交换、平台更改（例如 Windows Server 和 Linux 之间）时的停机时间或修补过程中的停机时间。

> [!NOTE]
> 其他方法（例如使用备份和在别处还原）也可用于迁移和升级。 本文中不作介绍。 

### <a name="always-on-availability-groups"></a>AlwaysOn 可用性组

包含一个或多个可用性组的现有实例可就地升级至 SQL Server 2017。 虽然这要求一定的停机时间，但可通过适量计划，将此时间减至最少。 

如果目标是迁移到新服务器，而不更改配置（包括操作系统或 SQL Server 版本），那么可将这些服务器作为节点添加到现有基础群集，并添加到可用性组。 副本处于正确状态后，新服务器可能会发生手动故障转移，之后可将旧服务器从可用性组中删除并最终停止使用。 

分布式 AG 也是另一种迁移到新配置或升级 SQL Server 的方法。 因为分布式 AG 在不同体系结构上支持不同的基础 AG，例如，可以从在 Windows Server 2012 R2 上运行的 SQL Server 2016 更改为在 Windows Sever 2016 上运行的 SQL Server 2017。 

![分布式 AG](media/sql-server-ha-story/image10.png)

最后，群集类型为“无”的可用性组也可用于迁移或升级。 在典型的可用性组配置中，不能混合搭配群集类型，因此所有副本都需是“无”类型。 分布式可用性组可用于跨越配置了不同群集类型的可用性组。 不同的 OS 平台上也支持这种方法。

用于迁移和升级的可用性组的所有变体都允许超时完成工作中最耗时的部分 - 数据同步。 开始切换到新配置时，相对于长时间的停机，直接转换是短暂的中断，在此期间需完成包括数据同步在内的所有工作。 

可用性组在正在完成修补时手动将主要副本故障转移到次要副本，从而最大限度地缩短基础 OS 修补期间的停机时间。 从操作系统的角度来看，在 Windows Server 上执行此操作更为常见，因为该基础 OS 的维护可能经常（但非始终）需要重启。 修补 Linux 有时也需要重启，但并不频繁。 

[修补参与可用性组的 SQL Server 实例](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md)也可以尽量减少停机时间，具体取决于可用性组体系结构的复杂程度。 若要修补参与可用性组的服务器，需先修补次要副本。 正确数量的副本修补完成后，将主要副本手动故障转移到另一个节点，进行升级。 此时还可升级任何剩余的次要副本。 

### <a name="always-on-failover-cluster-instances"></a>AlwaysOn 故障转移群集实例

FCI 自身无法为传统的迁移或升级提供帮助；必须为 FCI 中的数据库以及参与其中的所有其他对象配置可用性组或日志传送。 然而，需要修补基础 Windows Server 时，Windows Server 中的 FCI 仍然是常用选项。 可启动手动故障转移，这意味着会出现短暂的中断，而不是使实例在整个 Windows Server 修补期间完全不可用。
FCI 可就地升级到 SQL Server 2017。 有关详细信息，请参阅[升级 SQL Server 故障转移群集实例](../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)。

### <a name="log-shipping"></a>日志传送

日志传送仍然是迁移和升级数据库的常用选项。 与可用性组相似，但在这种情况下使用事务日志作为同步方法，可在服务器切换之前启动数据传播。 切换时，一旦所有流量在来源处停止，则记录最终事务日志，并将其复制、应用到新配置。 此时，数据库即可联机工作。 通常，日志传送对速度较慢的网络的包容性更强，虽然相较于使用可用性组或分布式可用性组执行的切换而言，这种切换可能耗时稍长，但通常也以分钟为单位，而不是以小时、天或周为单位。

与可用性组相似，日志传送可提供一种在修补时切换到其他服务器的方法。

### <a name="other-sql-server-deployment-methods-and-availability"></a>其他 SQL Server 部署方法和可用性

还可通过另外两种方法部署 Linux 上的 SQL Server：容器和使用 Azure（或其他公有云提供程序）。 无论如何部署 SQL Server，一般都需要本文中介绍的可用性。 若要使 SQL Server 具有高可用性，使用这两种方法时需留意一些特别的注意事项。

[使用 Docker 的容器](../linux/quickstart-install-connect-docker.md)是部署 SQL Server 的新方法，适用于 Windows Server 或 Linux。 容器是可供运行的 SQL Server 的完整映像。 但是，目前没有对群集的本机支持，因此也没有对高可用性或灾难恢复的本机支持。 目前，借助容器使 SQL Server 数据库可用的可选方法是日志传送以及备份和还原。 虽然可以配置群集类型为“无”的可用性组，但如前所述，并不将此视为真正的可用性配置。 Microsoft 正在寻找使用容器启用可用性组或 FCI 的方法。 

如果现在使用容器，若容器丢失，可再次部署并附加到之前所用的共享存储（具体取决于容器平台）。 这种机制中的某些内容由容器业务流程协调程序提供。 虽然这确实提供了一些复原能力，但是会出现与数据库恢复相关的停机时间，而且并不像使用可用性组或 FCI 那样真正的高度可用。 

可通过使用 Azure 安装的 SQL Server 部署 Linux IaaS 虚拟机。 与基于本地的安装一样，支持的安装需要使用 Pacemaker 本身以外的 STONITH（俗称将其它节点“爆头”）。 通过隔离可用性代理提供 STONITH。 一些分发将其作为平台的一部分提供，其他则依赖于外部硬件和软件供应商。 查看你的首选 Linux 分发，了解其提供的是何种形式的 STONITH，以便能在公有云中部署支持的解决方案。

## <a name="cross-platform-and-linux-distribution-interoperability"></a>跨平台和 Linux 分发互操作性

现在，Windows Server 和 Linux 都支持 SQL Server，本部分介绍它们如何协同工作提供可用性和其他用途的相关方案，以及包含多个 Linux 分发的解决方案。

在介绍跨平台和互操作性方案之前，需要说明以下两个事实：

* 不存在基于 WSFC 的 FCI 或可用性组直接与基于 Linux 的 FCI 或可用性组一起使用的情况。 Pacemaker 节点不能扩展 WSFC，反之亦然。 
* FCI 或群集类型为“外部”的可用性组不支持混合 Linux 分发。 在这种情况下，所有可用性组副本必须配置相同的 Linux 分发，以及相同的版本。 支持 SQL Server 在两个平台或 Linux 的多个分发中运行的两种方式分别是可用性组和日志传送。

## <a name="distributed-availability-groups"></a>分布式可用性组 

分布式可用性组旨在跨可用性组配置，无论可用性组下的两个基础群集是两个不同的 WSFC、Linux 分发，还是一个在 WSFC 上，另一个在 Linux 上。 它会成为具有跨平台解决方案的主要方法。 也是迁移的主要解决方案，例如，公司需要从基于 Windows Server 的 SQL Server 基础结构迁移到基于 Linux 的基础结构。 如上所述，可用性组，尤其是分布式可用性组，可尽量减少应用程序无法使用的时间。 下面显示了跨 WSFC 和 Pacemaker 的分布式可用性组示例。

![显示跨 WSFC 和 Pacemaker 的分布式可用性组的关系图。](media/sql-server-ha-story/image9.png)
 
如果可用性组配置了“无”群集类型，则可跨越 Windows Server 和 Linux 以及多个 Linux 分发。 这不是真正的高可用性配置，所以不应用于任务关键型部署，但可用于读取缩放或迁移/升级方案。

## <a name="log-shipping"></a>日志传送

由于日志传送只基于备份和还原，所以 Windows Server 上的 SQL Server 与 Linux 上的 SQL Server 在数据库、文件结构等方面没有任何差异。 这意味着可在基于 Windows Server 的 SQL Server 安装和基于 Linux 的安装之间以及在 Linux 分发之间配置日志传送。 其他所有内容保持不变。 唯一需要注意的是，就像可用性组一样，若源的 SQL Server 主版本高于目标的 SQL Server 版本，则无法使用日志传送。 

## <a name="read-scale"></a><a name = "ReadScaleOut"></a>读取缩放

自在 SQL Server 2012 中引入次要副本后，它们已用于只读查询。 可用性组可实现两种方式：允许直接访问次要副本以及[配置只读路由](../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)，后者需要使用侦听器。  SQL Server 2016 引入了通过使用轮循机制算法的侦听器负载均衡只读连接的功能，允许只读请求分布在所有可读副本中。 

> [!NOTE]
> 可读次要副本仅是 Enterprise 版本中的功能，承载可读副本的每个实例都需要 SQL Server 许可证。

通过可用性组缩放数据库的可读副本这一功能最先是与分布式可用性组一起引入 SQL Server 2016 的。 这使公司能够通过少量配置，不仅在本地，并且可以在区域范围和全局范围内拥有数据库的只读副本，并通过在本地执行查询来减少网络流量和延迟。 可用性组的每个主要副本即使不是完全读/写副本，也可播种另外两个可用性组，因此每个分布式可用性组最多可支持 27 个可读取数据副本。 

![显示与读取扩展相关的分布式可用性组的关系图。](media/sql-server-ha-story/image11.png)

从 SQL Server 2017 开始，可以创建准实时只读解决方案，其中可用性组的群集类型配置为“无”。 如果目标是将可用性组用于可读次要副本而不是可用性，那么此操作可去除使用 WSFC 或 Pacemaker 的复杂性，并以更简单的部署方法提供可用性组的可读优势。 

唯一需要特别注意的是，由于没有群集类型为“无”的基础群集，配置只读路由稍有不同。 从 SQL Server 的角度来看，即使没有群集，仍然需要侦听器来路由请求。 使用主要副本的 IP 地址或名称，而不是配置传统的侦听器。 然后，使用主要副本路由只读请求。

在技术上，可通过还原数据库 WITH STANDBY 为可读使用配置日志传送热备用状态。 但是，由于事务日志需要独占使用数据库进行恢复，这意味着用户不能在这种时候访问数据库。 因此，日志传送并非理想解决方案，特别是在需要准实时数据的情况下。 

对于具有可用性组的所有读取缩放方案，应注意的一点是，与使用所有数据都是实时数据的事务复制不同，每个次要副本都不处于可应用唯一索引的状态，该副本是对主要副本的精确复制。 这意味着如果需要任何索引进行报告或需要处理数据，则必须在主要副本的数据库上执行。 如果需要灵活性，复制是对可读数据而言较佳的解决方案。

## <a name="summary"></a>总结

在 Windows Server 和 Linux 上使用相同的功能，可实现 SQL Server 2017 实例和数据库的高可用性。 除本地高可用性和灾难恢复的标准可用性方案之外，还可使用 SQL Server 中的可用性功能尽量减少与升级和迁移相关的停机时间。 可用性组还可提供数据库的其他副本，用作同一体系结构的一部分，扩大可读副本。 无论是使用 SQL Server 2017 部署新的解决方案还是考虑升级，SQL Server 2017 均可提供所需的可用性和可靠性。
 
[SimpleAG]:media\sql-server-ha-story\image1.png
[SSMSAGOptions]:media\sql-server-ha-story\image2.png
[BasicFCI]:media\sql-server-ha-story\image3.png
[PostFailoverFCI]:media\sql-server-ha-story\image4.png
[LogShipping]:media\sql-server-ha-story\image5.png
[AG]:media\sql-server-ha-story\image6.png
[DAG]:media\sql-server-ha-story\image7.png
[AlwaysOnFCI]:media\sql-server-ha-story\image8.png
[BasicDAG]:media\sql-server-ha-story\image9.png
[image10]:media\sql-server-ha-story\image10.png
[DAG]:media\sql-server-ha-story\image11.png