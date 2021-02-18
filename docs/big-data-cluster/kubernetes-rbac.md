---
title: Kubernetes RBAC
titleSuffix: SQL Server big data clusters
description: 本文介绍 SQL Server 大数据群集如何将 RBAC 与 Kubernetes 配合使用。
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: f8759714fe2846a86f53a4974960b511f4b4c3dd
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343888"
---
# <a name="kubernetes-rbac-model--impact-on-users-and-service-accounts-managing-bdc"></a>Kubernetes RBAC 模型及对用户和服务帐户管理 BDC 的影响

本文介绍了有关用户管理大数据群集的权限要求，以及关于从大数据群集中访问默认服务帐户和 Kubernetes 的语义。

> [!NOTE]
> 有关 Kubernetes RBAC 模型的其他资源，请参阅[使用 RBAC 授权 - Kubernetes](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) 及[使用 RBAC 定义和应用权限 - OpenShift](https://docs.openshift.com/container-platform/4.4/authentication/using-rbac.html)。

## <a name="role-required-for-deployment"></a>部署所需的角色

BDC 使用服务帐户（例如 `sa-mssql-controller` 或 `master`）来协调群集 Pod、服务、高可用性、监视等的预配。当 BDC 部署启动（例如 `azdata bdc create`）时，[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 执行以下操作：

1. 检查提供的命名空间是否存在。
2. 如果不存在，将创建一个，并应用 `MSSQL_CLUSTER` 标签。
3. 创建 `sa-mssql-controller` 服务帐户。
4. 创建对命名空间或项目具有完全权限但不是群集级别权限的 `<namespaced>-admin` 角色。
5. 为角色创建该服务帐户的角色分配。

完成这些步骤后，预配控制平面 Pod，并且服务帐户将部署大数据群集的其余部分。  

因此，进行部署的用户必须具有执行以下操作的权限：

- 列出群集中的命名空间 (1)。
- 用标签修补新的或现有的命名空间 (2)。
- 创建服务帐户 `sa-mssql-controller`、`<namespaced>-admin` 角色和角色绑定 (3-5)。

默认 `admin` 角色不具有这些权限，因此部署大数据群集的用户必须至少具有命名空间级别的管理员权限。

## <a name="cluster-role-required-for-pods-and-nodes-metrics-collection"></a>收集 Pod 和节点指标所需的群集角色

从 SQL Server 2019 CU5 开始，Telegraf 需要具有群集级别角色权限的服务帐户来收集 Pod 和节点指标。 部署（或者升级现有部署）期间，尝试创建必要的服务帐户和群集角色，但如果部署群集或执行升级的用户没有足够的权限，部署或升级将在出现警告的情况下继续进行并获得成功。 这种情况下，不会收集 Pod 和节点指标。 部署群集的用户必须要求群集管理员创建角色和服务帐户（部署或升级之前或之后）。 创建后，BDC 将使用它们。 

以下步骤说明如何创建所需项目：

1. 使用以下内容创建 metrics-role.yaml 文件。 务必要将占位符 <clusterName> 替换为大数据群集的名称。

   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRole
   metadata:
     name: <clusterName>:cr-mssql-metricsdc-reader
   rules:
   - apiGroups:
     - '*'
     resources:
     - pods
     - nodes/stats
     - nodes/proxy
     verbs:
     - get
   ---
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: <clusterName>:crb-mssql-metricsdc-reader
   roleRef:
     apiGroup: rbac.authorization.k8s.io
     kind: ClusterRole
     name: <clusterName>:cr-mssql-metricsdc-reader
   subjects:
   - kind: ServiceAccount
     name: sa-mssql-metricsdc-reader
     namespace: <clusterName>
   ```

2. 创建群集角色和群集角色绑定：

   ```bash
   kubectl create -f metrics-role.yaml
   ```

服务帐户、群集角色和群集角色绑定可以在 BDC 部署之前或之后创建。 Kubernetes 自动更新 Telegraf 服务帐户的权限。 如果这些帐户和角色创建为 Pod 部署，将在收集的 Pod 和节点指标中看到几分钟的延迟。

> [!NOTE]
> SQL Server 2019 CU5 引入了两个功能开关来控制 Pod 和节点指标的收集。 默认情况下，这些参数在所有环境目标中都设为 true，除了重写默认值的 OpenShift。 

可以在 `control.json` 部署配置文件的安全性部分中自定义这些设置：

```json
  "security": {
    …
    "allowNodeMetricsCollection": false,
    "allowPodMetricsCollection": false
  }
```

如果这些设置设为 `false`，则 BDC 部署工作流不会尝试为 Telegraf 创建服务帐户、群集角色和绑定。

## <a name="default-service-account-usage-from-within-a-bdc-pod"></a>BDC Pod 中的默认服务帐户使用情况

为了获得更严格的安全模型，SQL Server 2019 CU5 禁用了 BDC Pod 中默认 Kubernetes 服务帐户的默认凭据装入。 这适用于 CU5 或更高版本中的新部署和升级的部署。
Pod 中的凭据令牌可用于访问 Kubernetes API 服务器，权限级别取决于 Kubernetes 授权策略设置。 如果有需要恢复到以前的 CU5 行为的特定用例，我们将在 CU6 中引入新的功能切换，以便仅在部署时启用自动装入。 为此，可以使用 control.json 配置部署文件，并将 automountServiceAccountToken 设置为 true。 运行此命令，以使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 在 control.json 自定义配置文件中更新此设置： 

``` bash
azdata bdc config replace -c custom-bdc/control.json -j "$.security.automountServiceAccountToken=true"
```
