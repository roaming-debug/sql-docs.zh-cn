---
title: Python 教程：浏览并可视化数据
titleSuffix: SQL machine learning
description: 浏览示例数据，并生成一些图表，以准备在 SQL 机器学习的 Python 中使用二元分类。
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 0f52df34bb04a94baf14076a9ea345712166ae5a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470318"
---
# <a name="python-tutorial-explore-and-visualize-data"></a>Python 教程：浏览并可视化数据
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

在此系列教程的第二部分中（共五部分），你将浏览示例数据，并生成一些图表。 之后，你可了解如何在 Python 中序列化图形对象，然后对这些对象进行反序列化并制作图表。

在本文中，你将：

> [!div class="checklist"]
> + 查看示例数据
> + 使用 T-SQL 中的 Python 创建绘图

在[第一部分](python-taxi-classification-introduction.md)中，你安装了必备条件并还原了示例数据库。

在[第三部分](python-taxi-classification-create-features.md)中，你将学习如何使用 Transact-SQL 函数根据原始数据创建特征。 然后从存储过程调用该函数，创建包含该功能值的表。

在[第四部分](python-taxi-classification-train-model.md)中，你将加载模块，并调用必要的函数，以使用 SQL Server 存储过程来创建和训练模型。

在[第五部分](python-taxi-classification-deploy-model.md)中，你将了解如何操作在第四部分中训练和保存的模型。

## <a name="review-the-data"></a>查看数据

首先，请花一分钟时间浏览数据架构，因为我们做了一些更改，以便更轻松地使用 NYC 出租车数据

+ 原始数据集对出租车标识符和行程记录使用了不同文件。 我们已将两个原始数据集加入列 medallion、hack_license 和 pickup_datetime。  
+ 原始数据集跨多个文件，且非常庞大。 我们缩减了采样，仅获取 1% 的原始记录。 当前数据表包含 1,703,957 行和 23 列。

**出租车标识符**

其中的 medallion 列表示出租车的唯一 ID 号。

其中的 hack_license 列包含出租车司机的驾照号码（匿名）。

**行程和费用记录**

每条行程记录都包括上车和下车地点和时间，以及行程距离。

每条费用记录都包括付费信息，如付款类型、总付款和小费金额。

最后三列可用于各种机器学习任务。  Tip_amount 列包含连续数值，并且可用作回归分析的 **label** 列。 tipped 列只有是/否值，用于二元分类。  Tip_class 列有多个 **级别标签**，因此可以用作多级分类任务的标签。

标签列使用的值都基于 `tip_amount` 列，并使用以下业务规则：

+ 标签列 `tipped` 可能的值包括 0 和 1

    如果 `tip_amount` > 0，则 `tipped` = 1；否则 `tipped` = 0

+ 标签列 `tip_class` 可能的类值包括 0-4

    类 0：`tip_amount` = $0

    类 1：`tip_amount` > $0 且 `tip_amount` <= $5
    
    类 2：`tip_amount` > $5 且 `tip_amount` <= $10
    
    类 3：`tip_amount` > $10 且 `tip_amount` <= $20
    
    类 4：`tip_amount` > $20

## <a name="create-plots-using-python-in-t-sql"></a>使用 T-SQL 中的 Python 创建绘图

开发数据科学解决方案通常包括深入的数据探索和数据可视化。 由于可视化是理解数据和离群值的分布的强大工具，因此 Python 提供了许多用于可视化数据的包。 Matplotlib 模块是最受欢迎的可视化库之一，其中包含许多用于创建直方图、散点图、箱线图和其他数据浏览图表的函数。

在本部分中，你可了解如何使用存储过程处理各种绘图。 将 Python 对象 `plot` 存储为 varbinary 数据，然后将其写入可在其他位置共享或查看的文件，而不是在服务器上打开该图像。

### <a name="create-a-plot-as-varbinary-data"></a>创建绘图作为 varbinary 数据

存储过程将序列化的 Python `figure` 对象作为 varbinary 数据流返回。 无法直接查看二进制数据，但可以在客户端上使用 Python 代码来反序列化和查看这些图形，然后将该图像文件保存到客户端计算机上。

