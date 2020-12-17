---
title: Python 教程：准备数据
titleSuffix: SQL machine learning
description: 本教程系列由四个部分组成，在第二部分中，你将通过 SQL 机器学习使用 Python 准备数据，以预测雪橇租赁次数。
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e80e3414271d520b5fd74d4a943a173a1ecdbfac
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470398"
---
# <a name="python-tutorial-prepare-data-to-train-a-linear-regression-model-with-sql-machine-learning"></a>Python 教程：准备数据以通过 SQL 机器学习训练线性回归模型
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
此教程系列包括四个部分，在第二部分中，你将使用 Python 从数据库准备数据。 在本系列的后面部分，你将在 SQL Server 机器学习服务中或大数据群集上通过 Python 使用此数据训练并部署线性回归模型。
::: moniker-end
::: moniker range="=sql-server-2017"
此教程系列包括四个部分，在第二部分中，你将使用 Python 从数据库准备数据。 在本系列的后面部分，你将通过 SQL Server 机器学习服务在 Python 中使用此数据定型和部署线性回归模型。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
此教程系列包括四个部分，在第二部分中，你将使用 Python 从数据库准备数据。 在本系列的后面部分，你将通过 Azure SQL 托管实例机器学习服务在 Python 中使用此数据定型和部署线性回归模型。
::: moniker-end

本文将指导如何进行以下操作：

> [!div class="checklist"]
> * 将数据库中的数据加载到 **pandas** 数据帧中
> * 通过删除某些列，在 Python 中准备数据

在[第一部分](python-ski-rental-linear-regression.md)中，你了解了如何还原示例数据库。

[第三部分](python-ski-rental-linear-regression-train-model.md)中已介绍如何在 Python 中定型线性回归机器学习模型。

在[第四部分](python-ski-rental-linear-regression-deploy-model.md)中，你将了解如何将模型存储到数据库中，然后根据你在第二和第三部分中开发的 Python 脚本来创建存储过程。 存储过程将在服务器上运行，以便基于新数据进行预测。

## <a name="prerequisites"></a>先决条件

* 本教程的第二部分假设你已完成[第一部分](python-ski-rental-linear-regression.md)且满足其必备条件。

## <a name="explore-and-prepare-the-data"></a>浏览和准备数据

若要在 Python 中使用数据，需要将数据库中的数据加载到 pandas 数据帧中。

在 Azure Data Studio 中创建新的 Python 笔记本，并运行以下脚本。 

下面的 Python 脚本将数据库中 dbo.rental_data 表中的数据集导入到 pandas 数据帧 df 。

在连接字符串中，根据需要替换连接详细信息。

```python
import pyodbc
import pandas
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error

# Connection string to your SQL Server instance
conn_str = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server}; SERVER=<server>; DATABASE=TutorialDB;UID=<username>;PWD=<password>)

query_str = 'SELECT Year, Month, Day, Rentalcount, Weekday, Holiday, Snow FROM dbo.rental_data'

df = pandas.read_sql(sql=query_str, con=conn_str)

print("Data frame:", df)

# Get all the columns from the dataframe.
columns = df.columns.tolist()

# Filter the columns to remove ones we don't want to use in the training
columns = [c for c in columns if c not in ["Year"]]
```

可得到类似于下面的结果。

```results
Data frame:      Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
0    2014      1   20          445        2        1     0
1    2014      2   13           40        5        0     0
2    2013      3   10          456        1        0     0
3    2014      3   31           38        2        0     0
4    2014      4   24           23        5        0     0
..    ...    ...  ...          ...      ...      ...   ...
448  2013      2   19           57        3        0     1
449  2015      3   18           26        4        0     0
450  2015      3   24           29        3        0     1
451  2014      3   26           50        4        0     1
452  2015     12    6          377        1        0     1

[453 rows x 7 columns]
```

## <a name="next-steps"></a>后续步骤

在本教程系列的第二部分中，你已完成这些步骤：

* 将数据库中的数据加载到 **pandas** 数据帧中
* 通过删除某些列，在 Python 中准备数据

若要对使用 TutorialDB 数据库中数据的机器学习模型进行定型，请按照本教程系列的第三部分进行操作：

> [!div class="nextstepaction"]
> [Python 教程：定型线性回归模型](python-ski-rental-linear-regression-train-model.md)