---
title: 加载示例数据
titleSuffix: SQL Server big data clusters
description: 本教程演示如何将示例数据加载到 SQL Server 大数据群集中。 示例数据包括 SQL Server 主实例中的关系数据。 它还包括存储池中的 HDFS 数据。 此数据支持本部分中的其他教程。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9b4e7667a3f109a0b5cca2f0af60af38613c5ce8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045641"
---
# <a name="tutorial-load-sample-data-into-a-sql-server-big-data-cluster"></a>教程：将示例数据加载到 SQL Server 大数据群集中

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本教程介绍如何使用脚本将示例数据加载到 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 中。 文档中的许多其他教程也使用此示例数据。

> [!TIP]
> 可在 [sql-server-samples](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster) GitHub 存储库中找到 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 的其他示例。 它们位于 **sql-server-samples/samples/features/sql-big-data-cluster/** 路径中。

## <a name="prerequisites"></a>先决条件

- [已部署的大数据群集](deployment-guidance.md)
- [大数据工具](deploy-big-data-tools.md)
   - **azdata**
   - **kubectl**
   - **sqlcmd**
   - **curl**
 
## <a name="load-sample-data"></a><a id="sampledata"></a> 加载示例数据

以下步骤使用启动脚本下载 SQL Server 数据库备份并将数据加载到大数据群集中。 为了便于使用，这些步骤已划分为 [Windows](#windows) 和 [Linux](#linux) 部分。 若要将基本用户名/密码用作身份验证机制，请先设置 AZDATA_USERNAME 和 AZDATA_PASSWORD 环境变量，再执行脚本。 否则，脚本会使用集成身份验证连接到 SQL Server 主实例和 Knox 网关。 此外，若要使用集成身份验证，还应为终结点指定 DNS 名称。

## <a name="windows"></a><a id="windows"></a> Windows

以下步骤介绍如何使用 Windows 客户端将示例数据加载到大数据群集中。

1. 打开新的 Windows 命令提示符。

   > [!IMPORTANT]
   > 不要使用 Windows PowerShell 执行这些步骤。 在 PowerShell 中，脚本将失败，因为它将使用 PowerShell 的 **curl** 版本。

1. 使用 **curl** 下载示例数据的启动脚本。

   ```cmd
   curl -o bootstrap-sample-db.cmd "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.cmd"
   ```

1. 下载 **bootstrap-sample-db.sql** Transact-SQL 脚本。 此脚本由启动脚本调用。

   ```cmd
   curl -o bootstrap-sample-db.sql "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.sql"
   ```

1. 启动脚本需要大数据群集的以下位置参数：

   | 参数 | 说明 |
   |---|---|
   | <CLUSTER_NAMESPACE> | 为大数据群集提供的名称。 |
   | <SQL_MASTER_ENDPOINT> | 主实例的 DNS 名称或 IP 地址。 |
   | <KNOX_ENDPOINT> | HDFS/Spark 网关的 DNS 名称或 IP 地址。 |
   
   > [!TIP]
   > 使用 [kubectl](cluster-troubleshooting-commands.md) 查找 SQL Server 主实例和 Knox 的 IP 地址。 运行 `kubectl get svc -n <your-big-data-cluster-name>` 并查看主实例 (**master-svc-external**) 和 Knox (**gateway-svc-external**) 的外部 IP 地址。 群集的默认名称为 **mssql-cluster**。

1. 运行启动脚本。

   ```cmd
   .\bootstrap-sample-db.cmd <CLUSTER_NAMESPACE> <SQL_MASTER_ENDPOINT> <KNOX_ENDPOINT>
   ```

## <a name="linux"></a><a id="linux"></a> Linux

以下步骤介绍如何使用 Linux 客户端将示例数据加载到大数据群集中。

1. 下载启动脚本并为其分配可执行权限。

   ```bash
   curl -o bootstrap-sample-db.sh "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.sh"
   chmod +x bootstrap-sample-db.sh
   ```

1. 下载 **bootstrap-sample-db.sql** Transact-SQL 脚本。 此脚本由启动脚本调用。

   ```bash
   curl -o bootstrap-sample-db.sql "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/bootstrap-sample-db.sql"
   ```

1. 启动脚本需要大数据群集的以下位置参数：

   | 参数 | 说明 |
   |---|---|
   | <CLUSTER_NAMESPACE> | 为大数据群集提供的名称。 |
   | <SQL_MASTER_ENDPOINT> | 主实例的 DNS 名称或 IP 地址。 |
   | <KNOX_ENDPOINT> | HDFS/Spark 网关的 DNS 名称或 IP 地址。 |

   > [!TIP]
   > 使用 [kubectl](cluster-troubleshooting-commands.md) 查找 SQL Server 主实例和 Knox 的 IP 地址。 运行 `kubectl get svc -n <your-big-data-cluster-name>` 并查看主实例 (**master-svc-external**) 和 Knox (**gateway-svc-external**) 的外部 IP 地址。 群集的默认名称为 **mssql-cluster**。

1. 运行启动脚本。

   ```bash
   ./bootstrap-sample-db.sh <CLUSTER_NAMESPACE> <SQL_MASTER_ENDPOINT> <KNOX_ENDPOINT>
   ```

## <a name="next-steps"></a>后续步骤

运行启动脚本后，大数据群集具有示例数据库和 HDFS 数据。 以下教程使用示例数据来演示大数据群集功能：

数据虚拟化：

- [教程：在 SQL Server 大数据群集中查询 HDFS](tutorial-query-hdfs-storage-pool.md)
- [教程：从 SQL Server 大数据群集中查询 Oracle](tutorial-query-oracle.md)

数据引入：

- [教程：使用 Transact-SQL 将数据引入 SQL Server 数据池](tutorial-data-pool-ingest-sql.md)
- [教程：使用 Spark 作业将数据引入 SQL Server 数据池](tutorial-data-pool-ingest-spark.md)

笔记本：

- [教程：在 SQL Server 2019 大数据群集上运行示例笔记本](notebooks-tutorial-spark.md)
