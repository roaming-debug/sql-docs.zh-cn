---
title: 机器学习服务 (Python、R)
titleSuffix: SQL Server Big Data Clusters
description: 了解如何通过机器学习服务在 SQL Server 大数据群集的主实例上运行 Python 和 R 脚本。
author: dphansen
ms.author: davidph
ms.date: 11/19/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: aa71450a1c16c9239a0dc74403a1989b5a9a1986
ms.sourcegitcommit: ce15cbbcb0d5f820f328262ff5451818e508b480
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94947962"
---
# <a name="run-python-and-r-scripts-with-machine-learning-services-on-sql-server-big-data-clusters"></a>通过机器学习服务在 SQL Server 大数据群集上运行 Python 和 R 脚本

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

可以通过[机器学习服务](../machine-learning/index.yml)在 [SQL Server 大数据群集](big-data-cluster-overview.md)的主实例上运行 Python 和 R 脚本。

> [!NOTE]
> 还可以通过 [Java 语言扩展](../language-extensions/java-overview.md)在 SQL Server 大数据群集的主实例上运行 Java 代码。 按照以下步骤操作还将启用 [SQL Server 语言扩展](../language-extensions/language-extensions-overview.md)。

## <a name="enable-machine-learning-services"></a>启用机器学习服务

机器学习服务默认安装在大数据群集上，并且不需要单独安装。

若要启用机器学习服务，请在主实例上运行以下语句：

```sql
EXEC sp_configure 'external scripts enabled', 1
RECONFIGURE WITH OVERRIDE
GO
```

现在可以在大数据群集的主实例上运行 Python 和 R 脚本。 请参阅[后续步骤](#next-steps)下的快速入门，运行第一个脚本。

>[!NOTE]
>无法在可用性组侦听程序连接上设置配置设置。 如果大数据群集部署了高可用性，则对每个副本设置 `external scripts enabled`。 请参阅[在具有高可用性的群集上启用](#enable-on-cluster-with-high-availability)。

## <a name="enable-on-cluster-with-high-availability"></a>具有高可用性的群集上启用

当[部署高可用性 SQL Server 大数据群集](deployment-high-availability.md)时，部署会为主实例创建可用性组。 若要启用机器学习服务，请在可用性组的每个实例上设置 `external scripts enabled`。 对于大数据群集，需要在 SQL Server 主实例的每个副本上运行 `sp_configure`

以下部分介绍如何在每个实例上启用外部脚本。

### <a name="create-an-external-load-balancer-for-each-instance"></a>为每个实例创建外部负载均衡器

对于可用性组上的每个副本，创建负载均衡器以便可以连接到实例。 

`kubectl expose pod <pod-name> --port=<connection port number> --name=<load-balancer-name> --type=LoadBalancer -n <kubernetes namespace>`

本文中的示例使用以下值：

- `<pod-name>`: `master-#`
- `<connection port number>`: `1533`
- `<load-balancer-name>`: `mymaster-#`
- `<kubernetes namespace>`: `mssql-cluster`

为环境更新以下脚本，并运行命令：

```bash
kubectl expose pod master-0 --port=1533 --name=mymaster-0 --type=LoadBalancer -n mssql-cluster 
kubectl expose pod master-1 --port=1533 --name=mymaster-1 --type=LoadBalancer -n mssql-cluster
kubectl expose pod master-2 --port=1533 --name=mymaster-2 --type=LoadBalancer -n mssql-cluster 
```

`kubectl` 会返回以下输出。

```bash
service/mymaster-0 exposed
service/mymaster-1 exposed
service/mymaster-2 exposed
```

每个负载均衡器都是主副本终结点。

### <a name="enable-script-execution-on-each-replica"></a>对每个副本启用脚本执行

1. 获取主副本终结点的 IP 地址。

   以下命令返回副本终结点的外部 IP 地址。 

   `kubectl get services <load-balancer-name> -n <kubernetes namespace>`

   若要在此方案中获取每个副本的外部 IP 地址，请运行以下命令：

   ```bash
   kubectl get services mymaster-0 -n mssql-cluster
   kubectl get services mymaster-1 -n mssql-cluster
   kubectl get services mymaster-2 -n mssql-cluster
   ```

   >[!NOTE]
   > 可能需要一些时间才能获得外部 IP 地址。 定期运行上述脚本，直到每个终结点都返回外部 IP 地址。

1. 连接到主副本终结点并启用脚本执行。

    运行以下语句：

    ```sql
    EXEC sp_configure 'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    GO
    ```

   例如，可以使用 `sqlcmd` 运行上述命令。 下面的示例连接到主副本终结点并启用脚本执行。 将脚本中的值更新为适用于你环境的值。

   ```bash
   sqlcmd -S <IP address>,1533 -U <user name> -P <password> -Q "EXEC sp_configure 'external scripts enabled', 1; RECONFIGURE WITH OVERRIDE;"
   ```

   为每个副本重复该步骤。

### <a name="demonstration"></a>演示

下图演示了此过程。

[![演示](media/machine-learning-services/example-kube-enable-scripts.png "演示 Kubernetes 上的启用功能")](media/machine-learning-services/example-kube-enable-scripts.png#lightbox)

现在可以在大数据群集的主实例上运行 Python 和 R 脚本。 请参阅[后续步骤](#next-steps)下的快速入门，运行第一个脚本。

### <a name="delete-the-master-replica-endpoints"></a>删除主副本终结点

在 Kubernetes 群集上，删除每个副本的终结点。 终结点在 Kubernetes 中作为负载均衡服务公开。

以下命令删除负载均衡服务。

`kubectl delete svc <load-balancer-name> -n mssql-cluster`

对于本文中的示例，请运行以下命令。

```bash
kubectl delete svc mymaster-0 -n mssql-cluster
kubectl delete svc mymaster-1 -n mssql-cluster
kubectl delete svc mymaster-2 -n mssql-cluster
```

## <a name="next-steps"></a>后续步骤

+ [运行简单的 Python 脚本](../machine-learning/tutorials/quickstart-python-create-script.md?toc=/sql/toc.json)
+ [在 Python 中定型预测模型并对其进行评分](../machine-learning/tutorials/quickstart-python-train-score-model.md?toc=/sql/toc.json)
+ [运行简单的 R 脚本](../machine-learning/tutorials/quickstart-r-create-script.md?toc=/sql/toc.json)
+ [在 R 中定型预测模型并对其进行评分](../machine-learning/tutorials/quickstart-r-train-score-model.md?toc=/sql/toc.json)
