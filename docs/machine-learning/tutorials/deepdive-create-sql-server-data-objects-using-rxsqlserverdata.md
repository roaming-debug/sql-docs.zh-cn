---
title: 创建 RxSqlServerData 对象
description: 了解如何将 RevoScaleR 函数与 SQL Server 结合使用。 本课程将继续介绍如何创建数据库：添加表和加载数据。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 47864a26c3a5090543c842b45fb6f39ef9862380
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470598"
---
# <a name="create-sql-server-data-objects-using-rxsqlserverdata-sql-server-and-revoscaler-tutorial"></a>使用 RxSqlServerData 创建 SQL Server 数据对象（SQL Server 和 RevoScaleR 教程）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

这是 [RevoScaleR 教程系列](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)的第 2 个教程，RevoScaleR 教程介绍如何在 SQL Server 中使用 [RevoScaleR 函数](/machine-learning-server/r-reference/revoscaler/revoscaler)。

本课程将继续介绍如何创建数据库：添加表和加载数据。 如果 DBA 在[教程 2](deepdive-work-with-sql-server-data-using-r.md) 中创建了数据库和登录名，则可以使用 R IDE（如 RStudio）或内置工具（如 Rgui）来添加表。

在 R 中，连接到 SQL Server 并使用 RevoScaleR 函数执行以下任务：

> [!div class="checklist"]
> * 创建用于定型数据和预测的表
> * 将本地 .csv 文件中的数据加载到表中

示例数据是模拟信用卡欺诈数据（ccFraud 数据集），这些数据分区为定型数据集和评分数据集。 数据文件包含在 **RevoScaleR** 中。

使用 R IDE 或 **Rgui** 完成这些任务。 请确保使用在此位置找到的 R 可执行文件：C:\Program Files\Microsoft\R Client\R_SERVER\bin\x64（Rgui.exe [如果使用该工具]，或指向 C:\Program Files\Microsoft\R Client\R_SERVER 的 R IDE）。 拥有包含这些可执行文件的 [R 客户端工作站](../r/set-up-a-data-science-client.md)被视为本教程的先决条件。

## <a name="create-the-training-data-table"></a>创建定型数据表

1. 在 R 变量中存储数据库连接字符串。 下面是适用于 SQL Server 的有效 ODBC 连接字符串的两个示例：一个使用 SQL 登录名，另一个适用于 Windows 集成身份验证。 

   请务必酌情修改服务器名称、用户名和密码。

    **SQL 登录名**
    ```R
    sqlConnString <- "Driver=SQL Server;Server=<server-name>; Database=RevoDeepDive;Uid=<user_name>;Pwd=<password>"
    ```

    **Windows 身份验证**
    ```R
    sqlConnString <- "Driver=SQL Server;Server=<server-name>;Database=RevoDeepDive;Trusted_Connection=True"
    ```

2. 指定要创建的表的名称，并将它保存在一个 R 变量中。
  
    ```R
    sqlFraudTable <- "ccFraudSmall"
    ```
  
    由于服务器实例和数据库名称已作为连接字符串的一部分进行指定，在合并两个变量时，新表的 *完全限定* 名称会变为 *instance.database.schema.ccFraudSmall*。
  
3.  （可选）指定 *rowsPerRead* 以控制在每个批次中读取的数据行数。
  
    ```R
    sqlRowsPerRead = 5000
    ```
  
    虽然此参数为可选参数，但设置它可获得更高的计算效率。 **RevoScaleR** 和 **MicrosoftML** 中的大部分增强型分析函数按区块处理数据。 *rowsPerRead* 参数确定每个区块中的行数。
  
    可能需要通过对此设置进行尝试来找出适当的平衡。 如果值太大，则在内存不足以处理该大小的区块中的数据时，数据访问可能会很慢。 相反，在某些系统中，如果 *rowsPerRead* 的值太小，则性能也可能会较低。
  
    作为初始值，请使用数据库引擎实例定义的默认批处理大小来控制每个区块中的行数（5000 行）。 将该值保存在变量 *sqlRowsPerRead* 中。
  
4.  为新数据源对象定义一个变量，并将以前定义的参数传递给 **RxSqlServerData** 构造函数。 请注意，这只会创建数据源对象而不会填充它。 加载数据是单独的步骤。
  
    ```R
    sqlFraudDS <- RxSqlServerData(connectionString = sqlConnString,
       table = sqlFraudTable,
       rowsPerRead = sqlRowsPerRead)
    ```

## <a name="create-the-scoring-data-table"></a>创建评分数据表

使用相同的步骤，通过相同的过程创建保存评分数据的表。

1. 创建一个新的 R 变量 sqlScoreTable，以存储用于计分的表的名称。
  
    ```R
    sqlScoreTable <- "ccFraudScoreSmall"
    ```
  
2. 将该变量作为参数提供给 RxSqlServerData 函数，以定义第二个数据源对象 sqlScoreDS。
  
    ```R
    sqlScoreDS <- RxSqlServerData(connectionString = sqlConnString,
       table = sqlScoreTable, rowsPerRead = sqlRowsPerRead)
    ```

由于已在 R 工作区中将连接字符串和其他参数定义为变量，可将其重复用于表示不同的表、视图或查询的新数据源。

> [!NOTE]
> 函数使用不同的参数定义基于整个表的数据源，而不是定义基于查询的数据源。 这是因为 SQL Server 数据库引擎必须以不同的方式准备查询。 本教程后面部分介绍如何基于 SQL 查询创建数据源对象。

