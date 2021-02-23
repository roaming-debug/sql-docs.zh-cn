---
title: SQL Server 大数据群集配置 CU9 之前的版本
titleSuffix: SQL Server big data clusters
description: 大数据群集配置 CU9 之前的版本
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b00ed57288d19f08555a00eec8c9e62edc0f8cf6
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343973"
---
# <a name="configure-a-sql-server-big-data-cluster---pre-cu9-release"></a>配置 SQL Server 大数据群集 - CU9 之前的版本

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

> [!NOTE]
> 在 CU9 版本之前以及支持启用配置的群集的之前，只能在部署时配置大数据群集，但 SQL Server 主实例除外，它只能在部署后使用 mssql-conf 进行配置。 可在[此处](configure-bdc-overview.md)找到有关配置 CU9 以及 BDC 后续版本的说明。


在 BDC 版本 CU8 及更早版本中，可在部署时通过部署 `bdc.json` 文件配置 BDC 设置。 只能使用 mssql-conf 在部署后配置 SQL Server 主实例。

## <a name="configuration-scopes"></a>配置范围
大数据群集配置 CU9 之前的版本具有两个范围级别：`service` 和 `resource`。 这些设置的层次结构也遵循此顺序，即从最高到最低。 BDC 组件将使用在最低范围定义的设置的值。 如果未在给定范围定义设置，则它将继承其更高的父范围中的值。

例如，最好定义 Spark 驱动程序将在存储池和 `Sparkhead` 资源中使用的默认核心数。 可通过两种方式实现此目的：

* 在 `Spark` 服务范围指定默认内核值 
* 在 `storage-0` 和 `sparkhead` 资源范围指定默认内核值

在第一种方案中，Spark 服务的所有较低范围的资源（存储池和 `Sparkhead`）都将从 Spark 服务默认值继承默认核心数。

在第二种方案中，每个资源将使用在其各自范围中定义的值。

如果在服务和资源范围同时配置了默认内核数，则资源范围的值将替代服务范围值，因为这是用户针对给定设置配置的最低范围。

有关配置的具体信息，请参阅相应文章：

## <a name="configure-the-sql-server-master-instance"></a>配置 SQL Server 主实例
配置 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的主实例。

部署时无法为 SQL Server 主实例配置服务器配置设置。 本文介绍了有关如何配置设置（例如 SQL Server 版本、启用或禁用 SQL Server 代理、启用特定跟踪标志或启用/禁用客户反馈）的临时解决方法。

若要更改任何设置，请执行以下步骤：

1. 创建包括目标设置的自定义 `mssql-custom.conf` 文件。 以下示例启用 SQL 代理、遥测，为 Enterprise Edition 设置 PID 并启用了跟踪标志 1204.：

   ```
   [sqlagent]
   enabled=true
   
   [telemetry]
   customerfeedback=true
   userRequestedLocalAuditDirectory = /tmp/audit

   [DEFAULT]
   pid = Enterprise

   [traceflag]
   traceflag0 = 1204
   ```

1. 将 `mssql-custom.conf` 文件复制到 `master-0` Pod 中的 `mssql-server` 容器中的 `/var/opt/mssql`。 将 `<namespaceName>` 替换为大数据群集名称。

   ```bash
   kubectl cp mssql-custom.conf master-0:/var/opt/mssql/mssql-custom.conf -c mssql-server -n <namespaceName>
   ```

1. 重启 SQL Server 实例。  将 `<namespaceName>` 替换为大数据群集名称。

   ```bash
   kubectl exec -it master-0  -c mssql-server -n <namespaceName> -- /bin/bash
   supervisorctl restart mssql-server
   exit
   ```

> [!IMPORTANT]
> 如果 SQL Server 主实例在可用性组配置中，请将 `mssql-custom.conf` 文件复制到所有 `master` Pod 中。 请注意，每次重启都会导致故障转移，因此必须确保在停机的时段内进行此活动。

### <a name="known-limitations"></a>已知的限制

- 以上步骤需要 Kubernetes 群集管理员权限
- 部署后，无法更改大数据群集的 SQL Server 主实例的服务器排序规则。

## <a name="configure-apache-spark-and-apache-hadoop"></a>配置 Apache Spark 和 Apache Hadoop
为了在大数据群集中配置 Apache Spark 和 Apache Hadoop，需要在部署时修改群集配置文件。

大数据群集具有四个配置类别： 

- `sql` 
- `hdfs` 
- `spark` 
- `gateway` 

`sql`、`hdfs`、`spark`、`sql` 均为服务。 上述每个服务都对应于名称相同的配置类别。 所有网关配置都将跳到类别 `gateway`。 

例如，服务 `hdfs` 中的所有配置都属于类别 `hdfs`。 请注意，所有 Hadoop（核心站点）、HDFS 和 Zookeeper 配置都属于 `hdfs` 类别；所有 Livy、Spark、Yarn、Hive 元存储配置都属于 `spark` 类别。 

[支持的配置](reference-config-spark-hadoop.md#supported-configurations)列出了 Apache Spark 和 Hadoop 属性，你可以在部署 SQL Server 大数据群集时配置这些属性。

以下部分列出了无法在群集中修改的属性：

- [不支持的 `spark` 配置](reference-config-spark-hadoop.md#unsupported-spark-configurations)
- [不支持的 `hdfs` 配置](reference-config-spark-hadoop.md#unsupported-hdfs-configurations)
- [不支持的 `gateway` 配置](reference-config-spark-hadoop.md#unsupported-gateway-configurations)

## <a name="next-steps"></a>后续步骤

[配置 SQL Server 大数据群集](configure-bdc-overview.md)