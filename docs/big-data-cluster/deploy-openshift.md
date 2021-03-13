---
title: 在 OpenShift 上部署
titleSuffix: SQL Server Big Data Cluster
description: 了解如何在 OpenShift 上升级 SQL Server 大数据群集。
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d78e229bcbf2a088d42431abdf02bec3f9e51eab
ms.sourcegitcommit: 62c7b972db0ac28e3ae457ce44a4566ebd3bbdee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231495"
---
# <a name="deploy-big-data-clusters-2019-on-openshift-on-premises-and-azure-red-hat-openshift"></a>在 OpenShift 本地和 Azure Red Hat OpenShift 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文介绍如何在 OpenShift 环境（本地或 Azure Red Hat OpenShift (ARO)）上部署 SQL Server 大数据群集 (BDC)。

> [!TIP]
> 若要快速启动使用 ARO 的示例环境，然后在此平台上部署 BDC，可以使用[此处](quickstart-big-data-cluster-deploy-aro.md)提供的 Python 脚本。


SQL Server 2019 CU5 引入了对 OpenShift 上 SQL Server 大数据群集的支持。 可以在本地 OpenShift 或 Azure Red Hat OpenShift (ARO) 上部署大数据群集。 部署要求 OpenShift 群集版本最低为 4.3。 尽管部署工作流类似于在其他基于 Kubernetes 的平台（[kubeadm](deploy-with-kubeadm.md) 和 [AKS](deploy-on-aks.md)）中进行部署，但也存在一些差异。 不同之处在于，以非根用户身份运行应用程序以及用于部署 BDC 的命名空间的安全性上下文。

