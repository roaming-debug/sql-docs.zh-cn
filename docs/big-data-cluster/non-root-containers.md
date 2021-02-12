---
title: 非根大数据群集容器
titleSuffix: SQL Server big data clusters
description: 本文介绍如何在 SQL Server 大数据群集中部署非根容器
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 52a24c1dbe2a239450b440c07de1a7852f9c627a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046037"
---
# <a name="non-root-big-data-clusters-containers"></a>非根大数据群集容器

SQL Server 2019 CU5 引入了对非根容器的支持。 默认情况下，通过确保在所有受支持的平台上，以非根用户身份启动在 BDC 中运行的所有容器应用程序，平台实现会更加安全。 这些功能适用于使用 SQL Server 2019 CU5 对应映像标记的所有新部署。 此更改不会影响现有的 CU5 之前的 BDC 部署，这些群集中的应用程序将继续以根用户身份运行。 

## <a name="technical-background"></a>技术背景

请查看[此技术白皮书](https://aka.ms/sql-bdc-openshift-security)，其中包含有关使用非根用户身份进行部署的安全设计的详细信息，并重点介绍了大数据群集暂时提升权限的情况和原因。 白皮书的内容是与 SQL Server 和 Red Hat 的安全专家合作开发的，重点介绍 OpenShift 中的安全上下文和功能，但 BDC 安全概念和设计适用于所有受支持的平台。

> [!NOTE]
> 在 CU5 版本中，使用[应用部署](concept-application-deployment.md)接口部署的应用程序的安装步骤仍将以根用户身份运行。 这是必需的，因为在安装期间会安装应用程序将使用的其他包。 作为应用程序的一部分部署的其他用户代码将以低特权用户身份运行。 

> [!NOTE]
> 建议以默认的非根设置运行群集。 如果要还原回 CU5 之前的行为，并以 `root` 用户身份运行 BDC 内的容器，则可以使用新功能开关 `allowRunAsRoot` 关闭默认行为。 只能在部署时设置此设置。 若要设置此设置，请指定 `control.json` 部署配置文件中 `security` 部分下的设置：

```json
 "security": {
  …
    "allowRunAsRoot": true,
  …
}
```

> [!IMPORTANT]
> 不支持在 OpenShift 环境中更改此设置。

由于以非根用户身份运行 BDC 内的服务，因此用于通过网关终结点连接到服务的凭据不会使用 `root` 用户名。 如果用于连接到网关终结点的应用程序使用的凭据错误，你将看到身份验证错误。 网关终结点的新用户名基于通过 `AZDATA_USERNAME` 环境变量传递的值。 网关终结点的用户名与用于控制器和 SQL Server 终结点的用户名相同。 这只会影响新部署，使用任何之前版本部署的现有大数据群集将继续使用 `root`。 将群集部署为使用 Active Directory 身份验证时，不会对凭据产生任何影响。 

## <a name="use-the-latest-azure-data-studio"></a>使用最新的 Azure Data Studio

Azure Data Studio 将以透明方式处理用于通过网关建立连接的凭据的更改，以在对象资源管理器中启用 HDFS 浏览体验，或通过笔记本提交 Spark 作业。 安装[最新的 Azure Data Studio 预览体验内部版本](../azure-data-studio/download-azure-data-studio.md#download-insiders-build-of-azure-data-studio)。 此内部版本包括此用例的必要更改。

对于必须提供凭据以通过网关访问服务的其他情况（例如使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 登录、访问 Spark 的 Web 仪表板），请确保使用正确的凭据。 如果你的目标是在 CU5 之前部署的现有群集，你将继续使用 `root` 用户名连接到网关，即使是在将群集升级到 CU5 之后也是如此。 如果使用 CU5 版本部署新群集，请通过提供与 `AZDATA_USERNAME` 环境变量对应的用户名进行登录。

## <a name="configuration-file-switches"></a>配置文件开关

从 CU5 开始，添加了两个功能开关来控制 pod 和节点指标的收集。 如果你使用不同的解决方案来监视 Kubernetes 基础结构，则可以通过在 `control.json` 部署配置文件中将 `allowNodeMetricsCollection` 和 `allowPodMetricsCollection` 设置为 `false`，以关闭 pod 和主机节点的内置指标收集。 

例如： 

```json
"security": {
  ...
  "allowPodMetricsCollection": true,
  "allowNodeMetricsCollection": true,
  ...
}
```

> [!NOTE]
> 对于 OpenShift 环境，默认情况下，这些设置在内置部署配置文件中设置为 false。 收集 pod 和节点指标所需的特权功能和建议的 OpenShift 安全上下文是基于限制的约束。

除了非特权容器（从 CU5 开始，对于 BDC 的所有新部署），默认情况下，容器在所有受支持的平台上以非根用户身份运行。 这些功能适用于使用 SQL Server 2019 CU5 对应映像标记的所有新部署。 现有的 CU5 之前的 BDC 部署将不受影响，这些群集中的应用程序将继续以根用户身份运行。

## <a name="next-steps"></a>后续步骤
[如何在 Kubernetes 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)

[在 OpenShift 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deploy-openshift.md)

[安全白皮书](https://aka.ms/sql-bdc-openshift-security)
