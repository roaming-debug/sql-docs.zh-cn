---
title: 静态加密
titleSuffix: SQL Server Big Data Clusters
description: 全面了解 SQL Server 2019 大数据群集的静态加密。
author: dacoelho
ms.author: dacoelho
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/19/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: df878f94c2ed6338ae28cbff156460ffdef87826
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046657"
---
# <a name="encryption-at-rest-concepts-and-configuration-guide"></a>静态加密概念和配置指南

自 SQL Server 大数据群集 CU8 起，可以使用全面的静态加密功能集为存储在平台中的所有数据提供应用程序级别加密。 本指南介绍了 SQL Server 大数据群集的静态加密功能集的概念、体系结构和配置。

SQL Server 大数据群集将数据存储在以下两个位置：

* SQL Server 主实例
* 存储池和 Spark 使用的 HDFS。

有两种方法可用来以透明方式对 SQL Server 大数据群集中的数据进行加密：

* 卷加密。 这种方法受 Kubernetes 平台支持，应作为大数据群集部署的最佳做法。 本指南并不涉及卷加密。 有关如何正确地对用于 SQL Server 大数据群集的卷进行加密的指南，请参阅 Kubernetes 平台或设备文档。
* 应用程序级别加密。 这种体系结构是指，在数据写入磁盘之前由处理数据的应用程序进行的数据加密。 在卷是公开的情况下，攻击者无法在其他地方还原数据项目，除非目标系统也配置了相同的加密密钥。 

SQL Server 大数据群集的静态加密功能集支持 SQL Server 和 HDFS 组件的应用程序级别加密的核心方案。

提供了以下功能：

* 系统管理的静态加密。 CU8 中提供了此功能。
* 用户管理的静态加密 (BYOK)，同时集成了服务管理的密钥提供程序和外部密钥提供程序。 目前，只支持服务管理的用户创建的密钥。

## <a name="key-definitions"></a>关键定义

### <a name="sql-server-big-data-clusters-key-management-service-kms"></a>SQL Server 大数据群集密钥管理服务 (KMS)

控制器托管的服务，负责管理 SQL Server BDC 群集的静态加密功能集的密钥和证书。 这是一种支持以下功能的服务：

* 安全地管理和存储用于静态加密的密钥和证书。
* Hadoop KMS 兼容性。 它作为 BDC 上 HDFS 组件的密钥管理服务。
* SQL Server TDE 证书管理。

暂不支持以下功能：
* 密钥版本控制支持。 

在本文档的其余部分中，我们将此服务称为“BDC KMS”。 “BDC”一词也用来指 SQL Server 大数据群集计算平台。

### <a name="system-managed-keys-and-certificates"></a>系统管理的密钥和证书

BDC KMS 服务会管理 SQL Server 和 HDFS 的所有密钥和证书。

### <a name="user-provided-certificates"></a>用户提供的证书

用户提供的密钥和证书由 BDC KMS 管理，通常称为“自带密钥 (BYOK)”。

### <a name="external-providers"></a>外部提供程序

与 BDC KMS 兼容的外部密钥解决方案，用于外部委派。 暂不支持此功能。

## <a name="encryption-at-rest-on-sql-server-big-data-clusters-cu8"></a>SQL Server 大数据群集 CU8 的静态加密

SQL Server 大数据群集 CU8 是静态加密功能集的初始版本。 请仔细阅读本文档，以全面评估你的方案。

此功能集引入了 BDC KMS 控制器服务，为 SQL Server 和 HDFS 上的静态数据加密提供了系统管理的密钥和证书。 这些密钥和证书是由服务管理的，本文档提供了关于如何与服务交互的操作指南。

* SQL Server 实例利用已建立的[透明数据加密 (TDE)](../relational-databases/security/encryption/transparent-data-encryption.md) 功能。
* HDFS 在每个 Pod 中使用原生 Hadoop KMS 与控制器上的 BDC KMS 交互。 这启用了 HDFS 加密区域，这些区域在 HDFS 上提供安全路径。

### <a name="sql-server-instances"></a>SQL Server 实例