若要在本地部署 OpenShift 群集，请参阅 [Red Hat OpenShift 文档](https://docs.openshift.com/container-platform/4.3/release_notes/ocp-4-3-release-notes.html#ocp-4-3-installation-and-upgrade)。 对于 ARO 部署，请参阅 [Azure Red Hat OpenShift](/azure/openshift/intro-openshift)。

本文概述特定于 OpenShift 平台的部署步骤，指出用于访问目标环境的选项，以及用于部署大数据群集的命名空间。

## <a name="pre-requisites"></a>先决条件

> [!IMPORTANT]
> 必须由具有足够的权限来创建这些群集级别对象的 OpenShift 群集管理员（群集管理员群集角色）才能执行以下先决条件。 有关 OpenShift 中的群集角色的详细信息，请参阅[使用 RBAC 定义和应用权限](https://docs.openshift.com/container-platform/4.4/authentication/using-rbac.html)。

1. 确保 OpenShift 上的 `pidsLimit` 设置已更新，以适应 SQL Server 工作负载。 对于工作负载这样的生产环境，OpenShift 中的默认值太低。 至少从 `4096` 开始，但最佳值取决于 SQL Server 中的 `max worker threads` 设置以及 OpenShift 主机节点上的 CPU 处理器数量。 
    - 若要了解如何为 OpenShift 群集更新 `pidsLimit`，请使用[这些说明]( https://github.com/openshift/machine-config-operator/blob/master/docs/ContainerRuntimeConfigDesign.md)。 请注意，低于 `4.3.5` 的 OpenShift 版本存在一个缺陷，该缺陷会导致更新的值无效。 请确保将 OpenShift 升级到最新版本。 
    - 为了帮助你根据环境和计划的 SQL Server 工作负载计算最佳值，可以使用以下估算和示例：

    |处理器数目|默认最大工作线程数|每个处理器的默认辅助角色|最小 pidsLimit 值|
    |--------------------|--------------------------|-----------------------------|-----------------------|
    |          64        |           512            |             16              | 512 + (64 *16) = 1536 |
    |         128        |           512            |             32              | 512 + (128*32) = 4608 |

    > [!NOTE]
    > 其他进程（例如备份、CLR、Fulltext 和 SQLAgent）也会增加一些开销，因此请在估算值中添加一个缓冲区。

1. 下载自定义安全性上下文约束 (SCC) [`bdc-scc.yaml`](#bdc-sccyaml-file)：

    ```console
    curl https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/openshift/bdc-scc.yaml -o bdc-scc.yaml
    ```

1. 将 SCC 应用于群集。

    ```console
    oc apply -f bdc-scc.yaml
    ```

    > [!NOTE]
    > BDC 的自定义 SCC 基于 OpenShift 中内置的 `nonroot` SCC，并具有其他权限。 若要详细了解 OpenShift 中的安全性上下文约束，请参阅[管理安全性上下文约束](https://docs.openshift.com/container-platform/4.3/authentication/managing-security-context-constraints.html)。 有关 `nonroot` SCC 上的大数据群集所需的其他权限的详细信息，请在[此处](https://aka.ms/sql-bdc-openshift-security)下载白皮书。

3. 创建命名空间/项目：

   ```console
   oc new-project <namespaceName>
   ```

4. 将自定义 SCC 分配给部署 BDC 的命名空间中用户的服务帐户：

   ```console
   oc create rolebinding bdc-rbac --clusterrole=system:scc:bdc-scc --group=system:serviceaccounts:<namespace>
   ```

5. 向部署 BDC 的用户分配适当的权限。 执行下列操作之一： 

   - 如果部署 BDC 的用户拥有群集管理员角色，请继续[部署大数据群集](#deploy-big-data-cluster)。

   - 如果部署 BDC 的用户是命名空间管理员，请为创建的命名空间分配用户群集管理员本地角色。 这是用户部署和管理大数据群集以获得命名空间级别管理员权限的首选选项。

   ```console
   oc adm policy add-role-to-user cluster-admin <deployingUser> -n <namespaceName>
   ```

   然后，部署大数据群集的用户必须登录到 OpenShift 控制台：

   ```console
   oc login -u <deployingUser> -p <password>
   ```

## <a name="deploy-big-data-cluster"></a>部署大数据群集

1. 安装最新 [azdata](../azdata/install/deploy-install-azdata.md)。

1. 根据目标环境（本地的 OpenShift 或 ARO）和部署场景，克隆 OpenShift 的某个内置配置文件。 若要了解内置配置文件中特定于 OpenShift 的设置，请参阅下面的“部署配置文件中的 OpenShift 特定设置”部分。 有关可用配置文件的更多详细信息，请参阅[部署指南](deployment-guidance.md)。

   列出所有可用的内置配置文件。

   ```console
   azdata bdc config list
   ```

   要克隆其中某个内置配置文件，请运行以下命令（你可以根据目标平台/场景选择替换配置文件）：

   ```console
   azdata bdc config init --source openshift-dev-test --target custom-openshift
   ```

   对于在 ARO 上进行的部署，从其中某个 `aro-` 配置文件开始，包括适用于该环境的 `serviceType` 和 `storageClass` 的默认值。 例如：

   ```console
   azdata bdc config init --source aro-dev-test --target custom-openshift
   ```

1. 自定义配置文件 control.json 和 bdc.json。 下面是一些其他资源，可指导你完成各种用例支持的自定义：

   - [存储](concept-data-persistence.md)
   - [AD 相关设置](active-directory-deploy.md)
   - [其他自定义](deployment-custom-configuration.md)

   > [!NOTE]
   > 不支持与 Azure Active Directory for BDC 集成，因此在 ARO 上进行部署时不能使用此身份验证方法。

1. 设置[环境变量](deployment-guidance.md#env)

1. 部署大数据群集

   ```console
   azdata bdc create --config custom-openshift --accept-eula yes
   ```

1. 部署成功后，你可以登录并列出外部群集终结点：

   ```console
      azdata login -n mssql-cluster
      azdata bdc endpoint list
   ```

## <a name="openshift-specific-settings-in-the-deployment-configuration-files"></a>部署配置文件中特定于 OpenShift 的设置

SQL Server 2019 CU5 引入了两个功能开关来控制 Pod 和节点指标的集合。 这些参数在 OpenShift 的内置配置文件中默认设置为 `false`，因为监视容器需要[特权安全性上下文](https://www.openshift.com/blog/managing-sccs-in-openshift)，这将放宽部署命名空间 BDC 的一些安全约束。

```json
    "security": {
      "allowNodeMetricsCollection": false,
      "allowPodMetricsCollection": false
}
```

ARO 中默认存储类的名称是 Managed-premium（这与 AKS 相反，AKS 的默认存储类名为 default）。 可以在与 `aro-dev-test` 和 `aro-dev-test-ha` 相对应的 `control.json` 中找到它：

```json
    },
    "storage": {
      "data": {
        "className": "managed-premium",
        "accessMode": "ReadWriteOnce",
        "size": "15Gi"
      },
      "logs": {
        "className": "managed-premium",
        "accessMode": "ReadWriteOnce",
        "size": "10Gi"
      }
```

## <a name="bdc-sccyaml-file"></a>`bdc-scc.yaml` 文件

用于此部署的 SCC 文件是：

:::code language="yaml" source="../../sql-server-samples/samples/features/sql-big-data-cluster/deployment/openshift/bdc-scc.yaml":::

## <a name="next-steps"></a>后续步骤

[教程：将示例数据加载到 SQL Server 大数据群集中](tutorial-load-sample-data.md)
