---
title: 教程：在 R 中训练和比较预测模型
titleSuffix: SQL machine learning
description: 此系列教程由四个部分组成，这是第三部分。你将通过 SQL 机器学习在 R 中开发两个预测模型，然后选择最准确的模型。
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 3f9cd6dd00e45f89d178bad737b4bd958e07de7c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470128"
---
# <a name="tutorial-create-a-predictive-model-in-r-with-sql-machine-learning"></a>教程：通过 SQL 机器学习在 R 中创建预测模型
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
此系列教程由四个部分组成，这是第三部分。你将在 R 中训练预测模型。在此系列的下一部分中，你将在机器学习服务中或大数据群集上将此模型部署到 SQL Server 数据库中。
::: moniker-end
::: moniker range="=sql-server-2017"
此系列教程由四个部分组成，这是第三部分。你将在 R 中训练预测模型。在此系列的下一部分中，你将通过机器学习服务将此模型部署到 SQL Server 数据库中。
::: moniker-end
::: moniker range="=sql-server-2016"
此系列教程由四个部分组成，这是第三部分，将在 R 中训练预测模型。在此系列的下一部分中，将通过 SQL Server R Services 将此模型部署到数据库中。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
此系列教程由四个部分组成，这是第三部分，将在 R 中训练预测模型。在此系列的下一部分中，将通过机器学习服务将此模型部署到 Azure SQL 托管实例数据库中。
::: moniker-end

本文将指导如何进行以下操作：

> [!div class="checklist"]
> * 训练两个机器学习模型
> * 通过这两个模型进行预测
> * 比较结果以选择最准确的模型

在[第一部分](r-predictive-model-introduction.md)中，你了解了如何还原示例数据库。

在[第二部分](r-predictive-model-prepare-data.md)中，了解了如何将数据从数据库加载到 Python 数据框中，并在 R 中准备数据。

在[第四部分](r-predictive-model-deploy.md)中，你将了解如何将模型存储到数据库中，然后通过你在第二和第三部分中开发的 Python 脚本来创建存储过程。 存储过程将在服务器上运行，以便基于新数据进行预测。

## <a name="prerequisites"></a>先决条件

本教程系列的第三部分假设你已符合[第一部分](r-predictive-model-introduction.md)的先决条件，并完成了[第二部分](r-predictive-model-prepare-data.md)中的步骤 。

## <a name="train-two-models"></a>训练两个模型

若要找出滑雪器具租赁数据的最佳模型，请创建两个不同的模型（线性回归和决策树），看哪个模型做出的预测更准确。 你将使用在本系列的第一部分创建的数据帧 `rentaldata`。

```r
#First, split the dataset into two different sets:
# one for training the model and the other for validating it
train_data = rentaldata[rentaldata$Year < 2015,];
test_data  = rentaldata[rentaldata$Year == 2015,];


#Use the RentalCount column to check the quality of the prediction against actual values
actual_counts <- test_data$RentalCount;

#Model 1: Use lm to create a linear regression model, trained with the training data set
model_lm <- lm(RentalCount ~  Month + Day + WeekDay + Snow + Holiday, data = train_data);

#Model 2: Use rpart to create a decision tree model, trained with the training data set
library(rpart);
model_rpart  <- rpart(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = train_data);
```

## <a name="make-predictions-from-both-models"></a>通过这两个模型进行预测

使用预测函数通过每个已训练的模型预测租赁计数。

```r
#Use both models to make predictions using the test data set.
predict_lm <- predict(model_lm, test_data)
predict_lm <- data.frame(RentalCount_Pred = predict_lm, RentalCount = test_data$RentalCount, 
                         Year = test_data$Year, Month = test_data$Month,
                         Day = test_data$Day, Weekday = test_data$WeekDay,
                         Snow = test_data$Snow, Holiday = test_data$Holiday)

predict_rpart  <- predict(model_rpart,  test_data)
predict_rpart <- data.frame(RentalCount_Pred = predict_rpart, RentalCount = test_data$RentalCount, 
                         Year = test_data$Year, Month = test_data$Month,
                         Day = test_data$Day, Weekday = test_data$WeekDay,
                         Snow = test_data$Snow, Holiday = test_data$Holiday)

#To verify it worked, look at the top rows of the two prediction data sets.
head(predict_lm);
head(predict_rpart);
```

```results
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1         27.45858          42       2     11     4      0       0
2        387.29344         360       3     29     1      0       0
3         16.37349          20       4     22     4      0       0
4         31.07058          42       3      6     6      0       0
5        463.97263         405       2     28     7      1       0
6        102.21695          38       1     12     2      1       0
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1          40.0000          42       2     11     4      0       0
2         332.5714         360       3     29     1      0       0
3          27.7500          20       4     22     4      0       0
4          34.2500          42       3      6     6      0       0
5         645.7059         405       2     28     7      1       0
6          40.0000          38       1     12     2      1       0
```

## <a name="compare-the-results"></a>比较结果

现在，你想了解哪个模型提供最佳预测。 一种快速简便的方法是使用基本绘图函数来查看训练数据中实际值与预测值之间的差异。

```r
#Use the plotting functionality in R to visualize the results from the predictions
par(mfrow = c(1, 1));
plot(predict_lm$RentalCount_Pred - predict_lm$RentalCount, main = "Difference between actual and predicted. lm")
plot(predict_rpart$RentalCount_Pred  - predict_rpart$RentalCount,  main = "Difference between actual and predicted. rpart")
```

![比较两个模型](./media/compare-models.png)

看起来决策树模型是两个模型中更准确的。

## <a name="clean-up-resources"></a>清理资源

如果不打算继续学习本教程，请删除 TutorialDB 数据库。

## <a name="next-steps"></a>后续步骤

在此教程系列的第三部分中，你已了解如何执行以下操作：

* 训练两个机器学习模型
* 通过这两个模型进行预测
* 比较结果以选择最准确的模型

若要部署已创建的机器学习模型，请按照本系列教程的第四部分进行操作：

> [!div class="nextstepaction"]
> [通过 SQL 机器学习在 R 中部署预测模型](r-predictive-model-deploy.md)
