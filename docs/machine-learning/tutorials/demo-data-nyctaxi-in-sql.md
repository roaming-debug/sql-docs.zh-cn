---
title: 教程适用的纽约市出租车演示数据
description: 创建包含纽约市出租车示例数据的数据库。 此数据集用于 SQL Server 机器学习服务的 R 和 Python 教程。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/31/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: 2dab1d48ca2aa98e4a70a08bac492366f2632b79
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584949"
---
# <a name="nyc-taxi-demo-data-for-sql-server-python-and-r-tutorials"></a>用于 SQL Server Python 和 R 教程的纽约市出租车演示数据
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

本文介绍了如何设置一个示例数据库，该数据库包含来自[纽约市出租车和轿车委员会](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml)的公共数据。 此数据用于多个 R 和 Python 教程，用于 SQL Server 上的数据库内分析。 为了使示例代码运行速度更快，我们创建了一个具有代表性的 1% 采样数据。 在你的系统上，数据库备份文件略大于 90 MB，在主数据表中提供 170 万行。

若要完成此练习，你应该拥有 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md?view=sql-server-2017&preserve-view=true) 或其他可以还原数据库备份文件并运行 T-SQL 查询的工具。

使用此数据集的教程和快速入门包括以下内容：

+ [在 SQL Server 中使用 R 学习数据库内分析](r-taxi-classification-introduction.md)
+ [在 SQL Server 中使用 Python 学习数据库内分析](python-taxi-classification-introduction.md)

## <a name="download-files"></a>下载文件

示例数据库为 Microsoft 托管的 SQL Server 2016 BAK 文件。 可在 SQL Server 2016 及更高版本上还原它。 单击链接后，将立即开始文件下载。 

文件大小约为 90 MB。

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
>[!NOTE]
>若要还原 [SQL Server 大数据群集](../../big-data-cluster/big-data-cluster-overview.md)上的示例数据库，请下载[NYCTaxi_Sample.bak](https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak) 并遵循[将数据库还原到 SQL Server 大数据群集主实例](../../big-data-cluster/data-ingestion-restore-database.md)中的说明进行操作。
::: moniker-end

::: moniker range=">=azuresqldb-mi-current||=sqlallproducts-allversions"
>[!NOTE]
>若要还原 [Azure SQL 托管实例中的机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)上的示例数据库，请按照[快速入门：将数据库还原到 Azure SQL 托管实例](/azure/azure-sql/managed-instance/restore-sample-database-quickstart)中的说明，使用纽约市出租车演示数据库 .bak 文件进行操作：[https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak](https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak)。
::: moniker-end

1. 单击 [NYCTaxi_Sample.bak](https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak) 下载数据库备份文件。

2. 将该文件复制到 C:\Program files\Microsoft SQL Server\MSSQL-instance-name\MSSQL\Backup 文件夹。

3. 在 Management Studio 中，右键单击“数据库”，然后选择“还原文件和文件组” 。

4. 输入“NYCTaxi_Sample”作为数据库名称。

5. 单击“从设备”，然后打开文件选择页以选择备份文件。 单击“添加”以选择 NYCTaxi_Sample.bak。

6. 选中“还原”复选框，然后单击“确定”以还原数据库 。

## <a name="review-database-objects"></a>查看数据库对象
   
使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 确认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上存在数据库对象。 你可看到数据库、表、函数和存储过程。
  
   ![rsql_devtut_BrowseTables](media/rsql-devtut-browsetables.png "rsql_devtut_BrowseTables")

### <a name="objects-in-nyctaxi_sample-database"></a>NYCTaxi_Sample 数据库中的对象

下表总结了纽约市出租车演示数据库中创建的对象。

