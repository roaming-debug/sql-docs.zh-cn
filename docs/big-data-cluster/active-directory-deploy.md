---
title: 在 Active Directory 模式下部署
titleSuffix: SQL Server Big Data Cluster
description: 了解如何在 Active Directory 域中升级 SQL Server 大数据群集。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2a79c82f2c3fd443d7237fc3b0a1f7c51102bceb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048012"
---
# <a name="deploy-sql-server-big-data-cluster-in-active-directory-mode"></a>在 Active Directory 模式下部署 SQL Server 大数据群集

本文介绍如何在 Active Directory 模式下部署 SQL Server 大数据群集。 本文中的步骤要求访问 Active Directory 域。 在继续操作之前，需要完成[在 Active Directory 模式下部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](active-directory-prerequisites.md) 中所说明的要求。

## <a name="prepare-deployment"></a>准备部署

要部署具有 AD 集成的 BDC，需要提供一些附加信息以在 AD 中创建与 BDC 相关的对象。

通过使用 `kubeadm-prod` 配置文件或 `openshift-prod`（从 CU5 版本开始），你将自动获得 AD 集成所需的安全相关信息和终结点相关信息的占位符。

此外，还需要提供 [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] 将用于在 AD 中创建必要对象的凭据。 这些凭据作为环境变量提供。

## <a name="set-security-environment-variables"></a>设置安全环境变量

以下环境变量提供 BDC 域服务帐户的凭据，该帐户将用于设置 AD 集成。 BDC 还使用此帐户来维护与 BDC 相关的 AD 对象。

```bash
export DOMAIN_SERVICE_ACCOUNT_USERNAME=<AD principal account name>
export DOMAIN_SERVICE_ACCOUNT_PASSWORD=<AD principal password>
```

## <a name="provide-security-and-endpoint-parameters"></a>提供安全参数和终结点参数

