---
title: 在 R 中创建基于分区的模型
description: 了解如何对在使用 SQL Server 机器学习基于分区的建模功能时动态创建的分区数据进行建模、训练和使用。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 04/30/2020
ms.topic: tutorial
ms.author: davidph
author: dphansen
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 8b9d2d95bb16ea5d94245a2a50866d6ff0701b92
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470078"
---
# <a name="tutorial-create-partition-based-models-in-r-on-sql-server"></a>教程：在 SQL Server 中使用 R 创建基于分区的模型
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

在 SQL Server 2019 中，“基于分区的建模”功能可用于根据分区数据创建和训练模型。 对于自然分割为给定分类方案的分层数据（例如地理区域、日期和时间、年龄或性别），可以对整个数据集执行脚本，并能够对在所有这些操作中保持不变的分区进行建模、训练和评分。 

基于分区的建模可通过 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 上的两个新参数启用：

+ input_data_1_partition_by_columns，指定要按其进行分区的列。
+ input_data_1_order_by_columns，指定要按其进行排序的列。 

本教程介绍如何使用经典纽约市出租车示例数据和 R 脚本进行基于分区的建模。 分区列是付款方式。

> [!div class="checklist"]
> * 分区基于付款类型 (5)。
> * 在每个分区上创建和训练模型，并将对象存储在数据库中。
> * 使用为预测每个分区模型实现提示结果的可能性而保留的样本数据来预测该可能性。

## <a name="prerequisites"></a>先决条件
 
要完成本教程，必须满足下列要求：

+ 系统资源充足。 数据集较大，且训练操作需要大量资源。 如果可能，请使用至少具有 8 GB RAM 的系统。 或者，可以使用较小的数据集来应对资源约束。 有关缩减数据集的指令是内联的。 

+ 具有用于执行 T-SQL 查询的工具，如 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)。

+ 具有 [NYCTaxi_Sample](https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak)，可以将其[下载并还原](demo-data-nyctaxi-in-sql.md)到本地数据库引擎实例。 文件大小约为 90 MB。

+ SQL Server 2019 数据库引擎实例，集成了机器学习服务和 R。

+ 该教程使用[通过 ODBC 进行的从 R 脚本到 SQL Server 的环回连接](../connect/loopback-connection.md)。 因此，需要[为 SQLRUserGroup 创建登录名](../security/create-a-login-for-sqlrusergroup.md)。

通过在查询工具中以 T-SQL 查询的形式执行 `SELECT @@Version` 来检查版本。

通过返回当前与数据库引擎实例一起安装的所有 R 包的格式正确的列表，检查 R 包的可用性：

```sql
EXECUTE sp_execute_external_script
  @language=N'R',
  @script = N'str(OutputDataSet);
  packagematrix <- installed.packages();
  Name <- packagematrix[,1];
  Version <- packagematrix[,3];
  OutputDataSet <- data.frame(Name, Version);',
  @input_data_1 = N''
WITH RESULT SETS ((PackageName nvarchar(250), PackageVersion nvarchar(max) ))
```

## <a name="connect-to-the-database"></a>连接到数据库

启动 Management Studio 并连接到数据库引擎实例。 在对象资源管理器中，验证 [NYCTaxi_Sample 数据库](demo-data-nyctaxi-in-sql.md) 是否存在。 

## <a name="create-calculatedistance"></a>创建 CalculateDistance

