---
title: 使用 Jupyter 笔记本和 Azure Data Studio 对大数据群集 (BDC) 进行故障排除
titleSuffix: SQL Server Big Data Clusters
description: 在 SQL Server 2019 大数据群集上，使用 Jupyter 笔记本和 Azure Data Studio 对 BDC 进行故障排除。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 51286acc7f963b8d680bd81121cc22bab1c1a0a6
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364379"
---
# <a name="troubleshooting-big-data-clusters-bdc-with-notebooks"></a>使用笔记本对大数据群集 (BDC) 进行故障排除

此页是适用于 SQL Server 大数据群集的笔记本的索引。 这些可执行的笔记本 (.ipynb) 旨在用于 SQL Server 2019，以帮助对大数据群集进行故障排除。

每个笔记本均可检查其自身的依赖项。 “运行所有单元格”要么成功完成，要么抛出异常，并显示指向另一个笔记本的超链接提示，以解决缺少依赖项的问题。 单击提示超链接转到后续的笔记本，按“运行所有单元格”，成功后返回到原始笔记本，再按“运行所有单元格”。

一旦安装了所有依赖项，但“运行所有单元格”失败，每个笔记本就会分析结果，并尽可能生成指向另一个笔记本的超链接提示，以进一步帮助解决问题。


## <a name="troubleshooting-big-data-cluster-bdc"></a>对大数据群集 (BDC) 进行故障排除

此部分包含一组笔记本，用于从 SQL Server 大数据群集 (BDC) 获取日志。

|名称 |说明 |
|---|---|---|---|
|TSG100 - 大数据群集疑难解答|概述了所有可用于排查大数据群集 (BDC) 问题的笔记本，以及何时使用它们  |
|TSG101 - SQL Server 疑难解答|概述了所有可用于排查 SQL Server 问题的笔记本，以及何时使用它们  |
|TSG102 - HDFS 疑难解答|概述了所有可用于排查 HDFS 问题的笔记本，以及何时使用它们  |
|TSG103 - Spark 疑难解答|概述了所有可用于排查 Spark 问题的笔记本，以及何时使用它们  |
|TSG104 - 控件疑难解答|概述了所有可用于排查控制器问题的笔记本，以及何时使用它们  |
|TSG105 - 网关疑难解答|概述了所有可用于排查 Knox Gateway 问题的笔记本，以及何时使用它们  |
|TSG106 - 应用疑难解答|概述了所有可用于排查 App-Deploy 问题的笔记本，以及何时使用它们  |



## <a name="diagnose-issues-from-big-data-clusters-bdc"></a>诊断大数据群集 (BDC) 问题

一组用于诊断大数据群集的情况和状态的笔记本。

|名称 |说明 |
|---|---|---|---|
|TSG002 - CrashLoopBackoff|此 TSG 会连接到上次尝试进入“正在运行”状态失败的每个容器，并获取当前和以前的容器日志。 这对于调试 kubectl get pods 中报告的 CrashLoopBackOff 问题很有用。|
|TSG025 - FSM 浏览器 - 查询控制器 FSM 状态|使用此笔记本来连接到控制器数据库，并浏览有限状态机 (FSM) 状态。 使用此笔记本列出活动状态机并识别卡住的工作流程。|
|TSG026 - 连接到数据池节点（以运行 T-SQL）|使用此笔记本来连接到数据池节点（以运行 T-SQL）|
|TSG027 - 观察群集部署|使用此笔记本来观察群集部署，它提供了关于排查 SQL Server 大数据群集创建问题的指南，下面的命令对于查明潜在原因通常很有用。 |
|TSG029 - 在群集中查找转储|使用此笔记本从大数据群集的 SQL Server 或控制器等进程中查找核心转储和小型转储。|
|TSG032 - 所有容器的 CPU 和内存使用情况|使用此笔记本来检查所有容器的 CPU 和内存使用情况。|
|TSG037 - 确定托管主要副本的主池 Pod|当启用主池高可用性时，使用此笔记本来确定托管大数据群集的主要副本的主池 Pod。|
|TSG044 - 在主池容器中运行 sqlcmd|使用此笔记本来通过 T-SQL 直接连接到主池节点。|
|TSG055 - 到 Sparkhead 的时间 Curl|使用此笔记本来进行步骤诊断，以了解从 Controller Pod 到 sparkhead Pod 的 Curl 响应时间。|
|TSG060 - 所有 BDC PVC 的永久卷磁盘空间|使用此笔记本来连接到每个容器，并获取映射到大数据群集 (BDC) 的每个永久性卷声明 (PVC) 的每个永久性卷 (PV) 已使用/可用的磁盘空间。|
|TSG078 - 群集是否正常|使用此笔记本来检查大数据群集 (BDC) 是否正常。|
|TSG079 - 生成控制器核心转储|使用此笔记本来生成控制器核心转储。|
|TSG086 - 在所有容器中的顶层运行|使用此笔记本在所有容器中的顶层运行。|
|TSG087 - 在 namenode Pod 上使用 hadoop fs CLI|使用此笔记本在 namenode Pod 上使用 hadoop fs CLI。|
|TSG108 - 查看控制器升级 ConfigMap|当使用 azdata bdc upgrade 运行大数据群集升级时，使用此笔记本来进行故障排除。|
|TSG112 - Active Directory 部署前检查|使用此笔记本来验证大数据群集 (BDC) 配置对于 Active Directory (AD) 部署是否有效。|
|TSG115 - Linux 上的 SQL Server 安全日志转换器|使用此笔记本来分析 Linux 上的 SQL Server 的 secuirty.ldap 和 security.kerberos 记录器生成的日志。 若要启用这些记录器，请将下面的代码行置于运行 Linux 上的 SQL Server 的计算机上 /var/opt/mssql/logger.ini 中。 注意：此文件是区分大小写的。|
|TSG116 - SQL BDC 安全支持日志转换器|使用此笔记本来分析 SQL BDC 中的安全支持服务生成的日志。 为了获取日志，我们将从群集中复制调试日志，然后提取它们。 按照以下步骤操作 - 运行“azdata bdc debug copy-logs -n <namespace> *”。这会创建多个 .tar.gz 文件 - 提取 debuglogs-* <namespace>-<date>-<time>.tar.gz 的内容 - 找到存储在 ./<namespace>/control-<…>/security-support/supervisol/log/secsupp-stderr---<…>.log 中的安全支持日志。|
|TSG119 - Active Directory 部署后检查|此笔记本旨在用于在 AD 部署后验证 BDC 配置。 它会检查所有包含 dnsName 属性的终结点是否存在 DNS 条目，并且这些 DNS 条目应为主机记录，而不是别名（即为 A 记录，而不是 CNAME 记录）。它还检查是否存在已知的 AD 帐户、是否启用了这些帐户，以及是否存在预期的 SPN|






