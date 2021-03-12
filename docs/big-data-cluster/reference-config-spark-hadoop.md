---
title: Apache Spark 和 Apache Hadoop 配置属性
titleSuffix: SQL Server big data clusters
description: 有关 Apache Spark 和 Apache Hadoop (HDFS) 的配置属性的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 08/04/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c6a73d8cf4a110990260db4917d565c33bd59766
ms.sourcegitcommit: 765262cdc6352a5325148afc22fa4f1499fe1aa3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514884"
---
# <a name="apache-spark--apache-hadoop-hdfs-configuration-properties"></a>Apache Spark 和 Apache Hadoop (HDFS) 配置属性

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

大数据群集支持 Apache Spark 和 Hadoop 组件在服务和资源范围内的部署时间和部署后时间配置。 大数据群集在大多数设置中使用与各自的开源项目相同的默认配置值。 下面列出了我们更改的设置，并提供了说明及其默认值。 除了网关资源之外，在服务范围和资源范围内可配置的设置之间没有区别。

可以在关联的 Apache 文档网站上找到每个配置的所有可行配置和默认值：
- Apache Spark： https://spark.apache.org/docs/latest/configuration.html
- Apache Hadoop：
  - HDFS HDFS 站点： https://hadoop.apache.org/docs/r2.7.1/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml
  - HDFS 核心站点： https://hadoop.apache.org/docs/r2.8.0/hadoop-project-dist/hadoop-common/core-default.xml  
  - Yarn： https://hadoop.apache.org/docs/r3.1.1/hadoop-yarn/hadoop-yarn-site/ResourceModel.html
- Hive： https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties#ConfigurationProperties-MetaStore
- Livy： https://github.com/cloudera/livy/blob/master/conf/livy.conf.template
- Apache Knox 网关： https://knox.apache.org/books/knox-0-14-0/user-guide.html#Gateway+Details

下面还列出了不支持配置的设置。

> [!NOTE]
> 若要在存储池中包含 Spark，请在 `spec.resources.storage-0.spec.settings.spark` 的 `bdc.json` 配置文件中设置布尔值 `includeSpark`。 有关说明，请参阅[在大数据群集中配置 Apache Spark 和 Apache Hadoop](configure-spark-hdfs.md)。


##  <a name="big-data-clusters-specific-default-spark-settings"></a>大数据群集特定的默认 Spark 配置
以下 Spark 设置是具有 BDC 特定默认值但用户可配置的设置。 不包含系统管理的设置。

