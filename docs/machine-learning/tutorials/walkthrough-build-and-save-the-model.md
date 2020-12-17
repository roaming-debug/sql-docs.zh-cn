---
title: R 教程：生成并保存模型
description: 了解有关如何生成用于 SQL Server 数据库内分析的 R 语言机器学习模型的详细信息。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 1974c58ad2adbad3b7e136ffa36ffa88b5783fc6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470048"
---
# <a name="build-an-r-model-and-save-to-sql-server-walkthrough"></a>生成 R 模型并保存到 SQL（演练）
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

在此步骤中，你将了解如何生成机器学习模型并将该模型保存在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中。 通过保存模型，可以使用系统存储过程 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 或 [PREDICT (T-SQL) 函数](../../t-sql/queries/predict-transact-sql.md)从 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码中直接调用模型。

## <a name="prerequisites"></a>先决条件

此步骤基于本演练之前的步骤假设一个正在进行的 R 会话。 它使用在这些步骤中创建的连接字符串和数据源对象。 以下工具和包用于运行脚本。

+ Rgui.exe 用于运行 R 命令
+ Management Studio 用于运行 T-SQL
+ ROCR 包
+ RODBC 包

### <a name="create-a-stored-procedure-to-save-models"></a>创建存储过程以保存模型

此步骤使用存储过程将已定型的模型保存到 SQL Server。 创建存储过程来执行此操作可以使任务变得更加容易。

在 Management Studio 中的查询窗口中运行以下 T-SQL 代码来创建存储过程。

```sql
USE [NYCTaxi_Sample]
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'PersistModel')
  DROP PROCEDURE PersistModel
GO

CREATE PROCEDURE [dbo].[PersistModel] @m nvarchar(max)
AS
BEGIN
    -- SET NOCOUNT ON added to prevent extra result sets from
    -- interfering with SELECT statements.
    SET NOCOUNT ON;
    insert into nyc_taxi_models (model) values (convert(varbinary(max),@m,2))
END
GO
```

> [!NOTE]
> 如果遇到错误，请确保你的登录名具有创建对象的权限。 可以通过运行与下面类似的 T-SQL 语句来授予创建对象的显式权限：`exec sp_addrolemember 'db_owner', '<user_name>'`。

## <a name="create-a-classification-model-using-rxlogit"></a>使用 rxLogit 创建分类模型

模型是一个二元分类器，可以预测出租车司机是否有可能在某次载客后获得小费。 你将使用在上一课中创建的数据源，通过逻辑回归来定型小费分类器。

1. 调用包括在 [RevoScaleR](/r-server/r-reference/revoscaler/rxlogit) 包中的 **rxLogit** 函数，创建逻辑回归模型。 

    ```R
    system.time(logitObj <- rxLogit(tipped ~ passenger_count + trip_distance + trip_time_in_secs + direct_distance, data = featureDataSource));
    ```

    生成模型的调用封装在 system.time 函数中。 这样，你便可获得生成模型所需的时间。

2. 生成模型后，你可以使用 `summary` 函数来检查模型，并查看系数。

    ```R
    summary(logitObj);
    ```

    **结果**
    
    ```R
     *Logistic Regression Results for: tipped ~ passenger_count + trip_distance + trip_time_in_secs +*
     direct_distance* 
     *Data: featureDataSource (RxSqlServerData Data Source)*
     *Dependent variable(s): tipped*
     *Total independent variables: 5*
     *Number of valid observations: 17068*
     *Number of missing observations: 0*
     *-2\*LogLikelihood: 23540.0602 (Residual deviance on 17063 degrees of freedom)*
     *Coefficients:*
     *Estimate Std. Error z value Pr(>|z|)*
     *(Intercept)       -2.509e-03  3.223e-02  -0.078  0.93793*
     *passenger_count   -5.753e-02  1.088e-02  -5.289 1.23e-07 \*\*\**
     *trip_distance     -3.896e-02  1.466e-02  -2.658  0.00786 \*\**
     *trip_time_in_secs  2.115e-04  4.336e-05   4.878 1.07e-06 \*\*\**
     *direct_distance    6.156e-02  2.076e-02   2.966  0.00302 \*\**
     *---*
     *Signif. codes:  0 ‘\*\*\*’ 0.001 ‘\*\*’ 0.01 ‘\*’ 0.05 ‘.’ 0.1 ‘ ’ 1*
     *Condition number of final variance-covariance matrix: 48.3933*
     *Number of iterations: 4*
   ```

