---
title: R 教程：特征工程
description: 本教程演示如何使用 SQL Server 函数创建数据特征用于数据库内分析。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 502600980a3cb1f80eaeca64284f0a189cfcb405
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272978"
---
# <a name="create-data-features-using-r-and-sql-server-walkthrough"></a>使用 R 和 SQL Server 函数创建数据特征（演练）
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

数据工程是机器学习的一个重要组成部分。 通常需要先转换数据，然后才能使用数据进行预测建模。 如果数据不具有所需的功能，可根据现有值进行创建。

对于此建模任务，想要以英里表示的上车和下车位置之间的距离，而不是使用这两个位置的原始纬度和经度值。 若要创建此特征，需要使用 [haversine 公式](https://en.wikipedia.org/wiki/Haversine_formula)计算两个点之间的直线距离。

在此步骤中，学习用于通过数据创建特征的两种不同方法：

> [!div class="checklist"]
> * 使用自定义 R 函数
> * 在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中使用自定义 T-SQL 函数

目标在于创建新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据集，其中包括原始列以及新的数值特征 *direct_distance*。

## <a name="prerequisites"></a>先决条件

此步骤基于本演练之前的步骤假设一个正在进行的 R 会话。 它使用在这些步骤中创建的连接字符串和数据源对象。 以下工具和包用于运行脚本。

+ Rgui.exe 用于运行 R 命令
+ Management Studio 用于运行 T-SQL

## <a name="featurization-using-r"></a>使用 R 进行特征化

虽然 R 语言以其丰富多样的统计库而闻名，但用户可能仍需创建自定义数据转换。

首先，让我们按照 R 用户习惯的方式进行操作：将数据获取到笔记本电脑上，然后运行自定义 R 函数 *ComputeDist*，该函数计算由纬度和经度值指定的两个点之间的直线距离。

1. 请记住，之前创建的数据源对象仅获取前 1000 行。 因此，让我们定义获取所有数据的查询。

    ```R
    bigQuery <- "SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,  pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude FROM nyctaxi_sample";
    ```

2. 使用该查询创建新的数据源对象。

    ```R
    featureDataSource <- RxSqlServerData(sqlQuery = bigQuery,colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric", dropoff_longitude = "numeric", dropoff_latitude = "numeric", passenger_count  = "numeric", trip_distance  = "numeric", trip_time_in_secs  = "numeric", direct_distance  = "numeric"), connectionString = connStr);
    ```

    - [RxSqlServerData](/r-server/r-reference/revoscaler/rxsqlserverdata) 可以采用由有效 SELECT 查询（作为 _sqlQuery_ 参数的自变量提供）组成的查询，也可以采用表对象的名称（作为 _table_ 参数提供）。
    
    - 如果想要从表中采样数据，则必须使用 _sqlQuery_ 参数，使用 T-SQL TABLESAMPLE 子句定义采样参数，并将 _rowBuffering_ 自变量设为 FALSE。

3. 运行以下代码来创建自定义 R 函数。 ComputeDist 采用两对纬度和经度值，并计算它们之间的直线距离，以英里为单位返回距离。

    ```R
    env <- new.env();
    env$ComputeDist <- function(pickup_long, pickup_lat, dropoff_long, dropoff_lat){
      R <- 6371/1.609344 #radius in mile
      delta_lat <- dropoff_lat - pickup_lat
      delta_long <- dropoff_long - pickup_long
      degrees_to_radians = pi/180.0
      a1 <- sin(delta_lat/2*degrees_to_radians)
      a2 <- as.numeric(a1)^2
      a3 <- cos(pickup_lat*degrees_to_radians)
      a4 <- cos(dropoff_lat*degrees_to_radians)
      a5 <- sin(delta_long/2*degrees_to_radians)
      a6 <- as.numeric(a5)^2
      a <- a2+a3*a4*a6
      c <- 2*atan2(sqrt(a),sqrt(1-a))
      d <- R*c
      return (d)
    }
    ```
  
    + 第一行定义一个新环境。 在 R 中，环境可用于将命名空间封装在包及类似对象中。 可以使用 `search()` 函数查看工作区中的环境。 若要查看特定环境中的对象，请键入 `ls(<envname>)`。
    + 以 `$env.ComputeDist` 开头的行包含定义 haversine 公式的代码，该公式计算球面上两点之间的 *大圆距离* 。

4. 定义函数后，可将其应用到数据，以创建新的特征列 *direct_distance*。 但在运行转换之前，请将计算上下文更改为本地。

    ```R
    rxSetComputeContext("local");
    ```

5. 调用 [rxDataStep](/r-server/r-reference/revoscaler/rxdatastep) 函数以获取特征工程数据，并将 `env$ComputeDist` 函数应用于内存中的数据。

    ```R
    start.time <- proc.time();
  
    changed_ds <- rxDataStep(inData = featureDataSource,
    transforms = list(direct_distance=ComputeDist(pickup_longitude,pickup_latitude, dropoff_longitude, dropoff_latitude),
    tipped = "tipped", fare_amount = "fare_amount", passenger_count = "passenger_count",
    trip_time_in_secs = "trip_time_in_secs",  trip_distance="trip_distance",
    pickup_datetime = "pickup_datetime",  dropoff_datetime = "dropoff_datetime"),
    transformEnvir = env,
    rowsPerRead=500,
    reportProgress = 3);
  
    used.time <- proc.time() - start.time;
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate features.", sep=""));
    ```

    + rxDataStep 函数支持用于就地修改数据的各种方法。 有关详细信息，请参阅此文章：[如何在 Microsft R 中转换和子集化数据](/r-server/r/how-to-revoscaler-data-transform)
    
    但是，关于 rxDataStep 有几点值得注意： 
    
    在其他数据源中，可以使用自变量 *varsToKeep* 和 *varsToDrop*，但 SQL Server 数据源不支持这些自变量。 因此，在此示例中，我们使用了 _transforms_ 自变量来指定传递列和转换列。 另外，在 SQL Server 计算上下文中运行时，_inData_ 自变量只能采用 SQL Server 数据源。

    当在较大的数据集上运行时，上述代码还会生成警告消息。 当行数乘以要创建的列数超过设定值（默认值为 3,000,000）时，rxDataStep 将返回警告，并且返回的数据帧中的行数将被截断。 若要删除警告，可以在 rxDataStep 函数中修改 _maxRowsByCols_ 自变量。 但是，如果 _maxRowsByCols_ 太大，则在将数据帧加载到内存中时可能会遇到问题。

7. （可选）可以调用 [rxGetVarInfo](/r-server/r-reference/revoscaler/rxgetvarinfo) 来检查转换后的数据源的架构。

    ```R
    rxGetVarInfo(data = changed_ds);
    ```

## <a name="featurization-using-transact-sql"></a>使用 Transact-SQL 进行功能化

在本练习中，学习如何使用 SQL 函数（而不是自定义 R 函数）来完成同一任务。 

切换到 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) 或其他查询编辑器，以运行 T-SQL 脚本。

