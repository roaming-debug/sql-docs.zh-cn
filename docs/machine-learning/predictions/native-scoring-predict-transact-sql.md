---
title: 结合使用 T-SQL PREDICT 和本机评分功能
titleSuffix: SQL machine learning
description: 了解如何结合使用本机评分和 PREDICT T-SQL 函数，以近乎实时的方式为新数据输入生成预测值。
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest'
ms.openlocfilehash: 842daa6574dc660346733e7b74b539eba5c7f7b0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471028"
---
# <a name="native-scoring-using-the-predict-t-sql-function-with-sql-machine-learning"></a>结合利用 SQL 机器学习和 PREDICT T-SQL 函数的本机评分功能

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

了解如何结合使用 [PREDICT T-SQL 函数](../../t-sql/queries/predict-transact-sql.md)和本机评分功能，以近乎实时的方式为新数据输入生成预测值。 本机评分要求你具有已定型的模型。

`PREDICT` 函数使用 [SQL机器学习](../index.yml)中的本机 C++ 扩展功能。 该方法可以使预测工作负载的处理速度达到可能的最快速度，并支持 [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html) 格式的模型或使用 [RevoScaleR](../r/ref-r-revoscaler.md) 和 [revoscalepy](../python/ref-py-revoscalepy.md) 包定型的模型。

## <a name="how-native-scoring-works"></a>本机评分的工作原理

本机评分使用可以读取 ONNX 或预定义二进制格式的模型的库，并为你提供的新数据输入生成分数。 由于已对模型进行定型、部署和存储，因此它可用于评分，而无需调用 R 或 Python 解释器。 这意味着减少了多个流程交互的开销，从而提高了预测性能。

若要使用本机评分，请调用 `PREDICT` T-SQL 函数并传递以下必需的输入：

+ 基于受支持的模型和算法的兼容模型。
+ 输入数据，通常定义为 T-SQL 查询。

此函数将返回输入数据的预测以及要传递的源数据的任何列。

## <a name="prerequisites"></a>先决条件

`PREDICT` 适用于：

+ Windows 和 Linux 上的 SQL Server 2017 及更高版本的所有版本
+ Azure SQL 托管实例
+ Azure SQL Database
+ Azure SQL Edge
+ Azure Synapse Analytics

默认启用该函数。 无需安装 R、Python 或启用其他功能。

## <a name="supported-models"></a>支持的模型

`PREDICT` 函数支持的模型格式取决于执行本机评分的 SQL 平台。 请参阅下表，了解各平台上支持的模型格式。

| 平台 | ONNX 模型格式 | RevoScale 模型格式 |
|-|-|-|
| SQL Server | 否 | 是 |
| Azure SQL 托管实例 | “是” | 是 |
| Azure SQL 数据库 | 否 | 是 |
| Azure SQL Edge | 是 | 否 |
| Azure Synapse Analytics | 是 | 否 |

::: moniker range="=azuresqldb-mi-current||=azure-sqldw-latest"
### <a name="onnx-models"></a>ONNX 模型

模型须为 [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html) 模型格式。
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azuresqldb-current"
### <a name="revoscale-models"></a>RevoScale 模型

必须使用 [RevoScaleR](../r/ref-r-revoscaler.md) 或 [revoscalepy](../python/ref-py-revoscalepy.md) 包以及下面列出的某个支持的 rx 算法对该模型进行预定型。

使用适用于 R 的 [rxSerialize](/machine-learning-server/r-reference/revoscaler/rxserializemodel) 和适用于 Python 的 [rx_serialize_model](/machine-learning-server/python-reference/revoscalepy/rx-serialize-model) 对模型进行序列化。 已对这些序列化函数进行了优化，以支持快速评分。

<a name="bkmk_native_supported_algos"></a> 

#### <a name="supported-revoscale-algorithms"></a>支持的 RevoScale 算法

Revoscalepy 和 RevoScaleR 支持以下算法。

+ revoscalepy 算法

  + [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod)
  + [rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit) 
  + [rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees) 
  + [rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree) 
  + [rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest) 

+ RevoScaleR 算法

  + [rxLinMod](/r-server/r-reference/revoscaler/rxlinmod)
  + [rxLogit](/r-server/r-reference/revoscaler/rxlogit)
  + [rxBTrees](/r-server/r-reference/revoscaler/rxbtrees)
  + [rxDtree](/r-server/r-reference/revoscaler/rxdtree)
  + [rxDForest](/r-server/r-reference/revoscaler/rxdforest)

如果需要使用 MicrosoftML 或 microsoftml 中的算法，请[使用 sp_rxPredict 实时评分](../predictions/real-time-scoring.md)。

不受支持的模型类型包括以下类型：

+ 包含其他转换的模型
+ 使用 RevoScaleR 或 revoscalepy 等效项中的 `rxGlm` 或 `rxNaiveBayes` 算法的模型
+ PMML 模型
+ 使用其他开放源代码或第三方库创建的模型
::: moniker-end

## <a name="examples"></a>示例
::: moniker range="=azuresqldb-mi-current||=azure-sqldw-latest"
### <a name="predict-with-an-onnx-model"></a>使用 ONNX 模型进行预测