## <a name="use-the-logistic-regression-model-for-scoring"></a>使用逻辑回归模型进行计分

既然已生成模型，可以用其来预测出租车司机是否有可能在某次搭乘中获得小费。

1. 首先，使用 [RxSqlServerData](/r-server/r-reference/revoscaler/rxsqlserverdata) 函数定义用于存储评分结果的数据源对象。

    ```R
    scoredOutput <- RxSqlServerData(
      connectionString = connStr,
      table = "taxiScoreOutput"  )
    ```

    + 为了简化此示例，逻辑回归模型的输入与用于定型该模型的功能数据源 (`sql_feature_ds`) 是相同的。  更通常的情况下，可能具有一些新数据要进行评分，或者可能已留出一些数据用于测试或训练。
  
    + 预测结果将保存在表 _taxiscoreOutput_ 中。 请注意，在你使用 rxSqlServerData 创建该表时，尚未定义此表的架构。 架构是从 rxPredict 输出中获取的。
  
    + 若要创建存储预测值的表，运行 rxSqlServer 数据函数的 SQL 登录名必须在数据库中具有 DDL 特权。 如果此登录名不能创建表，语句则将失败。

2. 调用 [rxPredict](/r-server/r-reference/revoscaler/rxpredict) 函数以生成结果。

    ```R
    rxPredict(modelObject = logitObj,
        data = featureDataSource,
        outData = scoredOutput,
        predVarNames = "Score",
        type = "response",
        writeModelVars = TRUE, overwrite = TRUE)
    ```
    
    如果语句成功，则需要花费一段时间才能运行。 完成后，你可以打开 SQL Server Management Studio 并验证已创建此表并且它包含 Score 列和其他预期输出。

## <a name="plot-model-accuracy"></a>绘制模型准确性

若要了解模型的准确性，可以使用 [rxRoc](/r-server/r-reference/revoscaler/rxroc) 函数来绘制接受者操作曲线。 因为 rxRoc 是由 RevoScaleR 包提供的新函数之一，它支持远程计算上下文，因此你有以下两种选择：

+ 可以使用 rxRoc 函数在远程计算上下文中执行该绘图，然后将绘图返回到你的本地客户端。

+ 你还可以将数据导入 R 客户端计算机，并使用其他 R 绘图函数来创建性能图表。

在本节中，你将使用这两种技术。

### <a name="execute-a-plot-in-the-remote-sql-server-compute-context"></a>在远程 (SQL Server) 计算上下文中执行绘图

1. 调用函数 rxRoc 并提供之前定义的数据作为输入。

    ```R
    scoredOutput = rxImport(scoredOutput);
    rxRoc(actualVarName= "tipped", predVarNames = "Score", scoredOutput);
    ```

    此调用返回计算 ROC 图表时使用的值。 标签列是 tipped，它具有尝试要预测的实际结果，而 Score 列具有预测   。

2. 若要实际绘制图表，你可以保存此 ROC 对象，然后用绘图函数绘制它。 此图形在远程计算上下文中创建，然后返回到你的 R 环境。

    ```R
    scoredOutput = rxImport(scoredOutput);
    rocObjectOut <- rxRoc(actualVarName= "tipped", predVarNames = "Score", scoredOutput);
    plot(rocObjectOut);
    ```

    可以通过打开 R 图形设备或在 RStudio 中单击“绘制”窗口来查看图形  。

    ![模型的 ROC 绘图](media/rsql-e2e-rocplot.png "模型的 ROC 绘图")