* 系统生成的证书将安装在 SQL Server Pod 上，以便与 TDE 命令一起使用。 系统管理的证书命名标准是 `TDECertificate` + `timestamp`。 例如 `TDECertificate2020_09_15_22_46_27`。
* 主实例 BDC 预配数据库和用户数据库不会自动加密。 DBA 可以使用已安装的证书对任何数据库进行加密。
* 计算池和存储池使用系统生成的证书进行自动加密。
* 尽管技术上可以使用 T-SQL `EXECUTE AT` 命令进行数据池加密，但目前不鼓励也不支持这样做。 使用这种技术对数据池数据库进行加密可能不是有效的，可能无法实现所需状态的加密。 它还创建了升级到后续版本的不兼容升级路径。
* 暂时没有证书轮换。 如果不是在 HA 部署上，则支持使用 T-SQL 命令来解密，然后使用新证书进行加密。
* 加密监视是通过适用于 TDE 的现有标准 SQL Server DMV 进行的。
* 支持将启用了 TDE 的数据库备份和还原到群集中。
* 支持 HA。 如果 SQL Server 的主实例上的数据库已加密，则此数据库的所有次要副本也会被加密。

### <a name="hdfs-encryption-zones"></a>HDFS 加密区域

* 必须有 [Active Directory 集成](active-directory-prerequisites.md)，才能启用 HDFS 的加密区域功能。
* 系统生成的密钥会在 Hadoop KMS 中进行预配。 密钥名称为 `securelakekey`。 在 CU8 上，默认密钥为 256 位，我们支持 256 位 AES 加密。
* 将使用以上系统生成的密钥在名为 `/securelake` 的路径上对默认加密区域进行预配。
* 用户可以按照本指南中提供的特定说明操作，创建其他密钥和加密区域。 用户可以创建密钥期间选择 128、192 或 256 作为密钥大小。
* 在 CU8 中，无法使用 HDFS 的就地密钥轮换。 作为一种替代方法，可以使用 DistCp 将数据从一个加密区域移动到另一个加密区域。
* 不支持在加密区域的顶层执行 HDFS 分层装载。

## <a name="configuration-guide"></a>配置指南

SQL Server 大数据群集静态加密是一种服务管理的功能，可能需要执行额外步骤，具体视你的部署路径而定。

在 SQL Server 大数据群集的新部署期间，自 CU8 起，静态加密将默认启用和配置。 也就是说：

* BDC KMS 组件将部署在控制器中，并将生成一组默认的密钥和证书。
* SQL Server 将在启用 TDE 的情况下部署，证书将由控制器安装。
* Hadoop KMS（适用于 HDFS）将被配置为与 BDC KMS 交互，以执行加密操作。 HDFS 加密区域已经配置好，可供使用。

上一部分中所述的要求和默认行为都适用。

若要将群集升级到 CU8，请仔细阅读下一部分。

### <a name="upgrading-to-cu8"></a>升级到 CU8

   > [!CAUTION]
   > 在升级到 SQL Server 大数据群集 CU8 之前，请对数据执行完整备份。

在现有群集上，升级过程不会对用户数据进行强制加密，也不会配置 HDFS 加密区域。 此行为是专门设计的，每个组件需要考虑以下注意事项：

* __SQL Server__

    1. SQL Server 主实例。 虽然升级过程不会影响任何主实例数据库和已安装的 TDE 证书，但强烈建议在升级过程之前备份你的数据库和手动安装的 TDE 证书。 还建议将这些项目存储在 SQL Server BDC 群集之外。
    1. 计算池和存储池。 这些数据库是系统管理的，具有不稳定性，将在群集升级后重新创建并自动加密。
    1. 数据池。 升级不会影响数据池的 SQL Server 实例部分中的数据库。

* __HDFS__

    1. HDFS。 升级过程不会触及加密区域以外的 HDFS 文件和文件夹。
    1. 不会配置加密区域。 不会将 Hadoop KMS 组件配置为使用 BDC KMS。 若要在升级后配置和启用 HDFS 加密区域功能，请按照下一部分中的步骤操作。

### <a name="enable-hdfs-encryption-zones-after-upgrade"></a>升级后启用 HDFS 加密区域

如果你已将群集升级到 CU8 (`azdata upgrade`)，并且想要启用 HDFS 加密区域，请执行以下操作。

要求：

- [Active Directory](active-directory-prerequisites.md) 集成群集。

- 在 AD 模式下配置并记录到群集中的 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]。

若要重新配置具有加密区域支持的群集，请遵循以下过程。

