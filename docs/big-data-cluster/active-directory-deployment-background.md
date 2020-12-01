---
title: 在 Active Directory 域中部署多个大数据群集
titleSuffix: SQL Server Big Data Cluster
description: 了解如何在单个 Active Directory 域中部署多个 SQL Server 大数据群集。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 144b769ce42b192099678cda4cfe6fb2935c1c2f
ms.sourcegitcommit: af663bdca0df8a1f34a14667390662f6f0e17766
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2020
ms.locfileid: "94924164"
---
# <a name="deploy-multiple-big-data-clusters-2019-in-the-same-active-directory-domain"></a>在同一 Active Directory 域中部署多个 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文介绍 SQL Server 2019 CU5 的更新，这些更新支持将多个 SQL Server 2019 大数据群集部署并集成到同一 Active Directory 域。

在 SQL 2019 CU5 之前，存在两个问题阻止在 AD 域中部署多个 BDC。

- 服务主体名称和 DNS 域的命名冲突
- 域帐户主体名称

## <a name="object-name-collisions"></a>对象名称冲突

### <a name="service-principal-names-spn-and-dns-domain-naming-conflict"></a>服务主体名称 (SPN) 和 DNS 域命名冲突

部署时提供的域名用作 AD DNS 域。 这意味着，在内部网络中 Pod 可以使用此 DNS 域连接到彼此。 此外，用户使用此 DNS 域连接到 BDC 终结点。 因此，为 BDC 中服务创建的任何服务主体名称 (SPN) 都具有使用此 AD DNS 域限定的 Kubernetes Pod、服务或终结点名称。 如果用户在域中部署第二个群集，则生成的 SPN 具有相同的 FQDN，因为在这两个群集之间 Pod 名称和 DNS 域名相同。 例如 AD DNS 域是 `contoso.local`。 为 Pod `master-0` 中主池 SQL Server 生成的某个 SPN 为 `MSSQLSvc/master-0.contoso.local:1433`。 在用户尝试部署的第二个群集中，`master-0` 的 Pod 名称相同，并且用户将提供相同的 AD DNS 域 (``contoso.local``)，导致相同的 SPN 字符串。 Active Directory 禁止创建冲突的 SPN，导致第二个群集的部署失败。

### <a name="domain-account-principal-names"></a>域帐户主体名称

在 Active Directory 域的 BDC 部署过程中，将为在 BDC 内运行的服务生成多个帐户主体。 它们实质上是 AD 用户帐户。 在 SQL 2019 CU5 之前，这些帐户的名称在群集之间不唯一。 尝试使用此清单在两个不同的群集中为 BDC 中的特定服务创建相同的用户帐户名。 第二个部署的群集将在 AD 中遇到冲突，无法创建其帐户。

## <a name="resolution-for-collisions"></a>解决冲突

### <a name="solution-to-solve-the-problem-with-spns-and-dns-domain---sql-2019-cu5"></a>SPN 和 DNS 域问题的解决方案 - SQL 2019 CU5

由于任意两个群集中的 SPN 必须不同，因此部署时传入的 DNS 域名必须不同。 可以使用部署配置文件中新引入的设置指定不同的 DNS 名称：`subdomain`。 如果这两个群集之间的子域不同，并且可以通过此子域进行内部通信，则 SPN 包括实现所需唯一性的子域。

>[!NOTE]
>通过子域设置传递的值不是新的 AD 域，而是内部使用的 DNS 域。

再次考虑使用主池 SQL Server SPN 的例子。 如果子域是 `bdc`，则前面讨论的 SPN 将更改为 `MSSQLSvc/master-0.bdc.contoso.local:1433`。  

对 active directory 配置规范中新引入的子域参数的值进行自定义是可选的。 默认情况下，BDC 群集名称或命名空间名称将用于计算子域设置的值。 当用户想要替代子域名称时，可以使用 active directory 配置规范中引入的新子域参数来进行。

### <a name="solution-to-solve-the-problem-regarding-account-names-uniqueness"></a>有关帐户名称唯一性问题的解决方案

为了将帐户名称更新为保证唯一性的方案，引入了帐户前缀的概念。 帐户前缀是在任何两个群集之间都具唯一性的帐户名称的一部分。 帐户名称的剩余部分对于给定服务是不变的。 帐户名的新格式类似于 `<prefix>-<name>-<podId>`。 

>[!NOTE]
>Active Directory 要求帐户名称限制在 20 个字符以内。 BDC 群集需要使用 8 个字符来区分 Pod 和 StatefulSets。 这使得帐户前缀有 12 个字符的限制

对帐户名称进行自定义是可选的。 使用 active directory 配置规范中的 `accountPrefix` 参数。SQL Server 2019 CU5 在配置规范中引入 `accountPrefix`。默认情况下，子域名称用作帐户前缀。 如果子域名称长度超过 12 个字符，则将子域名的前 12 个字符的子字符串用作帐户前缀。

