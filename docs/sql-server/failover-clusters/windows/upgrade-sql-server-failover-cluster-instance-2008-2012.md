---
title: 升级 SQL Server 实例：Windows Server 2012 及更高版本群集
description: 介绍了如何升级在 Windows Server 2008、Windows Server 2008 R2 和 Windows Server 2012 上运行的 SQL Server 故障转移群集实例。
ms.date: 06/15/2020
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- upgrading failover clusters
- clusters [SQL Server], upgrading
- failover clustering [SQL Server], upgrading
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8077a17676d38937d82241c2e58b556bd322f8a2
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96120983"
---
# <a name="upgrade-sql-server-instances-running-on-windows-server-20082008-r22012-clusters"></a>升级在 Windows Server 2008/2008 R2/2012 群集上运行的 SQL Server 实例

[!INCLUDE[nextref-longhorn-md](../../../includes/nextref-longhorn-md.md)]、[!INCLUDE[winserver2008r2-md](../../../includes/winserver2008r2-md.md)] 和 [!INCLUDE[win8srv-md](../../../includes/win8srv-md.md)] 阻止 Windows Server 故障转移群集执行操作系统就地升级，限制了群集允许的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本。 群集升级到 [!INCLUDE[winblue-server-2-md](../../../includes/winblue-server-2-md.md)] 或更高版本后，便可保持最新。

## <a name="prerequisites"></a>先决条件

-   在执行任何迁移策略之前，必须先准备好 Windows Server 2016/2012 R2 的并行 Windows Server 故障转移群集。 所有包含 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例 (FCI) 的节点都必须加入安装了并行 FCI 的 Windows 群集。 任何独立计算机都不得在迁移之前加入 Windows Server 故障转移群集  。 迁移之前，应在新环境中同步用户数据库。
-   所有目标实例必须与其在原始坏境中的并行实例在相同版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 上运行，并使用相同的实例名称和 ID，安装相同的功能。 目标计算机上的安装路径和目录结构应完全相同。 这不包括 FCI 虚拟网络名称，在迁移之前，该名称不能相同。 目标实例还应启用原始实例所启用的任何功能（Always On、FILESTREAM 等）。

-   迁移前，并行群集不应安装 [!INCLUDE[sshadrc-md](../../../includes/sshadrc-md.md)]。

-   对于使用严格意义的可用性组（包含或不包含 SQL FCI）的群集，使用分布式可用性组可极大地限制迁移该群集时的停机时间，但这需要所有实例都运行 [!INCLUDE[sssql15-md](../../../includes/sssql15-md.md)] RTM（或更高）版本。

-   所有迁移策略都需要 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sysadmin 角色。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务所用的所有 Windows 用户（即运行复制代理的 Windows 帐户）都必须具有新环境中每台计算机的 OS 级别权限。

-   原始 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 群集环境中使用的任何网络文件共享和网络映射驱动器都必须仍然存在，并可供与原始实例具有相同权限的目标群集访问。

-   原始 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例侦听的任何 TCP/IP 端口都必须是未使用的，并允许目标计算机上的入站流量。
-   所有与 SQL 相关的服务都应由同一个 Windows 用户安装并运行。

-   必须使用与原始实例相同的区域设置安装目标实例。

## <a name="migration-scenarios"></a>迁移方案

适用的迁移策略取决于原始 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 群集拓扑的某些参数，即 [!INCLUDE[sshadrc-md](../../../includes/sshadrc-md.md)]的使用情况和 SQL 故障转移群集实例。 所选策略还取决于目标环境的需求。 如果新环境要求每台计算机或 SQL FCI 保留原始虚拟网络名称 (VNN)，或如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 拓扑所依赖的新实例继承了原始实例的所有系统对象，则必须选择可迁移这些对象的策略。

