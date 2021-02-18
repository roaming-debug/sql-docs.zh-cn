---
title: 教程：使用 R 开发预测模型
titleSuffix: SQL machine learning
description: 此系列教程由四个部分组成。你将开发数据，以便通过 SQL 机器学习在 R 中训练预测模型。
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/26/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 5376d2b7876914c28d656574a57f5fc80294c63c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348041"
---
# <a name="tutorial-develop-a-predictive-model-in-r-with-sql-machine-learning"></a>教程：通过 SQL 机器学习在 R 中开发预测模型
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
此系列教程由四个部分组成。你将在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中或[大数据群集](../../big-data-cluster/machine-learning-services.md)上使用 R 和一个机器学习模型来预测雪橇租赁次数。
::: moniker-end
::: moniker range="=sql-server-2017"
此系列教程由四个部分组成。你将在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中使用 R 和一个机器学习模型来预测雪橇租赁次数。
::: moniker-end
::: moniker range="=sql-server-2016"
此系列教程由四个部分组成。你将在 [SQL Server R Services](../r/sql-server-r-services.md) 中使用 R 和一个机器学习模型来预测雪橇租赁次数。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
此系列教程由四个部分组成。你将在 [Azure SQL 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)中使用 R 和一个机器学习模型来预测雪橇租赁次数。
::: moniker-end

假设你有一家雪橇租赁公司，你希望预测未来某个日期的雪橇租赁次数。 此信息可帮助你准备好库存、人员和设施。

在本系列的第一部分，你将设置必备组件。 在第二和第三部分，你将在某个笔记本中开发一些 R 脚本，以准备数据并训练机器学习模型。 然后，在第三部分中，将使用 T-SQL 存储过程在数据库中运行这些 R 脚本。

本文将指导如何进行以下操作：

> [!div class="checklist"]
> * 还原示例数据库 

在[第二部分](r-predictive-model-prepare-data.md)中，你将了解如何将数据从数据库加载到 Python 数据帧中，并在 R 中准备数据。

在[第三部分](r-predictive-model-train.md)中，你将了解如何在 R 中训练机器学习模型。

在[第四部分](r-predictive-model-deploy.md)中，你将了解如何将模型存储到数据库中，然后通过你在第二和第三部分中开发的 R 脚本来创建存储过程。 存储过程将在服务器上运行，以便基于新数据进行预测。

## <a name="prerequisites"></a>先决条件

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
* SQL Server 机器学习服务 — 如需安装机器学习服务，请参阅 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)或 [Linux 安装指南](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json)。 还可以[启用 SQL Server 大数据群集上的机器学习服务](../../big-data-cluster/machine-learning-services.md)。
::: moniker-end
::: moniker range="=sql-server-2017"
* SQL Server 机器学习服务 - 如需安装机器学习服务，请参阅 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)。 
::: moniker-end
::: moniker range="=sql-server-2016"
* SQL Server 2016 R Services。 如需安装 R Services，请参阅 [Windows 安装指南](../install/sql-r-services-windows-install.md)。 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
* Azure SQL 托管实例机器学习服务。 有关信息，请参阅 [Azure SQL 托管实例机器学习服务概述](/azure/azure-sql/managed-instance/machine-learning-services-overview)。

* 用于将示例数据库还原到 Azure SQL 托管实例的 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)。
::: moniker-end

* R IDE - 此教程使用 [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/)。

* RODBC - 此驱动程序用于你在本教程中开发的 R 脚本。 如果尚未安装，请使用 R 命令 `install.packages("RODBC")` 安装它。 有关 RODBC 的详细信息，请参阅 [CRAN - RODBC 包](https://CRAN.R-project.org/package=RODBC)。

* SQL 查询工具 - 本教程假定使用的是 [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md)。 有关详细信息，请参阅[如何使用 Azure Data Studio 中的笔记本](../../azure-data-studio/notebooks/notebooks-guidance.md)。

## <a name="restore-the-sample-database"></a>还原示例数据库

本教程中使用的示例数据库已保存到 .bak 数据库备份文件，以供下载和使用。

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> 如果在大数据群集上使用机器学习服务，请了解如何[将数据库还原成 SQL Server 大数据群集主实例](../../big-data-cluster/data-ingestion-restore-database.md)。
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
1. 下载文件 [TutorialDB.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bak)。

1. 使用以下详细信息，按 Azure Data Studio 中[从备份文件还原数据库](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file)中的说明操作：

   * 从下载的 **TutorialDB.bak** 文件中导入
   * 将目标数据库命名为“TutorialDB”

1. 可以通过查询 dbo.rental_data 表来验证是否存在还原的数据集：

   ```sql
   USE TutorialDB;
   SELECT * FROM [dbo].[rental_data];
   ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
1. 下载文件 [TutorialDB.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/TutorialDB.bak)。

1. 按照 SQL Server Management Studio 中的[将数据库还原到托管实例](/azure/sql-database/sql-database-managed-instance-get-started-restore)中的说明，使用以下详细信息：

   * 从下载的 **TutorialDB.bak** 文件中导入
   * 将目标数据库命名为“TutorialDB”

1. 可以通过查询 dbo.rental_data 表来验证是否存在还原的数据集：

   ```sql
   USE TutorialDB;
   SELECT * FROM [dbo].[rental_data];
   ```
::: moniker-end

## <a name="clean-up-resources"></a>清理资源

如果不打算继续学习本教程，请删除 TutorialDB 数据库。
## <a name="next-steps"></a>后续步骤

在本系列教程的第一部分中，你已完成以下步骤：

* 安装必备组件
* 还原示例数据库

若要为机器学习模型准备数据，按本系列教程的第二部分进行操作：

> [!div class="nextstepaction"]
> [准备数据以在 R 中训练预测模型](r-predictive-model-prepare-data.md)