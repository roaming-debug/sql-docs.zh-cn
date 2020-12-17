---
title: 教程：在 R 中部署预测模型
titleSuffix: SQL machine learning
description: 在这个由四部分组成的教程的第四部分中，你将通过 SQL 机器学习在 R 中部署预测模型。
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: bc5cb5f0b5a79b7e9ff81171e0a1b7b31ad580db
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470158"
---
# <a name="tutorial-deploy-a-predictive-model-in-r-with-sql-machine-learning"></a>教程：通过 SQL 机器学习在 R 中部署预测模型
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
在这个由四个部分组成的教程系列的第四部分中，你将把用 R 开发的机器学习模型部署到 SQL Server 机器学习服务或大数据群集中。
::: moniker-end
::: moniker range="=sql-server-2017"
在这个由四个部分组成的教程系列的第四部分中，你将使用机器学习服务将在 R 中开发的机器学习模型部署到 SQL Server 中。
::: moniker-end
::: moniker range="=sql-server-2016"
在这个由四个部分组成的教程系列的第四部分中，你将使用 SQL Server R Services 将在 R 中开发的机器学习模型部署到 SQL Server 中。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
在这个由四个部分组成的教程系列的第四部分中，你将使用机器学习服务将在 R 中开发的机器学习模型部署到 Azure SQL 托管实例中。
::: moniker-end

本文将指导如何进行以下操作：

> [!div class="checklist"]
> * 创建生成机器学习模型的存储过程
> * 将模型存储在数据库表中
> * 创建使用模型进行预测的存储过程
> * 使用新数据执行模型

在[第一部分](r-predictive-model-introduction.md)中，你了解了如何还原示例数据库。

在[第二部分](r-predictive-model-prepare-data.md)中，你了解了如何导入示例数据库，然后准备要用于通过 R 训练预测模型的数据。

在[第三部分](r-predictive-model-train.md)中，你了解了如何使用 R 创建和训练多个机器学习模型，然后选择最准确的模型。

## <a name="prerequisites"></a>先决条件

本教程的第四部分假设你已符合[第一部分](r-predictive-model-introduction.md)的先决条件，并完成了[第二部分](r-predictive-model-prepare-data.md)和[第三部分](r-predictive-model-train.md)的步骤  。

## <a name="create-a-stored-procedure-that-generates-the-model"></a>创建生成模型的存储过程

在本教程系列的第三部分中，你已确定某个决策树 (dtree) 模型是最准确的。 现在，使用已开发的 R 脚本创建一个存储过程 (`generate_rental_model`)，以使用 R 包中的 rpart 训练并生成 dtree 模型。

在 Azure Data Studio 中运行以下命令。

```sql
USE [TutorialDB]
DROP PROCEDURE IF EXISTS generate_rental_model;
GO
CREATE PROCEDURE generate_rental_model (@trained_model VARBINARY(max) OUTPUT)
AS
BEGIN
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
rental_train_data$Month   <- factor(rental_train_data$Month);
rental_train_data$Day     <- factor(rental_train_data$Day);
rental_train_data$Holiday <- factor(rental_train_data$Holiday);
rental_train_data$Snow    <- factor(rental_train_data$Snow);
rental_train_data$WeekDay <- factor(rental_train_data$WeekDay);

#Create a dtree model and train it using the training data set
library(rpart);
model_dtree <- rpart(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = rental_train_data);
#Serialize the model before saving it to the database table
trained_model <- as.raw(serialize(model_dtree, connection=NULL));
'
        , @input_data_1 = N'
            SELECT RentalCount
                 , Year
                 , Month
                 , Day
                 , WeekDay
                 , Snow
                 , Holiday
            FROM dbo.rental_data
            WHERE Year < 2015
            '
        , @input_data_1_name = N'rental_train_data'
        , @params = N'@trained_model varbinary(max) OUTPUT'
        , @trained_model = @trained_model OUTPUT;
END;
GO
```

