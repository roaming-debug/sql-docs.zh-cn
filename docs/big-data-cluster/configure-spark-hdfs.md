---
title: Apache Spark 和 Apache Hadoop
titleSuffix: Configure Apache Spark and Apache Hadoop in Big Data Clusters
description: SQL Server 大数据群集允许 Spark 和 HDFS 解决方案。 了解如何配置它们。
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.date: 08/04/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3c018c3882f5a74bb55a3b4cb5fc23881b0daec8
ms.sourcegitcommit: 765262cdc6352a5325148afc22fa4f1499fe1aa3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514854"
---
# <a name="configure-apache-spark-and-apache-hadoop-in-big-data-clusters"></a>在大数据群集中配置 Apache Spark 和 Apache Hadoop

为了在大数据群集中配置 Apache Spark 和 Apache Hadoop，需要在部署时修改群集配置文件。

大数据群集具有四个配置类别： 

- `sql` 
- `hdfs` 
- `spark` 
- `gateway` 

`sql`、`hdfs`、`spark`、`sql` 均为服务。 上述每个服务都对应于名称相同的配置类别。 所有网关配置都将跳到类别 `gateway`。 

例如，服务 `hdfs` 中的所有配置都属于类别 `hdfs`。 请注意，所有 Hadoop（核心站点）、HDFS 和 Zookeeper 配置都属于 `hdfs` 类别；所有 Livy、Spark、Yarn、Hive 元存储配置都属于 `spark` 类别。 

[支持的配置](reference-config-spark-hadoop.md)列出了 Apache Spark 和 Hadoop 属性，你可以在部署 SQL Server 大数据群集时配置这些属性。

以下部分列出了无法在群集中修改的属性：

- [不支持的 `spark` 配置](reference-config-spark-hadoop.md#unsupported-spark-configurations)
- [不支持的 `hdfs` 配置](reference-config-spark-hadoop.md#unsupported-hdfs-configurations)
- [不支持的 `gateway` 配置](reference-config-spark-hadoop.md#unsupported-gateway-configurations)


## <a name="configurations-via-cluster-profile"></a>通过群集配置文件配置

群集配置文件中存在资源和服务。 部署时，我们可以通过以下两种方式之一来指定配置： 

* 第一，在资源级别： 

   以下示例是配置文件的修补程序文件： 

   ```json
   { 
         "op": "add", 
         "path": "spec.resources.zookeeper.spec.settings", 
         "value": { 
           "hdfs": { 
             "zoo-cfg.syncLimit": "6" 
           } 
         } 
   }
   ```

   或： 

   ```json
   { 
         "op": "add", 
         "path": "spec.resources.gateway.spec.settings", 
         "value": { 
           "gateway": { 
             "gateway-site.gateway.httpclient.socketTimeout": "95s" 
           } 
         } 
   } 
   ```

* 第二，在服务级别： 为服务分配多个资源，并指定服务的配置。

以下是用于设置 HDFS 块大小的配置文件的修补程序文件的示例： 

   ```json
   { 
         "op": "add", 
         "path": "spec.services.hdfs.settings", 
         "value": { 
           "hdfs-site.dfs.block.size": "268435456" 
        } 
   } 
   ```

服务 `hdfs` 的定义如下：

```json
{ 
  "spec": { 
   "services": { 
     "hdfs": { 
        "resources": [ 
          "nmnode-0", 
          "zookeeper", 
          "storage-0", 
          "sparkhead" 
        ], 
        "settings":{ 
          "hdfs-site.dfs.block.size": "268435456" 
        } 
      } 
    } 
  } 
} 
```
 
> [!NOTE]
> 资源级别配置替代服务级别配置。 一个资源可以分配给多个服务。

## <a name="enable-spark-in-the-storage-pool"></a>在存储池中启用 Spark
除了支持的 Apache 配置之外，我们还提供了配置 Spark 作业是否可以在存储池中运行的功能。 此布尔值 `includeSpark` `spec.resources.storage-0.spec.settings.spark` 处的配置文件 `bdc.json` 中。

bdc.json 中的存储池定义示例可能如下所示：
```json
...
"storage-0": {
                "metadata": {
                    "kind": "Pool",
                    "name": "default"
                },
                "spec": {
                    "type": "Storage",
                    "replicas": 2,
                    "settings": {
                        "spark": {
                            "includeSpark": "true"
                        }
                    }
                }
            }
```


## <a name="limitations"></a>限制

只能在类别级别指定配置。 若要指定具有相同子类别的多个配置，我们无法在群集配置文件中提取常用前缀。 

```json
{ 
      "op": "add", 
      "path": "spec.services.hdfs.settings.core-site.hadoop", 
      "value": { 
        “proxyuser.xyz.users”: “*”, 
        “proxyuser.abc.users”: “*” 
     } 
} 
```

## <a name="next-steps"></a>后续步骤

- [Apache Spark 和 Apache Hadoop (HDFS) 配置属性。](reference-config-spark-hadoop.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] 引用](../azdata/reference/reference-azdata.md)
- [什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)
