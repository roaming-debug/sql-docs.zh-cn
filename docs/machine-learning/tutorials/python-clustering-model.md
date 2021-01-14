---
title: Python 教程：对客户进行聚类分析
titleSuffix: SQL machine learning
description: 本系列教程由四个部分组成。你将使用 K-Means 在数据库中组合使用 Python 和 SQL 机器学习，对客户进行聚类分析。
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/26/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: dc119f4afe15606dc0d97e69abf0e3f055235bd7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101948"
---
# <a name="python-tutorial-categorizing-customers-using-k-means-clustering-with-sql-machine-learning"></a>Python 教程：将 K-Means 聚类分析与 SQL 机器学习配合使用，对客户进行聚类分析
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
本系列教程由四个部分组成，引导你使用 Python 在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中或在[大数据群集](../../big-data-cluster/machine-learning-services.md)上开发和部署 K-Means 聚类分析模型，以便对客户数据进行聚类分析。
::: moniker-end
::: moniker range="=sql-server-2017"
本系列教程由四个部分组成，引导你使用 Python 在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中开发和部署 K-Means 群集模型，以便对客户数据进行聚类分析。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
本系列教程由四个部分组成，引导你使用 Python 在 [Azure SQL 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)中开发和部署 K-Means 群集模型，以便对客户数据进行聚类分析。
::: moniker-end

在本系列的第一部分中，你将设置本教程的先决条件，然后将示例数据集还原到一个数据库。 在本系列的后续部分中，使用这些数据在 Python 中通过 SQL 机器学习来训练和部署聚类分析模型。

在本系列的第二和第三部分中，在 Azure Data Studio 笔记本中开发一些 Python 脚本，用于分析和准备数据以及定型机器学习模型。 然后，在第四部分中，使用存储过程在数据库中运行这些 Python 脚本。

聚类分析可解释为将数据组织成组，其中一个组的成员在某些方面类似。 对于本系列教程，假设你拥有一家零售企业。 你将使用 K-Means 算法在产品购买及退货的数据集中执行针对客户的聚类分析。 通过对客户进行聚类分析，可以将特定组定为目标，更加高效地专注于市场营销工作。 K-Means 群集是一种无监督式学习算法，该算法根据相似性寻找数据中的规律。

本文将指导如何进行以下操作：

> [!div class="checklist"]
> * 还原示例数据库

[第二部分](python-clustering-model-prepare-data.md)介绍如何从数据库准备数据以执行聚类分析。

[第三部分](python-clustering-model-build.md)介绍如何在 Python 中创建和定型 K-Means 群集模型。

在[第四部分](python-clustering-model-deploy.md)中，你将了解如何在数据库中创建存储过程，以便基于新数据在 Python 中执行聚类分析。

## <a name="prerequisites"></a>先决条件

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* 支持 Python 语言的 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md) - 按照 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)或 [Linux 安装指南](../../linux/sql-server-linux-setup-machine-learning.md?toc=%252fsql%252fmachine-learning%252ftoc.json&view=sql-server-linux-ver15&preserve-view=true)中的安装说明进行操作。 还可以[启用 SQL Server 大数据群集上的机器学习服务](../../big-data-cluster/machine-learning-services.md)。
::: moniker-end
::: moniker range="=sql-server-2017"
* 支持 Python 语言的 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md) - 按照 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)中的安装说明进行操作。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Azure SQL 托管实例机器学习服务。 有关信息，请参阅 [Azure SQL 托管实例机器学习服务概述](/azure/azure-sql/managed-instance/machine-learning-services-overview)。

* 用于将示例数据库还原到 Azure SQL 托管实例的 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)。
::: moniker-end

* [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md)。 你将使用 Azure Data Studio 中同时适用于 Python 和 SQL 的笔记本。 若要详细了解笔记本，请参阅[如何使用 Azure Data Studio 中的笔记本](../../azure-data-studio/notebooks/notebooks-guidance.md)。

* 附加 Python 包 - 在本教程系列中的示例所使用的 Python 包中，有些可能是你已经安装了的，有些可能是你尚未安装的。

  打开命令提示符，并更改为 Azure Data Studio 中使用的 Python 版本的安装路径。 例如，`cd %LocalAppData%\Programs\Python\Python37-32` 。 然后，运行下面的命令，以安装所有尚未安装的包。

  ```console
  pip install matplotlib
  pip install pandas
  pip install pyodbc
  pip install scipy
  pip install sklearn
  ```

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

* 还原示例数据库

若要为机器学习模型准备数据，按本系列教程的第二部分进行操作：

> [!div class="nextstepaction"]
> [Python 教程：准备数据以执行聚类分析](python-clustering-model-prepare-data.md)