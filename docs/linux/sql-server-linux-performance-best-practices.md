---
title: Linux 上的 SQL Server 的性能最佳做法
description: 本文提供运行 Linux 上的 SQL Server 的性能最佳做法和指南。
author: tejasaks
ms.author: tejasaks
ms.reviewer: vanto
ms.date: 12/11/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 89b8a7c087fb87ed911be640126ec81021b045a7
ms.sourcegitcommit: 2991ad5324601c8618739915aec9b184a8a49c74
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97323254"
---
# <a name="performance-best-practices-and-configuration-guidelines-for-sql-server-on-linux"></a>Linux 上的 SQL Server 的性能最佳做法和配置指南

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文提供了最佳做法和建议，以最大程度地提高连接到 Linux 上的 SQL Server 的数据库应用程序的性能。 这些建议特定在 Linux 平台上运行。 所有正常 SQL Server 建议（例如索引设计）仍适用。

以下指南包含配置 SQL Server 和 Linux 操作系统 (OS) 的建议。

## <a name="linux-os-configuration"></a>Linux OS 配置

请考虑使用以下 Linux OS 配置设置，以体验 SQL Server 安装的最佳性能。

### <a name="storage-configuration-recommendation"></a>存储配置建议

#### <a name="use-storage-subsystem-with-appropriate-iops-throughput-and-redundancy"></a>使用具有适当 IOPS、吞吐量和冗余的存储子系统

托管数据、事务日志和其他关联文件（如内存中 OLTP 的检查点文件）的存储子系统应能够正常管理平均和峰值工作负载。 通常情况下，在本地环境中，存储供应商支持在多个磁盘之间进行条带化的适当硬件 RAID 配置，以确保适当的 IOPS、吞吐量和冗余。 不过，在不同存储供应商和具有不同体系结构的不同存储产品之间，这可能有所不同。

