---
title: Python 教程：在 SQL 存储过程中运行预测
titleSuffix: SQL machine learning
description: 在本系列教程的第五部分，你将使用 SQL 机器学习中的 T-SQL 函数来操作 SQL 存储过程中的嵌入式 Python 脚本。
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 39717778ca2456d77b6ceff4ba72e8de06dac75f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339698"
---
# <a name="python-tutorial-run-predictions-using-python-embedded-in-a-stored-procedure"></a>Python 教程：使用存储过程中嵌入的 Python 来运行预测
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

本系列教程的第五部分介绍了如何操作你在前面部分训练和保存的模型。

在此方案中，操作意味着将模型部署到生产中以进行评分。 与 SQL Server 的集成使得此操作变得非常简单，因为可以在存储过程中嵌入 Python 代码。 若要基于新输入从模型中获取预测，只需从应用程序中调用存储过程并传递新的数据。

本教程的此部分演示了两种基于 Python 模型创建预测的方法：批评分以及按行评分。

+ **批评分：** 若要提供多行输入数据，请将 SELECT 查询作为参数传递给存储过程。 结果是与输入事例相对应的观察表。
+ **单个评分：** 传递一组单独的参数值作为输入。  存储过程将返回单个行或值。

评分所需的所有 Python 代码都作为存储过程的一部分进行提供。

在本文中，你将：

> [!div class="checklist"]
> + 为批处理评分创建和使用存储过程
> + 为对单个行进行评分创建和使用存储过程

在[第一部分](python-taxi-classification-introduction.md)中，你安装了必备条件并还原了示例数据库。

在[第二部分](python-taxi-classification-explore-data.md)中，你探索了示例数据，并生成了一些绘图。

在[第三部分](python-taxi-classification-create-features.md)中，你学习了如何使用 Transact-SQL 函数根据原始数据创建特征。 然后从存储过程调用了该函数，创建了包含该功能值的表。

在[第四部分](python-taxi-classification-train-model.md)中，你加载了模块，并调用了必要的函数，以使用 SQL Server 存储过程来创建和训练模型。

## <a name="batch-scoring"></a>批评分

前两个存储过程说明了在存储过程中包装 Python 预测调用的基本语法。 这两个存储过程都需要数据表作为输入。

+ 提供要使用的确切模型的名称作为存储过程的输入参数。 该存储过程使用存储过程中的 SELECT 语句，从数据库表 `nyc_taxi_models` 中加载序列化模型。
+ 序列化模型存储在 Python 变量 `mod` 中，以便使用 Python 进行进一步处理。
+ 需要进行评分的新事例将从 `@input_data_1` 中指定的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询获得。 读取查询数据时，行保存在默认数据帧 `InputDataSet`中。
+ 两个存储过程都使用 `sklearn` 中的函数来计算准确度指标 AUC（曲线下面积）。 仅当同时提供目标标签（给小费的列）时，才能生成诸如 AUC 之类的准确度指标。 预测不需要目标标签（变量 `y`），但准确度指标需要。

  因此，如果没有要评分的数据的目标标签，则可以修改存储过程以删除 AUC 计算，并从特征中仅返回给小费的概率（存储过程中的变量 `X`）。

### <a name="predicttipscikitpy"></a>PredictTipSciKitPy

运行以下 T-SQL 语句以创建存储过程。 此存储过程需要基于 scikit-learn 包的模型，因为它使用特定于该包的函数。

包含输入的数据帧将传递到逻辑回归模型 `mod` 的 `predict_proba` 函数。 `predict_proba` 函数 (`probArray = mod.predict_proba(X)`) 返回一个 float，其表示将给小费（任意金额）的概率。

```sql
DROP PROCEDURE IF EXISTS PredictTipSciKitPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSciKitPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from sklearn import metrics

mod = pickle.loads(lmodel2)
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
y = numpy.ravel(InputDataSet[["tipped"]])

probArray = mod.predict_proba(X)
probList = []
for i in range(len(probArray)):
  probList.append((probArray[i])[1])

probArray = numpy.asarray(probList)
fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
aucResult = metrics.auc(fpr, tpr)
print ("AUC on testing data is: " + str(aucResult))

OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',  
  @input_data_1 = @inquery,
  @input_data_1_name = N'InputDataSet',
  @params = N'@lmodel2 varbinary(max)',
  @lmodel2 = @lmodel2
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttiprxpy"></a>PredictTipRxPy

此存储过程使用与先前存储过程相同的输入并创建相同的评分类型，但使用具有 SQL Server 机器学习的 revoscalepy 包中的函数。

```sql
DROP PROCEDURE IF EXISTS PredictTipRxPy;
GO

