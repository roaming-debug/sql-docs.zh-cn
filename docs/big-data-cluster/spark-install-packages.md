---
title: Spark 库管理
titleSuffix: SQL Server big data clusters
description: Spark 库管理
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 01/25/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 70611d3f7d4ed6825911908942d9ed707dabbd2e
ms.sourcegitcommit: 8bdb5a51f87a6ff3b94360555973ca0cd0b6223f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/16/2021
ms.locfileid: "100549965"
---
# <a name="spark-library-management"></a>Spark 库管理

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文提供了有关如何通过会话和笔记本配置为 Spark 会话导入和安装包的指导。

## <a name="built-in-tools"></a>内置工具
Spark 和 Hadoop 基本包 Python 3.7 和 Python 2.7 Pandas、Spark-sklearn、Numpy 和其他数据处理包。
R 和 MRO 包 Sparklyr

## <a name="install-packages-from-a-maven-repository-onto-the-spark-cluster-at-runtime"></a>在运行时将包从 Maven 存储库安装到 Spark 群集
在 Spark 会话开始时，可以使用笔记本单元配置将 Maven 包安装到 Spark 群集。 在 Azure Data Studio 中启动 Spark 会话之前，请运行以下代码：

```
%%configure -f \
{"conf": {"spark.jars.packages": "com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.1"}}
```

## <a name="install-python-packages-at-pyspark-job-submission-time"></a>在 PySpark 作业提交时安装 Python 包
1. 在 HDFS 中指定 requirements.txt 文件路径，将其用作要安装的包的引用。
```
%%configure -f \
{"conf": {
    "spark.pyspark.virtualenv.enabled" : "true",
    "spark.pyspark.virtualenv.type" : "conda",
    "spark.pyspark.virtualenv.requirements" : "requirements.txt",
    "spark.pyspark.virtualenv.bin.path" : "/opt/mls/python/bin/conda"
    }, 
"files": ["hdfs://nmnode-0/tmp/requirements.txt"]
}
```
2. 创建无要求文件的 conda virtualenv，并在 Spark 会话期间动态添加包。
```
%%configure -f \
{"conf": {
    'spark.pyspark.virtualenv.enabled' : 'true',
    'spark.pyspark.virtualenv.type' : 'conda',
    'spark.pyspark.virtualenv.bin.path' : '/opt/mls/python/bin/conda',
    'spark.pyspark.virtualenv.python_version': '3.6'
 }
 ```

 ```python
sc.install_packages("numpy==1.11.0")
import numpy as np
```

## <a name="import-jar-from-hdfs-for-use-at-runtime"></a>从 HDFS 导入 .jar 以便在运行时使用
在运行时通过 Azure Data Studio 笔记本单元配置导入 jar。

```
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

### <a name="import-jar-at-runtime-through-azure-data-studio-notebook-cell-configuration"></a>在运行时通过 Azure Data Studio 笔记本单元配置导入 .jar
```
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

## <a name="next-steps"></a>后续步骤

有关 SQL Server 大数据群集和相关方案的详细信息，请参阅 [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md)。