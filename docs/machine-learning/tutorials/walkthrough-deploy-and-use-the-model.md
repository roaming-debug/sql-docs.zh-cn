---
title: R 教程：部署模型
description: 了解如何通过从存储过程中调用经过定型的模型在生产环境中部署 R 模型。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 1830c441e51465f9c7cf7eab7ca503eadd677caf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272968"
---
# <a name="deploy-the-r-model-and-use-it-in-sql-server-walkthrough"></a>部署 R 模型并在 SQL Server 中使用它（演练）
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

在本课程中，了解如何通过从存储过程中调用经过定型的模型在生产环境中部署 R 模型。 可以通过 R 或支持 [!INCLUDE[tsql](../../includes/tsql-md.md)] 的任何应用程序编程语言（如 C#、Java、Python 等）调用存储过程，并使用模型对新的观测值进行预测。

本文介绍了在评分中使用模型的两种最常见的方法：

> [!div class="checklist"]
> * 批处理计分模式，可生成多个预测 
> * 个体计分模式，一次生成一个预测 

## <a name="batch-scoring"></a>批评分

创建一个存储过程“PredictTipBatchMode”，该存储过程生成多个预测，并传递一个 SQL 查询或表作为输入  。 返回一个结果表，可以将其直接插入表中或写入文件。

- 以 SQL 查询的形式获取一组输入数据
- 调用在上一课中保存的已定型逻辑回归模型
- 预测司机获得非零小费的概率

1. 在 Management Studio 中，打开一个新的查询窗口，然后运行以下 T-SQL 脚本来创建 PredictTipBatchMode 存储过程。
  
    ```sql
    USE [NYCTaxi_Sample]
    GO

    SET ANSI_NULLS ON
    GO
    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'PredictTipBatchMode')
    DROP PROCEDURE v
    GO

    CREATE PROCEDURE [dbo].[PredictTipBatchMode] @input nvarchar(max)
    AS
    BEGIN
      DECLARE @lmodel2 varbinary(max) = (SELECT TOP 1 model  FROM nyc_taxi_models);
      EXEC sp_execute_external_script @language = N'R',
         @script = N'
           mod <- unserialize(as.raw(model));
           print(summary(mod))
           OutputDataSet<-rxPredict(modelObject = mod,
             data = InputDataSet,
             outData = NULL,
             predVarNames = "Score", type = "response",
             writeModelVars = FALSE, overwrite = TRUE);
           str(OutputDataSet)
           print(OutputDataSet)',
      @input_data_1 = @input,
      @params = N'@model varbinary(max)',
      @model = @lmodel2
      WITH RESULT SETS ((Score float));
    END
    ```

    + 使用 SELECT 语句从 SQL 表中调用存储模型。 从表中检索模型，将其作为 varbinary(max) 数据存储在 SQL 变量 \@lmodel2 中，并作为参数 mod 传递给系统存储过程 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)    。

    + 用作计分输入的数据定义为 SQL 查询，并作为字符串存储在 SQL 变量 \@input 中  。 从数据库中检索数据时，数据存储在一个名为 InputDataSet  的数据框中，该数据框只是 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 过程的输入数据的默认名称；可以根据需要使用参数 \@input_data_1_name  定义其他变量名。

    + 为了生成分数，存储过程会从 RevoScaleR 库调用 rxPredict 函数  。

    + 返回值 Score 是给定模型的司机获得小费的概率  。 （可选）可轻松将某种类型的筛选器应用于返回值，将返回值分类到“有小费”和“无小费”组中。  例如，概率小于 0.5 意味着可能无小费。
  
2.  若要以批处理模式调用存储过程，则需定义所需的查询作为存储过程的输入。 下面是 SQL 查询，可以在 SSMS 中运行该查询以验证其是否有效。

    ```sql
    SELECT TOP 10
      a.passenger_count AS passenger_count,
      a.trip_time_in_secs AS trip_time_in_secs,
      a.trip_distance AS trip_distance,
      a.dropoff_datetime AS dropoff_datetime,
      dbo.fnCalculateDistance( pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS direct_distance
      FROM 
        (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude 
        FROM nyctaxi_sample)a 
      LEFT OUTER JOIN
      ( SELECT medallion, hack_license, pickup_datetime
      FROM nyctaxi_sample  tablesample (1 percent) repeatable (98052)  )b
      ON a.medallion=b.medallion
      AND a.hack_license=b.hack_license
      AND a.pickup_datetime=b.pickup_datetime
      WHERE b.medallion is null
    ```

3. 使用此 R 代码从 SQL 查询创建输入字符串：

    ```R
    input <- "N'SELECT TOP 10 a.passenger_count AS passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS direct_distance FROM (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample)a LEFT OUTER JOIN ( SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample  tablesample (1 percent) repeatable (98052)  )b ON a.medallion=b.medallion AND a.hack_license=b.hack_license AND  a.pickup_datetime=b.pickup_datetime WHERE b.medallion is null'";
    q <- paste("EXEC PredictTipBatchMode @input = ", input, sep="");
    ```

