---
title: '`pyspark` 笔记本故障排除'
titleSuffix: SQL Server Big Data Cluster
description: '`pyspark` 笔记本故障排除'
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mikeray
ms.date: 06/01/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 02ac9f72b7cd688cd38bdc5e785732e8b97bc2dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043397"
---
# <a name="troubleshoot-pyspark-notebook"></a>`pyspark` 笔记本故障排除

本文演示如何排除失败的 `pyspark` 笔记本的故障。

## <a name="architecture-of-a-pyspark-job-under-azure-data-studio"></a>Azure Data Studio 下 PySpark 作业的体系结构

Azure Data Studio 与 SQL Server BDC 上的 `livy` 终结点通信。 

`livy` 终结点在 BDC 群集中发出 `spark-submit` 命令。 每个 `spark-submit` 命令都有一个参数将 YARN 指定为群集资源管理器。

为了有效排除 PySpark 会话的问题，将收集并查看每个层中的日志：Livy、YARN 和 Spark。

此故障排除步骤要求你：

1. 安装了 [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]，并将配置正确设为群集。
2. 熟悉如何运行 Linux 命令以及一些日志故障排除技巧。

## <a name="troubleshooting-steps"></a>疑难解答步骤

1. 查看 `pyspark` 中的堆栈和错误消息。

   从笔记本的第一个单元格中获取应用程序 ID。 使用此应用程序 ID 调查 `livy`、YARN 和 Spark 日志。 `SparkContext` 使用此 YARN 应用程序 ID。

   :::image type="content" source="../big-data-cluster/media/troubleshoot-pyspark-notebook/1-failed-cell.png" alt-text="失败的单元格":::

1. 获取日志。

   使用 `azdata bdc debug copy-logs` 调查

   下面的示例连接大数据群集终结点以复制日志。 在运行之前更新示例中的以下值。
   - `<ip_address>`：大数据群集终结点
   - `<username>`：大数据群集用户名
   - `<namespace>`：群集的 Kubernetes 命名空间
   - `<folder_to_copy_logs>`：要将日志复制到其中的本地文件夹路径

   ```console
   azdata login --auth basic --username <username> --endpoint https://<ip_address>:30080
   azdata bdc debug copy-logs -n <namespace> -d <folder_to_copy_logs>
   ```

   示例输出

   ```output
   <user>@<server>:~$ azdata bdc debug copy-logs -n <namespace> -d copy_logs
   Collecting the logs for cluster '<namespace>'.
   Collecting logs for containers...
   Creating an archive from logs-tmp/<namespace>.
   Log files are archived in /home/<user>/copy_logs/debuglogs-<namespace>-YYYYMMDD-HHMMSS.tar.gz.
   Creating an archive from logs-tmp/dumps.
   Log files are archived in /home/<user>/copy_logs/debuglogs-<namespace>-YYYYMMDD-HHMMSS-dumps.tar.gz.
   Collecting the logs for cluster 'kube-system'.
   Collecting logs for containers...
   Creating an archive from logs-tmp/kube-system.
   Log files are archived in /home/<user>/copy_logs/debuglogs-kube-system-YYYYMMDD-HHMMSS.tar.gz.
   Creating an archive from logs-tmp/dumps.
   Log files are archived in /home/<user>/copy_logs/debuglogs-kube-system-YYYYMMDD-HHMMSS-dumps.tar.gz.
   ```

1. 检查 Livy 日志。 Livy 日志位于 `<namespace>\sparkhead-0\hadoop-livy-sparkhistory\supervisor\log`。

   - 从 pyspark 笔记本第一个单元格中搜索 YARN 应用程序 ID。
   - 搜索 `ERR` 状态。
   
   状态为 `YARN ACCEPTED` 的 Livy 日志的示例。 Livy 已提交 yarn 应用程序。

   ```output
   HH:MM:SS INFO utils.LineBufferedStream: YYY-MM-DD HH:MM:SS INFO impl.YarnClientImpl: Submitted application application_<application_id>
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream: YYY-MM-DD HH:MM:SS INFO yarn.Client: Application report for application_<application_id> (state: ACCEPTED)
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream: YYY-MM-DD HH:MM:SS INFO yarn.Client: 
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      client token: N/A
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      diagnostics: N/A
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      ApplicationMaster host: N/A
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      ApplicationMaster RPC port: -1
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      queue: default
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      start time: ############
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      final status: UNDEFINED
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      tracking URL: https://sparkhead-1.fnbm.corp:8090/proxy/application_<application_id>/
   YY/MM/DD HH:MM:SS INFO utils.LineBufferedStream:      user: <account>
   ```

