---
title: 教程：准备数据以在 R 中执行聚类
titleSuffix: SQL machine learning
description: 此系列教程由四个部分组成，这是第二部分。你将从数据库准备数据，以便通过 SQL 机器学习在 R 中执行聚类分析。
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 31e6b8705054001cb603af324ffb87d55bc18100
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470208"
---
# <a name="tutorial-prepare-data-to-perform-clustering-in-r-with-sql-machine-learning"></a>教程：准备数据以通过 SQL 机器学习在 R 中执行聚类分析
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
此系列教程由四个部分组成，这是第二部分。你将从数据库准备数据，以便在 SQL Server 机器学习服务中或大数据群集上通过 R 执行聚类分析。
::: moniker-end
::: moniker range="=sql-server-2017"
此系列教程由四个部分组成，这是第二部分。你将从数据库准备数据，以便通过 SQL Server 机器学习服务在 R 中执行聚类分析。
::: moniker-end
::: moniker range="=sql-server-2016"
此系列教程由四个部分组成，这是第二部分。你将从数据库准备数据，以便通过 SQL Server 2016 R Services 在 R 中执行聚类分析。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
此系列教程由四个部分组成，这是第二部分。你将从数据库准备数据，以便通过 Azure SQL 托管实例机器学习服务在 R 中执行聚类分析。
::: moniker-end

本文将指导如何进行以下操作：

> [!div class="checklist"]
> * 使用 R 沿不同维度分离客户
> * 将数据从数据库加载到 R 数据帧中

在[第一部分](r-clustering-model-introduction.md)中，你安装了必备条件并还原了示例数据库。

[第三部分](r-clustering-model-build.md)介绍如何在 R 中创建和训练 K-Means 聚类分析模型。

在[第四部分](r-clustering-model-deploy.md)中，你将了解如何在数据库中创建存储过程，以便基于新数据在 R 中执行聚类分析。

## <a name="prerequisites"></a>先决条件

* 本教程的第二部分假设你已完成 [**第一部分**](r-clustering-model-introduction.md)。

## <a name="separate-customers"></a>分隔客户

在 RStudio 中创建新的 RScript 文件并运行以下脚本。
在 SQL 查询中，在以下维上分隔客户：

* orderRatio = 退单率（部分或全部退货的订单总数与订单总数的比率）
* itemsRatio = 退货率（退货总数与购买商品数量的比率）
* monetaryRatio = 退款率（退货的总货币金额与购买总金额的比率）
* frequency = 退货频率

在 **connStr** 函数中，将 **ServerName** 替换为你自己的连接信息。

```r
# Define the connection string to connect to the tpcxbb_1gb database

connStr <- "Driver=SQL Server;Server=ServerName;Database=tpcxbb_1gb;uid=Username;pwd=Password"

#Define the query to select data
input_query <- "
SELECT ss_customer_sk AS customer
    ,round(CASE 
            WHEN (
                       (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio
    ,round(CASE 
            WHEN (
                     (orders_items = 0)
                  OR (returns_items IS NULL)
                  OR (orders_items IS NULL)
                  OR ((returns_items / orders_items) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio
    ,round(CASE 
            WHEN (
                     (orders_money = 0)
                  OR (returns_money IS NULL)
                  OR (orders_money IS NULL)
                  OR ((returns_money / orders_money) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio
    ,round(CASE 
            WHEN (returns_count IS NULL)
            THEN 0.0
            ELSE returns_count
            END, 0) AS frequency
FROM (
    SELECT ss_customer_sk,
        -- return order ratio
        COUNT(DISTINCT (ss_ticket_number)) AS orders_count,
        -- return ss_item_sk ratio
        COUNT(ss_item_sk) AS orders_items,
        -- return monetary amount ratio
        SUM(ss_net_paid) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
    ) orders
LEFT OUTER JOIN (
    SELECT sr_customer_sk,
        -- return order ratio
        count(DISTINCT (sr_ticket_number)) AS returns_count,
        -- return ss_item_sk ratio
        COUNT(sr_item_sk) AS returns_items,
        -- return monetary amount ratio
        SUM(sr_return_amt) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
    ) returned ON ss_customer_sk = sr_customer_sk";
```

## <a name="load-the-data-into-a-data-frame"></a>将数据加载到数据帧中

现在，使用以下脚本将查询结果返回到一个 R 数据帧中。

```r
# Query using input_query and get the results back
# to data frame customer_data

library(RODBC)

ch <- odbcDriverConnect(connStr)

customer_data <- sqlQuery(ch, input_query)

# Take a look at the data just loaded
head(customer_data, n = 5);
```

可得到类似于下面的结果。

```results
  customer orderRatio itemsRatio monetaryRatio frequency
1    29727          0          0      0.000000         0
2    26429          0          0      0.041979         1
3    60053          0          0      0.065762         3
4    97643          0          0      0.037034         3
5    32549          0          0      0.031281         4
```

## <a name="clean-up-resources"></a>清理资源

如果不打算继续学习本教程，请删除 tpcxbb_1gb 数据库。

## <a name="next-steps"></a>后续步骤

在此教程系列的第二部分中，你已了解如何执行以下操作：

* 使用 R 沿不同维度分离客户
* 将数据从数据库加载到 R 数据帧中

若要创建使用此客户数据的机器学习模型，请按照本教程系列的第三部分进行操作：

> [!div class="nextstepaction"]
> [通过 SQL 机器学习在 R 中创建预测模型](r-clustering-model-build.md)
