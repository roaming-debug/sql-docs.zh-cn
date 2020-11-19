---
title: Python 教程：准备群集数据
titleSuffix: SQL machine learning
description: 此教程包括四个部分，在第二部分中，你将准备 SQL 数据，以便通过 SQL 机器学习在 Python 中执行聚类分析。
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/21/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: 2cd244454b78e1199d59dcfe6539498328eac674
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870424"
---
# <a name="python-tutorial-prepare-data-to-categorize-customers-with-sql-machine-learning"></a>Python 教程：准备数据以通过 SQL 机器学习对客户进行聚类分析
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
此教程包括四分部分，在第二部分中，你将使用 Python 从数据库还原和准备数据。 在本系列的后续部分中，你将在 SQL Server 机器学习服务中或大数据群集上通过 Python 使用这些数据训练并部署聚类分析模型。
::: moniker-end
::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
此教程包括四分部分，在第二部分中，你将使用 Python 从数据库还原和准备数据。 在本系列的后续部分中，使用这些数据在 Python 中通过 SQL Server 机器学习服务来定型和部署聚类分析模型。
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
此教程包括四分部分，在第二部分中，你将使用 Python 从数据库还原和准备数据。 在本系列的后面部分，将通过 Azure SQL 托管实例机器学习服务在 Python 中使用此数据训练和部署聚类分析模型。
::: moniker-end

本文将指导如何进行以下操作：

> [!div class="checklist"]
> * 使用 Python 按不同维度对客户进行分隔
> * 将数据库中的数据加载到 Python 数据帧中

在[第一部分](python-clustering-model.md)中，你安装了必备条件并还原了示例数据库。

[第三部分](python-clustering-model-build.md)介绍如何在 Python 中创建和定型 K-Means 群集模型。

在[第四部分](python-clustering-model-deploy.md)中，你将了解如何在数据库中创建存储过程，以便基于新数据在 Python 中执行聚类分析。

## <a name="prerequisites"></a>先决条件

* 本教程的第二部分假设你已满足[第一部分](python-clustering-model.md)的必备条件。

## <a name="separate-customers"></a>分隔客户

若要准备对客户进行聚类分析，首先要将客户按以下维度进行分隔：

* orderRatio = 退单率（部分或全部退货的订单总数与订单总数的比率）
* itemsRatio = 退货率（退货总数与购买商品数量的比率）
* monetaryRatio = 退款率（退货的总货币金额与购买总金额的比率）
* frequency = 退货频率

在 Azure Data Studio 中打开一个新笔记本，然后输入以下脚本。

在连接字符串中，根据需要替换连接详细信息。

```python
# Load packages.
import pyodbc
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
from scipy.spatial import distance as sci_distance
from sklearn import cluster as sk_cluster

################################################################################################

## Connect to DB and select data

################################################################################################

# Connection string to connect to SQL Server named instance.
conn_str = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server}; SERVER=<server>; DATABASE=tpcxbb_1gb; UID=<username>; PWD=<password>')

input_query = '''SELECT
ss_customer_sk AS customer,
ROUND(COALESCE(returns_count / NULLIF(1.0*orders_count, 0), 0), 7) AS orderRatio,
ROUND(COALESCE(returns_items / NULLIF(1.0*orders_items, 0), 0), 7) AS itemsRatio,
ROUND(COALESCE(returns_money / NULLIF(1.0*orders_money, 0), 0), 7) AS monetaryRatio,
COALESCE(returns_count, 0) AS frequency
FROM
(
  SELECT
    ss_customer_sk,
    -- return order ratio
    COUNT(distinct(ss_ticket_number)) AS orders_count,
    -- return ss_item_sk ratio
    COUNT(ss_item_sk) AS orders_items,
    -- return monetary amount ratio
    SUM( ss_net_paid ) AS orders_money
  FROM store_sales s
  GROUP BY ss_customer_sk
) orders
LEFT OUTER JOIN
(
  SELECT
    sr_customer_sk,
    -- return order ratio
    count(distinct(sr_ticket_number)) as returns_count,
    -- return ss_item_sk ratio
    COUNT(sr_item_sk) as returns_items,
    -- return monetary amount ratio
    SUM( sr_return_amt ) AS returns_money
FROM store_returns
GROUP BY sr_customer_sk ) returned ON ss_customer_sk=sr_customer_sk'''


# Define the columns we wish to import.
column_info = {
    "customer": {"type": "integer"},
    "orderRatio": {"type": "integer"},
    "itemsRatio": {"type": "integer"},
    "frequency": {"type": "integer"}
}
```

## <a name="load-the-data-into-a-data-frame"></a>将数据加载到数据帧中

使用 Pandas read_sql 函数将查询结果返回到 Python。 在此过程中，将使用在前面的脚本中定义的列信息。

```python
customer_data = pandas.read_sql(input_query, conn_str)
```

现在显示数据帧的开头，验证其是否正确。

```python
print("Data frame:", customer_data.head(n=5))
```

```results
Rows Read: 37336, Total Rows Processed: 37336, Total Chunk Time: 0.172 seconds
Data frame:     customer  orderRatio  itemsRatio  monetaryRatio  frequency
0    29727.0    0.000000    0.000000       0.000000          0
1    97643.0    0.068182    0.078176       0.037034          3
2    57247.0    0.000000    0.000000       0.000000          0
3    32549.0    0.086957    0.068657       0.031281          4
4     2040.0    0.000000    0.000000       0.000000          0
```

## <a name="clean-up-resources"></a>清理资源

如果不打算继续学习本教程，请删除 tpcxbb_1gb 数据库。

## <a name="next-steps"></a>后续步骤

在本教程系列的第二部分中，你已完成这些步骤：

* 使用 Python 按不同维度对客户进行分隔
* 将数据库中的数据加载到 Python 数据帧中

若要创建使用此客户数据的机器学习模型，请按照本教程系列的第三部分进行操作：

> [!div class="nextstepaction"]
> [Python 教程：创建预测模型](python-clustering-model-build.md)