1. 使用 `azdata` 执行升级后，保存以下脚本。

    脚本执行要求：
        
    * 这两个脚本应位于同一个目录中。 
    * `PATH 上的 `kubectl`
    * 文件夹 ```$HOME/.kube``` 中用于 Kubernetes 的 ```config``` 文件
    
    应将此脚本命名为 ```run-key-provider-patch.sh```：

    ```console
    #!/bin/bash
    
    if [[ -z "${BDC_DOMAIN}" ]]; then
      echo "BDC_DOMAIN environment variable with the domain name of the cluster is not defined."
      exit 1
    fi
    
    if [[ -z "${BDC_CLUSTER_NS}" ]]; then
      echo "BDC_CLUSTER_NS environment variable with the cluster namespace is not defined."
      exit 1
    fi
    
    kubectl get configmaps -n test
    
    diff <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py) <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | python -m json.tool)
    
    diff <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py) <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | python -m json.tool)
    
    # Replace the config maps.
    #
    kubectl replace -n $BDC_CLUSTER_NS -o json -f <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py)
    
    kubectl replace -n $BDC_CLUSTER_NS -o json -f <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py)
    
    # Restart the pods which need to have the necessary changes with the core-site.xml
    kubectl delete pods -n $BDC_CLUSTER_NS nmnode-0-0
    kubectl delete pods -n $BDC_CLUSTER_NS storage-0-0
    kubectl delete pods -n $BDC_CLUSTER_NS storage-0-1
    
    # Wait for sometime for pods to restart
    #
    sleep 300
    
    # Check for the KMS process status.
    #
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop nmnode-0-0 -- bash -c 'ps aux | grep kms'
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop storage-0-0 -- bash -c 'ps aux | grep kms'
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop storage-0-1 -- bash -c 'ps aux | grep kms'
    ```

    应将此脚本命名为 ```updatekeyprovider.py```：

    ```python
    #!/usr/bin/env python3
    
    import json
    import re
    import sys
    import xml.etree.ElementTree as ET
    import os
    
    class CommentedTreeBuilder(ET.TreeBuilder):
        def comment(self, data):
            self.start(ET.Comment, {})
            self.data(data)
            self.end(ET.Comment)
    
    domain_name = os.environ['BDC_DOMAIN']
    
    parser = ET.XMLParser(target=CommentedTreeBuilder())
    
    core_site = 'core-site.xml'
    j = json.load(sys.stdin)
    cs = j['data'][core_site]
    csxml = ET.fromstring(cs, parser=parser)
    props = [prop.find('value').text for prop in csxml.findall(
        "./property/name/..[name='hadoop.security.key.provider.path']")]
    
    kms_provider_path=''
    
    for x in range(5):
        if len(kms_provider_path) != 0:
            kms_provider_path = kms_provider_path + ';'
        kms_provider_path = kms_provider_path + 'nmnode-0-0.' + domain_name
    
    if len(props) == 0:
        prop = ET.SubElement(csxml, 'property')
        name = ET.SubElement(prop, 'name')
        name.text = 'hadoop.security.key.provider.path'
        value = ET.SubElement(prop, 'value')
        value.text = 'kms://https@' + kms_provider_path + ':9600/kms'
        cs = ET.tostring(csxml, encoding='utf-8').decode('utf-8')
    
    j['data'][core_site] = cs
    
    kms_site = 'kms-site.xml.tmpl'
    ks = j['data'][kms_site]
    
    kp_uri_regex = re.compile('(<name>hadoop.kms.key.provider.uri</name>\s*<value>\s*)(.*)(\s*</value>)', re.MULTILINE)
    
    def replace_uri(match_obj):
        key_provider_uri = 'bdc://https@hdfsvault-svc.' + domain_name
        if match_obj.group(2) == 'jceks://file@/var/run/secrets/keystores/kms/kms.jceks' or match_obj.group(2) == key_provider_uri:
            return match_obj.group(1) + key_provider_uri + match_obj.group(3)
        return match_obj.group(0)
    
    ks = kp_uri_regex.sub(replace_uri, ks)
    
    j['data'][kms_site] = ks
    print(json.dumps(j, indent=4, sort_keys=True))
    ```

    使用适当的参数执行 ```run-key-provider-patch.sh```。 

## <a name="next-steps"></a>后续步骤

若要详细了解如何有效地使用 SQL Server 大数据群集的静态加密，请参阅以下文章：

- [静态加密 - SQL Server TDE](encryption-at-rest-sql-server-tde.md)
- [静态加密 - HDFS 加密区域](encryption-at-rest-hdfs-encryption-zones.md)

若要了解有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅以下概述：

- [什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)
