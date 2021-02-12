---
title: Active Directory 对象
titleSuffix: SQL Server Big Data Cluster
description: 介绍针对 SQL Server 大数据群集创建的 Active Directory 对象。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 347129c6627bce865ffd4962a9cf2cb3b0beb862
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039778"
---
# <a name="auto-generated-active-directory-objects"></a>自动生成的 Active Directory 对象

本文介绍了在大数据群集 (BDC) 部署期间 SQL Server 创建的 Active Directory (AD) 帐户和组。

## <a name="accounts--groups"></a>帐户和组

用户帐户和组在群集部署过程中提供的[组织单位 (OU)](/windows-server/identity/ad-ds/plan/reviewing-ou-design-concepts) 中生成。

每个帐户表示 BDC 中的一个服务。 帐户拥有每个服务所需的服务主体名称 (SPN)。 可使用 [setspn](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spn-setspn-syntax.aspx) 命令列出每个帐户拥有的 SPN。

部署会自动生成帐户名和组名。 从 SQL Server 2019 CU5 开始，帐户名或组名前缀是部署命名空间名称（BDC 群集名称）。 如果本文中项的集群名称是 `bdc`，请将 `<prefix>` 替换为 `bdc` 来标识帐户。

Pod 后缀 (-x) 表示下面的变量 Pod ID。 下面的名称不包括在部署过程中用户提供的变量前缀。

经典帐户名适用于使用 SQL Server 2019 CU5 之前版本的部署，以及在安全配置中将“useSubdomain”选项设置为 false 的部署。

