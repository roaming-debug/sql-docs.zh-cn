---
title: 运行作业：适用于 VS Code 的 Spark & Hive Tools
titleSuffix: SQL Server Big Data Clusters
description: 在 SQL Server 大数据群集上使用适用于 Visual Studio Code 的 Spark & Hive Tools 提交 Spark 作业。
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6474b863e5078b94da5dc79c1c1a7f90b3faf761
ms.sourcegitcommit: e120899c5e72ce3108d1e459703ccd2ea6a84a5b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2020
ms.locfileid: "97353708"
---
# <a name="submit-spark-jobs-on-sql-server-big-data-cluster-in-visual-studio-code"></a>在 Visual Studio Code 中的 SQL Server 大数据群集上提交 Spark 作业

了解如何使用适用于 Visual Studio Code 的 Spark & Hive Tools 来创建和提交 Apache Spark 的 PySpark 脚本，首先我们将介绍如何在 Visual Studio Code 中安装 Spark & Hive Tools，然后演示如何将作业提交到 Spark。  

Spark & Hive Tools 可以安装在 Visual Studio Code 支持的平台上，包括 Windows、Linux 和 macOS。 下面介绍了不同平台的必备条件。


## <a name="prerequisites"></a>先决条件

完成本文中的步骤需要以下各项：

