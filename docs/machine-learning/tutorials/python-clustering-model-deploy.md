---
title: Python 教程：部署群集模型
titleSuffix: SQL machine learning
description: 此系列教程由四个部分组成，这是第四部分。你将通过 SQL 机器学习在 Python 中部署聚类分析模型。
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/21/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 60119db288040111668868692eb5f3c6f1ce458c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470428"
---
# <a name="python-tutorial-deploy-a-model-to-categorize-customers-with-sql-machine-learning"></a>Python 教程：部署一个模型以通过 SQL 机器学习对客户进行分类
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
此系列教程由四个部分组成，这是第四部分。你将通过 SQL Server 机器学习服务或在大数据群集上将在 Python 中开发的聚类分析模型部署到数据库中。
::: moniker-end
::: moniker range="=sql-server-2017"
此系列教程由四个部分组成，这是第四部分。你将使用 SQL Server 机器学习服务将在 Python 中开发的聚类分析模型部署到数据库中。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
此系列教程由四个部分组成，这是第四部分。你将使用 Azure SQL 托管实例机器学习服务将在 Python 中开发的聚类分析模型部署到数据库中。
::: moniker-end

为了定期执行聚类分析，在新客户注册时，你需要能够从任何应用调用 Python 脚本。 为此，可以通过将 Python 脚本置于 SQL 存储过程中，在数据库中部署 Python 脚本。 由于模型在数据库中执行，因此可以轻松地使用存储在数据库中的数据对其进行训练。

在本部分中，你将把刚刚编写的 Python 代码移到服务器上并部署聚类分析。

本文将指导如何进行以下操作：

> [!div class="checklist"]
> * 创建生成模型的存储过程
> * 在服务器上执行聚类分析
> * 使用聚类分析信息

在[第一部分](python-clustering-model.md)中，你安装了必备条件并还原了示例数据库。

在[第二部分](python-clustering-model-prepare-data.md)中，你了解了如何从数据库准备数据以执行聚类分析。

在[第三部分](python-clustering-model-build.md)中，你了解了如何在 Python 中创建和定型 K-Means 聚类分析模型。

## <a name="prerequisites"></a>先决条件

* 本教程系列的第四部分假设你已完成[第一部分](python-clustering-model.md)的必备条件，并完成了[第二部分](python-clustering-model-prepare-data.md)和[第三部分](python-clustering-model-build.md)的步骤  。

## <a name="create-a-stored-procedure-that-generates-the-model"></a>创建生成模型的存储过程

运行以下 T-SQL 脚本，以创建存储过程。 此过程会重建本教程系列的第一部分和第二部分中开发的步骤：

* 根据客户的购买和返回历史记录，对客户进行分类
* 使用 K-Means 算法生成四个客户群集

```sql
USE [tpcxbb_1gb]
GO

CREATE procedure [dbo].[py_generate_customer_return_clusters]
AS

BEGIN
    DECLARE

-- Input query to generate the purchase history & return metrics
     @input_query NVARCHAR(MAX) = N'
SELECT
  ss_customer_sk AS customer,
  CAST( (ROUND(COALESCE(returns_count / NULLIF(1.0*orders_count, 0), 0), 7) ) AS FLOAT) AS orderRatio,
  CAST( (ROUND(COALESCE(returns_items / NULLIF(1.0*orders_items, 0), 0), 7) ) AS FLOAT) AS itemsRatio,
  CAST( (ROUND(COALESCE(returns_money / NULLIF(1.0*orders_money, 0), 0), 7) ) AS FLOAT) AS monetaryRatio,
  CAST( (COALESCE(returns_count, 0)) AS FLOAT) AS frequency
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
    GROUP BY sr_customer_sk
  ) returned ON ss_customer_sk=sr_customer_sk
 '

EXEC sp_execute_external_script
      @language = N'Python'
    , @script = N'

import pandas as pd
from sklearn.cluster import KMeans

#get data from input query
customer_data = my_input_data

#We concluded in step 2 in the tutorial that 4 would be a good number of clusters
n_clusters = 4

#Perform clustering
est = KMeans(n_clusters=n_clusters, random_state=111).fit(customer_data[["orderRatio","itemsRatio","monetaryRatio","frequency"]])
clusters = est.labels_
customer_data["cluster"] = clusters

OutputDataSet = customer_data
'
    , @input_data_1 = @input_query
    , @input_data_1_name = N'my_input_data'
             with result sets (("Customer" int, "orderRatio" float,"itemsRatio" float,"monetaryRatio" float,"frequency" float,"cluster" float));
END;
GO
```

## <a name="perform-clustering"></a>执行聚类分析

创建存储过程后，执行以下脚本，以使用该过程执行聚类分析。

```sql
--Create a table to store the predictions in

DROP TABLE IF EXISTS [dbo].[py_customer_clusters];
GO

CREATE TABLE [dbo].[py_customer_clusters] (
    [Customer] [bigint] NULL
  , [OrderRatio] [float] NULL
  , [itemsRatio] [float] NULL
  , [monetaryRatio] [float] NULL
  , [frequency] [float] NULL
  , [cluster] [int] NULL
  ,
    ) ON [PRIMARY]
GO

--Execute the clustering and insert results into table
INSERT INTO py_customer_clusters
EXEC [dbo].[py_generate_customer_return_clusters];

-- Select contents of the table to verify it works
SELECT * FROM py_customer_clusters;
```

## <a name="use-the-clustering-information"></a>使用聚类分析信息

由于在数据库中存储了聚类分析过程，因此它可以针对存储在同一数据库中的客户数据高效执行聚类分析。 每次更新客户数据并使用更新的聚类分析信息时，都可以执行该过程。

假设你想要向群集 0 中的客户发送促销电子邮件，该群集处于非活动状态（你可以在本教程的[第三部分](python-clustering-model-build.md#analyze-the-results)中查看这四个群集的介绍）。 以下代码选择群集 0 中客户的电子邮件地址。

```sql
USE [tpcxbb_1gb]
--Get email addresses of customers in cluster 0 for a promotion campaign
SELECT customer.[c_email_address], customer.c_customer_sk
  FROM dbo.customer
  JOIN
  [dbo].[py_customer_clusters] as c
  ON c.Customer = customer.c_customer_sk
  WHERE c.cluster = 0
```

你可以更改 c.cluster 值，以返回其他群集中客户的电子邮件地址。

## <a name="clean-up-resources"></a>清理资源

完成本教程后，可以删除 tpcxbb_1gb 数据库。

## <a name="next-steps"></a>后续步骤

在本教程系列的第四部分中，你完成了这些步骤：

* 创建生成模型的存储过程
* 在服务器上执行聚类分析
* 使用聚类分析信息

若要详细了解如何在 SQL 机器学习中使用 Python，请参阅：

* [快速入门：创建并运行简单的 Python 脚本](quickstart-python-create-script.md)
* [适用于 SQL 机器学习的其他 Python 教程](python-tutorials.md)
* [使用 sqlmlutils 安装 Python 包](../package-management/install-additional-python-packages-on-sql-server.md)