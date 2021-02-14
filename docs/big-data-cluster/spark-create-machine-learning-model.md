---
title: 创建、导出 Spark ML 模型：MLeap
titleSuffix: SQL Server Big Data Clusters
description: 在 SQL Server 大数据群集上使用 PySpark 通过 Spark 定型和创建机器学习模型。 使用 MLeap 导出，然后在 SQL Server 中使用 Java 对模型进行评分。
author: RogPodge
ms.author: roliu
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 4b403644de65f866d4bafb191356fd08212c706f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043597"
---
# <a name="create-export-and-score-spark-machine-learning-models-on-big-data-clusters-2019"></a>在 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]上创建和导出 Spark 机器学习模型并对其评分

以下示例演示如何使用 [Spark ML](https://spark.apache.org/docs/latest/ml-guide.html) 生成模型，将模型导出到 [MLeap](http://mleap-docs.combust.ml/)，以及在 SQL Server 中使用其 [Java 语言扩展](../language-extensions/language-extensions-overview.md)对模型进行评分。 这是在 SQL Server 2019 大数据群集的上下文中完成的。

下图说明了此示例中执行的工作：

![使用 spark 训练评分导出](./media/spark-create-machine-learning-model/train-score-export-with-spark.png)

## <a name="prerequisites"></a>先决条件

此示例的所有文件均位于 [https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark/sparkml](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark/sparkml) 中。

若要运行该示例，还必须具有以下系统必备组件：

- [SQL Server 大数据群集](deploy-get-started.md)

- [大数据工具](deploy-big-data-tools.md)
   - **kubectl**
   - **curl**
   - **Azure Data Studio**

## <a name="model-training-with-spark-ml"></a>使用 Spark ML 进行模型训练

在此示例中，人口普查数据 (**AdultCensusIncome.csv**) 用于生成 Spark ML 管道模型。

1. 使用 [mleap_sql_test/setup.sh](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/setup.sh) 文件从 Internet 下载数据集，并将其放在 SQL Server 大数据群集的 HDFS 上。 这可让 Spark 访问它。

1. 然后下载示例笔记本 [train_score_export_ml_models_with_spark.ipynb](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/train_score_export_ml_models_with_spark.ipynb)。 从 PowerShell 或 bash 命令行运行以下命令，以下载该笔记本：

   ```PowerShell
   curl -o mssql_spark_connector.ipynb "https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/spark/sparkml/train_score_export_ml_models_with_spark.ipynb"
   ```

   此笔记本包含具有此示例部分所需命令的单元格。

1. 在 Azure Data Studio 中打开笔记本，然后运行每个代码块。 有关使用笔记本的详细信息，请参阅[如何在 SQL Server 中使用笔记本](../azure-data-studio/notebooks/notebooks-guidance.md)。

数据会先读入 Spark 并拆分成训练和测试数据集。 然后，代码使用训练数据训练管道模型。 最后，它将模型导出为 MLeap 捆绑包。

> [!TIP]
> 还可以在 [mleap_sql_test/mleap_pyspark.py](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/mleap_pyspark.py) 文件中的笔记本外部查看或运行与这些步骤关联的 Python 代码。

## <a name="model-scoring-with-sql-server"></a>使用 SQL Server 进行模型评分

由于 Spark ML 管道模型采用常见的序列化 [MLeap 捆绑包](http://mleap-docs.combust.ml/core-concepts/mleap-bundles.html)格式，因此可以在没有 Spark 的情况下，在 Java 中对模型进行评分。

此示例使用 SQL Server 中的 [Java 语言扩展](../language-extensions/language-extensions-overview.md)。 若要在 SQL Server 中对模型进行评分，首先需要生成一个可以将模型加载到 Java 中并对其进行评分的 Java 应用程序。 可以在 [mssql-mleap-app 文件夹](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mssql-mleap-app)中找到此 Java 应用程序的示例代码。

生成示例后，可以使用 Transact-SQL 调用 Java 应用程序，并使用数据库表对模型进行评分。 这可以在 [mleap_sql_test/mleap_sql_tests.py](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/mleap_sql_tests.py) 源文件中看到。

## <a name="next-steps"></a>后续步骤

有关大数据群集的详细信息，请参阅[如何在 Kubernetes 部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)