1. 使用名为 *fnCalculateDistance* 的 SQL 函数。 该函数应该已经存在于 NYCTaxi_Sample 数据库中。 在对象资源管理器中，通过导航以下路径来验证该函数是否存在：“数据库”>“NYCTaxi_Sample”>“可编程性”>“函数”>“标量值函数”>“dbo.fnCalculateDistance”。

    如果该函数不存在，请使用 SQL Server Management Studio 在 NYCTaxi_Sample 数据库中生成该函数。

    ```sql
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)
    -- User-defined function calculates the direct distance between two geographical coordinates.
    RETURNS decimal(28, 10)
    AS
    BEGIN
      DECLARE @distance decimal(28, 10)
      -- Convert to radians
      SET @Lat1 = @Lat1 / 57.2958
      SET @Long1 = @Long1 / 57.2958
      SET @Lat2 = @Lat2 / 57.2958
      SET @Long2 = @Long2 / 57.2958
      -- Calculate distance
      SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
      --Convert to miles
      IF @distance <> 0
      BEGIN
        SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
      END
      RETURN @distance
    END
    ```

2. 在 Management Studio 的新查询窗口中，从任何支持 [!INCLUDE[tsql](../../includes/tsql-md.md)] 的应用程序运行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，查看该函数如何工作。

    ```sql
    USE nyctaxi_sample
    GO

    SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) as direct_distance, pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude 
    FROM nyctaxi_sample
    ```
