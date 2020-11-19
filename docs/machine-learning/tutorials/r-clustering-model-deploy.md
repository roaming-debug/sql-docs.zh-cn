---
title: 教程：在 R 中部署聚类模型
titleSuffix: SQL machine learning
description: 此系列教程由四个部分组成，这是第四部分。你将通过 SQL 机器学习在 R 中部署聚类分析模型。
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: 62b4f2d934a8591063e7c14ada914ac3a556866a
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870282"
---
# <a name="tutorial-deploy-a-clustering-model-in-r-with-sql-machine-learning"></a>教程：通过 SQL 机器学习在 R 中部署聚类分析模型
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
此系列教程由四个部分组成，这是第四部分。你将通过 SQL Server 机器学习服务或在大数据群集上将在 R 中开发的聚类分析模型部署到数据库中。
::: moniker-end
::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
此系列教程由四个部分组成，这是第四部分。你将使用 SQL Server 机器学习服务将在 R 中开发的聚类分析模型部署到数据库中。
::: moniker-end
::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
此系列教程由四个部分组成，这是第四部分。你将使用 SQL Server R Services 将在 R 中开发的聚类分析模型部署到数据库中。
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
此系列教程由四个部分组成，这是第四部分。你将使用 Azure SQL 托管实例机器学习服务将在 R 中开发的聚类分析模型部署到数据库中。
::: moniker-end

为了定期执行聚类分析，在新客户注册时，你需要能够从任何应用调用 R 脚本。 为此，可以通过将 R 脚本置于 SQL 存储过程中，在数据库中部署 R 脚本。 由于模型在数据库中执行，因此可以轻松地使用存储在数据库中的数据对其进行训练。

本文将指导如何进行以下操作：

> [!div class="checklist"]
> * 创建生成模型的存储过程
> * 执行聚类分析
> * 使用聚类分析信息

在[第一部分](r-clustering-model-introduction.md)中，你安装了必备条件并还原了示例数据库。

在[第二部分](r-clustering-model-prepare-data.md)中，你了解了如何从数据库准备数据以执行聚类分析。

在[第三部分](r-clustering-model-build.md)中，你了解了如何在 R 中创建和训练 K-Means 聚类分析模型。

## <a name="prerequisites"></a>先决条件

* 本教程系列的第四部分假设你已符合 [**第一部分**](r-clustering-model-introduction.md)的先决条件，并完成了 [**第二部分**](r-clustering-model-build.md)和 [**第三部分**](r-clustering-model-build.md)的步骤。

## <a name="create-a-stored-procedure-that-generates-the-model"></a>创建生成模型的存储过程

运行以下 T-SQL 脚本，以创建存储过程。 此过程会重建本教程系列的第二部分和第三部分中开发的步骤：

* 根据客户的购买和返回历史记录，对客户进行分类
* 使用 K-Means 算法生成四个客户群集

该过程将生成的客户聚类映射存储在数据库表 **customer_return_clusters** 中。

