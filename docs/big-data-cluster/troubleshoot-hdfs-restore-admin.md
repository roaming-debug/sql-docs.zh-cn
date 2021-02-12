---
title: 还原 HDFS 权限
titleSuffix: SQL Server Big Data Cluster
description: 恢复 HDFS 管理权限。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/21/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d914b2006656162b233351815e238f282d73e913
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045527"
---
# <a name="restore-hdfs-permissions"></a>还原 HDFS 权限

对 HDFS 访问控制列表 (ACL) 的修改可能已影响 HDFS 中的 `/system` 和 `/tmp` 文件夹。 执行 ACL 修改最可能的原因是用户手动处理文件夹 ACL。 不支持直接修改 /system 文件夹和 /tmp/logs 文件夹中的权限。

## <a name="symptom"></a>症状

Spark 作业是通过 ADS 提交的，如果出现 SparkContext 初始化错误和 AccessControlException，提交就会失败。

```
583 ERROR spark.SparkContext: Error initializing SparkContext.
org.apache.hadoop.security.AccessControlException: Permission denied: user=<UserAccount>, access=WRITE, inode="/system/spark-events":sph:BDCAdmin:drwxr-xr-x
```

Yarn UI 显示应用程序 ID 处于“已终止”状态。

在尝试以域用户的身份写入文件夹时，它也会失败。 可以使用以下示例进行测试：

```bash
kinit <UserAccount>
hdfs dfs -touch /system/spark-events/test
hdfs dfs -rm /system/spark-events/test
```

## <a name="cause"></a>原因

已为 BDC 用户域安全组修改了 HDFS ACL。 可能的修改已包括 /system 和 /tmp 文件夹的 ACL。 不支持修改这些文件夹。

验证 Livy 日志中的效果：

```
INFO utils.LineBufferedStream: YYYY-MM-DD-HH:MM:SS,858 INFO yarn.Client: Application report for application_1580771254352_0041 (state: ACCEPTED)
...
WARN rsc.RSCClient: Client RPC channel closed unexpectedly.
INFO interactive.InteractiveSession: Failed to ping RSC driver for session <ID>. Killing application
```

YARN UI 显示应用程序 ID 为“已终止”状态的应用程序。

若要了解 RPC 连接关闭的根本原因，请查看 YARN 应用程序日志中与该应用程序相对应的应用。 在前面的示例中，它指的是 `application_1580771254352_0041`。 使用 `kubectl` 连接到 sparkhead-0 pod，并运行以下命令：

以下命令查询应用程序的 YARN 日志。

```bash
yarn logs -applicationId application_1580771254352_0041
```

在下面的结果中，该用户的权限被拒绝。 

```
YYYY-MM-DD-HH:MM:SS,583 ERROR spark.SparkContext: Error initializing SparkContext.
org.apache.hadoop.security.AccessControlException: Permission denied: user=user1, access=WRITE, inode="/system/spark-events":sph:BDCAdmin:drwxr-xr-x
```

原因可能是 BDC 用户被通过递归方式添加到 HDFS 根文件夹中。 这可能影响了默认权限。

## <a name="resolution"></a>解决方法

使用以下脚本恢复权限：对管理员使用 `kinit`：

```bash
hdfs dfs -chmod 733 /system/spark-events
hdfs dfs -setfacl --set default:user:sph:rwx,default:other::--- /system/spark-events
hdfs dfs -setfacl --set default:user:app-setup:r-x,default:other::--- /system/appdeploy
hadoop fs -chmod 733 /tmp/logs
hdfs dfs -setfacl --set default:user:yarn:rwx,default:other::--- /tmp/logs
```
