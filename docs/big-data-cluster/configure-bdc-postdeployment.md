---
title: SQL Server 大数据群集后期部署配置概述
titleSuffix: SQL Server big data clusters
description: 大数据群集后期部署配置概述
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 478ecc9888bbd3c8f51ee96c6c796856472f93d5
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343916"
---
# <a name="how-to-configure-bdc-settings-post-deployment"></a>如何配置 BDC 设置后期部署

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

> [!NOTE]
> 后期部署设置配置仅适用于 BDC CU9 及更高版本的部署。 设置配置不包括缩放、存储或终结点配置。 可在[此处](configure-bdc-pre-configuration.md)找到有关使用 CU9 之前的版本配置 BDC 的选项和说明。

大数据群集的群集、服务和资源范围设置可在部署后使用 azdata CLI 进行配置。 此功能使 BDC 管理员可以调整配置，让其始终满足工作负载要求。 本文介绍了如何配置 BDC 以满足 Spark 工作负载要求的示例步骤。 后期部署配置功能遵循集、差异和应用流。

## <a name="step-by-step-configure-bdc-to-meet-your-spark-workload-requirements"></a>分步指南：配置 BDC 以满足 Spark 工作负载要求

### <a name="view-the-current-configurations-of-the-big-data-cluster-spark-service"></a>查看大数据群集 Spark 服务的当前配置
下面的示例演示如何查看 Spark 服务的用户配置的设置。 可以通过可选参数查看所有可能的可配置设置、系统管理的和所有可配置的设置，以及挂起的设置。 有关详细信息，请参阅 [`azdata bdc spark` 语句](../azdata/reference/reference-azdata-bdc-spark-statement.md)。

```bash
azdata bdc spark settings show
```
#### <a name="sample-output"></a>示例输出
Spark 服务 

|设置|运行值|
| --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1` |
|`spark-defaults-conf.spark.driver.memory`|`1664m` |

### <a name="change-the-default-number-of-cores-and-memory-for-the-spark-driver-across-all-resources-with-spark-ie-for-the-spark-service"></a>在 Spark 的所有资源中，更改 Spark 驱动程序（即 Spark 服务）的默认核心数和内存
将 Spark 服务的默认核心数更新为 2，将默认内存更新为 7424m。

```bash
azdata bdc spark settings set spark-defaults-conf.spark.driver.cores=2, spark-defaults-conf.spark.driver.memory=7424m
```

### <a name="change-the-default-number-of-cores-and-memory-for-the-spark-executors-in-the-storage-pool"></a>更改存储池中 Spark 执行程序的默认核心数和内存
将存储池的执行程序默认核心数更新为 4。

```bash
azdata bdc spark settings set spark-defaults-conf.spark.executor.cores=4 --resource=storage-0
```

### <a name="view-the-pending-settings-changes-staged-in-the-bdc"></a>查看 BDC 中暂存的挂起设置更改
仅查看针对 Spark 服务和整个 BDC 群集中挂起的设置更改。

#### <a name="pending-spark-service-settings"></a>挂起的 Spark 服务设置
```bash
azdata bdc spark settings show --filter-option=pending --include-details
```

### <a name="spark-service"></a>Spark 服务

|设置|运行值|配置的值|可配置|已配置 |上次更新时间|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1`| `2` | `true` | `true` |
|`spark-defaults-conf.spark.driver.memory`|`1664m`| `7424m` | `true` | `true` |

#### <a name="all-pending-settings"></a>所有挂起的设置
```bash
azdata bdc settings show --filter-option=pending --include-details --recursive
```

Spark 服务设置 - 挂起

|设置|运行值|配置的值|可配置|已配置|上次更新时间|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1`| `2` | `true` | `true` |
|`spark-defaults-conf.spark.driver.memory`|`1664m`| `7424m` | `true` | `true` |

Storage-0 资源 Spark 设置 - 挂起

|设置|运行值|配置的值|可配置|已配置|上次更新时间|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.executor.cores`|`1`| `4` | `true` | `true` |

### <a name="apply-the-pending-settings-to-the-bdc"></a>将挂起的设置应用于 BDC

```bash
azdata bdc settings apply
```

### <a name="monitor-the-status-of-the-bdc-configuration-update"></a>监视 BDC 配置更新的状态

```bash
azdata bdc status show -all
```

## <a name="optional-steps"></a>可选步骤

### <a name="revert-pending-configuration-settings"></a>还原挂起的配置设置

如果确定不再需要更改挂起的配置设置，则可以取消暂存这些设置。 这将还原所有范围内挂起的设置。

```bash
azdata bdc settings revert
```

### <a name="abort-the-configuration-upgrade"></a>中止配置升级

如果任何组件的配置升级失败，则可以取消升级过程，并将 BDC 返回其之前的配置。 升级期间暂存的更改设置将再次作为挂起的设置列出。

```bash
azdata bdc settings cancel-apply
```

## <a name="next-steps"></a>后续步骤

[配置 SQL Server 大数据群集](configure-bdc-overview.md)