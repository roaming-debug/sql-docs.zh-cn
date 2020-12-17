---
title: 教程：在 R 中开发聚类分析模型
titleSuffix: SQL Machine Learning
description: 本系列教程由四个部分组成。你将开发一个模型，以便通过 SQL 机器学习在 R 中执行聚类分析。
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/26/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 68f59975e621b5302700967e1dd90c685f66381e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470188"
---
# <a name="tutorial-develop-a-clustering-model-in-r-with-sql-machine-learning"></a>教程：通过 SQL 机器学习在 R 中部署聚类分析模型
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
本系列教程由四个部分组成，引导你使用 R 在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中或在[大数据群集](../../big-data-cluster/machine-learning-services.md)上开发和部署 K-Means 聚类分析模型，以便对客户数据进行聚类分析。
::: moniker-end
::: moniker range="=sql-server-2017"
本系列教程由四个部分组成，引导你使用 R 在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中开发和部署 K-Means 聚类分析模型，以便对客户数据进行聚类分析。
::: moniker-end
::: moniker range="=sql-server-2016"
本系列教程由四个部分组成，引导你使用 R 在 [SQL Server R Services](../r/sql-server-r-services.md) 中开发和部署 K-Means 聚类分析模型，以便对客户数据进行聚类分析。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
本系列教程由四个部分组成，引导你使用 R 在 [Azure SQL 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)中开发和部署 K-Means 聚类分析模型，以便对客户数据进行聚类分析。
::: moniker-end

在本系列的第一部分中，你将设置本教程的先决条件，然后将示例数据集还原到一个数据库。 在第二和第三部分中，你将在 Azure Data Studio 笔记本中开发一些 R 脚本，用于分析和准备此示例数据以及训练机器学习模型。 然后，在第四部分中，你将使用存储过程在数据库中运行这些 R 脚本。

聚类分析可解释为将数据组织成组，其中一个组的成员在某些方面类似。 对于本系列教程，假设你拥有一家零售企业。 你将使用 K-Means 算法在产品购买及退货的数据集中执行针对客户的聚类分析。 通过对客户进行聚类分析，可以将特定组定为目标，更加高效地专注于市场营销工作。 K-Means 群集是一种无监督式学习算法，该算法根据相似性寻找数据中的规律。

本文将指导如何进行以下操作：

> [!div class="checklist"]
> * 还原示例数据库

[第二部分](r-clustering-model-prepare-data.md)介绍如何从数据库准备数据以执行聚类分析。

[第三部分](r-clustering-model-build.md)介绍如何在 R 中创建和训练 K-Means 聚类分析模型。

在[第四部分](r-clustering-model-deploy.md)中，你将了解如何在数据库中创建存储过程，以便基于新数据在 R 中执行聚类分析。

## <a name="prerequisites"></a>先决条件

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* 支持 Python 语言的 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md) - 按照 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)或 [Linux 安装指南](../../linux/sql-server-linux-setup-machine-learning.md?toc=%252fsql%252fmachine-learning%252ftoc.json&view=sql-server-linux-ver15&preserve-view=true)中的安装说明进行操作。 还可以[启用 SQL Server 大数据群集上的机器学习服务](../../big-data-cluster/machine-learning-services.md)。
::: moniker-end
::: moniker range="=sql-server-2017"
* 支持 R 语言的 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md) - 按照 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)中的安装说明进行操作。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Azure SQL 托管实例机器学习服务。 有关信息，请参阅 [Azure SQL 托管实例机器学习服务概述](/azure/azure-sql/managed-instance/machine-learning-services-overview)。

* 用于将示例数据库还原到 Azure SQL 托管实例的 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)。
::: moniker-end

* [Azure Data Studio](../../azure-data-studio/what-is.md)。 你将使用 Azure Data Studio 中适用于 SQL 的笔记本。 若要详细了解笔记本，请参阅[如何使用 Azure Data Studio 中的笔记本](../../azure-data-studio/notebooks/notebooks-guidance.md)。

* R IDE - 此教程使用 [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/)。

* RODBC - 此驱动程序用于你在本教程中开发的 R 脚本。 如果尚未安装，请使用 R 命令 `install.packages("RODBC")` 安装它。 有关 RODBC 的详细信息，请参阅 [CRAN - RODBC 包](https://CRAN.R-project.org/package=RODBC)。

## <a name="restore-the-sample-database"></a>还原示例数据库

本教程中使用的示例数据集已保存到 **.bak** 数据库备份文件，以供下载和使用。 此数据集派生自 [事务处理性能委员会 (TPC)](http://www.tpc.org/) 提供的 [tpcx-bb](http://www.tpc.org/tpcx-bb/default5.asp) 数据集。

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> 如果在大数据群集上使用机器学习服务，请了解如何[将数据库还原成 SQL Server 大数据群集主实例](../../big-data-cluster/data-ingestion-restore-database.md)。
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
1. 下载 [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak) 文件。

1. 使用以下详细信息，按 Azure Data Studio 中[从备份文件还原数据库](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file)中的说明操作：

   * 从下载的 tpcxbb_1gb.bak 文件导入
   * 将目标数据库命名为“tpcxbb_1gb”

1. 可以查询 dbo.customer 表验证数据库还原后数据集是否存在：

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
1. 下载 [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak) 文件。

1. 使用以下详细信息，按 SQL Server Management Studio 中[将数据库还原到托管实例](/azure/sql-database/sql-database-managed-instance-get-started-restore)的说明操作：

   * 从下载的 tpcxbb_1gb.bak 文件导入
   * 将目标数据库命名为“tpcxbb_1gb”

1. 可以查询 dbo.customer 表验证数据库还原后数据集是否存在：

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end

## <a name="clean-up-resources"></a>清理资源

如果不打算继续学习本教程，请删除 tpcxbb_1gb 数据库。

## <a name="next-steps"></a>后续步骤

在本系列教程的第一部分中，你已完成以下步骤：

* 安装必备组件
* 还原示例数据库

若要为机器学习模型准备数据，按本系列教程的第二部分进行操作：

> [!div class="nextstepaction"]
> [准备数据以执行聚类分析](r-clustering-model-prepare-data.md)