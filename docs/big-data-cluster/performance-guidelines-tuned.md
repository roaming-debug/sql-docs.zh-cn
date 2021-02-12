---
title: 关于 SQL Server 大数据群集性能的最佳做法
description: 本文提供最佳做法和指南来确保运行 Kubernetes 上的 SQL Server 大数据群集时性能最佳
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7b430e09a06523bc3ee369434911738f83ab5ee4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044124"
---
# <a name="performance-best-practices-and-configuration-guidelines-for-sql-server-big-data-clusters"></a>关于 SQL Server 大数据群集的性能最佳做法和配置指南

[!INCLUDE [sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

本文提供最佳做法和建议，以最大程度地提高面向大数据群集中运行的服务的应用程序的性能。

以下指南重点介绍关于配置托管 Kubernetes 工作器节点（BDC 将部署在该工作节点上）的 Linux 操作系统的建议。 最佳做法是在部署大数据群集之前配置优化配置文件。 在 Microsoft 和 Intel 开展的案例研究期间，对建议的优化配置文件中包含的设置进行了验证。 本[白皮书](https://aka.ms/sql-bdc-spark-perf/)中发布了研究结果以供下载。

> [!TIP]
> 有关 Linux 上的 SQL Server 特定的优化配置，请参阅 [关于 Linux 上的 SQL Server 的性能最佳做法和配置指南](../linux/sql-server-linux-performance-best-practices.md)。 另外，其他一些最佳做法仍然适用，例如 SQL Server 数据库的索引设计。

## <a name="proposed-linux-settings-using-a-tuned-mssql-bdc-profile"></a>建议的 Linux 设置，它们使用优化的 `mssql-bdc` 配置文件

使用以下内容创建 tuned.conf 配置文件。

```bash
[main]
summary=Optimize for Microsoft SQL Server Big Data Clusters TPC-DS performance
include=throughput-performance
 
[sysctl]
#network tunings
net.ipv4.conf.default.rp_filter=1
net.ipv4.tcp_timestamps=0
net.ipv4.tcp_sack = 1
net.core.netdev_max_backlog = 25000
net.core.rmem_max = 2147483647
net.core.wmem_max = 2147483647
net.core.rmem_default = 33554431
net.core.wmem_default = 33554432
net.core.optmem_max = 33554432
net.ipv4.tcp_rmem =8192 33554432 2147483647
net.ipv4.tcp_wmem =8192 33554432 2147483647
net.ipv4.tcp_low_latency=1
net.ipv4.tcp_adv_win_scale=1
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv4.conf.all.arp_filter=1
net.ipv4.tcp_retries2=5
net.ipv6.conf.lo.disable_ipv6 = 1
net.core.somaxconn = 65535
 
#memory cache settings
vm.swappiness=1
vm.overcommit_memory=0
vm.dirty_background_ratio=1
 
#kernel NUMA
kernel.numa_balancing=0

#filesystem
fs.aio-max-nr=1048576
 
[vm]
#should be revisited for SQL large pages use in master/data/compute pods
transparent_hugepages=never
```

## <a name="install-tuned-utility-on-all-the-kubernetes-worker-nodes"></a>在所有 Kubernetes 工作器节点上安装优化的实用工具

若要安装优化项，请执行：

```bash
apt-get -y install tuned
```

## <a name="apply-tuning-settings-to-all-kubernetes-worker-nodes"></a>将优化设置应用到所有 Kubernetes 工作器节点

在每个目标工作器节点上，复制前面创建的 tuned.conf 文件：

```bash
cd /usr/lib/tuned
scp -r <sourcePath> ./mssql-bdc
```

若要启用此 mssql-bdc 优化配置文件，请将这些定义保存在所有 Kubernetes 工作器节点上的 `/usr/lib/tuned/mssql-bdc` 文件夹下的 tuned.conf 文件中，并使用以下命令启用配置文件 ：

```bash
chmod +x /usr/lib/tuned/mssql-bdc/tuned.conf
tuned-adm profile mssql-bdc
```

使用此命令验证是否已启用：

```bash
tuned-adm active
```

或

```bash
tuned-adm list
```

如果动态更改配置文件，则必须在所有受影响的工作器节点上重新启动优化，才能使新更改生效：

```bash
systemctl restart tuned
```
 
可在 /var/log/tuned/tuned.log 中找到优化服务的日志。

（可选）可在 Kubernetes 群集中的一个节点上配置优化配置文件，然后使用以下脚本在剩余节点上复制并配置该配置文件。

```bash
#!/bin/bash -e
# This script takes a list of servers (IPs like `cat ~administrator/workerhosts)) as input
# and update these servers with the local version of mssql-bdc tuned.conf.
 
is_root() {
    local is_root_set=0
    if [ "$EUID" -ne 0 ]; then
        echo "Please run as root"
    else
        is_root_set=1
    fi
    return "${is_root_set}"
}
 
# function main
if is_root -eq 0; then
    exit 0
fi
 
while [ $# -gt 0 ]
do
    # sometimes, people add non-breaking space characters to their *host* files.
    WORKER_IP=$(echo "$1" | sed -e 's/\xC2\xA0//g')
    echo -e "updating mssql-bdc tuned on \e[42m${WORKER_IP}\e[0m"
    # the following commands assume tuned was not set up and active...
    # TODO: add the tuned install and status checks
    ssh "${WORKER_IP}" mkdir -p /usr/lib/tuned/mssql-bdc
    scp ~administrator/tuned.conf "${WORKER_IP}":/usr/lib/tuned/mssql-bdc/tuned.conf
    ssh "${WORKER_IP}" tuned-adm profile mssql-bdc
    ssh "${WORKER_IP}" systemctl restart tuned
    ssh "${WORKER_IP}" tuned-adm active
    shift
done

```

## <a name="next-steps"></a>后续步骤

有关更多资源（包括 SQL Server 大数据群集的参考体系结构），请参阅：

* [案例研究：在 MS SQL Server 2019 大数据群集的 Apache Spark 上运行的 SQL 工作负载](https://aka.ms/sql-bdc-spark-perf/)

* [HPE 参考体系结构，通过 Microsoft SQL Server 2019 大数据群集跨所有数据提供见解](https://h20195.www2.hpe.com/V2/GetDocument.aspx?docname=a50001963enw)

* [Dell EMC PowerStore：Microsoft SQL Server 2019 大数据群集](https://www.dellemc.com/resources/en-us/asset/white-papers/products/storage/h18231-dell-emc-powerstore-sql-server-big-data-clusters.pdf)

* [Microsoft SQL Server 2019 大数据群集：使用 Dell EMC 基础结构的大数据解决方案](https://infohub.delltechnologies.com/t/microsoft-sql-server-2019-big-data-clusters-a-big-data-solution-using-dell-emc-infrastructure/)

* [Cisco UCS 参考体系结构上的 Microsoft SQL Server 2019 大数据群集](https://www.cisco.com/c/en/us/solutions/collateral/data-center-virtualization/unified-computing/sql-server-on-big-data-cluster-on-ucs.html)