## <a name="repair-issues-from-big-data-clusters-bdc"></a>修复大数据群集 (BDC) 问题

一组用于修复 SQL Server 大数据群集的已知情况和状态的笔记本。

|名称 |说明 |
|---|---|---|---|
|TSG005 - 检测到转接循环|使用此笔记本来处理检测到的转发循环，因为实用工具 dnsmasq 可以在 resolv.conf 中放置本地 Loopback，这可能会导致 Controller Pod 在初始群集部署期间进入 CrashLoopBackOff： https://askubuntu.com/questions/627899/nameserver-127-0-1-1-in-resolv-conf-wont-go-away|
|TSG011 - 重启 sparkhistory 服务器|使用此笔记本来重启 SparkHistory 服务器，因为 sparkhistory java 进程可能会在启动期间停止响应。 重启 SparkHistory 服务器 (supervisorctl restart sparkhistory) 可以解决此问题。|
|TSG018 - 在主池上终止 sqlservr.exe 进程| 当 T-SQL SHUTDOWN 没有成功循环使用 ./sqlservr 进程时，使用此笔记本。 使用此笔记本来终止 sqlservr 主进程，./sqlservr 前端进程将自动重启主进程。|
|TSG024 - Namenode 处于安全模式| 当 HDFS 本身进入安全模式时，使用此笔记本。 例如，如果在存储池中过快地循环使用了太多的 Pod，那么可能会自动启用安全模式。|
|TSG028 - 重启所有存储池节点上的节点管理器| 当需要重启所有存储池节点上的节点管理器时，使用此笔记本。|
|TSG038 - BDC 创建失败，原因：文档缺少密钥| 当由于 - doc 缺少密钥而导致 BDC 创建失败时，使用此笔记本。|
|TSG039 - 对象名称“role_permissions”无效| 当由于 Knox gateway.log 中的角色权限而导致无效对象问题时，使用此笔记本|
|TSG040 - 无法从控制器获取文件名，出现错误| 如果在从控制器获取文件名时遇到“504 网关超时”，则使用此笔记本。|
|TSG041 - 无法创建新的异步 I/O 上下文（增加 sysctl fs.aio-max-nr）| 当无法新建异步 I/O 上下文（增加 sysctl fs.aio-max-nr）时，使用此笔记本。|
|TSG045 - 允许连接到此大小 (AKS) 的 VM 的最大数据磁盘数| 当允许将最大数量的数据磁盘附加到此大小的 VM (AKS) 时，使用此笔记本。|
|TSG047 - ConfigException - 预期只有一个带有名称的对象| 当 ConfigException 只期望一个带有名称的对象时，使用此笔记本。|
|TSG048 - 部署卡在“等待控制器 Pod 启动”| 当部署卡在“等待控制器 Pod 启动”时，使用此笔记本。|
|TSG050 - 群集创建挂起时显示“等待卷附加或装入 Pod 超时”| 当群集创建挂起并显示“等待卷为 Pod 附加或装载时超时”时，使用此笔记本。|
|TSG052 - 尝试获取 master-svc DNS 失败，将重试| 当群集创建挂起并显示“等待卷为 Pod 附加或装载时超时”时，使用此笔记本。|
|TSG057 - 启动控制器服务 .System.TimeoutException 时失败| 当启动控制器服务并收到 System.TimeoutException 时，使用此笔记本。|
|TSG067 - 未能完成 kube 配置安装| 当无法完成 kube 配置安装时，使用此笔记本。|
|TSG074 -删除 App-Deploy| 如果在 BDC 群集中删除应用时遇到问题，则使用此笔记本。|
|TSG075 - FailedCreatePodSandBox due to NetworkPlugin cni failed to set up pod| 当由于 NetworkPlugin cni 无法创建 Pod 而导致 FailedCreatePodSandBox 异常抛出时，使用此笔记本。|
|TSG080 - 使用 azdata 删除 Spark 会话| 如果在删除 Spark 会话时遇到问题，则使用此笔记本。|
|TSG109 - 设置升级超时| 当遇到 BDC 升级问题时，使用此笔记本。|
|TSG110 - Azdata 返回 ApiError| 当 Azdata 返回 ApiError 时，使用此笔记本。|

## <a name="next-steps"></a>后续步骤

有关大数据群集的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md)。