CREATE PROCEDURE [dbo].[PredictTipRxPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from sklearn import metrics
from revoscalepy.functions.RxPredict import rx_predict;

mod = pickle.loads(lmodel2)
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
y = numpy.ravel(InputDataSet[["tipped"]])

probArray = rx_predict(mod, X)
probList = probArray["tipped_Pred"].values 

probArray = numpy.asarray(probList)
fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
aucResult = metrics.auc(fpr, tpr)
print ("AUC on testing data is: " + str(aucResult))

OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @input_data_1_name = N'InputDataSet',
  @params = N'@lmodel2 varbinary(max)',
  @lmodel2 = @lmodel2
WITH RESULT SETS ((Score float));
END
GO
```

## <a name="run-batch-scoring-using-a-select-query"></a>使用 SELECT 查询运行批评分

PredictTipSciKitPy 存储过程和 PredictTipRxPy 存储过程需要两个输入参数：

+ 检索用于评分的数据的查询
+ 已定型模型的名称

通过将这些参数传递给存储过程，可选择特定的模型或更改用于评分的数据。

1. 若要使用 scikit-learn 模型进行评分，请调用存储过程 PredictTipSciKitPy，并将模型名称和查询字符串作为输入传递。

   ```sql
   DECLARE @query_string nvarchar(max) -- Specify input query
     SET @query_string='
     select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance,
     dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
     from nyctaxi_sample_testing'
   EXEC [dbo].[PredictTipSciKitPy] 'SciKit_model', @query_string;
   ```

   存储过程返回作为输入查询的一部分传递的每个行程的预测概率。 

   如果使用 SSMS (SQL Server Management Studio) 来运行查询，则概率将在“结果”窗格中显示为表。 “消息”窗格输出精度指标（AUC 或曲线下面积），值约 0.56。

2. 若要使用 revoscalepy 模型进行评分，请调用存储过程 PredictTipRxPy，并将模型名称和查询字符串作为输入传递。

   ```sql
   DECLARE @query_string nvarchar(max) -- Specify input query
     SET @query_string='
     select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance,
     dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
     from nyctaxi_sample_testing'
   EXEC [dbo].[PredictTipRxPy] 'revoscalepy_model', @query_string;
   ```

## <a name="single-row-scoring"></a>单行评分

有时，你可能不想进行批评分，而是想要传递单个事例，从应用程序中获取值，并基于这些值返回单个结果。 例如，可以设置 Excel 工作表、Web 应用或报表来调用存储过程，并将其传递给用户键入或选择的输入。

在本节中，你将了解如何通过调用两个存储过程来创建单个预测：

+ [PredictTipSingleModeSciKitPy](#predicttipsinglemodescikitpy) 旨在使用 scikit-learn 模型进行单行评分。
+ [PredictTipSingleModeRxPy](#predicttipsinglemoderxpy) 旨在使用 revoscalepy 模型进行单行评分。
+ 如果尚未训练模型，请返回[第 5 部分](python-taxi-classification-train-model.md)！

这两种模型都将一系列单个值作为输入，例如乘客人数、行程距离等。 表值函数 (`fnEngineerFeatures`) 用于将输入中的纬度和经度值转换为新特征和直接距离。 [第 4 部分](python-taxi-classification-create-features.md)包含对此表值函数的说明。

两个存储过程都基于 Python 模型创建分数。

> [!NOTE]
>
> 从外部应用程序调用存储过程时，必须提供 Python 模型所需的所有输入功能，这一点很重要。 为避免错误，除了验证数据类型和数据长度外，还可能需要将输入数据强制转换或转换为 Python 数据类型。

### <a name="predicttipsinglemodescikitpy"></a>PredictTipSingleModeSciKitPy

花一点时间查看使用 scikit-learn 模型执行评分的存储过程的代码。

```sql
DROP PROCEDURE IF EXISTS PredictTipSingleModeSciKitPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSingleModeSciKitPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
  DECLARE @inquery nvarchar(max) = N'
  SELECT * FROM [dbo].[fnEngineerFeatures]( 
    @passenger_count,
    @trip_distance,
    @trip_time_in_secs,
    @pickup_latitude,
    @pickup_longitude,
    @dropoff_latitude,
    @dropoff_longitude)
    '
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;

# Load model and unserialize
mod = pickle.loads(model)

# Get features for scoring from input data
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]