- SQL Server 大数据群集。 请参阅 [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://docs.microsoft.com/sql/big-data-cluster/big-data-cluster-overview)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Mono](https://www.mono-project.com/docs/getting-started/install/)。 Mono 仅适用于 Linux 和 macOS。
- [为 Visual Studio Code 设置 PySpark 交互式环境](/azure/hdinsight/set-up-pyspark-interactive-environment)。
- 名为“SQLBDCexample”的本地目录  。  本文使用“C:\SQLBDC\SQLBDCexample”  。

## <a name="install-spark--hive-tools"></a>安装 Spark & Hive Tools

完成必备条件后，可以安装适用于 Visual Studio Code 的 Spark & Hive Tools。  完成以下步骤以安装 Spark & Hive Tools：

1. 打开 Visual Studio Code。

2. 从菜单栏中，导航到“查看” > “扩展”   。

3. 在搜索框中，输入“Spark & Hive”  。

4. 从搜索结果中选择“Spark & Hive Tools”，然后选择“安装”   。  

   ![安装扩展](./media/spark-hive-tools-vscode/extension.png)

5. 需要时重新加载。

## <a name="open-work-folder"></a>打开工作文件夹

完成以下步骤以打开工作文件夹，并在 Visual Studio Code 中创建一个文件：

1. 从菜单栏中，导航到“文件” > “打开文件夹...” > “C:\SQLBDC\SQLBDCexample”，然后选择“选择文件夹”按钮     。 该文件夹显示在左侧的“资源管理器”视图中  。

2. 在“资源管理器”视图中，选择文件夹“SQLBDCexample”，然后选择工作文件夹旁边的“新建文件”图标    。

   ![新建文件](./media/spark-hive-tools-vscode/new-file.png)

3. 使用 `.py`（Spark 脚本）文件扩展名命名新文件。  此示例使用 HelloWorld.py  。
4. 将以下代码复制并粘贴到脚本文件中：
   ```python
    import sys
    from operator import add
    from pyspark.sql import SparkSession, Row
    
    spark = SparkSession\
        .builder\
        .appName("PythonWordCount")\
        .getOrCreate()
    
    data = [Row(col1='pyspark and spark', col2=1), Row(col1='pyspark', col2=2), Row(col1='spark vs hadoop', col2=2), Row(col1='spark', col2=2), Row(col1='hadoop', col2=2)]
    df = spark.createDataFrame(data)
    lines = df.rdd.map(lambda r: r[0])

    counters = lines.flatMap(lambda x: x.split(' ')) \
        .map(lambda x: (x, 1)) \
        .reduceByKey(add)
    
    output = counters.collect()
    sortedCollection = sorted(output, key = lambda r: r[1], reverse = True)
    
    for (word, count) in sortedCollection:
        print("%s: %i" % (word, count))
   ```

## <a name="link-a-sql-server-big-data-cluster"></a>链接 SQL Server 大数据群集

在从 Visual Studio Code 将脚本提交到群集之前，需要链接 SQL Server 大数据群集。

1. 从菜单栏导航到“查看” > “命令面板…”，然后输入“Spark / Hive:    Link a Cluster”。

   ![链接群集命令](./media/spark-hive-tools-vscode/link-cluster-command.png)

2. 选择链接群集类型“SQL Server 大数据”  。

3. 输入 SQL Server 大数据终结点。

4. 输入 SQL Server 大数据群集用户名。

5. 输入用户管理员的密码。

6. 设置群集的显示名称（可选）。

7. 列出群集，查看“输出”视图以进行验证  。

## <a name="list-clusters"></a>列出群集

1. 从菜单栏导航到“查看” > “命令面板…”，然后输入“Spark / Hive:    List Cluster”。

2. 检查“输出”视图  。  该视图将显示链接群集。

    ![设置默认群集配置](./media/spark-hive-tools-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>设置默认群集

1. 如果已关闭，请重新打开[之前](#open-work-folder)创建的文件夹“SQLBDCexample”  。  

2. 选择[之前](#open-work-folder)创建的文件“HelloWorld.py”，它将在脚本编辑器中打开  。

3. 如果尚未链接群集，请将其链接。

4. 右键单击脚本编辑器，然后选择“Spark / Hive:  Set Default Cluster”。   

5. 选择一个群集作为当前脚本文件的默认群集。 这些工具会自动更新配置文件“.VSCode \ settings.json”  。 

   ![设置默认群集配置](./media/spark-hive-tools-vscode/set-default-cluster-configuration.png)

## <a name="submit-interactive-pyspark-queries"></a>提交交互式 PySpark 查询

可以按照以下步骤提交交互式 PySpark 查询：

1. 如果已关闭，请重新打开[之前](#open-work-folder)创建的文件夹“SQLBDCexample”  。  

2. 选择[之前](#open-work-folder)创建的文件“HelloWorld.py”，它将在脚本编辑器中打开  。

3. 如果尚未链接群集，请将其链接。

4. 选择所有代码并右键单击脚本编辑器，选择“Spark:  PySpark Interactive”以提交查询，或使用快捷键 Ctrl+Alt+I  。

   ![pyspark 交互式上下文菜单](./media/spark-hive-tools-vscode/pyspark-interactive-right-click.png)

5. 如果尚未指定默认群集，请选择群集。 几分钟后，“Python Interactive”结果将显示在新选项卡中  。利用这些工具可以使用上下文菜单提交代码块而不是整个脚本文件。 

   ![pyspark 交互式 python 交互式窗口](./media/spark-hive-tools-vscode/pyspark-interactive-python-interactive-window.png) 

6. 输入“%%info”，然后按 Shift+Enter 查看作业信息   。 (可选)

   ![查看作业信息](./media/spark-hive-tools-vscode/pyspark-interactive-view-job-information.png)

   > [!NOTE] 
   >
   > 如果在设置中取消选中“Python 扩展已启用”（选中默认设置），则提交的 pyspark 交互结果将使用旧窗口  。
   >
   > ![pyspark 交互式 python 扩展已禁用](./media/spark-hive-tools-vscode/pyspark-interactive-python-extension-disabled.png)


## <a name="submit-pyspark-batch-job"></a>提交 PySpark 批处理作业

1. 如果已关闭，请重新打开[之前](#open-work-folder)创建的文件夹“SQLBDCexample”  。  

2. 选择[之前](#open-work-folder)创建的文件“HelloWorld.py”，它将在脚本编辑器中打开  。

3. 如果尚未链接群集，请将其链接。

4. 右键单击脚本编辑器，然后选择“Spark:  PySpark Batch”，或使用快捷方式 Ctrl+Alt+H  。 

5. 如果尚未指定默认群集，请选择群集。 提交 Python 作业后，提交日志将显示在 Visual Studio Code 的“输出”窗口中  。 还会显示“Spark UI URL”和“Yarn UI URL”   。 你可以在 Web 浏览器中打开 URL 以跟踪作业状态。

   ![提交 Python 作业结果](./media/spark-hive-tools-vscode/submit-pythonjob-result.png) 

## <a name="apache-livy-configuration"></a>Apache Livy 配置

支持 [Apache Livy](https://livy.incubator.apache.org/) 配置，在工作空间文件夹中的 .VSCode\settings.json 中可以设置该配置  。 目前，Livy 配置仅支持 Python 脚本。 更多详细信息，请参阅 [Livy 自述文件](https://github.com/cloudera/livy/blob/master/README.rst )。

### <a name="how-to-trigger-livy-configuration"></a><a id="triggerlivyconf"></a>如何触发 Livy 配置 

#### <a name="method-1"></a>方法 1

1. 从菜单栏中，导航到“文件” > “首选项” > “设置”    。  
2. 在“搜索设置”文本框中输入“HDInsight Job Sumission:   Livy Conf”。  
3. 选择“在 settings.json 中编辑”以获取相关搜索结果  。

#### <a name="method-2"></a>方法 2

提交文件，注意 `.vscode` 文件夹会自动添加到工作文件夹中。 可以通过单击 `.vscode\settings.json` 找到 Livy 配置。

+ 项目设置：

    ![Livy 配置](./media/spark-hive-tools-vscode/hdi-livyconfig.png)

>[!NOTE]
>对于设置“driverMomory”和“executorMomry”，请使用单位设置值，例如 1g 或 1024m   。 

### <a name="supported-livy-configurations"></a>支持的 Livy 配置

#### <a name="post-batches"></a>POST /批处理

**请求正文**

| name | description | type |
| :- | :- | :- |
| 文件 | 包含要执行的应用程序的文件 | 路径（必需） |
| proxyUser | 运行作业时要模拟的用户 | 字符串 |
| className | 应用程序 Java/Spark 主类 | 字符串 |
| args | 应用程序的命令行参数 | 字符串列表 |
| jars | 将在本次会话中使用的 jars | 字符串列表 |
| pyFiles | 将在本次会话中使用的 Python 文件 | 字符串列表 |
| files | 将在本次会话中使用的文件 | 字符串列表 |
| driverMemory | 用于驱动程序进程的内存量 | 字符串 |
| driverCores | 用于驱动程序进程的内核数 | int |
| executorMemory | 每个执行程序进程使用的内存量 | 字符串 |
| executorCores | 每个执行程序使用的内核数 | int |
| numExecutors | 为此会话启动的执行程序数 | int |
| archives | 将在本次会话中使用的存档 | 字符串列表 |
| queue | 提交到的 YARN 队列的名称 | 字符串 |
| name | 会话的名称 | 字符串 |
| conf | Spark 配置属性 | key=val 的映射 |

#### <a name="response-body"></a>响应正文

创建的批处理对象。

| name | description | type |
| :- | :- | :- |
| id | 会话 ID | int |
| appId | 此会话的应用程序 ID | String |
| appInfo | 详细的应用程序信息 | key=val 的映射 |
| log | 日志行 | 字符串列表 |
| state | 批处理状态 | 字符串 |

>[!NOTE]
>提交脚本时，分配的 Livy 配置将显示在输出窗格中。

## <a name="additional-features"></a>其他功能

适用于 Visual Studio Code 的 Spark & Hive 支持以下功能：

- **IntelliSense 自动完成**。 弹出关键字、方法、变量等的建议。 不同的图标代表不同类型的对象。

    ![适用于 Visual Studio Code 的 Spark & Hive Tools IntelliSense 对象类型](./media/spark-hive-tools-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense 错误标记**。 语言服务强调了 Hive 脚本的编辑错误。     
- **语法突出显示**。 语言服务使用不同的颜色来区分变量、关键字、数据类型、函数等。 

    ![适用于 Visual Studio Code 的 Spark & Hive Tools 语法突出显示](./media/spark-hive-tools-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="unlink-cluster"></a>取消链接群集

1. 从菜单栏导航到“查看” > “命令面板…”，然后输入“Spark / Hive:    Unlink a Cluster”。  

2. 选择要取消链接的群集。  

3. 查看“输出”视图以进行验证  。  

## <a name="next-steps"></a>后续步骤
有关 SQL Server 大数据群集和相关方案的详细信息，请参阅 [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](https://docs.microsoft.com/sql/big-data-cluster/big-data-cluster-overview)。
