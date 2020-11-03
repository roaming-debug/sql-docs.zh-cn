---
title: 什么是应用程序部署？
titleSuffix: SQL Server Big Data Clusters
description: 了解应用程序部署如何提供用于在 SQL Server 2019 大数据群集上创建、管理和运行应用程序的接口。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ac26973c4d1ff8b2a9e689f3aa372d3888f939d6
ms.sourcegitcommit: ab9ddcc16fdfc245cf9a49d1e90bb1ffe3958c38
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2020
ms.locfileid: "92914291"
---
# <a name="what-is-application-deployment-on-a-sql-server-big-data-cluster"></a>SQL Server 大数据群集上的应用程序部署是什么？

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

应用程序部署通过提供用于创建、管理和运行应用程序的界面，允许在 SQL Server 大数据群集上部署应用程序。 部署在 SQL Server 大数据群集上的应用程序可以受益于群集的计算能力，并且可以访问群集上可用的数据。 这会提高应用程序的可伸缩性和性能，同时管理数据所在的应用程序。 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 上支持的应用程序运行时包括 R、Python、SSIS、MLeap。

以下部分介绍了应用程序部署的体系结构和功能。

## <a name="application-deployment-architecture"></a>应用程序部署体系结构

应用程序部署由控制器和应用运行时处理程序组成。 创建应用程序时，会提供规范文件 (`spec.yaml`)。 此 `spec.yaml` 文件包含控制器成功部署应用程序所需知道的一切。 下面是 `spec.yaml` 的内容示例：

```yaml
#spec.yaml
name: add-app #name of your python script
version: v1  #version of the app
runtime: Python #the language this app uses (R or Python)
src: ./add.py #full path to the location of the app
entrypoint: add #the function that will be called upon execution
replicas: 1  #number of replicas needed
poolsize: 1  #the pool size that you need your app to scale
inputs:  #input parameters that the app expects and the type
  x: int
  y: int
output: #output parameter the app expects and the type
  result: int
```

控制器会检查 `spec.yaml` 文件中指定的 `runtime`，并调用相应的运行时处理程序。 运行时处理程序会创建应用程序。 首先，会创建包含一个或多个 pod 的 Kubernetes ReplicaSet，其中每个 pod 都包含要部署的应用程序。 pod 的数量由应用程序的 `spec.yaml` 文件中设置的 `replicas` 参数定义。 每个 pod 可以有一个或多个池。 池的数量由 `spec.yaml` 文件中设置的 `poolsize` 参数定义。

这些设置会影响部署可以并行处理的请求数量。 一个给定时间的最大请求数等于 `replicas` 乘以 `poolsize`。 如果有 5 个副本，每个副本有 2 个池，则部署可以并行处理 10 个请求。 有关 `replicas` 和 `poolsize` 的图形表示方式，请参阅下图：

![池大小和副本](media/big-data-cluster-create-apps/poolsize-vs-replicas.png)

如果在 `spec.yaml` 文件中设置了 `schedule`，那么，在创建 ReplicaSet 并启动 pod 后，会创建一个 cron 作业。 最后，会创建一个可用于管理和运行应用程序的 Kubernetes 服务（请参阅下文）。

执行应用程序时，应用程序的 Kubernetes 服务会将请求代理给副本并返回结果。

## <a name="security-considerations-for-applications-deployments-on-openshift"></a><a id="app-deploy-security"></a> OpenShift 上的应用程序部署的安全注意事项

SQL Server 2019 CU5 支持在 Red Hat OpenShift 上部署大数据群集，并且支持 BDC 的更新安全模型，因此不再需要特权容器。 对于使用 SQL Server 2019 CU5 的所有新部署，容器除了是非特权容器之外，还默认以非根用户身份运行。

在 CU5 版本中，使用[应用部署]()接口部署的应用程序的安装步骤仍将以根用户身份运行。 这是必需的，因为在安装期间会安装应用程序将使用的其他包。 作为应用程序的一部分部署的其他用户代码将以低特权用户身份运行。 

此外，CAP_AUDIT_WRITE 功能是允许使用 cron 作业计划 SSIS 应用程序所必需的一项可选功能。 当应用程序的 yaml 规范文件指定计划时，应用程序将通过 cron 作业触发，而这需要其他功能。  或者，可以通过 Web 服务调用使用 azdata app run 按需触发应用程序，而这不需要 CAP_AUDIT_WRITE 功能。 

> [!NOTE]
> [OpenShift 部署项目](deploy-openshift.md)中的自定义 SCC 不包括此功能，因为大数据群集的默认部署不需要此功能。 若要启用此功能，必须首先更新自定义 SCC yaml 文件，以将 CAP_AUDIT_WRITE 包含在 

```yml
...
allowedCapabilities:
- SETUID
- SETGID
- CHOWN
- SYS_PTRACE
- AUDIT_WRITE
...
```

## <a name="how-to-work-with-application-deployment"></a>如何使用应用程序部署

应用程序部署的两个主要接口如下： 
- [命令行接口 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](app-create.md)
- [Visual Studio Code 和 Azure Data Studio 扩展](app-deployment-extension.md)

还可以使用 RESTful Web 服务执行应用程序。 有关详细信息，请参阅[在大数据群集上使用应用程序](app-consume.md)。

## <a name="next-steps"></a>后续步骤

若要了解有关如何在 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 上创建和运行应用程序的详细信息，请参阅以下内容：

- [使用 azdate 部署应用程序](app-create.md)
- [使用应用部署扩展部署应用程序](app-deployment-extension.md)
- [在大数据群集上使用应用程序](app-consume.md)

若要了解有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅以下概述：

- [什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)