## <a name="load-data-into-sql-tables-using-r"></a>使用 R 将数据加载到 SQL 表中

现在已创建了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表，你可以使用相应的 **Rx** 函数将数据加载到这些表中。

**RevoScaleR** 包包含特定于数据源类型的函数。 对于文本数据，请使用 [RxTextData](/machine-learning-server/r-reference/revoscaler/rxtextdata) 生成数据源对象。 有一些其他函数可用于通过 Hadoop 数据、ODBC 数据等创建数据源对象。

> [!NOTE]
> 对于此部分，必须对数据库具有 **执行 DDL** 权限。

### <a name="load-data-into-the-training-table"></a>将数据加载到定型表中

1. 创建 R 变量 *ccFraudCsv*，并向其分配包含示例数据的 CSV 文件的文件路径。 此数据集在 **RevoScaleR** 中提供。 “SampleDataDir”是 **rxGetOption** 函数中的关键字。
  
    ```R
    ccFraudCsv <- file.path(rxGetOption("sampleDataDir"), "ccFraudSmall.csv")
    ```
  
    请注意对 **rxGetOption** 的调用，该方法是与 **RevoScaleR** 中的 [rxOptions](/machine-learning-server/r-reference/revoscaler/rxoptions) 关联的 GET 方法。 使用此实用工具设置和列出与本地及远程计算上下文相关的选项，例如默认共享目录或要在计算中使用的处理器（核心）数。
    
    无论从何处运行代码，此特别调用都可以从正确的库中获取示例。 例如，尝试在 SQL Server 和开发计算机上运行该函数，查看路径有何不同。
  
2. 定义一个变量来存储新数据，并使用 RxTextData 函数指定文本数据源。
  
    ```R
    inTextData <- RxTextData(file = ccFraudCsv,      colClasses = c(
        "custID" = "integer", "gender" = "integer", "state" = "integer",
        "cardholder" = "integer", "balance" = "integer",
        "numTrans" = "integer",
        "numIntlTrans" = "integer", "creditLine" = "integer",
        "fraudRisk" = "integer"))
    ```
  
    参数 colClasses 十分重要。 可使用它指示要将分配给从文本文件加载的每列数据的数据类型。 在此示例中，所有列都作为文本进行处理，除了命名列（它们作为整数进行处理）。
  
3. 此时，你可能要暂停片刻，并在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中查看数据库。 刷新数据库中表的列表。
  
    你将看到，尽管 R 数据对象已在本地工作区中创建，但表尚未在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中创建。 此外，未从文本文件加载任何数据到 R 变量中。
  
4. 通过调用 [rxDataStep](/machine-learning-server/r-reference/revoscaler/rxdatastep) 函数来插入数据。
  
    ```R
    rxDataStep(inData = inTextData, outFile = sqlFraudDS, overwrite = TRUE)
    ```
    
    假设连接字符串没有问题，短暂暂停之后，应看到类似下面这些的结果：
  
    写入的总行数：10000，总时间：0.466 *读取的行数：10000，已处理的总行数：10000，区块总时间：0.577 秒* 
  
5. 刷新表的列表。 若要验证是否每个变量都具有正确的数据类型并且已成功导入，还可以右键单击 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的表并选择“选择前 1000 行”。

### <a name="load-data-into-the-scoring-table"></a>将数据加载到评分表中

1. 重复用于将评分数据集加载到数据库中的步骤。
  
    首先提供源文件的路径。
  
    ```R
    ccScoreCsv <- file.path(rxGetOption("sampleDataDir"), "ccFraudScoreSmall.csv")
    ```
  
2. 使用 RxTextData 函数获取数据并将其保存在变量 inTextData 中。
  
    ```R
    inTextData <- RxTextData(file = ccScoreCsv,      colClasses = c(
        "custID" = "integer", "gender" = "integer", "state" = "integer",
        "cardholder" = "integer", "balance" = "integer",
        "numTrans" = "integer",
        "numIntlTrans" = "integer", "creditLine" = "integer"))
    ```
  
3.  调用 rxDataStep 函数以使用新架构和数据覆盖当前表。
  
    ```R
    rxDataStep(inData = inTextData, sqlScoreDS, overwrite = TRUE)
    ```
  
    - inData 参数定义要使用的数据源。
  
    - outFile 参数指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中要用于保存数据的表。
  
    - 如果该表已存在并且未使用 *overwrite* 选项，则结果会在无截断的情况下插入。
  
同样，如果连接成功，你将看到一条消息，表明完成以及将数据写入到表中所需的时间：

*写入的总行数：10000，总时间：0.384*
*读取的行数：10000，处理的总行数：10000，总区块时间：0.456 秒*

## <a name="more-about-rxdatastep"></a>有关 rxDataStep 的详细信息

[rxDataStep](/machine-learning-server/r-reference/revoscaler/rxdatastep) 是一个功能强大的函数，可以对 R 数据帧执行多个转换。 还可以使用 rxDataStep 将数据转换为目标所需的表示形式：在本例中为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。

（可选）可通过使用 **rxDataStep** 参数中的 R 函数来指定对数据的转换。 本教程稍后会提供这些操作的示例。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [查询和修改 SQL Server 数据](../../machine-learning/tutorials/deepdive-query-and-modify-the-sql-server-data.md)