1. 查看 YARN UI

   从 Azure Data Studio 大数据群集管理仪表板获取 YARN 终结点 URL，或者运行 `azdata bdc endpoint list –o table`。

   例如：

   ```console
   azdata bdc endpoint list -o table
   ```

   返回

   ```output
   Description                                             Endpoint                                                          Name                        Protocol
   ------------------------------------------------------  ----------------------------------------------------------------  --------------------------  ----------
   Gateway to access HDFS files, Spark                     https://knox.<namespace-value>.local:30443                               gateway                     https
   Spark Jobs Management and Monitoring Dashboard          https://knox.<namespace-value>.local:30443/gateway/default/sparkhistory  spark-history               https
   Spark Diagnostics and Monitoring Dashboard              https://knox.<namespace-value>.local:30443/gateway/default/yarn          yarn-ui                     https
   Application Proxy                                       https://proxy.<namespace-value>.local:30778                              app-proxy                   https
   Management Proxy                                        https://bdcmon.<namespace-value>.local:30777                             mgmtproxy                   https
   Log Search Dashboard                                    https://bdcmon.<namespace-value>.local:30777/kibana                      logsui                      https
   Metrics Dashboard                                       https://bdcmon.<namespace-value>.local:30777/grafana                     metricsui                   https
   Cluster Management Service                              https://bdcctl.<namespace-value>.local:30080                             controller                  https
   SQL Server Master Instance Front-End                    sqlmaster.<namespace-value>.local,31433                                  sql-server-master           tds
   SQL Server Master Readable Secondary Replicas           sqlsecondary.<namespace-value>.local,31436                               sql-server-master-readonly  tds
   HDFS File System Proxy                                  https://knox.<namespace-value>.local:30443/gateway/default/webhdfs/v1    webhdfs                     https
   Proxy for running Spark statements, jobs, applications  https://knox.<namespace-value>.local:30443/gateway/default/livy/v1       livy                        https
   ```

1. 检查应用程序 ID 以及单个 application_master 和容器日志。

   :::image type="content" source="media/troubleshoot-pyspark-notebook/15-hadoop-dashboard.png" alt-text="检查应用程序 ID":::

1. 查看 YARN 应用程序日志。

   获取应用的应用程序日志。 使用 `kubectl` 连接到 `sparkhead-0` Pod，例如：
   
   ```console
   kubectl exec -it sparkhead-0 -- /bin/bash
   ```
      
   然后使用正确的 `application_id` 在该 shell 中运行以下命令：

   ```console
   yarn logs -applicationId application_<application_id>
   ```

1. 搜索错误或堆栈。

   针对 hdfs 的权限错误的示例。 在 java 堆栈中查找 `Caused by:`

   ```output
   YYYY-MM-DD HH:MM:SS,MMM ERROR spark.SparkContext: Error initializing SparkContext.
   org.apache.hadoop.security.AccessControlException: Permission denied: user=<account>, access=WRITE, inode="/system/spark-events":sph:<bdc-admin>:drwxr-xr-x
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:399)
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:255)
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:193)
        at org.apache.hadoop.hdfs.server.namenode.FSDirectory.checkPermission(FSDirectory.java:1852)
        at org.apache.hadoop.hdfs.server.namenode.FSDirectory.checkPermission(FSDirectory.java:1836)
        at org.apache.hadoop.hdfs.server.namenode.FSDirectory.checkAncestorAccess(FSDirectory.java:1795)
        at org.apache.hadoop.hdfs.server.namenode.FSDirWriteFileOp.resolvePathForStartFile(FSDirWriteFileOp.java:324)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.startFileInt(FSNamesystem.java:2504)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.startFileChecked(FSNamesystem.java:2448)
   
   Caused by: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.security.AccessControlException): Permission denied: user=<account>, access=WRITE, inode="/system/spark-events":sph:<bdc-admin>:drwxr-xr-x
   ```

1. 查看 SPARK UI。

   :::image type="content" source="media/troubleshoot-pyspark-notebook/30-spark-ui.png" alt-text="Spark UI":::

   向下解锁阶段任务，查找错误。

## <a name="next-steps"></a>后续步骤

[对 SQL Server 大数据群集 Active Directory 集成进行故障排除](troubleshoot-active-directory.md)