子域仅适用于 DNS。 因此，新的 LDAP 用户帐户名为 `bdc-ldap@contoso.local`。 帐户名称不会是 `bdc-ldap@bdc.contoso.local`。

## <a name="semantics"></a>语义

总的来说，以下是在 SQL 2019 CU5 中为域中多个群集添加的参数的语义：

### `subdomain`

- 可选字段
- 数据类型：字符串
- 定义：用于此 BDC 群集的唯一 DNS 子域。 对于部署在 Active Directory 域中的每个群集而言，此值应有所不同。  
- 默认值：如未提供，则群集名称将用作默认值
- 最大长度：每个标签 63 个字符（标签是用点分隔的每个字符串）。
- 备注：终结点 DNS 名称应在其 FQDN 中使用子域。

### `accountPrefix`

- 可选字段
- 数据类型：字符串
- 定义：AD 帐户 BDC 群集将生成唯一的前缀。 对于部署在 Active Directory 域中的每个群集而言，此值应有所不同。
- 默认值：如未提供，则子域名称将用作默认值。 如未提供子域，群集名称将用作子域名称，因此群集名称也将成为 accountPrefix。 如果提供子域并且它是多部分名称（包含一个或多个点），则用户必须提供 accountPrefix。 
- 最大长度：12 个字符 

## <a name="impact-on-ad-domain-and-dns-server"></a>对 AD 域和 DNS 服务器的影响 

为了适应针对同一 Active Directory 域部署多个 BDC，AD 域或域控制器中无需进行任何更改。 注册外部终结点 DNS 名称时，DNS 子域将自动在 DNS 服务器中创建。 

## <a name="impact-on-setting-up-the-deployment-configuration-file-used-for-the-bdc-deployment"></a>对设置用于 BDC 部署的部署配置文件的影响 

控制平面配置 control.json 中的 activeDirectory 部分将有两个新的可选参数：`subdomain` 和 `accountPrefix` 。 仅在想要替代默认行为（为每个设置使用群集名称）时才提供这些设置的值。 群集名称与命名空间名称相同。

此外，可以使用所选终结点 DNS 名称，只要它们是完全限定的，并且在部署在同一域中的任何两个大数据群集之间不会发生冲突。 可以根据需要使用子域参数值来确保 DNS 名称在群集之间不同。  考虑使用网关终结点的例子。 如果要将名称 `gateway` 用于终结点，并将其作为 BDC 部署的一部分自动注册到 DNS 服务器，请使用 `gateway.bdc1.contoso.local` 作为 DNS 名称。 如果 `bdc1` 是子域，并且 `contoso.local` 是 AD DNS 域名的话。 其他可接受的值为 `gateway-bdc1.contoso.local` 或者就是 `gateway.contoso.local`。

## <a name="examples"></a>示例

如果想要替代子域和 accountPrefix，下面是 active directory 安全配置的示例。 

```json
    "security": { 
        "activeDirectory": { 
            "ouDistinguishedName":"OU=contosoou,DC=contoso,DC=local", 
            "dnsIpAddresses": [ "10.10.10.10" ], 
            "domainControllerFullyQualifiedDns": [ "contoso-win2016-dc.contoso.local" ], 
            "domainDnsName":"contoso.local", 
            "subdomain": "bdc", 
            "accountPrefix": "myprefix", 
            "clusterAdmins": [ "contosoadmins" ], 
            "clusterUsers": [ "contosousers1", "contosousers2" ] 
        } 
    } 
  
```

下面是控制平面终结点的终结点规范的示例。 可以对 DNS 名称使用任何值，前提是这些值是唯一的并且是完全限定的：
  
```json
        "endpoints": [ 
            { 
                "serviceType": "NodePort", 
                "port": 30080, 
                "name": "Controller", 
                "dnsName": "control-bdc1.contoso.local" 
            }, 
            { 
                "serviceType": "NodePort", 
                "port": 30777, 
                "name": "ServiceProxy", 
                "dnsName": "monitor-bdc1.contoso.local" 
            } 
        ] 
  
```

## <a name="questions"></a>问题

### <a name="do-you-need-to-create-separate-ous-for-different-clusters"></a>是否需要为不同群集创建不同的 OU？

这不是必需的，但建议这样做。 为不同的群集提供不同的 OU 有助于管理生成的用户帐户。

### <a name="how-to-revert-back-to-the-pre-cu5-behavior-in-sql-2019"></a>如何在 SQL 2019 中还原到 CU5 之前的行为？

在某些情况下，无法适应新引入的 `subdomain` 参数。 例如，必须部署低于 CU5 的版本，并且已升级 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]。 这不太可能发生，但如果需要还原到 CU5 之前的行为，可以在 `control.json` 的 active directory 部分中将 `useSubdomain` 参数设置为 `false`。

以下示例针对这种情况将 `useSubdomain` 设置为 `false`。

```console
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.useSubdomain=false" 
```

## <a name="next-steps"></a>后续步骤

[对 SQL Server 大数据群集 Active Directory 集成进行故障排除](troubleshoot-active-directory.md)
