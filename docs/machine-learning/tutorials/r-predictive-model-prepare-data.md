---
title: 教程：准备数据以在 R 中训练预测模型
titleSuffix: SQL machine learning
description: 此系列教程由四个部分组成，这是第二部分。你将准备数据，以便通过 SQL 机器学习在 R 中训练预测模型。
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: a4a12d71818ad4b900a7959904c47cb0baad4357
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870294"
---
# <a name="tutorial-prepare-data-to-train-a-predictive-model-in-r-with-sql-machine-learning"></a>教程：准备数据以通过 SQL 机器学习在 R 中训练预测模型
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
此系列教程由四个部分组成，这是第二部分。你将使用 R 从数据库准备数据。在此系列的后面部分，你将在 SQL Server 机器学习服务中或大数据群集上通过 R 使用该数据训练并部署预测模型。
::: moniker-end
::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
此系列教程由四个部分组成，这是第二部分。你将使用 R 从数据库准备数据。在此系列的后面部分，你将在 SQL Server 机器学习服务中通过 R 使用该数据训练并部署预测模型。
::: moniker-end
::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
此系列教程由四个部分组成，这是第二部分。你将使用 R 从数据库准备数据。在此系列的后面部分，你将在 SQL Server R Services 中通过 R 使用该数据训练并部署预测模型。
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
此系列教程由四个部分组成，这是第二部分，将使用 R 从数据库准备数据。在此系列的后面部分，将通过 Azure SQL 托管实例机器学习服务使用此数据来训练预测模型并将其部署到 R 中。
::: moniker-end

本文将指导如何进行以下操作：

> [!div class="checklist"]
> * 将示例数据库还原成一个数据库
> * 将数据库中的数据加载到 R 数据帧中
> * 将某些列标识为类别列以使用 R 准备数据

在[第一部分](r-predictive-model-introduction.md)中，你了解了如何还原示例数据库。

在[第三部分](r-predictive-model-train.md)中，你将了解如何在 R 中训练机器学习模型。

在[第四部分](r-predictive-model-deploy.md)中，你将了解如何将模型存储到数据库中，然后通过你在第二和第三部分中开发的 R 脚本来创建存储过程。 存储过程将在服务器上运行，以便基于新数据进行预测。

## <a name="prerequisites"></a>先决条件

本教程的第二部分假定你已完成 [**第一部分**](r-predictive-model-introduction.md)及其先决条件。

## <a name="load-the-data-into-a-data-frame"></a>将数据加载到数据帧中

若要在 R 中使用这些数据，请将数据库中的数据加载到数据帧 (`rentaldata`) 中。

在 RStudio 中创建新的 RScript 文件并运行以下脚本。 将 **ServerName** 替换为你自己的连接信息。

```r
#Define the connection string to connect to the TutorialDB database
connStr <- "Driver=SQL Server;Server=ServerName;Database=TutorialDB;uid=Username;pwd=Password"


#Get the data from the table
library(RODBC)

ch <- odbcDriverConnect(connStr)

#Import the data from the table
rentaldata <- sqlFetch(ch, "dbo.rental_data")

#Take a look at the structure of the data and the top rows
head(rentaldata)
str(rentaldata)
```

可得到类似于下面的结果。

```results
   Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
1  2014    1     20      445         2        1      0
2  2014    2     13       40         5        0      0
3  2013    3     10      456         1        0      0
4  2014    3     31       38         2        0      0
5  2014    4     24       23         5        0      0
6  2015    2     11       42         4        0      0
'data.frame':       453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : num  2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : int  1 0 0 0 0 0 0 0 0 0 ...
$ Snow       : num  0 0 0 0 0 0 0 0 0 0 ...
```

## <a name="prepare-the-data"></a>准备数据

在此示例数据库中，大部分准备工作已经完成，但你需在此处再做一次准备工作。
通过将数据类型更改为“因素”，使用下面的 R 脚本将三列标识为“类别” 。



```r
#Changing the three factor columns to factor types
rentaldata$Holiday <- factor(rentaldata$Holiday);
rentaldata$Snow    <- factor(rentaldata$Snow);
rentaldata$WeekDay <- factor(rentaldata$WeekDay);



#Visualize the dataset after the change
str(rentaldata);
```

可得到类似于下面的结果。

```results
data.frame':      453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : Factor w/ 7 levels "1","2","3","4",..: 2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : Factor w/ 2 levels "0","1": 2 1 1 1 1 1 1 1 1 1 ...
$ Snow       : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
```

数据现在已准备好供培训使用。

## <a name="clean-up-resources"></a>清理资源

如果不打算继续学习本教程，请删除 TutorialDB 数据库。

## <a name="next-steps"></a>后续步骤

在此教程系列的第二部分中，你已了解如何执行以下操作：

* 将示例数据加载到 R 数据框中
* 将某些列标识为类别列以使用 R 准备数据

若要创建使用 TutorialDB 数据库中数据的机器学习模型，请按照本教程系列的第三部分进行操作：

> [!div class="nextstepaction"]
> [通过 SQL 机器学习在 R 中创建预测模型](r-predictive-model-train.md)
