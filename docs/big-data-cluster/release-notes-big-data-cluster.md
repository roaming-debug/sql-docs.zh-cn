---
title: SQL Server 大数据群集发行说明
titleSuffix: SQL Server big data clusters
description: 本文介绍 SQL Server 大数据群集的最新更新和已知问题。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 02fbb46968d51bc4dbe730fcc7d575793063bcff
ms.sourcegitcommit: 0f484f32709a414f05562bbaafeca9a9fc57c9ed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2020
ms.locfileid: "94631683"
---
# <a name="sql-server-2019-big-data-clusters-release-notes"></a>SQL Server 2019 大数据群集发行说明

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

以下发行说明适用于 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]。 本文按各版本分为不同的部分。 每个版本都有介绍 CU 更改的支持性文章的链接，还有 Linux 包下载的链接。 本文还列出了 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] (BDC) 最新版本的[已知问题](#known-issues)。

## <a name="supported-platforms"></a>支持的平台

本部分介绍 BDC 支持的平台。

### <a name="kubernetes-platforms"></a>Kubernetes 平台

|平台|支持的版本|
|---------|---------|
|Vanilla（上游）Kubernetes|使用 Kubernetes 群集最低版本 1.13 在本地部署 BDC。 请参阅 [Kubernetes version and version skew support policy](https://kubernetes.io/docs/setup/release/version-skew-policy/)（Kubernetes 版本和版本倾斜支持策略）。|
|Red Hat OpenShift|使用 OpenShift 群集最低版本 4.3 在本地部署 BDC。 请参阅 [Red Hat OpenShift Container Platform Life Cycle Policy](https://access.redhat.com/support/policy/updates/openshift)（Red Hat OpenShift 容器平台生命周期策略）。<br><br> SQL Server 2019 CU5 中引入的支持。|
|Azure Kubernetes 服务 (AKS)|在 AKS 群集最低版本 1.13 上部署 BDC。<br/>请参阅 [AKS 中支持的 Kubernetes 版本](/azure/aks/supported-kubernetes-versions)，详细了解版本支持策略。|
|Azure Red Hat OpenShift (ARO)|在 ARO 最低版本 4.3 上部署 BDC。 请参阅 [Azure Red Hat OpenShift](/azure/openshift/)。 <br><br> SQL Server 2019 CU5 中引入的支持。|

### <a name="host-os-for-kubernetes"></a>适用于 Kubernetes 的主机操作系统

|平台|主机 OS|支持的版本|
|---------|---------|---------|
|Kubernetes|Ubuntu|16.04|
|Kubernetes|Red Hat Enterprise Linux|版本 7.3、7.4、7.5、7.6|
|OpenShift|Red Hat Enterprise Linux / CoreOS |请参阅 [OpenShift 发行说明](https://docs.openshift.com/container-platform/4.3/release_notes/ocp-4-3-release-notes.html#ocp-4-3-about-this-release)|

### <a name="sql-server-editions"></a>SQL Server 版本

|版本|说明|
|---------|---------|
|Enterprise<br/>Standard<br/>开发人员| 大数据群集版本由 SQL Server 主实例的版本确定。 在部署时会默认部署开发人员版。 可在部署后更改版本。 请参阅[配置 SQL Server 主实例](./configure-sql-server-master-instance.md)。 |

## <a name="tools"></a>工具

|平台|支持的版本|
|---------|---------|
|[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]|最佳做法是使用可用的最新版本。 从 SQL Server 2019 CU5 版本开始，[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 具有来自服务器的独立语义版本。 <br/><br/>运行 `azdata –-version` 以验证该版本。<br/><br/>请参阅[版本历史记录](#release-history)获取最新版本。|
|Azure Data Studio|获取 [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) 的最新内部版本。|

有关完整列表，请参阅[需要哪些工具？](deploy-big-data-tools.md#which-tools-are-required)

## <a name="release-history"></a>版本历史记录

下表列出了 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 的版本历史记录。

| 版本 <sup>1</sup> | BDC 版本    | [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 版本 <sup>2</sup>| 发布日期 |
|------------------|----------------|-----------------|--------------|
| [CU8](#cu8)      | 15.0.4073.23   | 20.2.2          | 2020-10-19   |
| [CU6](#cu6)      | 15.0.4053.23   | 20.0.1          | 2020-08-04   |
| [CU5](#cu5)      | 15.0.4043.16   | 20.0.0          | 2020-06-22   |
| [CU4](#cu4)      | 15.0.4033.1    | 15.0.4033       | 2020 年 3 月 31 日   |
| [CU3](#cu3)      | 15.0.4023.6    | 15.0.4023       | 2020-03-12   |
| [CU2](#cu2)      | 15.0.4013.40   | 15.0.4013       | 2020-02-13   |
| [CU1](#cu1)      | 15.0.4003.23   | 15.0.4003       | 2020-01-07   |
| [GDR1](#rtm)     | 15.0.2070.34   | 15.0.2070       | 2019-11-04   |

<sup>1</sup> CU7 不可用于 BDC。

<sup>2</sup> [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 版本反映了发布 CU 时的工具的版本。 `azdata` 也可以独立于服务器版本进行发布，因此，在安装最新包时可能会获得较新版本。 较新版本与以前发布的 CU 兼容。

## <a name="how-to-install-updates"></a>如何安装更新

若要安装更新，请参阅[如何升级 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-upgrade.md)。

## <a name="cu8-september-2020"></a><a id="cu8"></a> CU8（2020 年 9 月）

SQL Server 2019 累积更新 8 (CU8) 版本。

|包版本 | 映像标记 |
|-----|-----|
|15.0.4073.23 |[2019-CU8-ubuntu-16.04]

此版本包含多个修补程序和一些增强功能。

### <a name="added-capabilities"></a>新增功能

- 使用系统管理的密钥和证书的 [SQL Server 大数据群集静态加密](encryption-at-rest-concepts-and-configuration.md)。
   > [!CAUTION]
   > 这是 SQL Server BDC 静态加密的初始版本。 请参阅以下文章： 
   > - [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的安全性概念](concept-security.md)
   > - [静态加密概念和配置指南](encryption-at-rest-concepts-and-configuration.md)
- 对数据虚拟化方案的 [Oracle 代理用户](tutorial-query-oracle.md)支持。

## <a name="cu6-july-2020"></a><a id="cu6"></a> CU6（2020 年 7 月）

SQL Server 2019 的累积更新 6 (CU6) 版本。

|包版本 | 映像标记 |
|-----|-----|
|15.0.4053.23 |[2019-CU6-ubuntu-16.04]

此版本包含少许修补程序和增强功能。 以下文章包括与这些更新相关的信息：

- [在 Active Directory 模式下管理大数据群集](manage-user-access.md)
- [在 Active Directory 模式下部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](active-directory-deploy.md)
- [在 AKS 的 Active Directory 模式下部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](active-directory-deployment-aks.md)
- [通过高可用性部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-high-availability.md)
- [配置 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](configure-cluster.md)
- [在大数据群集中配置 Apache Spark 和 Apache Hadoop](configure-spark-hdfs.md)
- [SQL Server 主实例配置属性](reference-config-master-instance.md)
- [Apache Spark 和 Apache Hadoop (HDFS) 配置属性](reference-config-spark-hadoop.md)
- [Kubernetes RBAC 模型及对用户和服务帐户管理 BDC 的影响](kubernetes-rbac.md)

## <a name="cu5-june-2020"></a><a id="cu5"></a> CU5（2020 年 6 月）

SQL Server 2019 的累积更新 5 (CU5) 版本。

|包版本 | 映像标记 |
|-----|-----|
|15.0.4043.16 |[2019-CU5-ubuntu-16.04]

### <a name="added-capabilities"></a>新增功能

- 支持在 Red Hat OpenShift 上部署大数据群集。 支持包括部署在本地版本 4.3 及更高版本的 OpenShift 容器平台以及 Azure Red Hat OpenShift。 请参阅[在 OpenShift 上部署 SQL Server 大数据群集](deploy-openshift.md)
- 更新了 BDC 部署安全模型，因此，不再需要作为 BDC 的一部分部署的特权容器。 对于使用 SQL Server 2019 CU5 的所有新部署，容器除了是非特权容器之外，还默认以非根用户身份运行。 
- 添加了对针对 Active Directory 域部署多个大数据群集的支持。
- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 有它自己的独立于服务器的语义版本。 删除了客户端与 azdata 的服务器版本之间的任何依赖项。 建议为客户端和服务器使用最新版本，确保从最新的增强功能和修补程序中获益。
- 引入了两个新的存储过程 sp_data_source_objects 和 sp_data_source_table_columns，以支持某些外部数据源的自检。 客户可通过 T-SQL 直接使用它们来发现架构，以及查看哪些表可进行虚拟化。 我们在 Azure Data Studio 的[数据虚拟化扩展](../azure-data-studio/extensions/data-virtualization-extension.md)的外部表向导中利用这些更改，这允许你通过 SQL Server、Oracle、MongoDB 和 Teradata 创建外部表。
- 添加了对在 Grafana 中执行的持久性自定义的支持。 在 CU5 之前，客户可以注意到，在 Grafana 配置中进行的任何编辑都会在 `metricsui` pod（托管 Grafana 仪表板）重启时丢失。 此问题已得到修复，现已保留所有配置。 
- 修复了与用于使用 Telegraf（在 `metricsdc` pod 中托管）收集 pod 和节点指标的 API 相关的安全问题。 此更改的结果是，Telegraf 现在需要服务帐户、群集角色和群集绑定，才能具有收集 pod 和节点指标所需的权限。 有关更多详细信息，请参阅[收集 pod 和节点指标所需的群集角色](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection)。
- 添加了两个功能开关来控制 pod 和节点指标的收集。 如果你使用不同的解决方案来监视 Kubernetes 基础结构，则可以通过在 control.json 部署配置文件中将 allowNodeMetricsCollection 和 allowPodMetricsCollection 设置为 false，以关闭 pod 和主机节点的内置指标收集 。 对于 OpenShift 环境，这些设置在内置部署配置文件中默认设置为 false，因为收集 pod 和节点指标需要特权功能。

## <a name="cu4-april-2020"></a><a id="cu4"></a> CU4（2020 年 4 月）

SQL Server 2019 的累积更新 4 (CU4) 版本。 此次发布的 SQL Server 数据库引擎版本是 15.0.4033.1。

|包版本 | 映像标记 |
|-----|-----|
|15.0.4033.1 |[2019-CU4-ubuntu-16.04]

## <a name="cu3-march-2020"></a><a id="cu3"></a> CU3（2020 年 3 月）

SQL Server 2019 的累积更新 3 (CU3) 版本。 此次发布的 SQL Server 数据库引擎版本是 15.0.4023.6。

|包版本 | 映像标记 |
|-----|-----|
|15.0.4023.6 |[2019-CU3-ubuntu-16.04]

### <a name="resolved-issues"></a>已解决的问题

SQL Server 2019 CU3 解决了以前版本中的以下问题。

- [通过专用存储库进行部署](#deployment-with-private-repository)
- [升级可能因超时而失败](#upgrade-may-fail-due-to-timeout)

## <a name="cu2-february-2020"></a><a id="cu2"></a> CU2（2020 年 2 月）

SQL Server 2019 的累积更新 2 (CU2) 版本。 此次发布的 SQL Server 数据库引擎版本是 15.0.4013.40。

|包版本 | 映像标记 |
|-----|-----|
|15.0.4013.40 |[2019-CU2-ubuntu-16.04]

## <a name="cu1-january-2020"></a><a id="cu1"></a> CU1 （2020 年 1 月）

SQL Server 2019 的累积更新 1 (CU1) 版本。 此次发布的 SQL Server 数据库引擎版本是 15.0.4003.23。

|包版本 | 映像标记 |
|-----|-----|
|15.0.4003.23|[2019-CU1-ubuntu-16.04]

## <a name="gdr1-november-2019"></a><a id="rtm"></a> GDR1（2019 年 11 月）

SQL Server 2019 常规分发版本 1 (GDR1) - 介绍 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-nover.md)] 的公开发布。 此次发布的 SQL Server 数据库引擎版本是 15.0.2070.34。

|包版本 | 映像标记 |
|-----|-----|
|15.0.2070.34|[2019-GDR1-ubuntu-16.04]

[!INCLUDE [sql-server-servicing-updates-version-15](../includes/sql-server-servicing-updates-version-15.md)]

## <a name="known-issues"></a>已知问题

### <a name="ha-sql-server-database-encryption-key-encryptor-rotation"></a>HA SQL Server 数据库加密密钥加密程序轮换

- **受影响的版本**：所有大数据群集 HA 部署（无论是何种版本）。

- **问题及其对客户的影响**：在使用 HA 部署 SQL Server 时，无法对加密数据库执行证书轮换。 如果对主池执行以下命令，就会出现错误消息：
    ```
    ALTER DATABASE ENCRYPTION KEY
    ENCRYPTION BY SERVER
    CERTIFICATE <NewCertificateName>
    ```
    这没有影响，虽然此命令失败，但目标数据库加密是使用旧证书进行保留的。
    
### <a name="empty-livy-jobs-before-you-apply-cumulative-updates"></a>应用累积更新前清空 Livy 作业

- **受影响的版本**：CU6 及所有更低版本。 已为 CU8 解决。

- **问题及其对客户的影响**：在升级过程中，`sparkhead` 返回 404 错误。

- **解决方法**：升级 BDC 之前，请确保没有活动的 Livy 会话或批处理作业。 按照[从支持的版本升级](deployment-upgrade.md#upgrade-from-supported-release)中的说明进行操作，以避免这种情况。 

   如果在升级过程中 Livy 返回 404 错误，请在两个 `sparkhead` 节点上重启 Livy 服务器。 例如：

   ```console
   kubectl -n <clustername> exec -it sparkhead-0/sparkhead-1 -c hadoop-livy-sparkhistory -- exec supervisorctl restart livy
   ```

### <a name="big-data-cluster-generated-service-accounts-passwords-expiration"></a>大数据群集生成的服务帐户密码过期

- **受影响的版本**：与 Active Directory 集成的所有大数据群集部署（不考虑版本）

- **问题及其对客户的影响**：在大数据群集部署过程中，工作流将生成一组 [服务帐户](active-directory-objects.md)。根据域控制器中设置的密码过期策略，这些帐户的密码可能会过期（默认值为 42 天）。 目前，没有任何机制可以轮换 BDC 中所有帐户的凭据，因此一到过期时间，群集将变为不可操作。

- **解决方法**：在域控制器中将 BDC 服务帐户的过期策略更新为“密码永不过期”。 有关这些帐户的完整列表，请参阅[自动生成的 Active Directory 对象](active-directory-objects.md)。 此操作可在过期时间之前或之后完成。 在后一种情况下，Active Directory 将重新激活过期的密码。

### <a name="credentials-for-accessing-services-through-gateway-endpoint"></a>通过网关终结点访问服务所用的凭据

- **受影响的版本**：从 CU5 开始部署的新群集。

- **问题及其对客户的影响**：对于使用 SQL Server 2019 CU5 部署的新大数据群集，网关用户名不是“根”。 如果用于连接到网关终结点的应用程序使用的凭据错误，你将看到身份验证错误。 此更改是在大数据群集中以非根用户身份运行应用程序（一种从 SQL Server 2019 CU5 版本开始的新默认行为，在使用 CU5 部署新的大数据群集时，网关终结点的用户名基于通过 AZDATA_USERNAME 环境变量传递的值）的结果。 网关终结点的用户名与用于控制器和 SQL Server 终结点的用户名相同。 这只会影响新部署，使用任何之前版本部署的现有大数据群集将继续使用“根”。 将群集部署为使用 Active Directory 身份验证时，不会对凭据产生任何影响。 

- **解决方法**：Azure Data Studio 将以透明方式处理用于连接到网关的凭据的更改，以在 ObjectExplorer 中启用 HDFS 浏览体验。 必须安装包括解决此用例所需的更改的[最新 Azure Data Studio 版本](../azure-data-studio/download-azure-data-studio.md)。
对于必须提供凭据以通过网关访问服务的其他情况（例如使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 登录、访问 Spark 的 Web 仪表板），必须确保使用正确的凭据。 如果你的目标是在 CU5 之前部署的现有群集，你将继续使用“根”用户名连接到网关，即使是在将群集升级到 CU5 之后也是如此。 如果使用 CU5 版本部署新群集，请通过提供与 AZDATA_USERNAME 环境变量对应的用户名进行登录。

### <a name="pods-and-nodes-metrics-not-being-collected"></a>未收集的 pod 和节点指标

- **受影响的版本**：使用 CU5 映像的新群集和现有群集

- **问题及其对客户的影响**：由于与 `telegraf` 用于收集指标 pod 和主机节点指标的 API 相关的安全修补程序，客户可能会注意到未收集指标。 这在新的和现有的 BDC 部署（升级到 CU5 之后）中都有可能发生。 由于该修补程序，Telegraf 现在需要具有群集范围的角色权限的服务帐户。 部署尝试创建必要的服务帐户和群集角色，但如果部署群集或执行升级的用户没有足够的权限，部署/升级将在出现警告的情况下继续进行并获得成功，但不会收集 pod 和节点指标。

- **解决方法**：你可以要求管理员创建角色和服务帐户（部署/升级之前或之后），BDC 将使用它们。 [本文](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection)介绍了如何创建所需的项目。

### <a name="azdata-bdc-copy-logs-command-failure"></a>`azdata bdc copy-logs` 命令失败

- **受影响的版本**：[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 版本 20.0.0

- **问题及其对客户的影响**：copy-logs 命令的实现假定 `kubectl` 客户端工具安装在发出该命令的客户端计算机上。 如果要针对安装在 OpenShift 上的 BDC 群集发出该命令，则在仅安装了 `oc` 工具的客户端上，你将收到错误：收集日志时出错: [WinError 2] 系统找不到指定的文件。

- **解决方法**：在同一台客户端计算机上安装 `kubectl` 工具，然后重新发出 `azdata bdc copy-logs` 命令。 请参阅[此处](deploy-big-data-tools.md)的说明，了解如何安装 `kubectl`。

### <a name="deployment-with-private-repository"></a>通过专用存储库进行部署

- **受影响的版本**：GDR1、CU1、CU2。 CU3 已解决。

- **问题及其对客户的影响**：从专用存储库升级需要满足特定要求

- **解决方法**：如果使用专用存储库来预提取用于部署或升级 BDC 的映像，请确保当前版本映像和目标版本映像位于专用存储库中。 这样，在必要时可以成功回退。 此外，如果在原始部署后更改了专用存储库的凭据，请在升级之前更新 Kubernetes 中的相应机密。 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 不支持通过 `AZDATA_PASSWORD` 和 `AZDATA_USERNAME` 环境变量来更新凭据。 使用 [`kubectl edit secrets`](https://kubernetes.io/docs/concepts/configuration/secret/#editing-a-secret) 更新机密。 

不支持对当前版本和目标版本使用不同的存储库进行升级。

### <a name="upgrade-may-fail-due-to-timeout"></a>升级可能因超时而失败

- **受影响的版本**：GDR1、CU1、CU2。 CU3 已解决。

- **问题及其对客户的影响**：升级可能因超时而失败。

   下面的代码显示了提示失败的消息：

   ```
   >azdata.EXE bdc upgrade --name <mssql-cluster>
   Upgrading cluster to version 15.0.4003

   NOTE: Cluster upgrade can take a significant amount of time depending on
   configuration, network speed, and the number of nodes in the cluster.

   Upgrading Control Plane.
   Control plane upgrade failed. Failed to upgrade controller.
   ```

   在 Azure Kubernetes Service (AKS) 中升级 BDC 时，更有可能出现此错误。

- **解决方法**：增加升级的超时时间值。 

   若要增加升级的超时时间值，请编辑升级配置映射。 编辑升级配置映射：

   1. 运行以下命令：

      ```bash
      kubectl edit configmap controller-upgrade-configmap
      ```

   2. 编辑以下字段：

       **`controllerUpgradeTimeoutInMinutes`** 指定等待控制器或控制器 db 完成升级所需的分钟数。 默认值为 5。 至少更新为 20。

       **`totalUpgradeTimeoutInMinutes`** ：指定控制器和控制器 db 完成升级所需的总时间（`controller` + `controllerdb` 升级）。 默认值为 10。 至少更新为 40。

       **`componentUpgradeTimeoutInMinutes`** ：指定升级的每个后续阶段必须完成的时间量。 默认值为 30。 更新为 45。

   3. 保存并退出

   还有另一种方法可设置超时时间值，即使用下面的 python 脚本：

   ```python
   from kubernetes import client, config
   import json

   def set_upgrade_timeouts(namespace, controller_timeout=20, controller_total_timeout=40, component_timeout=45):
       """ Set the timeouts for upgrades

       The timeout settings are as follows

       controllerUpgradeTimeoutInMinutes: sets the max amount of time for the controller
           or controllerdb to finish upgrading

       totalUpgradeTimeoutInMinutes: sets the max amount of time to wait for both the
           controller and controllerdb to complete their upgrade

       componentUpgradeTimeoutInMinutes: sets the max amount of time allowed for
           subsequent phases of the upgrade to complete
       """
       config.load_kube_config()

       upgrade_config_map = client.CoreV1Api().read_namespaced_config_map("controller-upgrade-configmap", namespace)

       upgrade_config = json.loads(upgrade_config_map.data["controller-upgrade"])

       upgrade_config["controllerUpgradeTimeoutInMinutes"] = controller_timeout

       upgrade_config["totalUpgradeTimeoutInMinutes"] = controller_total_timeout

       upgrade_config["componentUpgradeTimeoutInMinutes"] = component_timeout

       upgrade_config_map.data["controller-upgrade"] = json.dumps(upgrade_config)

       client.CoreV1Api().patch_namespaced_config_map("controller-upgrade-configmap", namespace, upgrade_config_map)
   ```

### <a name="livy-job-submission-from-azure-data-studio-ads-or-curl-fail-with-500-error"></a>从 Azure Data Studio (ADS) 或 curl 提交 Livy 作业失败，出现 500 错误

- **问题及其对客户的影响**：在 HA 配置中，Spark 共享资源 `sparkhead` 配置有多个副本。 在这种情况下，可能会遇到从 Azure Data Studio (ADS) 或 `curl` 提交 Livy 作业失败的问题。 如果 `curl` 到任何 `sparkhead` Pod 都会导致连接被拒绝，则可以验证该问题。 例如，`curl https://sparkhead-0:8998/` 或 `curl https://sparkhead-1:8998` 返回 500 错误。

   在下列情况下会发生这种情况：

   - 每个 Zookeeper 实例的 Zookeeper Pod 或进程重启几次时。
   - 当 `sparkhead` Pod 和 Zookeeper Pod 之间的网络连接不可靠时。

- **解决方法**：重启两个 Livy 服务器。

   ```bash
   kubectl -n <clustername> exec sparkhead-0 -c hadoop-livy-sparkhistory supervisorctl restart livy
   ```

   ```bash
   kubectl -n <clustername> exec sparkhead-1 -c hadoop-livy-sparkhistory supervisorctl restart livy
   ```

### <a name="create-memory-optimized-table-when-master-instance-in-an-availability-group"></a>当主实例位于可用性组中时，创建内存优化表

- **问题及其对客户的影响**：不能使用为了连接到可用性组数据库（侦听器）而公开的主终结点来创建内存优化表。

- **解决方法**：若要在 SQL Server 主实例为可用性组配置时创建内存优化表，请 [连接到 SQL Server 实例](deployment-high-availability.md#instance-connect)，公开一个终结点，连接到 SQL Server 数据库，并在使用新连接创建的会话中创建内存优化表。

### <a name="insert-to-external-tables-active-directory-authentication-mode"></a>在 Active Directory 身份验证模式下插入外部表

- **问题及其对客户的影响**：当 SQL Server 主实例处于 Active Directory 身份验证模式时，如果查询仅从外部表（至少有一个外部表在存储池中）中选择并插入到另一个外部表中，该查询返回：

   ```
   Msg 7320, Level 16, State 102, Line 1
   Cannot execute the query "Remote Query" against OLE DB provider "SQLNCLI11" for linked server "SQLNCLI11". Only domain logins can be used to query Kerberized storage pool.
   ```

- **解决方法**：通过下列方式之一修改查询。 将存储池表联接到本地表，或者先插入到本地表，然后从本地表进行读取以插入到数据池。

### <a name="transparent-data-encryption-capabilities-can-not-be-used-with-databases-that-are-part-of-the-availability-group-in-the-sql-server-master-instance"></a>透明数据加密功能不能与 SQL Server 主实例中可用性组中的数据库一起使用

- **问题及其对客户的影响**：在 HA 配置中，由于每个副本上用于加密的主密钥不同，因此在故障转移后不能使用启用了加密的数据库。 

- **解决方法**：没有针对此问题的解决方法。 建议在准备好修复之前，不要在此配置中启用加密。

## <a name="next-steps"></a>后续步骤

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)