---
title: Spark 库管理
titleSuffix: SQL Server big data clusters
description: Spark 库管理
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/25/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bed687cb003bfc7748aefa3c98ae5e19089f9685
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837054"
---
# <a name="spark-library-management"></a>Spark 库管理

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文提供了有关如何通过会话和笔记本配置为 Spark 会话导入和安装包的指导。

## <a name="built-in-tools"></a>内置工具

Scala Spark (Scala 2.11) 和 Hadoop 基础包。 

PySpark (Python 3.7)。 Pandas、Sklearn、Numpy 以及其他数据处理和机器学习包。

MRO 3.5.2 包。 面向 R Spark 工作负载的 Sparklyr 和 SparkR。

## <a name="install-packages-from-a-maven-repository-onto-the-spark-cluster-at-runtime"></a>在运行时将包从 Maven 存储库安装到 Spark 群集

在 Spark 会话开始时，可以使用笔记本单元配置将 Maven 包安装到 Spark 群集。 在 Azure Data Studio 中启动 Spark 会话之前，请运行以下代码：

```python
%%configure -f \
{"conf": {"spark.jars.packages": "com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.1"}}
```

## <a name="install-python-packages-at-pyspark-at-runtime"></a>在运行时在 PySpark 上安装 Python 包

会话和作业级包管理可保证库的一致性和隔离性。 配置是可应用于 Livy 会话的 Spark 标准库配置。 azdata spark 支持这些配置。 下面的示例显示为 Azure Data Studio Notebooks 配置单元，在附加到使用 PySpark 内核的群集后需要运行该单元。

如果未设置 "spark.pyspark.virtualenv.enabled" : "true" 配置，则会话将使用群集默认 python 和已安装的库。

### <a name="sessionjob-configuration-with-requirementstxt"></a>requirements.txt 的会话/作业配置

在 HDFS 中指定 requirements.txt 文件路径，将其用作要安装的包的引用。

```python
%%configure -f \
{
    "conf": {
        "spark.pyspark.virtualenv.enabled" : "true",
        "spark.pyspark.virtualenv.python_version": "3.7",
        "spark.pyspark.virtualenv.requirements" : "hdfs://user/project-A/requirements.txt"
    }
}
```

### <a name="sessionjob-configuration-with-different-python-versions"></a>不同 python 版本的会话/作业配置

创建无要求文件的 conda virtualenv，并在 Spark 会话期间动态添加包。

```python
%%configure -f \
{
    "conf": {
        "spark.pyspark.virtualenv.enabled" : "true",
        "spark.pyspark.virtualenv.python_version": "3.6"
    }
}
```

### <a name="library-installation"></a>库安装

执行 sc.install_packages，以在会话中动态安装库。 这些库将安装到驱动程序和所有执行器节点上。

 ```python
sc.install_packages("numpy==1.11.0")
import numpy as np
```

还可以使用数组在同一命令中安装多个库。

 ```python
sc.install_packages(["numpy==1.11.0", "xgboost"])
import numpy as np
import xgboost as xgb
```

## <a name="import-jar-from-hdfs-for-use-at-runtime"></a>从 HDFS 导入 .jar 以便在运行时使用
在运行时通过 Azure Data Studio 笔记本单元配置导入 jar。

```python
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

### <a name="import-jar-at-runtime-through-azure-data-studio-notebook-cell-configuration"></a>在运行时通过 Azure Data Studio 笔记本单元配置导入 .jar

```python
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

## <a name="next-steps"></a>后续步骤

有关 SQL Server 大数据群集和相关方案的详细信息，请参阅 [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md)。