| 帐户名或组名|详细信息|
|----|---|
|`<prefix>-ctrl`|[控制器服务帐户](#controller-service-account)|
|`<prefix>-ngxm`|[监视服务代理服务帐户](#monitoring-service-proxy-service-account)|
|`<prefix>-ldap`|[LDAP 查找用户](#ldap-lookup-user)|
|`<prefix>-sqc0-x/<prefix>-sqc0`|[计算池 SQL Server 用户](#compute-pool-sql-server-user)|
|`<prefix>-dmc0-x`|[计算池数据仓库 DMS 用户](#compute-pool-data-warehouse-dms-user)|
|`<prefix>-dec0-x`|[计算池数据仓库引擎用户](#compute-pool-data-warehouse-engine-user)|
|`<prefix>-sqd0`|[数据池 SQL Server 用户](#data-pool-sql-server-user)|
|`<prefix>-sqs0`|[存储池 SQL Server 用户](#storage-pool-sql-server-user)|
|`<prefix>-ynt0-x`|[存储池 Yarn 节点管理器服务用户](#storage-pool-yarn-node-manager-service-user)|
|`<prefix>-htt0`|[存储池 HTTP 服务用户](#storage-pool-http-service-user)|
|`<prefix>-hdt0`|[存储池 HDFS 数据节点服务用户](#storage-pool-hdfs-datanode-service-user)|
|`<prefix>-hdnn`|[HDFS 名称节点服务用户](#hdfs-name-node-service-user)|
|`<prefix>-htnn`|[HDFS 名称节点 HTTP 服务用户](#hdfs-name-node-http-service-user)|
|`<prefix>-kmnn-x`|[名称节点 KMS 服务用户](#name-node-kms-service-user)|
|`<prefix>-jnzk-x`|[Zookeeper JournalNode 服务用户](#zookeeper-journalnode-service-users)|
|`<prefix>-htzk`|[Zookeeper HTTP 服务用户](#zookeeper-http-service-user)|
|`<prefix>-yrsh-x`|[Sparkhead Yarn 资源管理器服务用户](#sparkhead-yarn-resource-manager-service-user)|
|`<prefix>-htsh`|[Sparkhead HTTP 用户](#sparkhead-http-user)|
|`<prefix>-shsh-x`|[Sparkhead Spark 历史记录服务用户](#sparkhead-spark-history-service-user)|
|`<prefix>-lvsh-x`|[Sparkhead Livy 服务用户](#sparkhead-livy-service-user)|
|`<prefix>-hvsh-x`|[Sparkhead Hive 服务用户](#sparkhead-hive-service-user)|
|`<prefix>-yns0-x`|[Spark 池 Yarn 节点管理器服务用户](#spark-pool-yarn-node-manager-service-user)|
|`<prefix>-hts0`|[Spark 池 Yarn 节点管理器 HTTP 用户](#spark-pool-yarn-node-manager-http-user)|
|`<prefix>-knox-x`|[Knox 网关用户](#knox-gateway-user)|
|`<prefix>-htgw`|[Knox 网关 HTTP 用户](#knox-gateway-http-user)|
|`<prefix>-apst`|[应用设置用户](#app-setup-user)|
|`<prefix>-dmsvc`|[数据仓库 DMS 服务组](#data-warehouse-dms-service-group)|
|`<prefix>-desvc`|[数据仓库引擎服务组](#data-warehouse-engine-service-group)|

以下部分提供了有关每个帐户的更多详细信息。 有关组的信息，请跳到[组](#groups)。

## <a name="controller-management--ldap-related-accounts"></a>控制器、管理和 LDAP 相关帐户

### <a name="controller-service-account"></a>控制器服务帐户

|对象|帐户名|
|---|---|
|规模集名称|`control`|
|Pod 名称|`control-x`|
|容器名称|`controller`|
|Service name|`controller`|
|帐户名（无前缀）| `ctrl`|
|帐户（带有命名空间前缀）|`<prefix>-ctrl`|
|经典帐户名|`ctrl-controller`|

### <a name="monitoring-service-proxy-service-account"></a>监视服务代理服务帐户

|对象|帐户名|
|---|---|
|规模集名称|`mgmtproxy`|
|Pod 名称|`mgmtproxy-x`|
|容器名称|`service-proxy`|
|Service name|`nginx`|
|帐户（无前缀）| `ngxm`|
|帐户（带有命名空间前缀）|`<prefix>-ngxm`|
|经典帐户名|`nginx-mgmtproxy`|

### <a name="ldap-lookup-user"></a>LDAP 查找用户

供 grafana 和 hadoop 服务用于通过 LDAP 查找用户。

|对象|帐户名|
|---|---|
|规模集名称|`metricsui`|
|Pod 名称|`metricsui-x`|
|容器名称|`grafana`|
|Service name|`grafana`|
|帐户名（无前缀）| `ldap`|
|帐户名（带有命名空间前缀）|`<prefix>-ldap`|
|经典帐户名|`ldap-user`|

## <a name="master-pool-accounts"></a>主池帐户

### <a name="master-pool-sql-server-user"></a>主池 SQL Server 用户

|对象|帐户名|
|---|---|
|规模集名称|`master`|
|Pod 名称|`master-x`|
|容器名称|`mssql-server`|
|Service name|`mssql`|
|帐户名（无前缀）| `sqmp-x/sqmp`|
|帐户名（带有命名空间前缀）|`<prefix>-sqmp-x/<prefix>-sqmp`|
|经典帐户名|`mssql-master-x`|

### <a name="master-pool-data-warehouse-dms-user"></a>主池数据仓库 DMS 用户

|对象|帐户名|
|---|---|
|规模集名称|`master`|
|Pod 名称|`master-x`|
|容器名称|`mssql-server`|
|Service name|`dwdms`|
|帐户（无前缀）| `dmmp-x`|
|帐户（带有命名空间前缀）|`<prefix>-dmmp-x`|
|经典帐户名|`dwdms-master-x`|

### <a name="master-pool-data-warehouse-engine-user"></a>主池数据仓库引擎用户

|对象|帐户名|
|---|---|
|规模集名称|`master`|
|Pod 名称|`master-x`|
|容器名称|`mssql-server`|
|Service name|`dweng`|
|帐户（无前缀）| `demp`|
|帐户（带有命名空间前缀）|`<prefix>-demp-x`|
|经典帐户名|`dweng-master-x`|

## <a name="compute-pool-accounts"></a>计算池帐户

### <a name="compute-pool-sql-server-user"></a>计算池 SQL Server 用户

|对象|帐户名|
|---|---|
|规模集名称|`compute-0`|
|Pod 名称|`compute-0-x`|
|容器名称|`mssql-server`|
|Service name|`mssql`|
|帐户（无前缀）| `sqc0-x/sqlc0`|
|帐户（带有命名空间前缀）|`<prefix>-sqc0-x/<prefix>-sqc0`|
|经典帐户名|`mssql-compute-0-x`|

### <a name="compute-pool-data-warehouse-dms-user"></a>计算池数据仓库 DMS 用户

|对象|帐户名|
|---|---|
|规模集名称|`compute-0`|
|Pod 名称|`compute-0-x`|
|容器名称|`mssql-server`|
|Service name|`dwdms`|
|帐户（无前缀）| `dmc0-x`|
|帐户（带有命名空间前缀）|`<prefix>-dmc0-x`|
|经典帐户名|`dwdms-compute-0-x`|

### <a name="compute-pool-data-warehouse-engine-user"></a>计算池数据仓库引擎用户

|对象|帐户名|
|---|---|
|规模集名称|`compute-0`|
|Pod 名称|`compute-0-x`|
|容器名称|`mssql-server`|
|Service name|`dweng`|
|帐户（无前缀）| `dec0-x`|
|帐户（带有命名空间前缀）|`<prefix>-dec0-x`|
|经典帐户名|`dweng-compute-0-x`|

## <a name="data-pool-accounts"></a>数据池帐户

### <a name="data-pool-sql-server-user"></a>数据池 SQL Server 用户

|对象|帐户名|
|---|---|
|规模集名称|`data-0`|
|Pod 名称|`data-0-x`|
|容器名称|`mssql-server`|
|Service name|`mssql`|
|帐户（无前缀）| `sqd0`|
|帐户（带有命名空间前缀）|`<prefix>-sqd0`|
|经典帐户名|`mssql-data-0`|

## <a name="storage-pool-accounts"></a>存储池帐户

### <a name="storage-pool-sql-server-user"></a>存储池 SQL Server 用户

|对象|帐户名|
|---|---|
|规模集名称|`storage-0`|
|Pod 名称|`storage-0-x`|
|容器名称|`mssql-server`|
|Service name|`mssql`|
|帐户（无前缀）| `sqs0`|
|帐户（带有命名空间前缀）|`<prefix>-sqs0`|
|经典帐户名|`mssql-storage-0`|

### <a name="storage-pool-yarn-node-manager-service-user"></a>存储池 Yarn 节点管理器服务用户

|对象|帐户名|
|---|---|
|规模集名称|`storage-0`|
|Pod 名称|`storage-0-x`|
|容器名称|`hadoop`|
|Service name|`Yarn Node Manager`|
|帐户（无前缀）| `ynt0-x`|
|帐户（带有命名空间前缀）|`<prefix>-ynt0-x`|
|经典帐户名|`yarnnm-storage-0-x`|

### <a name="storage-pool-http-service-user"></a>存储池 HTTP 服务用户

|对象|帐户名|
|---|---|
|规模集名称|`storage-0`|
|Pod 名称|`storage-0-x`|
|容器名称|`hadoop`|
|Service name|`HDFS Datanode`|
|帐户（无前缀）| `hdt0`|
|帐户（带有命名空间前缀）|`<prefix>-hdt0`|
|经典帐户名|`http-storage-0`|

### <a name="storage-pool-hdfs-datanode-service-user"></a>存储池 HDFS 数据节点服务用户

|对象|帐户名|
|---|---|
|规模集名称|`storage-0`|
|Pod 名称|`storage-0-x`|
|容器名称|`hadoop`|
|Service name|`HDFS Datanode`|
|帐户（无前缀）| `hdt0`|
|帐户（带有命名空间前缀）|`<prefix>-hdt0`|
|经典帐户名|`hdfsdn-storage-0`|

## <a name="hdfs-accounts"></a>HDFS 帐户

### <a name="hdfs-name-node-service-user"></a>HDFS 名称节点服务用户

|对象|帐户名|
|---|---|
|规模集名称|`nmnode-0`|
|Pod 名称|`nmnode-0-x`|
|容器名称|`hadoop`|
|Service name|`HDFS Namenode`|
|帐户（无前缀）| `hdnn`|
|帐户（带有命名空间前缀）|`<prefix>-hdnn`|
|经典帐户名|`hdfsnn-nmnode`|

### <a name="hdfs-name-node-http-service-user"></a>HDFS 名称节点 HTTP 服务用户

|对象|帐户名|
|---|---|
|规模集名称|`nmnode-0`|
|Pod 名称|`nmnode-0-x`|
|容器名称|`hadoop`|
|Service name|`HDFS Namenode`|
|帐户（无前缀）| `htnn`|
|帐户（带有命名空间前缀）|`<prefix>-htnn`|
|经典帐户名|`http-nmnode`|

## <a name="kms-accounts"></a>KMS 帐户

### <a name="name-node-kms-service-user"></a>名称节点 KMS 服务用户

|对象|帐户名|
|---|---|
|规模集名称|`nmnode-0`|
|Pod 名称|`nmnode-0-x`|
|容器名称|`hadoop`|
|Service name|`KMS`|
|帐户（无前缀）| `kmnn-x`|
|帐户（带有命名空间前缀）|`<prefix>-kmnn-x`|
|经典帐户名|`kms-nmnode-x`|

## <a name="zookeeper-accounts"></a>Zookeeper 帐户

### <a name="zookeeper-journalnode-service-users"></a>Zookeeper JournalNode 服务用户

|对象|帐户名|
|---|---|
|规模集名称|`zookeeper`|
|Pod 名称|`zookeeper-x`|
|容器名称|`zookeeper`|
|Service name|`Journal node`|
|帐户（无前缀）| `jnzk-x`|
|帐户（带有命名空间前缀）|`<prefix>-jnzk-x`|
|经典帐户名|`jn-zookeeper-x`|

### <a name="zookeeper-http-service-user"></a>Zookeeper HTTP 服务用户

|对象|帐户名|
|---|---|
|规模集名称|`zookeeper`|
|Pod 名称|`zookeeper-x`|
|容器名称|`zookeeper`|
|Service name|`Zookeeper`|
|帐户（无前缀）| `htzk`|
|帐户（带有命名空间前缀）|`<prefix>-htzk`|
|经典帐户名|`http-zookeeper`|

## <a name="spark-related-accounts"></a>Spark 相关帐户

### <a name="sparkhead-yarn-resource-manager-service-user"></a>Sparkhead Yarn 资源管理器服务用户

|对象|帐户名|
|---|---|
|规模集名称|`sparkhead`|
|Pod 名称|`sparkhead-x`|
|容器名称|`hadoop-yarn-jobhistory`|
|Service name|`Yarn Resource Manager`|
|帐户（无前缀）| `yrsh-x`|
|帐户（带有命名空间前缀）|`<prefix>-yrsh-x`|
|经典帐户名|`yarnrm-sparkhead-x`|

### <a name="sparkhead-http-user"></a>Sparkhead HTTP 用户

|对象|帐户名|
|---|---|
|规模集名称|`sparkhead`|
|Pod 名称|`sparkhead-x`|
|容器名称|`*`|
|Service name|`*`|
|帐户（无前缀）| `htsh`|
|帐户（带有命名空间前缀）|`<prefix>-htsh`|
|经典帐户名|`http-sparkhead`|

### <a name="sparkhead-spark-history-service-user"></a>Sparkhead Spark 历史记录服务用户

|对象|帐户名|
|---|---|
|规模集名称|`sparkhead`|
|Pod 名称|`sparkhead-x`|
|容器名称|`hadoop-livy-sparkhistory`|
|Service name|`Spark History Server`|
|帐户（无前缀）| `shsh-x`|
|帐户（带有命名空间前缀）|`<prefix>-shsh-x`|
|经典帐户名|`sph-sparkhead-x`|

### <a name="sparkhead-livy-service-user"></a>Sparkhead Livy 服务用户

|对象|帐户名|
|---|---|
|规模集名称|`sparkhead`|
|Pod 名称|`sparkhead-x`|
|容器名称|`hadoop-livy-sparkhistory`|
|Service name|`Livy`|
|帐户（无前缀）| `lvsh-x`|
|帐户（带有命名空间前缀）|`<prefix>-lvsh-x`|
|经典帐户名|`livy-sparkhead-x`|

### <a name="sparkhead-hive-service-user"></a>Sparkhead Hive 服务用户

|对象|帐户名|
|---|---|
|规模集名称|`sparkhead`|
|Pod 名称|`sparkhead-x`|
|容器名称|`hadoop-hivemetastore`|
|Service name|`Hive Metastore`|
|帐户（无前缀）| `hvsh-x`|
|帐户（带有命名空间前缀）|`<prefix>-hvsh-x`|
|经典帐户名|`hive-sparkhead-x`|

### <a name="spark-pool-yarn-node-manager-service-user"></a>Spark 池 Yarn 节点管理器服务用户

|对象|帐户名|
|---|---|
|规模集名称|`spark-0`|
|Pod 名称|`spark-0-x`|
|容器名称|`hadoop`|
|Service name|`Yarn Node Manager`|
|帐户（无前缀）| `yns0-x`|
|帐户（带有命名空间前缀）|`<prefix>-yns0-x`|
|经典帐户名|`yarnnm-spark-0-x`|

### <a name="spark-pool-yarn-node-manager-http-user"></a>Spark 池 Yarn 节点管理器 HTTP 用户

|对象|帐户名|
|---|---|
|规模集名称|`spark-0`|
|Pod 名称|`spark-0-x`|
|容器名称|`hadoop`|
|Service name|`Yarn Node Manager`|
|帐户（无前缀）| `hts0`|
|帐户（带有命名空间前缀）|`<prefix>-hts0`|
|经典帐户名|`http-spark-0`|

## <a name="knox-accounts"></a>Knox 帐户

### <a name="knox-gateway-user"></a>Knox 网关用户

|对象|帐户名|
|---|---|
|规模集名称|`gateway`|
|Pod 名称|`gateway-x`|
|容器名称|`knox`|
|Service name|`Knox`|
|帐户（无前缀）| `knox-x`|
|帐户（带有命名空间前缀）|`<prefix>-knox-x`|
|经典帐户名|`knox-gateway-x`|

### <a name="knox-gateway-http-user"></a>Knox 网关 HTTP 用户

|对象|帐户名|
|---|---|
|规模集名称|`gateway`|
|Pod 名称|`gateway-x`|
|容器名称|`knox`|
|Service name|`Knox`|
|帐户（无前缀）| `htgw`|
|帐户（带有命名空间前缀）|`<prefix>-htgw`|
|经典帐户名|`http-gateway`|

## <a name="app-accounts"></a>应用帐户

### <a name="app-setup-user"></a>应用设置用户

|对象|帐户名|
|---|---|
|规模集名称|`appproxy`|
|Pod 名称|`appproxy-x`|
|容器名称|`App Service Proxy`|
|Service name|`nginx`|
|帐户（无前缀）| `apst`|
|帐户（带有命名空间前缀）|`<prefix>-apst`|
|经典帐户名|`app-setup`|

## <a name="groups"></a>组

以下组是在用户提供的 OU 中创建的。 这些组的成员是上面为相应服务创建的用户。

### <a name="data-warehouse-dms-service-group"></a>数据仓库 DMS 服务组

|对象|组名称|
|---|---|
|规模集名称|`master/compute-0`|
|Pod 名称|`master-x/compute-0-x`|
|容器名称|`mssql-server`|
|Service name|`dwdms`|
|组（无前缀）| `dmsvc`|
|帐户（带有命名空间前缀）|`<prefix>-dmsvc`|
|经典帐户名|`dwdms-service`|

### <a name="data-warehouse-engine-service-group"></a>数据仓库引擎服务组

|对象|组名称|
|---|---|
|规模集名称|`master/compute-0`|
|Pod 名称|`master-x/compute-0-x`|
|容器名称|`mssql-server`|
|Service name|`dweng`|
|组（无前缀）| `desvc`|
|帐户（带有命名空间前缀）|`<prefix>-desvc`|
|经典帐户名|`desvc`|

## <a name="next-steps"></a>后续步骤

[在 Active Directory 模式下部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](active-directory-deploy.md)

[在同一 Active Directory 域中部署多个 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](active-directory-deployment-background.md)