|                                   | 需要所有服务器对象和 VNN | 需要所有服务器对象和 VNN | 不需要服务器对象/VNN\* | 不需要服务器对象/VNN\* |
|-----------------------------------|--------------------------------------|--------------------------------------------------------------------|------------|------------|
| **是否为可用性组？  （是/否）**                  | **_是_**                              | **_否_**                                                            | **_是_**    | **_否_**    |
| **群集仅使用 SQL FCI**         | [应用场景 3](#scenario-3-windows-cluster-has-both-sql-fcis-and-sql-server-availability-groups)                           | [应用场景 2](#scenario-2-windows-clusters-with-sql-server-failover-cluster-instances-fcis)                                                        | [应用场景 1](#scenario-1-windows-cluster-with-sql-server-availability-groups-and-no-failover-cluster-instances-fcis) | [应用场景 2](#scenario-2-windows-clusters-with-sql-server-failover-cluster-instances-fcis) |
| **群集使用独立实例** | [应用场景 5](#scenario-5-windows-cluster-with-standalone-sql-server-instances-and-availability-groups)                           | [应用场景 4](#scenario-4-windows-cluster-with-standalone-sql-server-instances-and-no-availability-groups)                                                         | [应用场景 1](#scenario-1-windows-cluster-with-sql-server-availability-groups-and-no-failover-cluster-instances-fcis) | [应用场景 4](#scenario-4-windows-cluster-with-standalone-sql-server-instances-and-no-availability-groups) |

\*不包括可用性组侦听程序名称

## <a name="scenario-1-windows-cluster-with-sql-server-availability-groups-and-no-failover-cluster-instances-fcis"></a>应用场景 1：采用 SQL Server 可用性组且不使用故障转移群集实例 (FCI) 的 Windows 群集
如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序使用可用性组 (AG)，且没有故障转移群集实例，则可通过在 Windows Server 2016/2012 R2 的其他 Windows 群集上创建并行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 部署，将其迁移到新群集。 此后，可以创建分布式 AG，其中的目标群集为当前生产群集的辅助群集。 这不需要用户升级到 [!INCLUDE[sssql15-md](../../../includes/sssql15-md.md)] 或更高版本。

###  <a name="to-perform-the-upgrade"></a>执行升级

1.  根据需要将任意实例升级到 [!INCLUDE[sssql15-md](../../../includes/sssql15-md.md)] 或更高版本。 并行实例应运行相同版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。

2.  为目标群集创建可用性组。 如果目标群集的主节点不是 FCI，则创建一个侦听程序。

3.  组成一个分布式可用性组，其中的目标群集为辅助可用性组。

    >[!NOTE]
    >对于将 SQL FCI 用作主实例的 AG，创建分布式 AG T-SQL 的 LISTENER\_URL 参数的行为有所不同。 如果是针对主 AG 或辅助 AG 的情况，则将主 SQL FCI 的 VNN 用作侦听程序 URL 代替侦听程序的网络名称，且使用数据库镜像终结点端口。

4.  将辅助可用性组联接到分布式 AG。

5.  在辅助 AG 上联接辅助数据库。

    >[!NOTE]
    >如果目标可用性组使用自动种子设定，则可自动完成此操作；仅当所有副本的数据和日志路径都相同时才可完成此操作。

6.  截断到主 AG 的所有流量，并允许同步辅助 AG。

7.  将两个可用性组上的提交策略都更改为 SYNCHRONOUS_COMMIT，并在状态为“已同步”时故障转移到目标群集。

8.  删除分布式 AG。

9.  删除或重命名原始 AG 上的侦听程序。

10. 使用原始 AG 的侦听程序名称重命名或创建新 AG 的侦听程序。

    >[!NOTE]
    >已存在原始 AG 侦听程序的 DNS 记录时，尝试使用此名称创建侦听程序将失败。

11. 恢复通向侦听程序的流量。

## <a name="scenario-2-windows-clusters-with-sql-server-failover-cluster-instances-fcis"></a>应用场景 2：采用 SQL Server 故障转移群集实例 (FCI) 的 Windows 群集

如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 环境只包括 SQL FCI 实例，则可通过在 Windows Server 2016/2012 R2 的其他 Windows 群集上创建并行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 环境，将其迁移到新群集。 通过“窃取”旧 SQL FCI 的 VNN 并在新群集上获取这些 VNN，迁移到目标群集。 这将导致额外的停机时间，具体取决于 DNS 传播时间。

###  <a name="to-perform-the-upgrade"></a>执行升级

1.  进行完整备份，并停止通向原始 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 群集的流量。

2.  对用户数据库进行结尾日志备份，并在新环境中恢复还原。

3.  在故障转移群集管理器中的目标群集上，关闭每个 SQL FCI 群集角色。

4.  仍是在故障转移群集管理器中的目标群集上，重新打开每个 SQL FCI 所用的群集磁盘。

5.  在故障转移群集管理器中的原始群集上，关闭每个 SQL FCI 群集角色。

6.  仍是在故障转移群集管理器中的原始群集上，重新打开每个 SQL FCI 所用的群集磁盘。

7.  将系统数据库从原始计算机复制到其并行目标计算机。

8.  在故障转移群集管理器的原始环境中，更改每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 角色的“服务器名称”资源的名称。

9.  现在，只需将每个 SQL FCI 角色已重命名的服务器名称资源重新联机。

10. 现在，在故障转移群集管理器中的目标群集上，将每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 角色的“服务器名称”资源重命名为原始群集以前保留的名称。

    >[!NOTE]
    >删除名称的 DNS 记录后，不会再出现因另一台计算机已保留名称引发的错误。

11. 重命名所有 FCI 后，在新群集中重启每台计算机。

12. 随着计算机重启后恢复联机，在故障转移群集管理器中启动每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 角色。

## <a name="scenario-3-windows-cluster-has-both-sql-fcis-and-sql-server-availability-groups"></a>应用场景 3：兼具 SQL FCI 和 SQL Server 可用性组的 Windows 群集

如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序未使用任何独立的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，仅使用至少包含在一个可用性组中的 SQL FCI，则可以使用类似于“无可用性组，无独立实例”应用场景的方法将其迁移到新的群集。 在将系统表复制到目标 FCI 共享磁盘之前，必须在原始环境中删除所有可用性组。 将所有数据库迁移到目标计算机后，使用相同的架构和侦听程序名称重新创建可用性组。 通过执行此操作，可在目标群集上正确组成和管理 Windows Server 故障转移群集资源。 在迁移之前，必须在目标环境中每台计算机上的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器中启用 Always On  。

### <a name="to-perform-the-upgrade"></a>执行升级

1.  停止通向 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的流量。

2.  对用户数据库进行结尾日志备份，并在新环境的目标主要副本上恢复还原，而不在所有目标次要副本上恢复。

3.  在故障转移群集管理器中的目标群集上，关闭每个 SQL FCI 群集角色。

4.  仍是在故障转移群集管理器中的目标群集上，重新打开每个 SQL FCI 所用的群集磁盘。

5.  在原始群集上，删除可用性组。

6.  在故障转移群集管理器中的原始群集上，关闭每个 SQL FCI 群集角色。

7.  仍是在故障转移群集管理器中的原始群集上，重新打开每个 SQL FCI 所用的群集磁盘。

8.  将系统数据库从原始计算机复制到其并行目标计算机。

9.  在故障转移群集管理器的原始环境中，更改每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 角色的“服务器名称”资源的名称。

10. 现在，只需将每个 SQL FCI 角色已重命名的服务器名称资源重新联机。

11. 现在，在故障转移群集管理器中的目标群集上，将每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 角色的“服务器名称”资源重命名为原始群集以前保留的名称。

12. 重命名所有 FCI 后，在新群集中重启每台计算机。

13. 随着计算机重启后恢复联机，在故障转移群集管理器中启动每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 角色。

14. 所有实例都联机后，可在恢复还原数据库的副本上形成可用性组。

15. 将所有次要副本联接到 AG，然后将所有辅助数据库联接到 AG。

16. 在新 AG 中使用原始可用性组侦听程序的名称创建侦听程序。

## <a name="scenario-4-windows-cluster-with-standalone-sql-server-instances-and-no-availability-groups"></a>应用场景 4：采用独立 SQL Server 实例且不使用可用性组的 Windows 群集

迁移包含独立实例的群集的过程与迁移仅包含 FCI 的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 群集相似，但是不更改 FCI 网络名称群集资源的 VNN，而是更改原始独立计算机的计算机名称，并在目标计算机上“窃取”旧计算机的名称。 与不包含独立实例的应用场景相比，这确实产生了额外的停机时间，因为在获取旧计算机的网络名称之前，无法将目标独立计算机联接到 WSFC。

###  <a name="to-perform-the-upgrade"></a>执行升级

1.  停止通向 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的流量。

2.  对用户数据库进行结尾日志备份，并在每天计算机上的新环境中恢复还原。

3.  在故障转移群集管理器中的目标群集上，关闭每个 SQL FCI 群集角色。

4.  仍是在故障转移群集管理器中的目标群集上，重新打开每个 SQL FCI 所用的群集磁盘。

5.  在故障转移群集管理器中的原始群集上，关闭每个 SQL FCI 群集角色，并在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器中停止 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 独立实例。

6.  对于原始群集中的每个独立计算机，将每台计算机重命名为唯一的新计算机名称。 按指示重启每台计算机。

7.  在故障转移群集管理器中的原始群集上，重新打开每个 SQL FCI 所用的群集磁盘。

8.  将系统数据库复制到目标计算机。

9.  在故障转移群集管理器的原始环境中，将每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 角色的“服务器名称”资源更改为唯一的新名称。

10. 现在，只需将每个 SQL FCI 角色已重命名的服务器名称资源重新联机。

11. 现在，在并行独立实例上，将计算机重命名为原始的独立计算机名称。 （删除旧服务器名称，使用本地参数添加新服务器名称。）按照指示重启计算机。

12. 重启后，将每个独立计算机联接到目标 Windows Server 故障转移群集。

13. 现在，在故障转移群集管理器中的目标群集上，将每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 角色的“服务器名称”资源重命名为原始群集以前保留的名称。

14. 重命名所有 FCI 后，在新群集中重启每台计算机。

15. 随着计算机重启后恢复联机，在故障转移群集管理器中启动每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 角色。

## <a name="scenario-5-windows-cluster-with-standalone-sql-server-instances-and-availability-groups"></a>应用场景 5：采用独立 SQL Server 实例和可用性组的 Windows 群集

对于使用可用性组且包含独立副本的群集，其迁移过程类似于迁移具有 FCI 且使用可用性组的群集。 仍然必须删除原始可用性组并在目标集群上重建；但是，由于迁移独立实例带来的额外开销，产生了额外的停机时间。 在迁移之前，必须在目标环境中的每个 FCI 上启用 Always On  。

###  <a name="to-perform-the-upgrade"></a>执行升级

1.  停止通向 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的流量。

2.  对用户数据库进行结尾日志备份，并在新环境的目标主要副本上恢复还原，而不在每个目标次要副本上恢复。

3.  删除原始群集上的可用性组。

4.  在故障转移群集管理器中的目标群集上，关闭每个 SQL FCI 群集角色。

5.  仍是在故障转移群集管理器中的目标群集上，重新打开每个 SQL FCI 所用的群集磁盘。

6.  在故障转移群集管理器中的原始群集上，关闭每个 SQL FCI 群集角色，并在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器中停止 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 独立实例。

7.  对于原始群集中的每个独立计算机，将每台计算机重命名为唯一的新计算机名称。 按指示重启每台计算机。

8.  在故障转移群集管理器中的原始群集上，重新打开每个 SQL FCI 所用的群集磁盘。

9.  将系统数据库复制到目标计算机。

10. 在故障转移群集管理器的原始环境中，将每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 角色的“服务器名称”资源更改为唯一的新名称。

11. 现在，只需将每个 SQL FCI 角色已重命名的服务器名称资源重新联机。

12. 现在，在并行独立实例上，将计算机重命名为原始的独立计算机名称。 （在 SQL 中删除然后添加服务器名称。）按照指示重启计算机。

13. 重启后，将每个独立计算机联接到目标 Windows Server 故障转移群集。

14. 现在，在故障转移群集管理器中的目标群集上，将每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 角色的“服务器名称”资源重命名为原始群集以前保留的名称。

15. 重命名所有 FCI 后，在新群集中重启每台计算机。

16. 随着计算机重启后恢复联机，在故障转移群集管理器中启动每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI 角色。

17. 所有实例都联机后，在目标主要副本上重新创建可用性组。

18. 联接每个次要副本和辅助数据库。

19. 重新创建与原始侦听程序同名的可用性组侦听程序。

## <a name="specific-concerns-for-individual-features"></a>各项功能的特定问题

### [!INCLUDE[sshadrc-md](../../../includes/sshadrc-md.md)]

-   **数据库镜像终结点**

    从 SQL 的角度来看，数据库镜像终结点与系统表一起迁移到新的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 在迁移之前，请确保已在防火墙中应用正确的规则，并且没有其他进程在侦听同一端口。

-   **可用性组**

    无法在实例之间迁移可用性组及其侦听程序。 无法在目标环境中轻松重新创建可用性组创建的 Windows Server 故障转移群集资源。 应在目标群集上删除并重新创建可用性组，而非尝试迁移可用性组。

-   **可用性组侦听程序**

    与可用性组本身类似，应删除并重新创建侦听程序，而不是直接进行迁移。

### <a name="replication"></a>复制

-   **远程分发服务器、发布服务器、订阅服务器**

    分发服务器和发布服务器之间的关系只依赖于承载这二者的计算机的 VNN，该 VNN 可正确解析为新计算机。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理作业也会随系统表一起正确地迁移，因此各种复制代理能够照常继续执行。 在迁移之前，任何运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理本身或任何 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理作业的 Windows 帐户必须在目标环境中拥有相同的权限。 与发布服务器和订阅服务器的通信将照常执行。

-   **快照文件夹**

    在迁移之前，任何 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能所用的任何网络共享需要可供目标环境中的计算机（具有与原始环境相同的权限）访问。 需要先确保满足此条件才可进行迁移。

### <a name="service-broker"></a>服务代理

-   **Service Broker 终结点**

    从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的角度来看，没有与终结点相关的问题。 在迁移之前，必须确保没有进程正在侦听同一端口，并且没有任何防火墙规则阻止该端口，或者存在专门允许该端口的防火墙规则。

-   **证书**

    如果需要将证书还原到新计算机，还应该备份证书并将其还原到目标计算机。

-   **路由**

    路由取决于目标的虚拟网络名称，在新环境中，计算机名称和 SQL FCI 网络名称的虚拟网络名称都将解析为正确的计算机。 引用的任何其他 VNN 也必须重定向到新计算机。

-   **远程服务绑定**

    迁移后，远程服务绑定按预期运行，因为任何使用远程服务绑定的用户都可正确迁移。

### <a name="ssnoversion-agent"></a>[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理

-   **作业**

    作业随系统数据库一起正确迁移。 运行 SQL 代理作业或 SQL 代理本身的任何用户在目标计算机上都具有必备条件中指定的相同权限。

-   **警报和运算符**

    警报和运算符随系统数据库一起正确迁移。

### <a name="filestream"></a>FILESTREAM

-   **Windows 文件共享端口**

    Windows 文件共享端口 139 和 445 都必须具有允许入站流量使用 FILESTREAM 的规则

-   **Windows 共享**

    Windows 共享路径取决于 SQL FCI 名称资源，因为它是通过 `\\ServerName\ShareName` 访问的。 迁移 FILESTREAM 要求目标 FCI 中的所有节点都启用 FILESTREAM，并且如果使用 Windows 共享，则配置为使用与原始计算机相同的 Windows 共享名称。 目标 FCI 获得正确的服务器名称后，可使用所需的路径承载 Windows 共享。

-   **FILESTREAM 数据**

    FILESTREAM 数据包含在备份中。

### <a name="integration-services"></a>Integration Services

-   **SSIS 项目**

    SSIS 项目随 SSIS 数据库一起迁移。 移动 SSIS 数据库后，可立即在移动任何系统表之前执行程序包。

-   **基于文件的数据源**

    必须可在 SSIS 包指定的相同位置访问平面文件、Excel 文件、XML 源以及其他数据源。

## <a name="next-steps"></a>后续步骤
- [完成数据库引擎升级](../../../database-engine/install-windows/complete-the-database-engine-upgrade.md)
- [更改数据库兼容性模式和使用 Query Store](../../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)
- [利用新的 SQL Server 2016 功能](../../what-s-new-in-sql-server-2017.md)
- [升级 SQL Server 故障转移群集实例](upgrade-a-sql-server-failover-cluster-instance.md)
- [查看和读取 SQL Server 安装程序日志文件](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [向 SQL Server 2016 实例添加功能（安装程序）](../../../database-engine/install-windows/add-features-to-an-instance-of-sql-server-setup.md)