3. 若要将值直接插入到新表（必须先创建它）中，可以添加指定表名的 **INTO** 子句。

    ```sql
    USE nyctaxi_sample
    GO

    SELECT tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance, pickup_datetime, dropoff_datetime,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) as direct_distance, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude
    INTO NewFeatureTable
    FROM nyctaxi_sample
    ```

4. 也可以从 R 代码调用 SQL 函数。 切换回 Rgui 并将 SQL 特征化查询存储到 R 变量中。

    ```R
    featureEngineeringQuery = "SELECT tipped, fare_amount, passenger_count,
        trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,
        dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance,
        pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude
        FROM nyctaxi_sample
        tablesample (1 percent) repeatable (98052)"
    ```
  
    > [!TIP]
    > 此查询经过修改，可获取更小的数据样本，从而让本演练的速度变快。 如果想要获取所有数据，可以删除 TABLESAMPLE 子句；但是，根据环境，可能无法将完整的数据集加载到 R 中，从而会导致错误。
  
5. 使用下面的代码行来从 R 环境中调用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数，并将其应用于 featureEngineeringQuery 中定义的数据。
  
    ```R
    featureDataSource = RxSqlServerData(sqlQuery = featureEngineeringQuery,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
        dropoff_longitude = "numeric", dropoff_latitude = "numeric",
        passenger_count  = "numeric", trip_distance  = "numeric",
        trip_time_in_secs  = "numeric", direct_distance  = "numeric"),
      connectionString = connStr)
    ```
  
6.  现在，新特征已创建，请调用 **rxGetVarsInfo** 来创建特征表中的数据摘要。
  
    ```R
    rxGetVarInfo(data = featureDataSource)
    ```

    **结果**

    ```R
    Var 1: tipped, Type: integer
    Var 2: fare_amount, Type: numeric
    Var 3: passenger_count, Type: numeric
    Var 4: trip_time_in_secs, Type: numeric
    Var 5: trip_distance, Type: numeric
    Var 6: pickup_datetime, Type: character
    Var 7: dropoff_datetime, Type: character
    Var 8: direct_distance, Type: numeric
    Var 9: pickup_latitude, Type: numeric
    Var 10: pickup_longitude, Type: numeric
    Var 11: dropoff_latitude, Type: numeric
    Var 12: dropoff_longitude, Type: numeric
    ```

    > [!NOTE]
    > 在某些情况下，可能会遇到以下错误：The EXECUTE permission was denied on the object 'fnCalculateDistance'（EXECUTE 权限在对象“fnCalculateDistance”上被拒绝）。如果遇到这种情况，请确保所使用的登录名具有在数据库上运行脚本和创建对象的权限，而不仅仅是在实例上具有类似权限。
    > 检查 fnCalculateDistance 对象的架构。 如果对象由数据库所有者创建，并且登录名属于 db_datareader 角色，则需要授予登录名显式权限才能运行脚本。

## <a name="comparing-r-functions-and-sql-functions"></a>比较 R 函数和 SQL 函数

还记得这段用于对 R 代码计时的代码吗？

```R
start.time <- proc.time()
<your code here>
used.time <- proc.time() - start.time
print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate features.", sep=""))
```

可以尝试将其与 SQL 自定义函数示例配合使用，查看调用 SQL 函数时数据转换需要多长时间。 另外，尝试使用 rxSetComputeContext 切换计算上下文并比较计时。

时间可能会有很大差异，具体取决于网络速度和硬件配置。 在我们测试的配置中，[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数方法比使用自定义 R 函数更快。 因此，在后续步骤中，我们将 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数用于这些计算。

> [!TIP]
> 通常，使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 的特征工程将比 R 更快。例如，T-SQL 包括可应用于常用数据科学计算（例如滚动移动平均值和 *n*-tiles）的快速窗口化和排名函数。 请根据你的数据和任务选择最高效的方法。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [生成 R 模型并保存到 SQL](walkthrough-build-and-save-the-model.md)