```sql
USE [tpcxbb_1gb]
DROP PROC IF EXISTS generate_customer_return_clusters;
GO
CREATE procedure [dbo].[generate_customer_return_clusters]
AS
/*
  This procedure uses R to classify customers into different groups
  based on their purchase & return history.
*/
BEGIN
    DECLARE @duration FLOAT
    , @instance_name NVARCHAR(100) = @@SERVERNAME
    , @database_name NVARCHAR(128) = db_name()
-- Input query to generate the purchase history & return metrics
    , @input_query NVARCHAR(MAX) = N'
SELECT ss_customer_sk AS customer,
    round(CASE 
            WHEN (
                    (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio,
    round(CASE 
            WHEN (
                    (orders_items = 0)
                    OR (returns_items IS NULL)
                    OR (orders_items IS NULL)
                    OR ((returns_items / orders_items) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio,
    round(CASE 
            WHEN (
                    (orders_money = 0)
                    OR (returns_money IS NULL)
                    OR (orders_money IS NULL)
                    OR ((returns_money / orders_money) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio,
    round(CASE 
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
    ) returned ON ss_customer_sk = sr_customer_sk
 '
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
# Define the connection string

connStr <- paste("Driver=SQL Server; Server=", instance_name,
                 "; Database=", database_name,
                 "; uid=Username;pwd=Password; ",
                 sep="" )

# Input customer data that needs to be classified.
# This is the result we get from the query.
library(RODBC)

ch <- odbcDriverConnect(connStr);

customer_data <- sqlQuery(ch, input_query)

sqlDrop(ch, "customer_return_clusters")

## create clustering model
clust <- kmeans(customer_data[,2:5],4)

## create clustering output for table
customer_cluster <- data.frame(cluster=clust$cluster,customer=customer_data$customer,orderRatio=customer_data$orderRatio,
            itemsRatio=customer_data$itemsRatio,monetaryRatio=customer_data$monetaryRatio,frequency=customer_data$frequency)

## write cluster output to DB table
sqlSave(ch, customer_cluster, tablename = "customer_return_clusters")

## clean up
odbcClose(ch)
'
    , @input_data_1 = N''
    , @params = N'@instance_name nvarchar(100), @database_name nvarchar(128), @input_query nvarchar(max), @duration float OUTPUT'
    , @instance_name = @instance_name
    , @database_name = @database_name
    , @input_query = @input_query
    , @duration = @duration OUTPUT;
END;

GO
```

## <a name="perform-clustering"></a>执行聚类分析

创建该存储过程后，请执行以下脚本来执行聚类。

```sql
--Empty table of the results before running the stored procedure
TRUNCATE TABLE customer_return_clusters;

--Execute the clustering
--This will load the table customer_return_clusters with cluster mappings
EXECUTE [dbo].[generate_customer_return_clusters];
```

确认该脚本是否正常运行，并实际返回了客户及其聚类映射的列表。

```sql
--Select data from table customer_return_clusters
--to verify that the clustering data was loaded
SELECT TOP (5) *
FROM customer_return_clusters;
```

```result
cluster  customer  orderRatio  itemsRatio  monetaryRatio  frequency
1        29727     0           0           0              0
4        26429     0           0           0.041979       1
2        60053     0           0           0.065762       3
2        97643     0           0           0.037034       3
2        32549     0           0           0.031281       4
```

## <a name="use-the-clustering-information"></a>使用聚类分析信息

由于在数据库中存储了聚类分析过程，因此它可以针对存储在同一数据库中的客户数据高效执行聚类分析。 每次更新客户数据并使用更新的聚类分析信息时，都可以执行该过程。

假设你想要向群集 0 中的客户发送促销电子邮件，该群集处于非活动状态（你可以在本教程的[第三部分](r-clustering-model-build.md#analyze-the-results)中查看这四个群集的介绍）。 以下代码选择群集 0 中客户的电子邮件地址。

```sql
USE [tpcxbb_1gb]
--Get email addresses of customers in cluster 0 for a promotion campaign
SELECT customer.[c_email_address], customer.c_customer_sk
  FROM dbo.customer
  JOIN
  [dbo].[customer_clusters] as c
  ON c.Customer = customer.c_customer_sk
  WHERE c.cluster = 0
```

你可以更改 c.cluster 值，以返回其他群集中客户的电子邮件地址。

## <a name="clean-up-resources"></a>清理资源

完成本教程后，可以删除 tpcxbb_1gb 数据库。

## <a name="next-steps"></a>后续步骤

在此教程系列的第四部分中，你已了解如何执行以下操作：

* 创建生成模型的存储过程
* 通过 SQL 机器学习执行聚类分析
* 使用聚类分析信息

若要详细了解如何在机器学习服务中使用 R，请参阅：

* [运行简单的 R 脚本](quickstart-r-create-script.md)
* [R 数据结构、类型和对象](quickstart-r-data-types-and-objects.md)
* [R 函数](quickstart-r-functions.md)
