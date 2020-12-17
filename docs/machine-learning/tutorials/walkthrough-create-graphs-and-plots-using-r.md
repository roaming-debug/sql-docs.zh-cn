---
title: R 教程：创建图形和绘图
description: 学习使用 R 语言和 SQL Server 数据生成绘图和地图的技巧。 创建简单的直方图，然后开发更复杂的地图绘图。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: ada99a4058b2b3657a9064e42f2f2ca1f2aedbb3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470008"
---
# <a name="create-graphs-and-plots-using-sql-and-r-walkthrough"></a>使用 SQL 和 R 创建图形和绘图（演练）
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

在本演练的此部分中，你将学习使用 R 和 SQL Server 数据生成绘图和地图的技术。 创建简单的直方图，然后开发更复杂的地图绘图。

## <a name="prerequisites"></a>必备知识

此步骤基于本演练之前的步骤假设一个正在进行的 R 会话。 它使用在这些步骤中创建的连接字符串和数据源对象。 以下工具和包用于运行脚本。

+ Rgui.exe 用于运行 R 命令
+ Management Studio 用于运行 T-SQL
+ googMap
+ ggmap 包
+ mapproj 包

## <a name="create-a-histogram"></a>创建直方图

1. 使用 [rxHistogram](/r-server/r-reference/revoscaler/rxdatasource) 函数生成第一个绘图。  rxHistogram 函数提供的功能类似于开放源代码 R 包的功能，但可在远程执行上下文中运行。

    ```R
    # Plot fare amount on SQL Server and return the plot
    start.time <- proc.time()
    rxHistogram(~fare_amount, data = inDataSource, title = "Fare Amount Histogram")
    used.time <- proc.time() - start.time
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2), " seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate plot.", sep=""))
    ```

2. 在开发环境的 R 图形设备中返回图像。  例如，在 RStudio 中，单击“绘图”  窗口。  [!INCLUDE[rsql_rtvs](../../includes/rsql-rtvs-md.md)]中将打开一个单独的图形窗口。

    ![使用 rxHistogram 绘制费用金额](media/rsql-e2e-rxhistogramresult.png "使用 rxHistogram 绘制费用金额")

    > [!NOTE]
    > 图形看起来是否不同？
    >  
    > 这是因为 inDataSource 仅使用前 1000 行。 由于没有 ORDER BY 子句，无法确定使用 TOP 的行顺序，因此数据和生成的图形可能会有所不同。
    > 这一特定图像使用大约 10,000 行数据生成。 建议试验不同数量的行，获取不同的图形，并记下它在环境中返回结果所需的时间。

## <a name="create-a-map-plot"></a>创建地图绘图

通常，数据库服务器会阻止 Internet 访问。 使用需要下载地图或其他图像以生成图的 R 包时，这可能会带来不便。 但是，在你开发自己的应用程序时，你可能会发现以下这种解决方法会很有用。 基本思路为，在客户端上生成地图表示形式，然后在地图上覆盖作为属性存储在 SQL Server 表中的点。

1. 定义创建 R 绘图对象的函数。 自定义 R 函数 mapPlot 会创建一个散点图，该散点图使用出租车接客位置，并绘制从每个位置开始的搭乘数。 它使用 ggplot2 和 ggmap 包，应[已安装并加载](walkthrough-data-science-end-to-end-walkthrough.md#add-packages)了这两个包 。

    ```R
    mapPlot <- function(inDataSource, googMap){
        library(ggmap)
        library(mapproj)
        ds <- rxImport(inDataSource)
        p <- ggmap(googMap)+
        geom_point(aes(x = pickup_longitude, y =pickup_latitude ), data=ds, alpha =.5,
    color="darkred", size = 1.5)
        return(list(myplot=p))
    }
    ```

    + mapPlot 函数采用两个参数：之前使用 RxSqlServerData 定义的现有数据对象，以及从客户端传递的地图表示形式。
    + 在以 ds 变量开头的行中，rxImport 用于将来自先前创建的数据源 inDataSource 的数据加载到内存中。 （该数据源仅包含 1000 行；如果要创建包含更多数据点的地图，则可以替换其他数据源。）
    + 只要使用开放源代码 R 函数，就必须将数据加载到本地内存中的数据帧。 但是，通过调用 [rxImport ](/r-server/r-reference/revoscaler/rximport)函数，你可以在远程计算上下文的内存中运行。

2. 将计算上下文更改为本地，并加载创建地图所需的库。

    ```R
    rxSetComputeContext("local")
    library(ggmap)
    library(mapproj)
    gc <- geocode("Times Square", source = "google")
    googMap <- get_googlemap(center = as.numeric(gc), zoom = 12, maptype = 'roadmap', color = 'color');
    ```

    + `gc` 变量存储一组纽约时代广场的坐标。

    + 以 `googmap` 开头的行生成中心具有指定坐标的地图。

3. 通过将绘图函数包装在 [rxExec](/r-server/r-reference/revoscaler/rxexec) 中，切换到 SQL Server 计算上下文并呈现结果，如下所示。 rxExec 函数属于 **RevoScaleR** 包，并支持在远程计算上下文中执行任意 R 函数。

    ```R
    rxSetComputeContext(sqlcc)
    myplots <- rxExec(mapPlot, inDataSource, googMap, timesToRun = 1)
    plot(myplots[[1]][["myplot"]]);
    ````

    + `googMap` 中的地图数据作为参数传递给远程执行的函数 mapPlot。 这些地图是在本地环境中生成的，因此必须将其传递到函数，以便在 SQL Server 的上下文中创建绘图。

    + 当以 `plot` 开头的行运行时，呈现的数据将序列化为本地 R 环境，以便可以在 R 客户端中查看它。

    > [!NOTE]
    > 如果在 Azure 虚拟机中使用 SQL Server，则此时可能会出现错误。 如果 Azure 中的默认防火墙规则通过 R 代码阻止网络访问，则会发生错误。 有关如何解决此错误的详细信息，请参阅[在 Azure VM 上安装机器学习 (R) 服务](../install/sql-machine-learning-azure-virtual-machine.md)。

4. 下图显示了输出绘图。 出租车打车位置已作为红色的点添加到地图中。 你的图像看起来可能有所不同，具体取决于使用的数据源中的位置数。

    ![使用自定义 R 函数绘制出租车行程](media/rsql-e2e-mapplot.png "使用自定义 R 函数绘制出租车行程")

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 R 和 SQL 创建数据功能](walkthrough-create-data-features.md)