1. 如果 PowerShell 脚本尚未创建存储过程 PyPlotMatplotlib，请执行此操作。

    - 变量 `@query` 定义查询文本 `SELECT tipped FROM nyctaxi_sample`，该文本作为脚本输入变量 `@input_data_1` 的参数传递给 Python 代码块。
    - Python 脚本非常简单：matplotlib `figure` 对象用于制作直方图和散点图，然后使用 `pickle` 库对这些对象进行序列化。
    - Python 图形对象序列化为 pandas 数据帧进行输出。
  
    ```sql
    DROP PROCEDURE IF EXISTS PyPlotMatplotlib;
    GO

    CREATE PROCEDURE [dbo].[PyPlotMatplotlib]
    AS
    BEGIN
        SET NOCOUNT ON;
        DECLARE @query nvarchar(max) =
        N'SELECT cast(tipped as int) as tipped, tip_amount, fare_amount FROM [dbo].[nyctaxi_sample]'
        EXECUTE sp_execute_external_script
        @language = N'Python',
        @script = N'
    import matplotlib
    matplotlib.use("Agg")
    import matplotlib.pyplot as plt
    import pandas as pd
    import pickle

    fig_handle = plt.figure()
    plt.hist(InputDataSet.tipped)
    plt.xlabel("Tipped")
    plt.ylabel("Counts")
    plt.title("Histogram, Tipped")
    plot0 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.hist(InputDataSet.tip_amount)
    plt.xlabel("Tip amount ($)")
    plt.ylabel("Counts")
    plt.title("Histogram, Tip amount")
    plot1 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.hist(InputDataSet.fare_amount)
    plt.xlabel("Fare amount ($)")
    plt.ylabel("Counts")
    plt.title("Histogram, Fare amount")
    plot2 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.scatter( InputDataSet.fare_amount, InputDataSet.tip_amount)
    plt.xlabel("Fare Amount ($)")
    plt.ylabel("Tip Amount ($)")
    plt.title("Tip amount by Fare amount")
    plot3 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    OutputDataSet = plot0.append(plot1, ignore_index=True).append(plot2, ignore_index=True).append(plot3, ignore_index=True)
    ',
    @input_data_1 = @query
    WITH RESULT SETS ((plot varbinary(max)))
    END
    GO
    ```

2. 现在，运行不含参数的存储过程，以通过硬编码为输入查询的数据生成绘图。

    ```sql
    EXEC [dbo].[PyPlotMatplotlib]
    ```

3. 结果应如下所示：
  
    ```sql
    plot
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    ```

4. 在 [Python 客户端](../python/setup-python-client-tools-sql.md)中，现在可以连接到生成二进制绘图对象的 SQL Server 实例，并查看绘图。 

    为此，请运行以下 Python 代码，根据需要替换服务器名称、数据库名称和凭据（对于 Windows 身份验证，将 `UID` 和 `PWD` 参数替换为 `Trusted_Connection=True`）。 请确保客户端和服务器上的 Python 版本相同。 此外，请确保客户端上的 Python 库（如 matplotlib）与安装在服务器上的库具有相同版本或比后者的版本更高。
  
    ```python
    %matplotlib notebook
    import pyodbc
    import pickle
    import os
    cnxn = pyodbc.connect('DRIVER=SQL Server;SERVER={SERVER_NAME};DATABASE={DB_NAME};UID={USER_NAME};PWD={PASSWORD}')
    cursor = cnxn.cursor()
    cursor.execute("EXECUTE [dbo].[PyPlotMatplotlib]")
    tables = cursor.fetchall()
    for i in range(0, len(tables)):
        fig = pickle.loads(tables[i][0])
        fig.savefig(str(i)+'.png')
    print("The plots are saved in directory: ",os.getcwd())
    ```

5. 如果连接成功，应看到如下所示的消息：
  
   绘图已保存到目录: xxxx
  
6. 已在 Python 工作目录中创建输出文件。 要查看绘图，请查找 Python 工作目录，然后打开该文件。 下图显示了保存到客户端计算机上的绘图。
  
   ![小费金额与车费金额](media/sqldev-python-sample-plot.png "小费金额与车费金额") 

## <a name="next-steps"></a>后续步骤

本文内容：

> [!div class="checklist"]
> + 已查看示例数据
> + 已使用 T-SQL 中的 Python 创建绘图

> [!div class="nextstepaction"]
> [Python 教程：使用 T-SQL 创建数据特征](python-taxi-classification-create-features.md)