除了凭据的环境变量外，还需要提供安全信息和终结点信息，以便 AD 集成正常工作。 所需参数自动属于 `kubeadm-prod`/`openshift-prod` [部署配置文件](deployment-guidance.md#configfile)。

AD 集成需要以下参数。 使用本文后面显示的 `config replace` 命令将这些参数添加到 `control.json` 和 `bdc.json` 文件中。 以下所有示例都使用示例域 `contoso.local`。

- `security.activeDirectory.ouDistinguishedName`：组织单位 (OU) 的可分辨名称，其中将添加群集部署创建的所有 AD 帐户。 如果域名为 `contoso.local`，则 OU 可分辨名称为：`OU=BDC,DC=contoso,DC=local`。

- `security.activeDirectory.dnsIpAddresses`：包含域的 DNS 服务器 IP 地址的列表。 

- `security.activeDirectory.domainControllerFullyQualifiedDns`：域控制器的 FQDN 列表。 FQDN 包含域控制器的计算机/主机名。 如果有多个域控制器，可以在此处提供列表。 示例：`HOSTNAME.CONTOSO.LOCAL`。

  > [!IMPORTANT]
  > 当多个域控制器为域提供服务时，使用主域控制器 (PDC) 作为安全配置中 `domainControllerFullyQualifiedDns` 列表的第一个条目。若要获取 PDC 名称，在命令提示符处键入 `netdom query fsmo`，然后按 Enter。

- `security.activeDirectory.realm` 可选参数：大多数情况下，领域等同于域名。 对于领域不同于域名的情况，请使用此参数定义领域名称（例如 `CONTOSO.LOCAL`）。 为此参数提供的值应为完全限定的值。

  > [!IMPORTANT]
  > 此时，BDC 不支持 Active Directory 域名与 Active Directory 域的 NETBIOS 名称不同的配置。

- `security.activeDirectory.domainDnsName`：将用于群集的 DNS 域的名称（例如 `contoso.local`）。

- `security.activeDirectory.clusterAdmins`：此参数采用一个 AD 组。 AD 组范围必须为通用或全局。 此组的成员将具有 `bdcAdmin` 群集角色，该角色将在群集中为他们授予管理员权限。 这意味着他们[在 SQL Server 中具有 `sysadmin` 权限](../relational-databases/security/authentication-access/server-level-roles.md#fixed-server-level-roles)、[在 HDFS 中具有 `superuser` 权限](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#The_Super-User)、连接到控制器时具有管理员权限。

  >[!IMPORTANT]
  >部署开始之前，在 AD 中创建此组。 如果此 AD 组的范围为本地域，则部署失败。

- `security.activeDirectory.clusterUsers`：大数据群集中常规用户（无管理员权限）的 AD 组列表。 此列表可以包括范围设置为通用组或全局组的 AD 组。 它们不能是本地域组。

此列表中的 AD 组映射到 `bdcUser` 大数据群集角色，需要对其授予对 SQL Server 的访问权限（请参阅 [SQL Server 权限](../relational-databases/security/permissions-hierarchy-database-engine.md)）或者对 HDFS 的访问权限（请参阅 [HDFS 权限指南](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#:~:text=Permission%20Checks%20%20%20%20Operation%20%20,%20%20N%2FA%20%2029%20more%20rows%20)）。 当连接到控制器终结点时，这些用户只能使用 `azdata bdc endpoint list` 命令列出群集中的可用终结点。

有关如何更新 AD 组的这个设置的详细信息，请参阅[管理 Active Directory 模式下的大数据群集访问](manage-user-access.md)。

  >[!TIP]
  >若要在 Azure Data Studio 中连接到 SQL Server 主机时启用 HDFS 浏览体验，必须向具有 bdcUser 角色的用户授予 VIEW SERVER STATE 权限，因为 Azure Data Studio 使用 `sys.dm_cluster_endpoints` DMV 将所需的 Knox 网关终结点连接到 HDFS。

  >[!IMPORTANT]
  >部署开始之前，请在 AD 中创建这些组。 如果这些 AD 组中的任何一个范围为本地域，则部署失败。

  >[!IMPORTANT]
  >如果你的域用户具有大量组成员身份，则应使用自定义 bdc.json 部署配置文件，以调整网关设置 `httpserver.requestHeaderBuffer` 的值（默认值为 `8192`）和 HDFS 设置 `hadoop.security.group.mapping.ldap.search.group.hierarchy.levels` 的值（默认值为 `10`）。 这是一种最佳做法，可以避免网关和/或 HTTP 响应的连接超时，其中状态代码为 431（请求标头字段太大）。 下面是配置文件的一部分，显示了如何定义这些设置的值，以及用于大量组成员身份的建议值：

```json
{
    ...
    "spec": {
        "resources": {
            ...
            "gateway": {
                "spec": {
                    "replicas": 1,
                    "endpoints": [{...}],
                    "settings": {
                        "gateway-site.gateway.httpserver.requestHeaderBuffer": "65536"
                    }
                }
            },
            ...
        },
        "services": {
            ...
            "hdfs": {
                "resources": [...],
                "settings": {
                  "core-site.hadoop.security.group.mapping.ldap.search.group.hierarchy.levels": "4"
                }
            },
            ...
        }
    }
}
```

  >[!IMPORTANT]
  >在部署开始之前，在 AD 中根据以下设置创建组。 如果这些 AD 组中的任何一个范围为本地域，则部署失败。

- `security.activeDirectory.appOwners` 可选参数：有权创建、删除和运行任何应用程序的 AD 组列表。 此列表可以包括范围设置为通用组或全局组的 AD 组。 它们不能是本地域组。

- `security.activeDirectory.appReaders` 可选参数：有权运行任何应用程序的 AD 组的列表。 此列表可以包括范围设置为通用组或全局组的 AD 组。 它们不能是本地域组。

下表显示了应用程序管理的授权模型：

|   授权的角色   |   [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 命令   |
|----------------------|--------------------|
|   appOwner           | azdata app create  |
|   appOwner           | azdata app update  |
|   appOwner、appReader| azdata app list    |
|   appOwner、appReader| azdata app describe|
|   appOwner           | azdata app delete  |
|   appOwner、appReader| azdata app run     |

- `security.activeDirectory.subdomain`：**可选参数** 此参数在 SQL Server 2019 CU5 版本中引入，以支持针对同一个域部署多个大数据群集。 使用此设置，可以为部署的每个大数据群集指定不同的 DNS 名称。 如果未在 `control.json` 文件的 active directory 部分指定此参数的值，默认情况下，将使用大数据群集名称（与 Kubernetes 命名空间名称相同）来计算子域设置的值。 

  >[!NOTE]
  >通过子域设置传递的值不是新的 AD 域，而只是 BDC 群集内部使用的 DNS 域。

  >[!IMPORTANT]
  >自 SQL Server 2019 CU5 版本起，需要安装或升级最新版 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]，才能利用这些新功能，并在同一域中部署多个大数据群集。

  请参阅[概念：在 Active Directory 模式下部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](active-directory-deployment-background.md)，了解有关在同一 Active Directory 域中部署多个大数据群集的详细信息。

- `security.activeDirectory.accountPrefix`：**可选参数** 此参数在 SQL Server 2019 CU5 版本中引入，以支持针对同一个域部署多个大数据群集。 此设置保证各种大数据群集服务的帐户名称的唯一性，这些名称必须在任意两个群集之间有所不同。 默认情况下，可以选择对帐户前缀名称进行自定义，子域名称用作帐户前缀。 如果子域名称长度超过 12 个字符，则将子域名的前 12 个字符用作帐户前缀。  

  >[!NOTE]
  >Active Directory 要求帐户名称限制在 20 个字符以内。 BDC 群集需要使用 8 个字符来区分 Pod 和 StatefulSets。 这使得帐户前缀有 12 个字符的限制

[检查 AD 组范围](/powershell/module/addsadministration/get-adgroup)，以确定它是否为 DomainLocal。

如果尚未初始化部署配置文件，则可以运行此命令来获取配置的副本。 下面的示例使用 `kubeadm-prod` 配置文件，这同样适用于 `openshift-prod`。

```bash
azdata bdc config init --source kubeadm-prod  --target custom-prod-kubeadm
```

要在 `control.json` 文件中设置上述参数，请使用以下 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 命令。 这些命令将替换配置并在部署之前提供你自己的值。

> [!IMPORTANT]
> 在 SQL Server 2019 CU2 版本中，部署配置文件中的安全配置部分的结构发生了明显的变化，并且所有 Active Directory 相关设置位于 `control.json` 文件的 `security` 下 json 树的新 `activeDirectory` 中。

>[!NOTE]
> 除了为此部分所述的子域提供不同的值外，在同一 Kubernetes 群集中部署多个 BDC 时，还必须为 BDC 终结点使用不同的端口号。 这些端口号可通过[部署配置](deployment-custom-configuration.md)配置文件在部署时进行配置。

下面的示例基于使用 SQL Server 2019 CU2 的情况。 该实例演示了如何替换部署配置中与 AD 相关的参数值。下面的域详细信息为示例值。

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.dnsIpAddresses=[\"10.100.10.100\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.domainControllerFullyQualifiedDns=[\"HOSTNAME.CONTOSO.LOCAL\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.domainDnsName=contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.clusterUsers=[\"bdcusersgroup\"]"
#Example for providing multiple clusterUser groups: [\"bdcusergroup1\",\"bdcusergroup2\"]
```

可以根据需要替代 `subdomain` 和 `accountPrefix` 设置的默认值（仅从 SQL Server 2019 CU5 版本开始）。

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.subdomain=[\"bdctest\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.accountPrefix=[\"bdctest\"]"
```

同样，在 SQL Server 2019 CU2 之前的版本中，你可以运行：

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.dnsIpAddresses=[\"10.100.10.100\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.domainControllerFullyQualifiedDns=[\"HOSTNAME.CONTOSO.LOCAL\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.domainDnsName=contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.clusterUsers=[\"bdcusersgroup\"]"
#Example for providing multiple clusterUser groups: [\"bdcusergroup1\",\"bdcusergroup2\"]
```

除了上述信息之外，还需要提供不同群集终结点的 DNS 名称。 在部署时，将自动在 DNS 服务器中创建使用提供的 DNS 名称的 DNS 条目。 在连接到不同群集终结点时，将使用这些名称。 例如，如果 SQL 主实例的 DNS 名称是 `mastersql`，并且考虑到子域将使用 `control.json` 中群集名称的默认值，则将使用 `mastersql.contoso.local,31433` 或 `mastersql.mssql-cluster.contoso.local,31433` 连接到工具的主实例（具体取决于部署配置文件中为终结点 DNS 名称提供的值）。 

```bash
# DNS names for BDC services
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[0].dnsName=<controller DNS name>.contoso.local"
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[1].dnsName=<monitoring services DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.master.spec.endpoints[0].dnsName=<SQL Master Primary DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.master.spec.endpoints[1].dnsName=<SQL Master Secondary DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].dnsName=<Gateway (Knox) DNS name>.<Domain name. e.g. contoso.local>"
azdata bdc config replace -c custom-prod-kubeadm/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].dnsName=<app proxy DNS name>.<Domain name. e.g. contoso.local>"
```

> [!IMPORTANT]
> 可以使用所选终结点 DNS 名称，只要它们是完全限定的，并且在部署在同一域中的任何两个大数据群集之间不会发生冲突。 可以根据需要使用 `subdomain` 参数值来确保 DNS 名称在群集之间不同。 例如：

```bash
# DNS names for BDC services
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.spec.endpoints[0].dnsName=<controller DNS name>.<subdomain e.g. mssql-cluster>.contoso.local"
```

可以在此处找到示例脚本，用于[在具有 AD 集成的单节点 Kubernetes 群集 (kubeadm) 上部署 SQL Server 大数据群集](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm-ad)。

> [!Note]
> 在某些情况下，无法适应新引入的 `subdomain` 参数。 例如，必须部署低于 CU5 的版本，并且已升级 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]。 这不太可能发生，但如果需要还原到 CU5 之前的行为，可以在 `control.json` 的 active directory 部分中将 `useSubdomain` 参数设置为 `false`。  命令如下：

```bash
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.useSubdomain=false"
```

现在，你应该已设置所有所需的参数，用于部署具有 Active Directory 集成的 BDC。

现在可以使用 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 命令和 kubeadm-prod 部署配置文件部署与 Active Directory 集成的 BDC 群集。 有关如何部署 [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] 的完整文档，请访问[如何在 Kubernetes 上部署 SQL Server 大数据群集](deployment-guidance.md)。

## <a name="verify-reverse-dns-entry-for-domain-controller"></a>验证域控制器的反向 DNS 条目

请确保在 DNS 服务器中注册了域控制器本身的反向 DNS 条目（PTR 记录）。 可以通过在域控制器上运行域名 `nslookup` 来验证这一点，以查看是否可以将其解析为域控制器 IP 地址。

## <a name="known-issues-and-limitations"></a>已知问题和限制

**SQL Server 2019 CU5 中需要注意的限制**

- 目前，“日志搜索”仪表板和“指标”仪表板不支持 AD 身份验证。 部署时设置的基本用户名和密码可用于对这些仪表板进行身份验证。 所有其他群集终结点都支持 AD 身份验证。

- 安全 AD 模式现在仅适用于 `kubeadm` 和 `openshift` 部署环境，而不适用于 AKS 或 ARO。 默认情况下，`kubeadm-prod` 和 `openshift-prod` 部署配置文件包含安全部分。

- 在 SQL Server 2019 CU5 版本以前，每个域 (Active Directory) 只能有一个 BDC。 从 CU5 版本开始，每个域可以有多个 BDC。

- 无法将安全配置中指定的任何 AD 组的作用域设置为 DomainLocal。 可以按照[这些说明](/powershell/module/addsadministration/get-adgroup)来查看 AD 组的作用域。

- 可用于登录到 BDC 的 AD 帐户可以来自为 BDC 配置的同一域。 不支持启用来自其他受信任域的登录。

## <a name="next-steps"></a>后续步骤

[连接 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]：Active Directory 模式](active-directory-connect.md)

[对 SQL Server 大数据群集 Active Directory 集成进行故障排除](troubleshoot-active-directory.md)

[概念：在 Active Directory 模式下部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](active-directory-deployment-background.md)