| 设置名称                                                                                 | 说明                                                                                                                                                           | 类型   | 默认值                                                                                                                              |
| ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| capacity-scheduler.yarn.scheduler.capacity.maximum-applications                      | 系统中可同时处于运行状态和挂起状态的应用程序的最大数目。                                                               | int    | 10000                                                                                                                                      |
| capacity-scheduler.yarn.scheduler.capacity.resource-calculator                       | 用于比较计划程序中资源的 ResourceCalculator 实现。                                                                               | 字符串 | org.apache.hadoop.yarn.util.resource.DominantResourceCalculator                                                                            |
| capacity-scheduler.yarn.scheduler.capacity.root.queues                               | 具有预定义队列调用根的容量计划程序。                                                                                                              | 字符串 | default                                                                                                                                    |
| capacity-scheduler.yarn.scheduler.capacity.root.default.capacity                     | 作为根队列的绝对资源队列最小容量的队列容量百分比 (% )。                                                                          | int    | 100                                                                                                                                        |
| spark-defaults-conf.spark.driver.cores                                               | 要用于驱动程序进程的核心数（仅限群集模式）。                                                                                                  | int    | 1                                                                                                                                          |
| spark-defaults-conf.spark.driver.memoryOverhead                                      | 要在群集模式下为每个驱动程序分配的离堆内存量。                                                                                             | int    | 384                                                                                                                                        |
| spark-defaults-conf.spark.executor.instances                                         | 静态分配的执行程序的数目。                                                                                                                        | int    | 1                                                                                                                                          |
| spark-defaults-conf.spark.executor.cores                                             | 每个执行程序上要使用的核心数。                                                                                                                          | int    | 1                                                                                                                                          |
| spark-defaults-conf.spark.driver.memory                                              | 用于驱动程序进程的内存量。                                                                                                                       | 字符串 | 1g                                                                                                                                         |
| spark-defaults-conf.spark.executor.memory                                            | 每个执行程序进程要使用的内存量。                                                                                                                         | 字符串 | 1g                                                                                                                                         |
| spark-defaults-conf.spark.executor.memoryOverhead                                    | 要为每个执行程序分配的离堆内存量。                                                                                                           | int    | 384                                                                                                                                        |
| yarn-site.yarn.nodemanager.resource.memory-mb                                        | 可为容器分配的物理内存量（以 MB 为单位）。                                                                                               | int    | 8192                                                                                                                                       |
| yarn-site.yarn.scheduler.maximum-allocation-mb                                       | 资源管理器上每个容器请求的分配上限。                                                                                           | int    | 8192                                                                                                                                       |
| yarn-site.yarn.nodemanager.resource.cpu-vcores                                       | 可为容器分配的 CPU 核心数。                                                                                                             | int    | 32                                                                                                                                         |
| yarn-site.yarn.scheduler.maximum-allocation-vcores                                   | 就虚拟 CPU 核心而言，资源管理器上每个容器请求的分配上限。                                                            | int    | 8                                                                                                                                          |
| yarn-site.yarn.nodemanager.linux-container-executor.secure-mode.pool-user-count      | 在安全模式下，Linux 容器执行程序的池用户数。                                                                                             | int    | 6                                                                                                                                          |
| yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent                        | 可用于运行应用程序主机的群集中资源的最大百分比。                                                                              | FLOAT  | 0.1                                                                                                                                        |
| yarn-site.yarn.nodemanager.container-executor.class                                  | 特定操作系统的容器执行程序。                                                                                                               | 字符串 | org.apache.hadoop.yarn.server.nodemanager.LinuxContainerExecutor                                                                           |
| capacity-scheduler.yarn.scheduler.capacity.root.default.user-limit-factor            | 队列容量的倍数，可配置为允许单个用户获取更多资源。                                                          | int    | 1                                                                                                                                          |
| capacity-scheduler.yarn.scheduler.capacity.root.default.maximum-capacity             | 作为浮点数或绝对资源队列最大容量的最大队列容量（以百分比 (%) 表示）。 将此值设置为 -1 会将最大容量设置为 100%。           | int    | 100                                                                                                                                        |
| capacity-scheduler.yarn.scheduler.capacity.root.default.state                        | 队列状态可以是“正在运行”或“已停止”其中一种。                                                                                                                      | 字符串 | RUNNING                                                                                                                                    |
| capacity-scheduler.yarn.scheduler.capacity.root.default.maximum-application-lifetime | 提交到队列的应用程序的最长生存期（以秒为单位）。 小于或等于零的任何值都将被视为已禁用。                     | int    | -1                                                                                                                                         |
| capacity-scheduler.yarn.scheduler.capacity.root.default.default-application-lifetime | 提交到队列的应用程序的默认生存期（以秒为单位）。 小于或等于零的任何值都将被视为已禁用。                     | int    | -1                                                                                                                                         |
| capacity-scheduler.yarn.scheduler.capacity.node-locality-delay                       | 错过计划机会的数目，之后 CapacityScheduler 将尝试计划 rack-local 容器。                                               | int    | 40                                                                                                                                         |
| capacity-scheduler.yarn.scheduler.capacity.rack-locality-additional-delay            | 除了 node-locality-delay 之外，其他错过的计划机会的数目，之后 CapacityScheduler 将尝试计划 off-switch 容器。 | int    | -1                                                                                                                                         |
| hadoop-env.HADOOP_HEAPSIZE_MAX                                                       | 所有 Hadoop JVM 进程的默认堆大小上限。                                                                                                                 | int    | 2048                                                                                                                                       |
| yarn-env.YARN_RESOURCEMANAGER_HEAPSIZE                                               | Yarn ResourceManager 的堆大小。                                                                                                                                     | int    | 2048                                                                                                                                       |
| yarn-env.YARN_NODEMANAGER_HEAPSIZE                                                   | Yarn NodeManager 的堆大小。                                                                                                                                         | int    | 2048                                                                                                                                       |
| mapred-env.HADOOP_JOB_HISTORYSERVER_HEAPSIZE                                         | Hadoop 作业 HistoryServer 的堆大小。                                                                                                                                 | int    | 2048                                                                                                                                       |
| hive-env.HADOOP_HEAPSIZE                                                             | Hadoop for Hive 的堆大小。                                                                                                                                          | int    | 2048                                                                                                                                       |
| livy-conf.livy.server.session.timeout-check                                          | 检查 Livy 服务器会话是否超时。                                                                                                                                | bool   | 是                                                                                                                                       |
| livy-conf.livy.server.session.timeout-check.skip-busy                                | Skip-busy 用于检查 Livy 服务器会话是否超时。                                                                                                                  | bool   | 是                                                                                                                                       |
| livy-conf.livy.server.session.timeout                                                | Livy 服务器会话的超时时间（以毫秒/秒/ | 分钟/小时/天/年为单位）。                                                                                                              | 字符串 | 2 小时                                                                                                                                         |
| livy-conf.livy.server.yarn.poll-interval                                             | Livy 服务器中 Yarn 的轮询间隔（以毫秒/秒/ | 分钟/小时/天/年为单位）。                                                                                                     | 字符串 | 500 毫秒                                                                                                                                      |
| livy-conf.livy.rsc.jars                                                              | Livy RSC jar。                                                                                                                                                        | 字符串 | local:/opt/livy/rsc-jars/livy-api.jar,local:/opt/livy/rsc-jars/livy-rsc.jar,local:/opt/livy/rsc-jars/netty-all.jar                         |
| livy-conf.livy.repl.jars                                                             | Livy repl jar。                                                                                                                                                       | 字符串 | local:/opt/livy/repl_2.11-jars/livy-core.jar,local:/opt/livy/repl_2.11-jars/livy-repl.jar,local:/opt/livy/repl_2.11-jars/commons-codec.jar |
| livy-conf.livy.rsc.sparkr.package                                                    | Livy RSC SparkR 包。                                                                                                                                              | 字符串 | hdfs:///system/livy/sparkr.zip                                                                                                             |
| livy-env.LIVY_SERVER_JAVA_OPTS                                                       | Livy Server Java 选项。                                                                                                                                             | 字符串 | -Xmx2g                                                                                                                                     |
| spark-defaults-conf.spark.r.backendConnectionTimeout                                 | R 进程在连接到 RBackend 时设置的连接超时时间（以秒为单位）。                                                                                         | int    | 86400                                                                                                                                      |
| spark-defaults-conf.spark.pyspark.python                                             | Spark 的 Python 选项。                                                                                                                                              | 字符串 | /opt/bin/python3                                                                                                                           |
| spark-defaults-conf.spark.yarn.jars                                                  | Yarn jar。                                                                                                                                                            | 字符串 | local:/opt/spark/jars/*                                                                                                                    |
| spark-history-server-conf.spark.history.fs.cleaner.maxAge                            | 作业历史记录文件的最长保留时间，超过该时间后，文件系统历史记录清理程序会将其删除（以毫秒/秒/ | 分钟/小时/天/年为单位）。                                                   | 字符串 | 7d                                                                                                                                         |
| spark-history-server-conf.spark.history.fs.cleaner.interval                          | Spark 历史记录的清理程序间隔（以毫秒/秒/ | 分钟/小时/天/年为单位）。                                                                                                        | 字符串 | 12h                                                                                                                                        |
| hadoop-env.HADOOP_CLASSPATH                                                          | 设置其他 Hadoop 类路径。                                                                                                                                 | 字符串 |                                                                                                                                            |
| spark-env.SPARK_DAEMON_MEMORY                                                        | Spark 守护程序内存。                                                                                                                                                  | 字符串 | 2g                                                                                                                                         |
| yarn-site.yarn.log-aggregation.retain-seconds                                        | 启用日志聚合后，此属性将确定保留日志的秒数。                                                                       | int    | 604800                                                                                                                                     |
| yarn-site.yarn.nodemanager.log-aggregation.compression-type                          | 用于 Yarn NodeManager 的日志聚合的压缩类型。                                                                                                            | 字符串 | gz                                                                                                                                         |
| yarn-site.yarn.nodemanager.log-aggregation.roll-monitoring-interval-seconds          | NodeManager 日志聚合中的滚动监视的间隔秒数。                                                                                                  | int    | 3600                                                                                                                                       |
| yarn-site.yarn.scheduler.minimum-allocation-mb                                       | 资源管理器上每个容器请求的分配下限（以 MB 为单位）。                                                                                   | int    | 512                                                                                                                                        |
| yarn-site.yarn.scheduler.minimum-allocation-vcores                                   | 就虚拟 CPU 核心而言，资源管理器上每个容器请求的分配下限。                                                             | int    | 1                                                                                                                                          |
| yarn-site.yarn.nm.liveness-monitor.expiry-interval-ms                                | 在节点管理器被视为处于不活动状态之前要等待的时间。                                                                                                             | int    | 180000                                                                                                                                     |
| yarn-site.yarn.resourcemanager.zk-timeout-ms                                         | “ZooKeeper”会话超时时间（以毫秒为单位）。                                                                                                                          | int    | 40000                                                                                                                                      |
| capacity-scheduler.yarn.scheduler.capacity.root.default.acl_application_max_priority | 可按配置的优先级提交应用程序的用户的 ACL。 例如：[user={name} group={name} max_priority={priority} default_priority={priority}]。             | 字符串 | *                                                                                                                                          |
| includeSpark                                                                         | 用于配置 Spark 作业是否可以在存储池中运行的布尔值。                                                                                           | bool   | 是                                                                                                                                       |
| enableSparkOnK8s                                                                     | 用于配置是否在 K8s 上启用 Spark（在 Spark 头中添加 K8s 容器）的布尔值。                                                               | bool   | false                                                                                                                                      |
| sparkVersion                                                                         | Spark 的版本                                                                                                                                                  | 字符串 | 2.4                                                                                                                                        |
| spark-env.PYSPARK_ARCHIVES_PATH                                                      | Spark 作业中使用的 pyspark 存档 jar 的路径。                                                                                                                      | 字符串 | local:/opt/spark/python/lib/pyspark.zip,local:/opt/spark/python/lib/py4j-0.10.7-src.zip                                                    |

以下部分列出了不支持的配置。

##  <a name="big-data-clusters-specific-default-hdfs-settings"></a>大数据群集特定的默认 HDFS 配置
以下 HDFS 设置是具有 BDC 特定默认值但用户可配置的设置。 不包含系统管理的设置。

| 设置名称                                                                       | 说明                                                                                         | 类型   | 默认值                                                                    |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ------ | -------------------------------------------------------------------------------------- |
| hdfs-site.dfs.replication                                                  | 默认的块复制。                                                                          | int    | 2                                                                                      |
| hdfs-site.dfs.namenode.provided.enabled                                    | 启用名称节点以处理提供的存储。                                                   | bool   | 是                                                                                   |
| hdfs-site.dfs.datanode.provided.enabled                                    | 启用数据节点以处理提供的存储。                                                   | bool   | 是                                                                                   |
| hdfs-site.dfs.datanode.provided.volume.lazy.load                           | 在数据节点中为提供的存储启用延迟加载。                                                 | bool   | 是                                                                                   |
| hdfs-site.dfs.provided.aliasmap.inmemory.enabled                           | 为提供的存储启用内存中别名映射。                                                     | bool   | 是                                                                                   |
| hdfs-site.dfs.provided.aliasmap.class                                      | 用于指定所提供存储上的块的输入格式的类。              | 字符串 | org.apache.hadoop.hdfs.server.common.blockaliasmap.impl.InMemoryLevelDBAliasMapClient  |
| hdfs-site.dfs.namenode.provided.aliasmap.class                             | 用于指定 namenode 的已提供存储上块的输入格式的类。 | 字符串 | org.apache.hadoop.hdfs.server.common.blockaliasmap.impl.NamenodeInMemoryAliasMapClient |
| hdfs-site.dfs.provided.aliasmap.load.retries                               | datanode 上的重试次数，用于加载提供的 aliasmap。                                    | int    | 0                                                                                      |
| hdfs-site.dfs.provided.aliasmap.inmemory.batch-size                        | 遍历支持 aliasmap 的数据库时的批大小。                               | int    | 500                                                                                    |
| hdfs-site.dfs.datanode.provided.volume.readthrough                         | 在 datanode 中为提供的存储启用 readthrough。                                               | bool   | 是                                                                                   |
| hdfs-site.dfs.provided.cache.capacity.mount                                | 为提供的存储启用缓存容量装载。                                                  | bool   | 是                                                                                   |
| hdfs-site.dfs.provided.overreplication.factor                              | 提供的存储的 Overreplication 因子。                                                       | FLOAT  | 1                                                                                      |
| hdfs-site.dfs.provided.cache.capacity.fraction                             | 所提供存储的缓存容量分数。                                                       | FLOAT  | 0.01                                                                                   |
| hdfs-site.dfs.ls.limit                                                     | 限制 ls 打印的文件数。                                                            | int    | 500                                                                                    |
| hdfs-env.HDFS_NAMENODE_OPTS                                                | HDFS Namenode 选项。                                                                              | 字符串 | -Dhadoop.security.logger=INFO,RFAS -Xmx2g                                              |
| hdfs-env.HDFS_DATANODE_OPTS                                                | HDFS Datanode 选项。                                                                              | 字符串 | -Dhadoop.security.logger=ERROR,RFAS -Xmx2g                                             |
| hdfs-env.HDFS_ZKFC_OPTS                                                    | HDFS ZKFC 选项。                                                                                  | 字符串 | -Xmx1g                                                                                 |
| hdfs-env.HDFS_JOURNALNODE_OPTS                                             | HDFS JournalNode 选项。                                                                           | 字符串 | -Xmx2g                                                                                 |
| hdfs-env.HDFS_AUDIT_LOGGER                                                 | HDFS 审核日志记录器选项。                                                                          | 字符串 | INFO,RFAAUDIT                                                                          |
| core-site.hadoop.security.group.mapping.ldap.search.group.hierarchy.levels | 核心站点 Hadoop LDAP 搜索组的层次结构级别。                                            | int    | 10                                                                                     |
| core-site.fs.permissions.umask-mode                                        | 权限 umask 模式。                                                                              | 字符串 | 077                                                                                    |
| core-site.hadoop.security.kms.client.failover.max.retries                  | 客户端故障转移的最大重试次数。                                                                    | int    | 20                                                                                     || zoo-cfg.tickTime                                       | “ZooKeeper”配置的时钟周期时间。                         | int    | 2000                |
| zoo-cfg.initLimit                                      | “ZooKeeper”配置的初始化时间。                         | int    | 10                  |
| zoo-cfg.syncLimit                                      | “ZooKeeper”配置的同步时间。                         | int    | 5                   |
| zoo-cfg.maxClientCnxns                                 | “ZooKeeper”配置的客户端连接数上限。            | int    | 60                  |
| zoo-cfg.minSessionTimeout                              | “ZooKeeper”配置的会话超时时间下限。           | int    | 4000                |
| zoo-cfg.maxSessionTimeout                              | “ZooKeeper”配置的会话超时时间上限。           | int    | 40000               |
| zoo-cfg.autopurge.snapRetainCount                      | Autopurge“ZooKeeper”配置的对齐保留计数。       | int    | 3                   |
| zoo-cfg.autopurge.purgeInterval                        | Autopurge“ZooKeeper”配置的清除时间间隔。          | int    | 0                   |
| zookeeper-java-env.JVMFLAGS                            | “ZooKeeper”中 Java 环境的 JVM 标志。            | 字符串 | -Xmx1G -Xms1G       |
| zookeeper-log4j-properties.zookeeper.console.threshold | “ZooKeeper”中 log4j 控制台的阈值。               | 字符串 | INFO                |
| zoo-cfg.zookeeper.request.timeout                      | 控制“ZooKeeper”请求的超时时间（以毫秒为单位）。 | int    | 40000               |
| kms-site.hadoop.security.kms.encrypted.key.cache.size | Hadoop kms 中加密密钥的缓存大小。 | int  | 500 |
    

##  <a name="big-data-clusters-specific-default-gateway-settings"></a>大数据群集特定的默认网关配置
以下网关设置是具有 BDC 特定默认值但用户可配置的设置。 不包含系统管理的设置。 只能在“资源”范围内配置网关设置。

| 设置名称                                          | 说明                                            | 类型   | 默认值 |
| --------------------------------------------- | ------------------------------------------------------ | ------ | ------------------- |
| gateway-site.gateway.httpclient.socketTimeout | 网关中 HTTP 客户端的套接字超时时间（毫秒/秒/分钟）。 | 字符串 | 90 秒                 |
| gateway-site.sun.security.krb5.debug          | 调试 Kerberos 安全性。                           | bool   | 是                |
| knox-env.KNOX_GATEWAY_MEM_OPTS                | Knox 网关内存选项。                           | 字符串 | -Xmx2g              |

## <a name="unsupported-spark-configurations"></a>不支持的 Spark 配置

以下 `spark` 配置不受支持，并且无法在大数据群集的上下文中进行更改。

| 类别  | 子类别               | 文件                       | 不支持的配置                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|           | yarn-site                  | yarn-site.xml              | yarn.log-aggregation-enable                                             |
|           |                            |                            | yarn.log.server.url                                                     |
|           |                            |                            | yarn.nodemanager.pmem-check-enabled                                     |
|           |                            |                            | yarn.nodemanager.vmem-check-enabled                                     |
|           |                            |                            | yarn.nodemanager.aux-services                                           |
|           |                            |                            | yarn.resourcemanager.address                                            |
|           |                            |                            | yarn.nodemanager.address                                                |
|           |                            |                            | yarn.client.failover-no-ha-proxy-provider                               |
|           |                            |                            | yarn.client.failover-proxy-provider                                     |
|           |                            |                            | yarn.http.policy                                                        |
|           |                            |                            | yarn.nodemanager.linux-container-executor.secure-mode.use-pool-user     |
|           |                            |                            | yarn.nodemanager.linux-container-executor.secure-mode.pool-user-prefix  |
|           |                            |                            | yarn.nodemanager.linux-container-executor.nonsecure-mode.local-user     |
|           |                            |                            | yarn.acl.enable                                                         |
|           |                            |                            | yarn.admin.acl                                                          |
|           |                            |                            | yarn.resourcemanager.hostname                                           |
|           |                            |                            | yarn.resourcemanager.principal                                          |
|           |                            |                            | yarn.resourcemanager.keytab                                             |
|           |                            |                            | yarn.resourcemanager.webapp.spnego-keytab-file                          |
|           |                            |                            | yarn.resourcemanager.webapp.spnego-principal                            |
|           |                            |                            | yarn.nodemanager.principal                                              |
|           |                            |                            | yarn.nodemanager.keytab                                                 |
|           |                            |                            | yarn.nodemanager.webapp.spnego-keytab-file                              |
|           |                            |                            | yarn.nodemanager.webapp.spnego-principal                                |
|           |                            |                            | yarn.resourcemanager.ha.enabled                                         |
|           |                            |                            | yarn.resourcemanager.cluster-id                                         |
|           |                            |                            | yarn.resourcemanager.zk-address                                         |
|           |                            |                            | yarn.resourcemanager.ha.rm-ids                                          |
|           |                            |                            | yarn.resourcemanager.hostname.*                                         |
|           | capacity-scheduler         | capacity-scheduler.xml     | yarn.scheduler.capacity.root.acl_submit_applications                    |
|           |                            |                            | yarn.scheduler.capacity.root.acl_administer_queue                       |
|           |                            |                            | yarn.scheduler.capacity.root.default.acl_application_max_priority       |
|           | yarn-env                   | yarn-env.sh                |                                                                         |
|           | spark-defaults-conf        | spark-defaults.conf        | spark.yarn.archive                                                      |
|           |                            |                            | spark.yarn.historyServer.address                                        |
|           |                            |                            | spark.eventLog.enabled                                                  |
|           |                            |                            | spark.eventLog.dir                                                      |
|           |                            |                            | spark.sql.warehouse.dir                                                 |
|           |                            |                            | spark.sql.hive.metastore.version                                        |
|           |                            |                            | spark.sql.hive.metastore.jars                                           |
|           |                            |                            | spark.extraListeners                                                    |
|           |                            |                            | spark.metrics.conf                                                      |
|           |                            |                            | spark.ssl.enabled                                                       |
|           |                            |                            | spark.authenticate                                                      |
|           |                            |                            | spark.network.crypto.enabled                                            |
|           |                            |                            | spark.ssl.keyStore                                                      |
|           |                            |                            | spark.ssl.keyStorePassword  
|           |                            |                            | spark.ui.enabled                                                        |
|           | spark-env                  | spark-env.sh               | SPARK_NO_DAEMONIZE                                                      |
|           |                            |                            | SPARK_DIST_CLASSPATH                                                    |
|           | spark-history-server-conf  | spark-history-server.conf  | spark.history.fs.logDirectory                                           |
|           |                            |                            | spark.ui.proxyBase                                                      |
|           |                            |                            | spark.history.fs.cleaner.enabled                                        |
|           |                            |                            | spark.ssl.enabled                                                       |
|           |                            |                            | spark.authenticate                                                      |
|           |                            |                            | spark.network.crypto.enabled                                            |
|           |                            |                            | spark.ssl.keyStore                                                      |
|           |                            |                            | spark.ssl.keyStorePassword                                              |
|           |                            |                            | spark.history.kerberos.enabled                                          |
|           |                            |                            | spark.history.kerberos.principal                                        |
|           |                            |                            | spark.history.kerberos.keytab                                           |
|           |                            |                            | spark.ui.filters                                                        |
|           |                            |                            | spark.acls.enable                                                       |
|           |                            |                            | spark.history.ui.acls.enable                                            |
|           |                            |                            | spark.history.ui.admin.acls                                             |
|           |                            |                            | spark.history.ui.admin.acls.groups                                      |
|           | livy-conf                  | livy.conf                  | livy.keystore                                                           |
|           |                            |                            | livy.keystore.password                                                  |
|           |                            |                            | livy.spark.master                                                       |
|           |                            |                            | livy.spark.deploy-mode                                                  |
|           |                            |                            | livy.rsc.jars                                                           |
|           |                            |                            | livy.repl.jars                                                          |
|           |                            |                            | livy.rsc.pyspark.archives                                               |
|           |                            |                            | livy.rsc.sparkr.package                                                 |
|           |                            |                            | livy.repl.enable-hive-context                                           |
|           |                            |                            | livy.superusers                                                         |
|           |                            |                            | livy.server.auth.type                                                   |
|           |                            |                            | livy.server.launch.kerberos.keytab                                      |
|           |                            |                            | livy.server.launch.kerberos.principal                                   |
|           |                            |                            | livy.server.auth.kerberos.principal                                     |
|           |                            |                            | livy.server.auth.kerberos.keytab                                        |
|           |                            |                            | livy.impersonation.enabled                                              |
|           |                            |                            | livy.server.access-control.enabled                                      |
|           |                            |                            | livy.server.access-control.*                                            |
|           | livy-env                   | livy-env.sh                |                                                                         |
|           | hive-site                  | hive-site.xml              | javax.jdo.option.ConnectionURL                                          |
|           |                            |                            | javax.jdo.option.ConnectionDriverName                                   |
|           |                            |                            | javax.jdo.option.ConnectionUserName                                     |
|           |                            |                            | javax.jdo.option.ConnectionPassword                                     |
|           |                            |                            | hive.metastore.uris                                                     |
|           |                            |                            | hive.metastore.pre.event.listeners                                      |
|           |                            |                            | hive.security.authorization.enabled                                     |
|           |                            |                            | hive.security.metastore.authenticator.manager                           |
|           |                            |                            | hive.security.metastore.authorization.manager                           |
|           |                            |                            | hive.metastore.use.SSL                                                  |
|           |                            |                            | hive.metastore.keystore.path                                            |
|           |                            |                            | hive.metastore.keystore.password                                        |
|           |                            |                            | hive.metastore.truststore.path                                          |
|           |                            |                            | hive.metastore.truststore.password                                      |
|           |                            |                            | hive.metastore.kerberos.keytab.file                                     |
|           |                            |                            | hive.metastore.kerberos.principal                                       |
|           |                            |                            | hive.metastore.sasl.enabled                                             |
|           |                            |                            | hive.metastore.execute.setugi                                           |
|           |                            |                            | hive.cluster.delegation.token.store.class                               |
|           | hive-env                   | hive-env.sh                

## <a name="unsupported-hdfs-configurations"></a>不支持的 HDFS 配置

以下 `hdfs` 配置不受支持，并且无法在大数据群集的上下文中进行更改。

| 类别  | 子类别               | 文件                       | 不支持的配置                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|          | core-site                   | core-site.xml                 | fs.defaultFS                                          |
|          |                             |                               | ha.zookeeper.quorum                                   |
|          |                             |                               | hadoop.tmp.dir                                        |
|          |                             |                               | hadoop.rpc.protection                                 |
|          |                             |                               | hadoop.security.auth_to_local                         |
|          |                             |                               | hadoop.security.authentication                        |
|          |                             |                               | hadoop.security.authorization                         |
|          |                             |                               | hadoop.http.authentication.simple.anonymous.allowed   |
|          |                             |                               | hadoop.http.authentication.type                       |
|          |                             |                               | hadoop.http.authentication.kerberos.principal         |
|          |                             |                               | hadoop.http.authentication.kerberos.keytab            |
|          |                             |                               | hadoop.http.filter.initializers                       |
|          |                             |                               | hadoop.security.group.mapping.*                       |                               |
|          |                             |                               | hadoop.security.key.provider.path                     |                               |
|          | mapred-env                  | mapred-env.sh                 |                                                       |
|          | hdfs-site                   | hdfs-site.xml                 | dfs.namenode.name.dir                                 |
|          |                             |                               | dfs.datanode.data.dir                                 |
|          |                             |                               | dfs.namenode.acls.enabled                             |
|          |                             |                               | dfs.namenode.datanode.registration.ip-hostname-check  |
|          |                             |                               | dfs.client.retry.policy.enabled                       |
|          |                             |                               | dfs.permissions.enabled                               |
|          |                             |                               | dfs.nameservices                                      |
|          |                             |                               | dfs.ha.namenodes.nmnode-0                             |
|          |                             |                               | dfs.namenode.rpc-address.nmnode-0.*                   |
|          |                             |                               | dfs.namenode.shared.edits.dir                         |
|          |                             |                               | dfs.ha.automatic-failover.enabled                     |
|          |                             |                               | dfs.ha.fencing.methods                                |
|          |                             |                               | dfs.journalnode.edits.dir                             |
|          |                             |                               | dfs.client.failover.proxy.provider.nmnode-0           |
|          |                             |                               | dfs.namenode.http-address                             |
|          |                             |                               | dfs.namenode.httpS-address                            |
|          |                             |                               | dfs.http.policy                                       |
|          |                             |                               | dfs.encrypt.data.transfer                             |
|          |                             |                               | dfs.block.access.token.enable                         |
|          |                             |                               | dfs.data.transfer.protection                          |
|          |                             |                               | dfs.encrypt.data.transfer.cipher.suites               |
|          |                             |                               | dfs.https.port                                        |
|          |                             |                               | dfs.namenode.keytab.file                              |
|          |                             |                               | dfs.namenode.kerberos.principal                       |
|          |                             |                               | dfs.namenode.kerberos.internal.spnego.principal       |
|          |                             |                               | dfs.datanode.data.dir.perm                            |
|          |                             |                               | dfs.datanode.address                                  |
|          |                             |                               | dfs.datanode.http.address                             |
|          |                             |                               | dfs.datanode.ipc.address                              |
|          |                             |                               | dfs.datanode.https.address                            |
|          |                             |                               | dfs.datanode.keytab.file                              |
|          |                             |                               | dfs.datanode.kerberos.principal                       |
|          |                             |                               | dfs.journalnode.keytab.file                           |
|          |                             |                               | dfs.journalnode.kerberos.principal                    |
|          |                             |                               | dfs.journalnode.kerberos.internal.spnego.principal    |
|          |                             |                               | dfs.web.authentication.kerberos.keytab                |
|          |                             |                               | dfs.web.authentication.kerberos.principal             |
|          |                             |                               | dfs.webhdfs.enabled                                   |
|          |                             |                               | dfs.permissions.superusergroup                        |
|          | hdfs-env                    | hdfs-env.sh                   | HADOOP_HEAPSIZE_MAX                                   |
|          | zoo-cfg                     | zoo.cfg                       | secureClientPort                                      |
|          |                             |                               | clientPort                                            |
|          |                             |                               | dataDir                                               |
|          |                             |                               | dataLogDir                                            |
|          |                             |                               | 4lw.commands.whitelist                                |
|          | zookeeper-java-env          | java.env                      | ZK_LOG_DIR                                            |
|          |                             |                               | SERVER_JVMFLAGS                                       |
|          | zookeeper-log4j-properties  | log4j.properties (zookeeper)  | log4j.rootLogger                                      |
|          |                             |                               | log4j.appender.CONSOLE.*                              |

> [!NOTE]
> 本文包含术语“whitelist”，Microsoft 认为该术语在此上下文中不属于敏感词汇。 本文使用该术语的原因是，当前软件中存在该术语。 在从软件中删除该术语后，我们会将其从本文中删除。

## <a name="unsupported-gateway-configurations"></a>不支持的 `gateway` 配置

以下 `gateway` 配置不受支持，并且无法在大数据群集的上下文中进行更改。

| 类别  | 子类别               | 文件                       | 不支持的配置                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|          | gateway-site                | gateway-site.xml              | gateway.port                                          |
|          |                             |                               | gateway.path                                          |
|          |                             |                               | gateway.gateway.conf.dir                              |
|          |                             |                               | gateway.hadoop.kerberos.secured                       |
|          |                             |                               | java.security.krb5.conf                               |
|          |                             |                               | java.security.auth.login.config                       |
|          |                             |                               | gateway.websocket.feature.enabled                     |
|          |                             |                               | gateway.scope.cookies.feature.enabled                 |
|          |                             |                               | ssl.exclude.protocols                                 |
|          |                             |                               | ssl.include.ciphers                                   |

## <a name="next-steps"></a>后续步骤

[配置 SQL Server 大数据群集](configure-bdc-overview.md)
