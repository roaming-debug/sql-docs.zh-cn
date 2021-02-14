---
title: 配置 SQL Server 主实例
titleSuffix: Configure SQL Server master instance of Big Data Cluster
description: 配置 SQL Server 大数据群集的主实例
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 11/04/2019
ms.topic: overview
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: de9bba220f985522926d610b36418d9607c8a568
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039050"
---
# <a name="configure-master-instance-of-big-data-clusters-2019"></a>配置 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的主实例

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

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

## <a name="known-limitations"></a>已知的限制

- 以上步骤需要 Kubernetes 群集管理员权限
- 部署后，无法更改大数据群集的 SQL Server 主实例的服务器排序规则。

## <a name="next-steps"></a>后续步骤

有关部署 SQL Server 大数据群集的详细信息，请参阅 [SQL Server 大数据群集入门](deploy-get-started.md)。