4. 若要从 R 运行存储过程，请调用 RODBC 包的 sqlQuery 方法，并使用之前定义的 SQL 连接 `conn`   ：

    ```R
    sqlQuery (conn, q);
    ```

    如果收到 ODBC 错误，请检查语法错误以及引号是否正确。 
    
    如果出现权限错误，请确保登录名能够执行存储过程。

## <a name="single-row-scoring"></a>单行计分

单行计分模式一次生成一个预测，将一组个体值作为输入传递给存储过程。 这些值对应于模型中的功能，模型可使用这些特征来创建预测或生成其他结果，如概率值。 然后，可以将该值返回给应用程序或用户。

逐行调用模型进行预测时，传递一组代表每种单独情况的功能的值。 然后，存储过程返回单个预测或概率。 

存储过程 PredictTipSingleMode 演示了这种方法  。 它以代表功能值的多个参数（例如，乘客数量和行程距离）作为输入，使用存储的 R 模型对这些功能进行计分，并输出有小费概率。

1. 运行以下 Transact-SQL 语句以创建存储过程。

    ```sql
    USE [NYCTaxi_Sample]
    GO

    SET ANSI_NULLS ON
    GO
    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'PredictTipSingleMode')
    DROP PROCEDURE v
    GO

    CREATE PROCEDURE [dbo].[PredictTipSingleMode] @passenger_count int = 0,
    @trip_distance float = 0,
    @trip_time_in_secs int = 0,
    @pickup_latitude float = 0,
    @pickup_longitude float = 0,
    @dropoff_latitude float = 0,
    @dropoff_longitude float = 0
    AS
    BEGIN
      DECLARE @inquery nvarchar(max) = N'
        SELECT * FROM [dbo].[fnEngineerFeatures](@passenger_count, @trip_distance, @trip_time_in_secs, @pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude)'
      DECLARE @lmodel2 varbinary(max) = (SELECT TOP 1 model FROM nyc_taxi_models);

      EXEC sp_execute_external_script @language = N'R',  @script = N'
            mod <- unserialize(as.raw(model));
            print(summary(mod))
            OutputDataSet<-rxPredict(
              modelObject = mod,
              data = InputDataSet,
              outData = NULL,
              predVarNames = "Score",
              type = "response",
              writeModelVars = FALSE,
              overwrite = TRUE);
            str(OutputDataSet)
            print(OutputDataSet)
            ',
      @input_data_1 = @inquery,
      @params = N'
      -- passthrough columns
      @model varbinary(max) ,
      @passenger_count int ,
      @trip_distance float ,
      @trip_time_in_secs int ,
      @pickup_latitude float ,
      @pickup_longitude float ,
      @dropoff_latitude float ,
      @dropoff_longitude float',
      -- mapped variables
      @model = @lmodel2 ,
      @passenger_count =@passenger_count ,
      @trip_distance=@trip_distance ,
      @trip_time_in_secs=@trip_time_in_secs ,
      @pickup_latitude=@pickup_latitude ,
      @pickup_longitude=@pickup_longitude ,
      @dropoff_latitude=@dropoff_latitude ,
      @dropoff_longitude=@dropoff_longitude
      WITH RESULT SETS ((Score float));
    END
    ```

2. 在 SQL Server Management Studio 中，可使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] EXEC 过程（或 EXECUTE）调用存储过程，并将其传递给所需的输入   。 例如，尝试在 Management Studio 中运行以下语句：

    ```sql
    EXEC [dbo].[PredictTipSingleMode] 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
    ```

    此处传递的值分别用于变量 passenger\_count、trip_distance、trip\_time\_in\_secs、pickup\_latitude、pickup\_longitude、dropoff\_latitude 和 dropoff\_longitude        。

3. 若要从 R 代码运行此相同的调用，只需定义包含整个存储过程调用的 R 变量，如下所示：

    ```R
    q2 = "EXEC PredictTipSingleMode 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303 ";
    ```

    此处传递的值分别是变量 passenger\_count、trip\_distance、trip\_time\_in\_secs、pickup\_latitude、pickup\_longitude、dropoff\_latitude 和 dropoff\_longitude        。

4. 调用 `sqlQuery`（从 RODBC 包中）并传递连接字符串和包含存储过程调用的字符串变量  。

    ```R
    # predict with stored procedure in single mode
    sqlQuery (conn, q2);
    ```

    >[!TIP]
    > 针对 Visual Studio 的 R 工具 (RTVS) 提供了与 SQL Server 和 R 的良好集成。有关将 RODBC 与 SQL Server 连接一起使用的更多示例，请参阅：[使用 SQL Server 和 R](/visualstudio/rtvs/sql-server)

## <a name="next-steps"></a>后续步骤

现在，你已学习了如何处理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据并将已定型的 R 模型保存到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，那么基于此数据集创建一些新模型对于你而言应该比较容易。 例如，你可能会尝试创建类似以下这些的模型：

+ 预测小费金额的回归模型
+ 预测小费是较多、中等还是较少的多类分类模型

你可能还想了解以下其他示例和资源：

+ [数据科学方案和解决方案模板](data-science-scenarios-and-solution-templates.md)
+ [数据库内高级分析](r-taxi-classification-introduction.md)
+ [Machine Learning Server 操作指南](/machine-learning-server/r/how-to-introduction)
+ [Machine Learning Server 其他资源](/machine-learning-server/resources-more)