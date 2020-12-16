---
title: 配置永久内存 (PMEM)
description: 了解如何为 Linux 上的 SQL Server 配置持久性内存 (PMEM)，以及如何为 PMEM 设备创建命名空间。
ms.custom: seo-lt-2019
author: briancarrig
ms.author: brcarrig
ms.date: 10/31/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
monikerRange: '>= sql-server-linux-ver15  || >= sql-server-ver15'
ms.openlocfilehash: 4630a96f1abf961174ece179aabfd160a5784ad9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471608"
---
# <a name="configure-persistent-memory-pmem-for-sql-server-on-linux"></a>为 Linux 上的 SQL Server 配置持久性内存 (PMEM)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍如何为 Linux 上的 [!INCLUDE[sqlv15](../includes/sssqlv15-md.md)] 配置永久内存 (PMEM)。

## <a name="overview"></a>概述

[!INCLUDE[sqlv15](../includes/sssqlv15-md.md)] 具有大量使用永久内存的内存中功能。 本文档介绍为 Linux 上的 SQL Server 配置永久内存所需的步骤。

> [!NOTE]
> 引入了“启发”一词，旨在传达使用永久内存感知文件系统的概念  。 通过使用内存映射 (`mmap()`)，可以从用户空间应用程序直接访问文件系统。 在创建文件的内存映射时，应用程序可能会完全绕过 I/O 堆栈发出加载/存储指令。 从主机扩展应用程序的角度来看，这是一种“启发式”文件访问方法，它是允许 SQLPAL 与 Windows 或 Linux OS 交互的黑盒代码。

## <a name="create-namespaces-for-pmem-devices"></a>为 PMEM 设备创建命名空间

### <a name="configure-the-devices"></a>配置设备

在 Linux 中，使用 `ndctl` 实用工具。

- 安装 `ndctl` 以配置 PMEM 设备。 可在[此处](https://docs.pmem.io/getting-started-guide/installing-ndctl)找到。
- 使用 `ndctl` 创建命名空间。 命名空间在 PMEM NVDIMM 之间交错，可以对设备上的内存区域提供不同类型的用户空间访问。 `fsdax` 是 SQL Server 的默认和所需模式。

```bash 
ndctl create-namespace -f -e namespace0.0 --mode=fsdax* --map=dev
```

请注意，我们已选择 `fsdax` 模式，并且使用系统内存来存储每页的元数据。 我们推荐使用 `--map=dev`。 这会将元数据直接存储在命名空间上。 目前，使用 `--map=mem` 将元数据存储在内存中被认为是一项试验性功能。

使用 `ndctl` 验证命名空间。 
  
示例输出如下：

```bash
# ndctl list -N
{
  "dev":"namespace0.0",
  "mode":"fsdax",
  "map":"dev",
  "size":4294967296,
  "sector_size":512,
  "blockdev":"pmem0",
  "numa_node":0
}
```

### <a name="create-and-mount-pmem-device"></a>创建和装载 PMEM 设备

例如，使用 XFS

```bash
mkfs.xfs -f /dev/pmem0
mount -o dax,noatime /dev/pmem0 /mnt/dax
xfs_io -c "extsize 2m" /mnt/dax
```

例如，使用 EXT4

```bash
mkfs.ext4 -b 4096 -E stride=512 -F /dev/pmem0
mount -o dax,noatime /dev/pmem0 /mnt/dax
```

## <a name="technical-considerations"></a>技术注意事项

- XFS/EXT4 的 2 MB 块分配，如上文所述
- 块分配和 `mmap` 之间的不一致会导致无提示回退到 4 KB
- 文件大小应为 2 MB 的倍数（取模 2 MB）
- 请勿禁用透明大页 (THP)（在大多数分发服务器上默认启用）

使用 `ndctl` 配置、创建和装载设备后，可以将数据库文件放在该设备中，也可以创建新的数据库。

由于 PMEM 设备是 O_DIRECT（直接 I/O）安全的，请考虑启用跟踪标志 3979 来禁用强制刷新机制。 有关详细信息，请参阅 [FUA 支持](https://support.microsoft.com/help/4131496/enable-forced-flush-mechanism-in-sql-server-2017-on-linux)。 [FUA 内部信息](/archive/blogs/bobsql/sql-server-on-linux-forced-unit-access-fua-internals)中介绍了强制单元访问的内部信息。

## <a name="next-steps"></a>后续步骤

有关 Linux 上 SQL Server 的详细信息，请参阅 [Linux 上的 SQL Server](sql-server-linux-overview.md)。
有关 Linux 上的 SQL Server 的性能最佳做法，请参阅[性能最佳做法](sql-server-linux-performance-best-practices.md)。