# Score data to get tip prediction probability as a list (of float)
probList = []
probList.append((mod.predict_proba(X)[0])[1])

# Create output data frame
OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
    @trip_time_in_secs int ,
    @pickup_latitude float ,
    @pickup_longitude float ,
    @dropoff_latitude float ,
    @dropoff_longitude float',
    @model = @lmodel2,
    @passenger_count =@passenger_count ,
    @trip_distance=@trip_distance,
    @trip_time_in_secs=@trip_time_in_secs,
    @pickup_latitude=@pickup_latitude,
    @pickup_longitude=@pickup_longitude,
    @dropoff_latitude=@dropoff_latitude,
    @dropoff_longitude=@dropoff_longitude
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttipsinglemoderxpy"></a>PredictTipSingleModeRxPy

以下存储过程使用 revoscalepy 模型执行评分。

```sql
DROP PROCEDURE IF EXISTS PredictTipSingleModeRxPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSingleModeRxPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
DECLARE @inquery nvarchar(max) = N'
  SELECT * FROM [dbo].[fnEngineerFeatures]( 
    @passenger_count,
    @trip_distance,
    @trip_time_in_secs,
    @pickup_latitude,
    @pickup_longitude,
    @dropoff_latitude,
    @dropoff_longitude)
  '
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from revoscalepy.functions.RxPredict import rx_predict;

# Load model and unserialize
mod = pickle.loads(model)

# Get features for scoring from input data
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]

# Score data to get tip prediction probability as a list (of float)

probArray = rx_predict(mod, X)

probList = []
probList = probArray["tipped_Pred"].values

# Create output data frame
OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
    @trip_time_in_secs int ,
    @pickup_latitude float ,
    @pickup_longitude float ,
    @dropoff_latitude float ,
    @dropoff_longitude float',
    @model = @lmodel2,
    @passenger_count =@passenger_count ,
    @trip_distance=@trip_distance,
    @trip_time_in_secs=@trip_time_in_secs,
    @pickup_latitude=@pickup_latitude,
    @pickup_longitude=@pickup_longitude,
    @dropoff_latitude=@dropoff_latitude,
    @dropoff_longitude=@dropoff_longitude
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="generate-scores-from-models"></a>从模型生成分数

创建存储过程后，就可以轻松地根据每个模型生成分数。 只需打开新的“查询”窗口，然后为每个特征列键入或粘贴参数。 这些特征列的七个必需值依次为：

+ *passenger_count*
+ *trip_distance*
+ *trip_time_in_secs*
+ *pickup_latitude*
+ *pickup_longitude*
+ *dropoff_latitude*
+ *dropoff_longitude*

1. 若要使用 revoscalepy 模型生成预测，请运行以下语句：
  
   ```sql
   EXEC [dbo].[PredictTipSingleModeRxPy] 'revoscalepy_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

2. 若要使用 scikit-learn 模型生成分数，请运行以下语句：

   ```sql
   EXEC [dbo].[PredictTipSingleModeSciKitPy] 'SciKit_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

这两个过程的输出都是使用指定参数或特征预测出租车行程支付小费的概率。

## <a name="conclusions"></a>结论

在本教程系列中，你已了解如何使用存储过程中嵌入的 Python 代码。 借助与 [!INCLUDE[tsql](../../includes/tsql-md.md)] 的集成，可更轻松地部署 Python 模型进行预测，并合并重新定型为企业数据工作流一部分的模型。

## <a name="next-steps"></a>后续步骤

本文内容：

> [!div class="checklist"]
> + 为批处理评分创建和使用存储过程
> + 为对单个行进行评分创建和使用存储过程

有关 Python 的详细信息，请参阅 [SQL Server 中的 Python 扩展](../concepts/extension-python.md)。
