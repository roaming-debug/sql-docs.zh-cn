---
description: SQL Server Python 和 R 教程适用的航空航班到达演示数据
title: 教程适用的航空航班演示数据
Description: 创建一个包含 R 和 Python 中的航空数据集的数据库。 此数据集用于 SQL Server 机器学习服务的 R 和 Python 教程。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/22/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 3b19e905d68d53764a496ab5ee7745e3529484cf
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489897"
---
#  <a name="airline-flight-arrival-demo-data-for-sql-server-python-and-r-tutorials"></a>SQL Server Python 和 R 教程适用的航空航班到达演示数据
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

在此练习中，创建一个 SQL Server 数据库，用于存储从 R 或 Python 内置航空演示数据集导入的数据。 R 和 Python 发行版提供了等效的数据，你可以使用 Management Studio 将这些数据导入到 SQL Server 数据库中。

若要完成此练习，应具有 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) 或其他可以运行 T-SQL 查询的工具。

使用此数据集的教程和快速入门包括以下内容：

+  [使用 revoscalepy 创建 Python 模型](use-python-revoscalepy-to-create-model.md)

## <a name="create-the-database"></a>创建数据库

1. 启动 SQL Server Management Studio，连接到一个具有 R 或 Python 集成的数据库引擎实例。  

2. 在对象资源管理器中，单击右键，点击“数据库”，并创建名为“flightdata”的新数据库。

3. 右键单击“flightdata”，单击“任务”和“导入平面文件”。

4. 根据安装的语言，打开 R 或 Python 发行版中提供的 AirlineDemoData csv 文件。

   对于 R，请在 C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR\SampleData 查找“AirlineDemoSmall.csv”
   
   对于 Python，请在 C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\Lib\site-packages\revoscalepy\data\sample_data 查找“AirlineDemoSmall.csv”
  
选择该文件时，会为表名称和架构填写默认值。

  ![导入显示航空公司演示默认值的平面文件向导](media/import-airlinedemosmall.png)

单击其余页面，接受默认值以导入数据。


## <a name="query-the-data"></a>查询数据

作为验证步骤，运行查询以确认已上传数据。

1. 在“对象资源管理器”中的“数据库”下，右键单击“flightdata”数据库，然后启动一个新查询。

2. 运行一些简单的查询：

    ```sql
    SELECT TOP(10) * FROM AirlineDemoSmall;
    SELECT COUNT(*) FROM AirlineDemoSmall;
    ```

## <a name="next-steps"></a>后续步骤

在下一课程中，将基于此数据创建线性回归模型。

+ [使用 revoscalepy 创建 Python 模型](use-python-revoscalepy-to-create-model.md)