演示数据库附带一个用于计算距离的标量函数，但我们的存储过程更适合使用表值函数。 通过运行以下脚本创建 CalculateDistance 函数，稍后的[训练步骤](#training-step)中会使用该函数。

要确认该函数已创建，请在对象资源管理器的 NYCTaxi_Sample 数据库下检查 \Programmability\Functions\Table-valued Functions。

```sql
USE NYCTaxi_sample
GO

SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE FUNCTION [dbo].[CalculateDistance] (
    @Lat1 FLOAT
    ,@Long1 FLOAT
    ,@Lat2 FLOAT
    ,@Long2 FLOAT
    )
    -- User-defined function calculates the direct distance between two geographical coordinates.
RETURNS TABLE
AS
RETURN

SELECT COALESCE(3958.75 * ATAN(SQRT(1 - POWER(t.distance, 2)) / nullif(t.distance, 0)), 0) AS direct_distance
FROM (
    VALUES (CAST((SIN(@Lat1 / 57.2958) * SIN(@Lat2 / 57.2958)) + (COS(@Lat1 / 57.2958) * COS(@Lat2 / 57.2958) * COS((@Long2 / 57.2958) - (@Long1 / 57.2958))) AS DECIMAL(28, 10)))
    ) AS t(distance)
GO
 ```

## <a name="define-a-procedure-for-creating-and-training-per-partition-models"></a>定义用于创建和训练每分区模型的过程

本教程将 R 脚本包装在存储过程中。 在本步骤中，你将创建一个存储过程，该存储过程使用 R 创建输入数据集，生成用于预测提示结果的分类模型，然后将该模型存储在数据库中。

在此脚本使用的参数输入中，可看到 input_data_1_partition_by_columns 和 input_data_1_order_by_columns 。 请记住，这些参数分区建模所依据的机制。 这些参数作为输入传递到 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 以处理分区，且外部脚本对每个分区执行一次。 

为了加快完成速度，此存储过程[使用并行执行](#parallel)。

运行此脚本后，应会在对象资源管理器中 NYCTaxi_Sample 数据库下的 \Programmability\Stored Procedures 中看到 train_rxLogIt_per_partition 。 还应看到用于存储模型的新表：nyctaxi_models。

```sql
USE NYCTaxi_Sample
GO

CREATE
    OR

ALTER PROCEDURE [dbo].[train_rxLogIt_per_partition] (@input_query NVARCHAR(max))
AS
BEGIN
    DECLARE @start DATETIME2 = SYSDATETIME()
        ,@model_generation_duration FLOAT
        ,@model VARBINARY(max)
        ,@instance_name NVARCHAR(100) = @@SERVERNAME
        ,@database_name NVARCHAR(128) = db_name();

    EXEC sp_execute_external_script @language = N'R'
        ,@script = 
        N'
    
    # Make sure InputDataSet is not empty. In parallel mode, if one thread gets zero data, an error occurs
    if (nrow(InputDataSet) > 0) {
    # Define the connection string
    connStr <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
    
    # build classification model to predict a tip outcome
    duration <- system.time(logitObj <- rxLogit(tipped ~ passenger_count + trip_distance + trip_time_in_secs + direct_distance, data = InputDataSet))[3];

    # First, serialize a model to and put it into a database table
    modelbin <- as.raw(serialize(logitObj, NULL));

    # Create the data source. To reduce data size, add rowsPerRead=500000 to cut the dataset by half.
    ds <- RxOdbcData(table="ml_models", connectionString=connStr);

    # Store the model in the database
    model_name <- paste0("nyctaxi.", InputDataSet[1,]$payment_type);
    
    rxWriteObject(ds, model_name, modelbin, version = "v1",
    keyName = "model_name", valueName = "model_object", versionName = "model_version", overwrite = TRUE, serialize = FALSE);
    } 
    
    '
        ,@input_data_1 = @input_query
        ,@input_data_1_partition_by_columns = N'payment_type'
        ,@input_data_1_order_by_columns = N'passenger_count'
        ,@parallel = 1
        ,@params = N'@instance_name nvarchar(100), @database_name nvarchar(128)'
        ,@instance_name = @instance_name
        ,@database_name = @database_name
    WITH RESULT SETS NONE
END;
GO
```

<a name="parallel"></a>

### <a name="parallel-execution"></a>并行执行

请注意，[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 输入包括 `@parallel=1`，用于启用并行处理。 与以前的版本相比，SQL Server 2019 中的 `@parallel=1` 设置为查询优化器提供更强大的提示，增大了并行执行的可能性。

默认情况下，查询优化器通常针对包含超过 256 行的表在 `@parallel=1` 下运行，但可以通过设置 `@parallel=1` 来进行显式处理，如此脚本所示。

> [!Tip]
> 对于训练工作负载，可以将 `@parallel` 用于任意训练脚本，甚至是那些使用非 Microsoft-rx 算法的脚本。 通常，只有 RevoScaleR 算法（带有 rx 前缀）支持在 SQL Server 的训练方案中并行执行。 但使用这个新参数，可以并行执行脚本来调用函数，包括并未专门设计使用该功能的开源 R 函数。 这是因为分区与特定线程关联，因此，在脚本中调用的所有操作都基于分区在给定 `thread.`<a name="training-step"></a> 上执行

## <a name="run-the-procedure-and-train-the-model"></a>运行过程并训练模型

在本部分中，将使用脚本训练在上一步中创建和保存的模型。 下面的示例演示了两种用于训练模型的方法：使用整个数据集或部分数据。 

执行此步骤需要一段时间。 训练需要进行大量计算，因此需要花费一些时间才能完成。 如果系统资源（尤其是内存）不足以承载负载，请使用数据的子集。 第二个示例提供语法。

```sql
--Example 1: train on entire dataset
EXEC train_rxLogIt_per_partition N'
SELECT payment_type, tipped, passenger_count, trip_time_in_secs, trip_distance, d.direct_distance
  FROM dbo.nyctaxi_sample CROSS APPLY [CalculateDistance](pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as d
';
GO
```

```sql
--Example 2: Train on 20 percent of the dataset to expedite processing.
EXEC train_rxLogIt_per_partition N'
  SELECT tipped, payment_type, passenger_count, trip_time_in_secs, trip_distance, d.direct_distance
  FROM dbo.nyctaxi_sample TABLESAMPLE (20 PERCENT) REPEATABLE (98074)
  CROSS APPLY [CalculateDistance](pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as d
';
GO
```

> [!NOTE]
> 如果正在运行其他工作负载，且希望要将查询处理限制为仅 2 个内核，则可以将 `OPTION(MAXDOP 2)` 追加到 SELECT 语句。

## <a name="check-results"></a>检查结果

模型表中的结果应该为 5 个不同的模型，这些模型基于 5 个划分为 5 种付款类型的分区。 模型位于 ml_models 数据源中。

```sql
SELECT *
FROM ml_models
```
 
## <a name="define-a-procedure-for-predicting-outcomes"></a>定义预测结果的过程

可以使用相同的参数进行评分。 以下示例中所含的 R 脚本使用正确的模型对当前正在处理的分区进行评分。

与之前一样，请创建一个存储过程来包装 R 代码。

```sql
USE NYCTaxi_Sample
GO

-- Stored procedure that scores per partition. 
-- Depending on the partition being processed, a model specific to that partition will be used
CREATE
    OR

ALTER PROCEDURE [dbo].[predict_per_partition]
AS
BEGIN
    DECLARE @predict_duration FLOAT
        ,@instance_name NVARCHAR(100) = @@SERVERNAME
        ,@database_name NVARCHAR(128) = db_name()
        ,@input_query NVARCHAR(max);

    SET @input_query = 'SELECT tipped, passenger_count, trip_time_in_secs, trip_distance, d.direct_distance, payment_type
                          FROM dbo.nyctaxi_sample TABLESAMPLE (1 PERCENT) REPEATABLE (98074)
                          CROSS APPLY [CalculateDistance](pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as d'

    EXEC sp_execute_external_script @language = N'R'
        ,@script = 
        N'
    
    if (nrow(InputDataSet) > 0) {

    #Get the partition that is currently being processed
    current_partition <- InputDataSet[1,]$payment_type;

    #Create the SQL query to select the right model
    query_getModel <- paste0("select model_object from ml_models where model_name = ", "''", "nyctaxi.",InputDataSet[1,]$payment_type,"''", ";")
    

    # Define the connection string
    connStr <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
        
    #Define data source to use for getting the model
    ds <- RxOdbcData(sqlQuery = query_getModel, connectionString = connStr)

    # Load the model
    modelbin <- rxReadObject(ds, deserialize = FALSE)
    # unserialize model
    logitObj <- unserialize(modelbin);

    # predict tipped or not based on model
    predictions <- rxPredict(logitObj, data = InputDataSet, overwrite = TRUE, type = "response", writeModelVars = TRUE
        , extraVarsToWrite = c("payment_type"));        
    OutputDataSet <- predictions
    
    } else {
        OutputDataSet <- data.frame(integer(), InputDataSet[,]);        
    }
    '
        ,@input_data_1 = @input_query
        ,@parallel = 1
        ,@input_data_1_partition_by_columns = N'payment_type'
        ,@params = N'@instance_name nvarchar(100), @database_name nvarchar(128)'
        ,@instance_name = @instance_name
        ,@database_name = @database_name
    WITH RESULT SETS((
                tipped_Pred INT
                ,payment_type VARCHAR(5)
                ,tipped INT
                ,passenger_count INT
                ,trip_distance FLOAT
                ,trip_time_in_secs INT
                ,direct_distance FLOAT
                ));
END;
GO
```

## <a name="create-a-table-to-store-predictions"></a>创建用于存储预测的表

```sql
CREATE TABLE prediction_results (
    tipped_Pred INT
    ,payment_type VARCHAR(5)
    ,tipped INT
    ,passenger_count INT
    ,trip_distance FLOAT
    ,trip_time_in_secs INT
    ,direct_distance FLOAT
    );

TRUNCATE TABLE prediction_results
GO
```

## <a name="run-the-procedure-and-save-predictions"></a>运行该过程并保存预测

```sql
INSERT INTO prediction_results (
    tipped_Pred
    ,payment_type
    ,tipped
    ,passenger_count
    ,trip_distance
    ,trip_time_in_secs
    ,direct_distance
    )
EXECUTE [predict_per_partition]
GO
```

## <a name="view-predictions"></a>查看预测

由于存储了预测，因此可以运行一个简单的查询来返回结果集。

```sql
SELECT *
FROM prediction_results;
```

## <a name="next-steps"></a>后续步骤

在本教程中，使用了 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 对分区数据迭代操作。 要详细了解如何在存储过程中调用外部脚本以及如何使用 RevoScaleR 函数，请继续学习以下教程。

> [!div class="nextstepaction"]
> [R 和 SQL Server 演练](walkthrough-data-science-end-to-end-walkthrough.md)