此示例演示了如何使用存储在 `dbo.models` 表中的 ONNX 模型进行本机评分。

```sql
DECLARE @model VARBINARY(max) = (
        SELECT DATA
        FROM dbo.models
        WHERE id = 1
        );

WITH predict_input
AS (
    SELECT TOP (1000) [id]
        , CRIM
        , ZN
        , INDUS
        , CHAS
        , NOX
        , RM
        , AGE
        , DIS
        , RAD
        , TAX
        , PTRATIO
        , B
        , LSTAT
    FROM [dbo].[features]
    )
SELECT predict_input.id
    , p.variable1 AS MEDV
FROM PREDICT(MODEL = @model, DATA = predict_input, RUNTIME=ONNX) WITH (variable1 FLOAT) AS p;
```

> [!NOTE]
> 由于 PREDICT 返回的列和值可能因模型类型而有所不同，因此必须使用 WITH 子句定义返回数据的架构   。
::: moniker-end

::: moniker range=">=sql-server-2017||=azuresqldb-mi-current||>=sql-server-linux-2017"
### <a name="predict-with-revoscale-model"></a>使用 RevoScale 模型进行预测

此示例使用 R 中的 RevoScaleR 创建一个模型，然后调用 T-SQL 中的实时预测函数。

#### <a name="step-1-prepare-and-save-the-model"></a>步骤 1。 准备并保存模型

运行以下代码以创建示例数据库和所需的表。

```sql
CREATE DATABASE NativeScoringTest;
GO
USE NativeScoringTest;
GO
DROP TABLE IF EXISTS iris_rx_data;
GO
CREATE TABLE iris_rx_data (
    "Sepal.Length" float not null, "Sepal.Width" float not null
  , "Petal.Length" float not null, "Petal.Width" float not null
  , "Species" varchar(100) null
);
GO
```

使用以下语句，以使用 iris 数据集的数据填充数据表  。

```sql
INSERT INTO iris_rx_data ("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width" , "Species")
EXECUTE sp_execute_external_script
  @language = N'R'
  , @script = N'iris_data <- iris;'
  , @input_data_1 = N''
  , @output_data_1_name = N'iris_data';
GO
```

现在，创建用于存储模型的表。

```sql
DROP TABLE IF EXISTS ml_models;
GO
CREATE TABLE ml_models ( model_name nvarchar(100) not null primary key
  , model_version nvarchar(100) not null
  , native_model_object varbinary(max) not null);
GO
```

以下代码创建基于 iris 数据集的模型，并将其保存到名为“模型”的表中   。

```sql
DECLARE @model varbinary(max);
EXECUTE sp_execute_external_script
  @language = N'R'
  , @script = N'
    iris.sub <- c(sample(1:50, 25), sample(51:100, 25), sample(101:150, 25))
    iris.dtree <- rxDTree(Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width, data = iris[iris.sub, ])
    model <- rxSerializeModel(iris.dtree, realtimeScoringOnly = TRUE)
    '
  , @params = N'@model varbinary(max) OUTPUT'
  , @model = @model OUTPUT
  INSERT [dbo].[ml_models]([model_name], [model_version], [native_model_object])
  VALUES('iris.dtree','v1', @model) ;
```

> [!NOTE]
> 请确保使用 RevoScaleR 的 [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel) 函数保存模型。 标准 R `serialize` 函数无法生成所需格式。

可以运行如下语句，以二进制格式查看存储的模型：

```sql
SELECT *, datalength(native_model_object)/1024. as model_size_kb
FROM ml_models;
```

#### <a name="step-2-run-predict-on-the-model"></a>步骤 2. 对模型运行 PREDICT

以下简单的 PREDICT 语句使用 native scoring 函数获取决策树模型中的分类  。 它根据提供的属性、花瓣长度和宽度预测 iris 种类。

```sql
DECLARE @model varbinary(max) = (
  SELECT native_model_object
  FROM ml_models
  WHERE model_name = 'iris.dtree'
  AND model_version = 'v1');
SELECT d.*, p.*
  FROM PREDICT(MODEL = @model, DATA = dbo.iris_rx_data as d)
  WITH(setosa_Pred float, versicolor_Pred float, virginica_Pred float) as p;
go
```

如果收到错误“函数 PREDICT 执行期间出错。 模型已损坏或无效”，这通常意味着查询未返回模型。 检查键入的模型名称是否正确，或模型表是否为空。

> [!NOTE]
> 由于 PREDICT 返回的列和值可能因模型类型而有所不同，因此必须使用 WITH 子句定义返回数据的架构   。
::: moniker-end

## <a name="next-steps"></a>后续步骤

+ [PREDICT T-SQL 函数](../../t-sql/queries/predict-transact-sql.md)
+ [SQL 机器学习文档](../index.yml)
+ [在 SQL Edge 中将机器学习和 AI 与 ONNX 结合使用](/azure/azure-sql-edge/onnx-overview)
+ [在 Azure SQL Edge 中部署 ONNX 模型并用它进行预测](/azure/azure-sql-edge/deploy-onnx)