## <a name="store-the-model-in-a-database-table"></a>将模型存储在数据库表中

在 TutorialDB 数据库中创建一个表，然后将模型保存到表中。

1. 创建表 (`rental_models`) 来存储模型。

    ```sql
    USE TutorialDB;
    DROP TABLE IF EXISTS rental_models;
    GO
    CREATE TABLE rental_models (
          model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(MAX) NOT NULL
        );
    GO
    ```

1. 将模型作为二进制对象保存到表中，模型名为“DTree”。

    ```sql
    -- Save model to table
    TRUNCATE TABLE rental_models;
    
    DECLARE @model VARBINARY(MAX);
    
    EXECUTE generate_rental_model @model OUTPUT;
    
    INSERT INTO rental_models (
          model_name
        , model
        )
    VALUES (
         'DTree'
        , @model
        );
    
    SELECT *
    FROM rental_models;
    ```

## <a name="create-a-stored-procedure-that-makes-predictions"></a>创建进行预测的存储过程

创建存储过程 (`predict_rentalcount_new`)，使用训练的模型和一组新数据进行预测。

```sql
-- Stored procedure that takes model name and new data as input parameters and predicts the rental count for the new data
USE [TutorialDB]
DROP PROCEDURE IF EXISTS predict_rentalcount_new;
GO
CREATE PROCEDURE predict_rentalcount_new (
      @model_name VARCHAR(100)
    , @input_query NVARCHAR(MAX)
    )
AS
BEGIN
    DECLARE @model VARBINARY(MAX) = (
            SELECT model
            FROM rental_models
            WHERE model_name = @model_name
            );

    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
#Convert types to factors
rentals$Month   <- factor(rentals$Month);
rentals$Day     <- factor(rentals$Day);
rentals$Holiday <- factor(rentals$Holiday);
rentals$Snow    <- factor(rentals$Snow);
rentals$WeekDay <- factor(rentals$WeekDay);

#Before using the model to predict, we need to unserialize it
rental_model <- unserialize(model);

#Call prediction function
rental_predictions <- predict(rental_model, rentals);
rental_predictions <- data.frame(rental_predictions);
'
        , @input_data_1 = @input_query
        , @input_data_1_name = N'rentals'
        , @output_data_1_name = N'rental_predictions'
        , @params = N'@model varbinary(max)'
        , @model = @model
    WITH RESULT SETS(("RentalCount_Predicted" FLOAT));
END;
GO
```

## <a name="execute-the-model-with-new-data"></a>使用新数据执行模型

现在，可使用存储过程 `predict_rentalcount_new` 来预测新数据的租借计数。

```sql
-- Use the predict_rentalcount_new stored procedure with the model name and a set of features to predict the rental count
EXECUTE dbo.predict_rentalcount_new @model_name = 'DTree'
    , @input_query = '
        SELECT CONVERT(INT,  3) AS Month
             , CONVERT(INT, 24) AS Day
             , CONVERT(INT,  4) AS WeekDay
             , CONVERT(INT,  1) AS Snow
             , CONVERT(INT,  1) AS Holiday
        ';
GO
```

应看到类似于以下的结果。

```results
RentalCount_Predicted
332.571428571429
```

你已在数据库中成功创建、训练并部署了一个模型。 然后，你在存储过程中使用了该模型来基于新数据对值进行预测。


## <a name="clean-up-resources"></a>清理资源

使用完 TutorialDB 数据库后，将其从服务器中删除。

## <a name="next-steps"></a>后续步骤

在此教程系列的第四部分中，你已了解如何执行以下操作：

* 创建生成机器学习模型的存储过程
* 将模型存储在数据库表中
* 创建使用模型进行预测的存储过程
* 使用新数据执行模型

若要详细了解如何在机器学习服务中使用 R，请参阅：

* [运行简单的 R 脚本](quickstart-r-create-script.md)
* [R 数据结构、类型和对象](quickstart-r-data-types-and-objects.md)
* [R 函数](quickstart-r-functions.md)
