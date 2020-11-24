---
title: Python 教程：雪橇租赁
titleSuffix: SQL machine learning
description: 本教程系列由四个部分组成。你将在 Python 中构建线性回归模型，以通过 SQL 机器学习预测雪橇租赁次数。
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: fc700631df6289c0529fdfd65d73b630cfac00f1
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94585037"
---
# <a name="python-tutorial-predict-ski-rental-with-linear-regression-with-sql-machine-learning"></a>Python 教程：通过 SQL 机器学习使用线性回归来预测雪橇租赁
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
在这个由四个部分组成的教程系列中，你将在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中或[大数据群集](../../big-data-cluster/machine-learning-services.md)上使用 Python 和线性回归来预测雪橇租赁次数。 本教程使用 [Azure Data Studio 中的 Python 笔记本](../../azure-data-studio/notebooks/notebooks-guidance.md)。
::: moniker-end
::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
在这个由四个部分组成的教程系列中，你将在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中使用 Python 和线性回归来预测雪橇租赁次数。 本教程使用 [Azure Data Studio 中的 Python 笔记本](../../azure-data-studio/notebooks/notebooks-guidance.md)。
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
在这个由四个部分组成的教程系列中，你将在 [Azure SQL Server 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)中使用 Python 和线性回归来预测雪橇租赁次数。 本教程使用 [Azure Data Studio 中的 Python 笔记本](../../azure-data-studio/notebooks/notebooks-guidance.md)。
::: moniker-end

假设你有一家雪橇租赁公司，你希望预测未来某个日期的雪橇租赁次数。 此信息可帮助你准备好库存、人员和设施。

在本系列的第一部分，你将设置必备组件。 在第二和第三部分，你将在某个笔记本中开发一些 Python 脚本，以准备数据并训练机器学习模型。 然后，在第三部分，你将使用 T-SQL 存储过程在数据库中运行这些 Python 脚本。

本文将指导如何进行以下操作：

> [!div class="checklist"]
> * 导入示例数据库

在[第二部分](python-ski-rental-linear-regression-prepare-data.md)中，你将了解如何将数据从数据库加载到 Python 数据帧中，并在 Python 中准备数据。

[第三部分](python-ski-rental-linear-regression-train-model.md)介绍如何在 Python 中定型线性回归模型。

在[第四部分](python-ski-rental-linear-regression-deploy-model.md)中，你将了解如何将模型存储到数据库中，然后根据你在第二和第三部分中开发的 Python 脚本来创建存储过程。 存储过程将在服务器上运行，以便基于新数据进行预测。

## <a name="prerequisites"></a>先决条件

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
* SQL Server 机器学习服务 — 如需安装机器学习服务，请参阅 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)或 [Linux 安装指南](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json)。 还可以[启用 SQL Server 大数据群集上的机器学习服务](../../big-data-cluster/machine-learning-services.md)。
::: moniker-end
::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
* SQL Server 机器学习服务 - 如需安装机器学习服务，请参阅 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)。 
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
* Azure SQL 托管实例机器学习服务 - 有关信息，请参阅 [Azure SQL 托管实例机器学习服务概述](/azure/azure-sql/managed-instance/machine-learning-services-overview)。

* 使用 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) 将示例数据库还原到 Azure SQL 托管实例。
::: moniker-end

* Python IDE - 本教程在 [Azure Data Studio](../../azure-data-studio/what-is.md) 中使用 Python 笔记本。 有关详细信息，请参阅[如何使用 Azure Data Studio 中的笔记本](../../azure-data-studio/notebooks/notebooks-guidance.md)。

* SQL 查询工具 - 本教程假定使用的是 [Azure Data Studio](../../azure-data-studio/what-is.md)。

* 附加 Python 包 - 在本教程系列中的示例所使用的以下 Python 包中，有些可能是默认未安装的：

  * pandas
  * pyodbc
  * sklearn

  若要安装这些包：
  1. 在 Azure Data Studio 笔记本中，选择“管理包”。
  2. 在“管理包”窗格中，选择“添加新包”选项卡。
  3. 对于以下每个包，输入包名称，单击“搜索”，然后单击“安装”。

  作为替代方法，你可以打开“命令提示符”，更改为在 Azure Data Studio 中使用的 Python 版本的安装路径（例如 `cd %LocalAppData%\Programs\Python\Python37-32`），然后针对每个包运行 `pip install`。

## <a name="restore-the-sample-database"></a>还原示例数据库

本教程中使用的示例数据库已保存到 .bak 数据库备份文件，以供下载和使用。

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
> [!NOTE]
> 如果在大数据群集上使用机器学习服务，请了解如何[将数据库还原成 SQL Server 大数据群集主实例](../../big-data-cluster/data-ingestion-restore-database.md)。
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15||=sqlallproducts-allversions"
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
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
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
* 导入示例数据库

若要从 TutorialDB 数据库中准备数据，请按照本教程系列的第二部分进行操作：

> [!div class="nextstepaction"]
> [Python 教程：准备数据以定型线性回归模型](python-ski-rental-linear-regression-prepare-data.md)