### <a name="create-the-plots-in-the-local-compute-context-using-data-from-sql-server"></a>使用 SQL Server 中的数据在本地计算上下文中创建绘图

可以通过在命令提示符下运行 `rxGetComputeContext()` 来验证计算上下文是否为本地。 返回值应为“RxLocalSeq Compute Context”。

1. 对于本地计算上下文，此过程几乎是相同的。 使用 [rxImport](/r-server/r-reference/revoscaler/rximport) 函数将指定数据引入本地 R 环境。

    ```R
    scoredOutput = rxImport(scoredOutput)
    ```

2. 通过使用本地内存中的数据，你可以加载 ROCR 包，并使用该包中的预测函数来创建一些新的预测  。

    ```R
    library('ROCR');
    pred <- prediction(scoredOutput$Score, scoredOutput$tipped);
    ```

3. 根据输出变量 `pred` 中存储的值生成本地绘图。

    ```R
    acc.perf = performance(pred, measure = 'acc');
    plot(acc.perf);
    ind = which.max( slot(acc.perf, 'y.values')[[1]] );
    acc = slot(acc.perf, 'y.values')[[1]][ind];
    cutoff = slot(acc.perf, 'x.values')[[1]][ind];
    ```

    ![使用 R 绘制模型性能](media/rsql-e2e-performanceplot.png "使用 R 绘制模型性能")

> [!NOTE]
> 你的图表可能与这些图表有所不同，具体因你使用的数据点数量而定。

## <a name="deploy-the-model"></a>部署模型

在构建模型并确定了它的性能良好后，你可能想将它部署到你的组织中的用户或人员可以使用此模型的站点，或者可能想定期重新定型和重新校准此模型。 此过程有时称为“操作”模型  。 在 SQL Server 中，操作化是通过在存储过程中嵌入 R 代码来实现的。 由于代码驻留在过程中，因此可以从任何可连接到 SQL Server 的应用程序对代码进行调用。

从外部应用程序调用模型之前，必须将模型保存到用于生产的数据库中。 已定型的模型以二进制形式存储在 varbinary(max) 类型的单个列中  。

典型的部署工作流包括以下步骤：

1. 将模型序列化为十六进制字符串
2. 将序列化的对象传输到数据库
3. 将模型保存在 varbinary(max) 列中

本部分介绍如何使用存储过程来保留模型并使它可用于预测。 本部分中使用的存储过程为 PersistModel。 PersistModel 的定义位于[必备条件](#prerequisites)。

1. 切换回你的本地 R 环境（如果尚未使用），序列化模型，并将它保存在变量中。

    ```R
    rxSetComputeContext("local");
    modelbin <- serialize(logitObj, NULL);
    modelbinstr=paste(modelbin, collapse="");
    ```

2. 使用 RODBC 打开 ODBC 连接  。 如果已加载了包，则可以省略对 RODBC 的调用。

    ```R
    library(RODBC);
    conn <- odbcDriverConnect(connStr);
    ```

3. 针对 SQL Server 调用 PersistModel 存储过程，将序列化的对象传输到数据库，并将模型的二进制表示形式存储在列中。 

    ```R
    q <- paste("EXEC PersistModel @m='", modelbinstr,"'", sep="");
    sqlQuery (conn, q);
    ```

4. 使用 Management Studio 验证模型是否已存在。 在“对象资源管理器”中，右键单击 nyc_taxi_models 表，然后单击“选择前 1000 行”   。 在结果中，models 列中应会出现一个二进制表示形式  。

将模型保存到表仅需要 INSERT 语句。 但是，通常将模型包装在存储过程中会更轻松，例如 PersistModel  。

## <a name="next-steps"></a>后续步骤

在下一课和最后一课中，你将了解如何使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 对已保存的模型进行评分。

> [!div class="nextstepaction"]
> [在 SQL 中部署 R 模型并使用](walkthrough-deploy-and-use-the-model.md)