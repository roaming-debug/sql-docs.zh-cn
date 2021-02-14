---
title: 在 RStudio 中使用 Sparklyr
titleSuffix: SQL Server big data clusters
description: 了解如何在 SQL Server 大数据群集中使用 sparklyr 来通过 R 接口连接到 Spark。
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 8a15336a22f564dd22870de8843732cbb583d2f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043491"
---
# <a name="use-sparklyr-in-sql-server-big-data-cluster"></a>在 SQL Server 大数据群集中使用 Sparklyr

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Sparklyr 为 Apache Spark 提供 R 接口。 Sparklyr 是 R 开发人员使用 Spark 的常用方法。 本文介绍如何使用 RStudio 在 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 中使用 sparklyr。

## <a name="prerequisites"></a>必备条件

- [部署 SQL Server 2019 大数据群集](quickstart-big-data-cluster-deploy.md)。

### <a name="install-rstudio-desktop"></a>安装 RStudio Desktop

使用以下步骤安装和配置 RStudio Desktop  ：

1. 如果在 Windows 客户端运行，请[下载并安装 R 3.4.4](https://cran.rstudio.com/bin/windows/base/old/3.4.4)。

1. [下载并安装 RStudio Desktop](https://www.rstudio.com/products/rstudio/download/)。

1. 安装完成后，在 RStudio Desktop 内运行以下命令以安装所需的包：

   ```RStudioDesktop
   install.packages("DBI", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   install.packages("dplyr", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   install.packages("sparklyr", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   ```

## <a name="connect-to-spark-in-a-big-data-cluster"></a>连接到大数据群集中的 Spark

可使用 sparklyr 从客户端连接到使用 Livy 和 HDFS/Spark 网关的大数据群集。 

在 RStudio 中，创建 R 脚本并连接到 Spark，如以下示例所示：

> [!TIP]
> 对于 `<AZDATA_USERNAME>` 和 `<AZDATA_PASSWORD>` 值，请使用在大数据群集部署过程中设置的用户名和密码。

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

有关 `<IP>` 和 `<PORT>` 值，请参阅有关[连接到大数据群集](connect-to-big-data-cluster.md)的文档。

```r
library(sparklyr)
library(dplyr)
library(DBI)

#Specify the Knox username and password
config <- livy_config(user = "<AZDATA_USERNAME>", password = "<AZDATA_PASSWORD>")

httr::set_config(httr::config(ssl_verifypeer = 0L, ssl_verifyhost = 0L))

sc <- spark_connect(master = "https://<IP>:<PORT>/gateway/default/livy/v1",
                    method = "livy",
                    config = config)
```

## <a name="run-sparklyr-queries"></a>运行 sparklyr 查询

连接到 Spark 后，可以运行 sparklyr。 下面的示例使用 sparklyr 对 iris 数据集执行查询：

```r
iris_tbl <- copy_to(sc, iris)

iris_count <- dbGetQuery(sc, "SELECT COUNT(*) FROM iris")

iris_count
```

## <a name="distributed-r-computations"></a>分布式 R 计算

sparklyr 的一项功能是能够使用 [spark_apply](https://spark.rstudio.com/guides/distributed-r/#apply-an-r-function-to-a-spark-object) 来[分布 计算](https://spark.rstudio.com/guides/distributed-r/)。

由于大数据群集使用 Livy 连接，因此必须在对“spark_apply”的调用中设置 `packages = FALSE`  。 有关详细信息，请参阅关于分布式 R 计算的 sparklyr 文档的 [Livy 部分](https://spark.rstudio.com/guides/distributed-r/#livy)。 使用此设置，只能在传递给“spark_apply”的 R 代码中使用已安装在 Spark 群集上的 R 包  。 下面的示例对此功能进行了演示：

```r
iris_tbl %>% spark_apply(function(e) nrow(e), names = "nrow", group_by = "Species", packages = FALSE)
```

## <a name="next-steps"></a>后续步骤

有关大数据群集的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md)。