对于在 Azure 虚拟机上部署的 Linux 上的 SQL Server，请考虑使用软件 RAID 来确保满足相应的 IOPS 和吞吐量要求。 在 Azure 虚拟机上配置 SQL Server 时，请参阅以下文章，了解类似的存储注意事项：[SQL Server VM 的存储配置](https://docs.microsoft.com/azure/azure-sql/virtual-machines/windows/storage-configuration)

以下示例介绍如何在 Azure 虚拟机上的 Linux 中创建软件 raid。 下面提供了一个示例，但你应该根据数据、事务日志和 tempdb IO 要求，使用适当数量的数据磁盘，为卷提供所需的吞吐量和 IOPS。 在此示例中，已向 Azure 虚拟机附加 8 个数据磁盘；4 个用于托管数据文件，2 个用于事务日志，2 个用于 tempdb 工作负载。

```bash
# To locate the devices (for example /dev/sdc) for RAID creation, use the lsblk command
# For Data volume, using 4 devices, in RAID 5 configuration with 8KB stripes
mdadm --create --verbose /dev/md0 --level=raid5 --chunk=8K --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf

# For Log volume, using 2 devices in RAID 10 configuration with 64KB stripes
mdadm --create --verbose /dev/md1 --level=raid10 --chunk=64K --raid-devices=2 /dev/sdg /dev/sdh

# For tempdb volume, using 2 devices in RAID 0 configuration with 64KB stripes
mdadm --create --verbose /dev/md2 --level=raid0 --chunk=64K --raid-devices=2 /dev/sdi /dev/sdj
```

#### <a name="file-system-configuration-recommendation"></a>文件系统配置建议

SQL Server 支持使用 EXT4 和 XFS 文件系统承载数据库、事务日志和其他文件，例如 SQL Server 中的内存中 OLTP 的检查点文件。 Microsoft 建议使用 XFS 文件系统来托管 SQL Server 数据和事务日志文件。

```bash
# Formatting the volume with XFS filesystem
mkfs.xfs /dev/md0 -f -L datavolume
mkfs.xfs /dev/md1 -f -L logvolume
mkfs.xfs /dev/md2 -f -L tempdb
```

> [!NOTE]
> 创建 XFS 卷并设置其格式时，可以将 XFS 文件系统配置为不区分大小写。 这不是 Linux 生态系统中经常使用的配置，但可出于兼容性原因而使用。
>
> 示例：mkfs.xfs /dev/md0 -f -n version=ci -L datavolume
>
> 在此示例中，使用参数 `-n version=ci` 将 XFS 文件系统配置为不区分大小写。

##### <a name="persistent-memory-filesystem-recommendation"></a>PMEM 文件系统建议

对于 PMEM 设备上的文件系统配置，基础文件系统的块分配应为 2 MB。 有关本主题的详细信息，请参阅[技术注意事项](sql-server-linux-configure-pmem.md#technical-considerations)一文。

#### <a name="disable-last-accessed-datetime-on-file-systems-for-sql-server-data-and-log-files"></a>在文件系统上禁用 SQL Server 数据和日志文件的上次访问日期/时间

为确保在重启后自动重新装载附加到系统的驱动器，必须将其添加到 `/etc/fstab` 文件。 此外，强烈建议在 `/etc/fstab` 中使用 UUID（全局唯一标识符）来引用驱动器，而不是只使用设备名称（例如 `/dev/sdc1`）。

对用于存储 SQL Server 数据和日志文件的任何文件系统，强烈建议使用 noatime 属性。 有关如何设置此属性的说明，请参阅 Linux 文档。 以下示例介绍如何为 Azure 虚拟机中装载的卷启用 noatime 选项。

**_/etc/fstab_* _ 中的装入点条目

```bash
UUID="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" /data1 xfs,rw,attr2,noatime 0 0
```

在上面的示例中，UUID 表示可使用 _*_blkid_*_ 命令找到的设备。

#### <a name="sql-server-and-forced-unit-access-fua-io-subsystem-capability"></a>SQL Server 和强制单元访问 (FUA) I/O 子系统功能

某些版本的受支持 Linux 分发版支持 FUA I/O 子系统功能，以提供数据持久性。 SQL Server 使用 FUA 功能为 SQL Server 工作负载提供高效且可靠的 I/O。 有关 Linux 分发版的 FUA 支持的额外信息及其对 SQL Server 的影响，请阅读以下博客：[Linux 上的 SQL Server：强制单元访问 (FUA) 内部结构](https://bobsql.com/sql-server-on-linux-forced-unit-access-fua-internals/)

SUSE Linux Enterprise Server 12 SP5 和 Red Hat Enterprise Linux 8.0 以上的版本支持 I/O 子系统中的 FUA 功能。 如果使用 SQL Server 2017 CU6 及更高版本或 SQL Server 2019，则应使用以下配置，以便通过 SQL Server 使用 FUA 进行高性能、高效的 I/O 实现。

如果满足以下条件，请使用下面列出的推荐配置。

- 使用 SQL Server 2017 CU6 或更高版本，或 SQL Server 2019
- 使用支持 FUA 功能的 Linux 分发和版本（Red Hat Enterprise Linux 8.0 或更高版本，或 SUSE Linux Enterprise Server 12 SP5）
- 在支持 FUA 功能并已进行相应配置的存储子系统和/或硬件上

推荐配置：

1. 启用跟踪标志 3979 作为启动参数
2. 使用 *mssql-conf** 配置 `control.writethrough = 1` 和 `control.alternatewritethrough = 0`

对于不符合上述条件的几乎所有其他配置，建议的配置如下所示：

1. 启用跟踪标志 3982 作为启动参数（这是 Linux 生态系统中 SQL Server 的默认值），同时确保未启用跟踪标志 3979 作为启动参数
2. 使用 **mssql-conf** 配置 `control.writethrough = 1` 和 `control.alternatewritethrough = 1`

### <a name="kernel-and-cpu-settings-for-high-performance"></a>高性能内核和 CPU 设置

下一部分介绍是与 SQL Server 安装的高性能和吞吐量相关的推荐 Linux OS 设置。 请参阅 Linux OS 文档，以了解如何配置这些设置。 根据说明，使用 [**_Tuned_* _](https://tuned-project.org) 帮助配置下述的许多 CPU 和内核配置。

#### <a name="using-__tuned__-to-configure-kernel-settings"></a>使用 _*_Tuned_*_ 来配置内核设置

对于 Red Hat Enterprise Linux (RHEL) 用户，[优化的](https://tuned-project.org)吞吐量-性能配置文件将自动配置某些内核和 CPU 设置（C 状态除外）。 自 RHEL 8.0 起，与 Red Hat 共同开发了名为 _ *mssql** 的 _*_Tuned_*_ 配置文件，它可为 SQL Server 工作负载提供更精细的 Linux 性能相关优化。 此配置文件包含 RHEL 吞吐量-性能配置文件，我们在下面提供了它的定义，供你查看其他 Linux 发行版和不带此配置文件的 RHEL 版本。

对于 SUSE Linux Enterprise Server 12 SP5、Ubuntu 18.04 和 Red Hat Enterprise Linux 7.x，可以手动安装 **_Tuned_ *_ 包。可以用它创建和配置 _* mssql** 配置文件，如下所述。

##### <a name="proposed-linux-settings-using-a-tuned-mssql-profile"></a>使用优化的 mssql 配置文件的建议 Linux 设置

```bash
#
# A Tuned configuration for SQL Server on Linux
#

[main]
summary=Optimize for Microsoft SQL Server
include=throughput-performance

[cpu]
force_latency=5

[sysctl]
vm.swappiness = 1
vm.dirty_background_ratio = 3
vm.dirty_ratio = 80
vm.dirty_expire_centisecs = 500
vm.dirty_writeback_centisecs = 100
vm.transparent_hugepages=always
# For multi-instance SQL deployments, use
# vm.transparent_hugepages=madvise
vm.max_map_count=1600000
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
kernel.numa_balancing=0
kernel.sched_latency_ns = 60000000
kernel.sched_migration_cost_ns = 500000
kernel.sched_min_granularity_ns = 15000000
kernel.sched_wakeup_granularity_ns = 2000000
```

若要启用此优化的配置文件，请将这些定义保存在 `/usr/lib/tuned/mssql` 文件夹下的 tuned.conf 文件中，并使用以下命令启用配置文件：

```bash
chmod +x /usr/lib/tuned/mssql/tuned.conf
tuned-adm profile mssql
```

通过以下命令验证是否已启用它：

```bash
tuned-adm active
```

或

```bash
tuned-adm list
```

#### <a name="cpu-settings-recommendation"></a>CPU 设置建议

下表提供了 CPU 设置的建议：

| 设置 | 值 | 详细信息 |
|---|---|---|
| CPU 频率调控器 | 性能 | 请参阅 **cpupower** 命令 |
| ENERGY_PERF_BIAS | 性能 | 请参阅 **x86_energy_perf_policy** 命令 |
| min_perf_pct | 100 | 查看有关 intel p 状态的文档 |
| C 状态 | 仅限 C1 | 请参阅 Linux 或系统文档，了解如何确保将 C 状态设置为仅限 C1 |

如上文所述，使用 **_Tuned_ *_ 将自动配置 CPU 频率调控器、ENERGY_PERF_BIAS 和 min_perf_pct 设置，因为使用吞吐量-性能配置文件作为 _* mssql** 配置文件的基础。 必须根据 Linux 或系统分销商提供的文档手动配置 C 状态参数。

#### <a name="disk-settings-recommendations"></a>磁盘设置建议

下表提供了磁盘设置的建议：

| 设置 | 值 | 详细信息 |
|---|---|---|
| disk `readahead` | 4096 | 请参阅 `blockdev` 命令 |
| sysctl 设置 | kernel.sched_min_granularity_ns = 10000000<br/>kernel.sched_wakeup_granularity_ns = 15000000<br/>vm.dirty_ratio = 40<br/>vm.dirty_background_ratio = 10<br/>vm.swappiness = 1 | 请参阅 **sysctl** 命令 |

**描述：**

- **vm.swappiness**：此参数控制赋予交换出运行时内存（通过限制内核以交换出 SQL Server 处理内存页面）的相对权重。

- **vm.dirty_\** _：不会缓存 SQL Server 文件写入访问，这样做满足其数据完整性要求。 通过分配足够大的缓存并限制刷新，这些参数可以实现 Linux 缓存写入的有效异步写入性能，并降低其存储 IO 影响。

- _*kernel.sched_\**_：这些参数值代表着最新建议，用于在 Linux 内核中调整完全公平计划 (CFS) 算法，以便在线程的进程内抢占和恢复方面提高网络和存储 IO 调用的吞吐量。

使用 _*mssql** **_Tuned_*_ 配置文件来配置 _*vm.swappiness**, **vm.dirty_\* *_ 和 _* kernel.sched_\**_ 设置。 使用 `blockdev` 命令的磁盘 `readahead` 配置以单个设备为基础，必须手动执行。

#### <a name="kernel-setting-auto-numa-balancing-for-multi-node-numa-systems"></a>多节点 NUMA 系统的内核设置自动 numa 平衡

如果在多节点 _ *NUMA** 系统上安装 SQL Server，则默认会启用以下 kernel.numa_balancing 内核设置。 若要使 SQL Server 在 NUMA  系统上以最高效率运行，请在多节点 NUMA 系统上禁用自动 NUMA 平衡：

```bash
sysctl -w kernel.numa_balancing=0
```

使用 **mssql** **_Tuned_ *_ 配置文件来配置 _* kernel.numa_balancing** 选项。

#### <a name="kernel-settings-for-virtual-address-space"></a>虚拟地址空间的内核设置

vm.max_map_count  的默认设置 (65536) 对 SQL Server 安装来说可能不够高。 出于此原因，请在 SQL Server 部署中，将 vm.max_map_count 值更改为至少 262144，并参阅[使用优化的 mssql 配置文件的建议 Linux 设置](#proposed-linux-settings-using-a-tuned-mssql-profile)部分，了解如何进一步优化这些内核参数。 vm.max_map_count 的最大值为 2147483647。

```bash
sysctl -w vm.max_map_count=1600000
```

使用 **mssql** **_Tuned_ *_ 配置文件来配置 _* vm.max_map_count** 选项。

#### <a name="leave-transparent-huge-pages-thp-enabled"></a>启用透明大页 (THP)

大多数 Linux 安装应在默认情况下启用此选项。 建议将此配置选项设置为启用，以获得最一致的性能体验。 但是，如果在具有多个实例的 SQL Server 部署中发生大量内存分页活动，或者在服务器上与其他内存需求较高的应用程序一起执行 SQL Server 的情况下，建议在执行以下命令后测试应用程序的性能：

```bash
echo madvise > /sys/kernel/mm/transparent_hugepage/enabled
```

或使用以下行修改 **mssql** **_Tuned_* _ 配置文件：

```bash
vm.transparent_hugepages=madvise
```

并在修改后使 _ *mssql** 配置文件处于活动状态：

```bash
tuned-adm off
tuned-adm profile mssql
```

使用 **mssql** **_Tuned_ *_ 配置文件来配置 _* transparent_hugepage** 选项。

#### <a name="additional-advanced-kernelos-configuration"></a>其他高级内核/OS 配置

1. 为获得最佳存储 IO 性能，建议使用适用于块设备的 Linux multiqueue 计划。 这使得块层性能可以很好地随高速固态硬盘 (SSD) 和多核系统一起缩放。 如果你的 Linux 发行版中默认启用了文档，请查看该文档。 在其他大多数情况下，使用 scsi_mod.use_blk_mq=y 启动内核将会启用此功能，即使正在使用的 Linux 分发版的文档对其提供了其他指导。 这与上游 Linux 内核一致。

1. 因为 SQL Server 部署中常使用多路径 IO，所以还应启用 dm_mod 内核启动选项，将设备映射器 (DM) 多路径目标配置为使用 `blk-mq` 基础结构。 默认值为 `n`（已禁用）。 当底层 SCSI 设备使用 `blk-mq` 时，此设置会在 DM 层降低锁定开销。 请参阅使用中 Linux 发行版的文档，以获取有关如何配置它的其他指导。

#### <a name="configure-swapfile"></a>配置交换文件

请确保已正确配置交换文件，以免出现内存不足的问题。 有关如何创建交换文件并正确调整其大小的详细说明，请参阅 Linux 文档。

#### <a name="virtual-machines-and-dynamic-memory"></a>虚拟机和动态内存

如果在虚拟机中运行 Linux 上的 SQL Server，请确保选择选项来修复为虚拟机预留的内存量。 请勿使用 Hyper-V 动态内存等功能。

## <a name="sql-server-configuration"></a>SQL Server 配置

建议在安装 Linux 上的 SQL Server 后执行以下配置任务，以实现应用程序的最佳性能。

### <a name="best-practices"></a>最佳做法

- **对节点和/或 Cpu 使用 PROCESS AFFINITY**

   对于所有用于 Linux OS 上的 SQL Server 的 NUMANODE 和/或 CPU（通常是所有节点和 CPU），建议使用 `ALTER SERVER CONFIGURATION` 设置 `PROCESS AFFINITY`。 处理器关联有助于保持高效的 Linux 和 SQL 计划行为。 使用 NUMANODE  选项是最简单的方法。 即使你的计算机上只有一个 NUMA 节点，也请使用 PROCESS AFFINITY。 有关如何设置 PROCESS AFFINITY 的详细信息，请参阅[更改服务器配置](../t-sql/statements/alter-server-configuration-transact-sql.md)文档。

- **配置多个 tempdb 数据文件**

   由于 Linux 上的 SQL Server 安装不提供配置多个 tempdb 文件的选项，因此建议在安装后考虑创建多个 tempdb 数据文件。 有关详细信息，请参阅[建议以减少 SQL Server tempdb 数据库中的分配争用](https://support.microsoft.com/help/2154845/recommendations-to-reduce-allocation-contention-in-sql-server-tempdb-d)一文。

### <a name="advanced-configuration"></a>高级配置

以下建议是可选的配置设置，你可以选择在安装 Linux 上的 SQL Server 之后执行这些设置。 这些选项取决于你的工作负载和 Linux OS 配置的要求。

- **使用 mssql-conf 设置内存限制**

   为了确保 Linux OS 有足够的可用物理内存，默认情况下 SQL Server 进程只使用 80% 的物理 RAM。 对于某些系统，如果物理 RAM 很大，20% 可能是一个很大的数字。 例如，在具有 1 TB RAM 的系统上，默认设置将保留 200 GB RAM 不使用。 在这种情况下，你可能希望将内存限制配置为较高的值。 请参阅有关 **mssql-conf** 工具的文档和 [memory.memorylimitmb](sql-server-linux-configure-mssql-conf.md#memorylimit) 设置，该设置可控制对 SQL Server 可见的内存（以 MB 为单位）。

   更改此设置时，请注意不要将此值设置得太高。 如果不留出足够的内存，则可能会遇到 Linux OS 和其他 Linux 应用程序的问题。

## <a name="next-steps"></a>后续步骤

若要详细了解提高性能的 SQL Server 功能，请参阅[性能功能入门](sql-server-linux-performance-get-started.md)。

有关 Linux 上的 SQL Server 的详细信息，请参阅 [Linux 上的 SQL Server 概述](sql-server-linux-overview.md)。