|**对象名称**|**对象类型**|**说明**|
|----------|------------------------|---------------|
|**NYCTaxi_Sample** | database | 创建一个数据库和两个表：<br /><br />dbo.nyctaxi_sample 表：包含纽约市出租车主数据集。 将在表中添加一个聚集列存储索引，改善存储和查询性能。 此表中插入了纽约市出租车数据集的 1% 采样。<br /><br />dbo.nyc_taxi_models 表：用于保留已定型的高级分析模型。|
|**fnCalculateDistance** |标量值函数 (scalar-valued function) | 计算搭乘位置和下车位置之间的直接距离。 在[创建数据功能](r-taxi-classification-create-features.md)、[定型和保存模型](r-taxi-classification-train-model.md)以及[操作 R 模型](r-taxi-classification-deploy-model.md)中使用此功能。|
|**fnEngineerFeatures** |表值函数 (table-valued function) | 为模型定型创建新的数据功能。 在[创建数据功能](r-taxi-classification-create-features.md)和[操作 R 模型](r-taxi-classification-deploy-model.md)中使用此功能。|


使用在各种教程中找到的 R 和 Python 脚本创建存储过程。 下表总结了从不同课程运行脚本时可以选择添加到纽约市出租车演示数据库的存储过程。

|**存储过程**|**语言**|**说明**|
|-------------------------|------------|---------------|
|**RxPlotHistogram** |R | 调用 RevoScaleR rxHistogram 函数绘制变量的直方图，然后将绘图作为二进制对象返回。 在[浏览并可视化数据](r-taxi-classification-explore-data.md)中使用此存储过程。|
|**RPlotRHist** |R| 使用 Hist 函数创建图形，然后将输出保存为本地 PDF 文件。 在[浏览并可视化数据](r-taxi-classification-explore-data.md)中使用此存储过程。|
|**RxTrainLogitModel**  |R| 通过调用 R 程序包定型逻辑回归模型。 该模型预测附属列的值，并使用随机选择的 70% 的数据进行定型。 存储过程的输出是定型模型，保存在表 nyc_taxi_models 中。 在[定型和保存模型](r-taxi-classification-train-model.md)中使用此存储过程。|
|**RxPredictBatchOutput**  |R | 调用已定型模型以便使用模型创建预测。 该存储过程接受查询作为其输入参数，并返回包含输入行分数的数值的列。 在[预测潜在结果](r-taxi-classification-deploy-model.md)中使用此存储过程。|
|**RxPredictSingleRow**  |R| 调用已定型模型以便使用模型创建预测。 该存储过程接受一个新观察值作为输入，而传递的单个功能值作为嵌入式参数，并返回一个预测新观察值结果的值。 在[预测潜在结果](r-taxi-classification-deploy-model.md)中使用此存储过程。|

## <a name="query-the-data"></a>查询数据

作为验证步骤，运行查询以确认已上传数据。

1. 在“对象资源管理器”中的“数据库”下，右键单击“NYCTaxi_Sample”数据库，然后启动一个新查询。

2. 运行一些简单的查询：

    ```sql
    SELECT TOP(10) * FROM dbo.nyctaxi_sample;
    SELECT COUNT(*) FROM dbo.nyctaxi_sample;
    ```
数据库包含 170 万行。

3. 数据库内是一个包含数据集的 nyctaxi_sample 表。 表已针对基于集的计算进行了优化，并且添加了[列存储索引](../../relational-databases/indexes/columnstore-indexes-overview.md)。 运行此语句，以生成表的快速摘要。

    ```sql
    SELECT DISTINCT [passenger_count]
        , ROUND (SUM ([fare_amount]),0) as TotalFares
        , ROUND (AVG ([fare_amount]),0) as AvgFares
    FROM [dbo].[nyctaxi_sample]
    GROUP BY [passenger_count]
    ORDER BY  AvgFares DESC
    ````
结果应类似于以下屏幕截图中显示的结果。

  ![表摘要信息](media/nyctaxidatatablesummary.png "查询结果")

## <a name="next-steps"></a>后续步骤

纽约市出租车示例数据现在可用于实践学习。

+ [在 SQL Server 中使用 R 学习数据库内分析](r-taxi-classification-introduction.md)
+ [在 SQL Server 中使用 Python 学习数据库内分析](python-taxi-classification-introduction.md)