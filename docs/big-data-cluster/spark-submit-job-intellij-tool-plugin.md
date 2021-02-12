---
title: 运行 Spark 作业：Azure Toolkit for IntelliJ
titleSuffix: SQL Server Big Data Clusters
description: 了解如何通过提交一个本地 Jar 或 Py 文件在 Azure Toolkit for IntelliJ 中的 SQL Server 大数据群集上提交 Spark 作业。
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.topic: conceptual
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 36718ef7ea9def99a2b2629119cbd7dfee63dac3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043627"
---
# <a name="submit-spark-jobs-on-big-data-clusters-2019-in-intellij"></a>在 IntelliJ 的 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 上提交 Spark 作业

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的主要方案之一是能够提交 Spark 作业。 使用 Spark 作业提交功能，可以提交引用 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的本地 Jar 或 Py 文件。 它还允许你执行已经位于 HDFS 文件系统中的 Jar 或 Py 文件。 

## <a name="prerequisites"></a>先决条件

- SQL Server 大数据群集。
- Oracle Java 开发工具包。 可以从 [Oracle 网站](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)安装该工具包。
- IntelliJ IDEA。 可以从 [JetBrains 网站](https://www.jetbrains.com/idea/download/)进行安装。
- Azure Toolkit for IntelliJ 扩展。 要获取安装说明，请参阅[安装 Azure Toolkit for IntelliJ](/azure/azure-toolkit-for-intellij-installation)。

## <a name="link-sql-server-big-data-cluster"></a>链接 SQL Server 大数据群集
1. 打开 IntelliJ IDEA 工具。

2. 如果使用的是自签名证书，请在“工具”  菜单中依次选择“Azure”  、“验证 Spark 群集 SSL 证书”  和“禁用”  ，以禁用 TLS/SSL 证书验证。

    ![链接 SQL Server 大数据群集 - 禁用 TLS/SSL](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-disableSSL.png)

3. 从“视图”菜单中选择“工具窗口”，然后选择“Azure Explorer”，以打开“Azure 资源管理器”    。
4. 右键单击“SQL Server 大数据群集”，然后选择“链接 SQL Server 大数据群集”   。 输入“服务器”、“用户名”和“密码”，然后单击“确定”     。

    ![链接大数据群集 - 对话框](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-dialog.png)

5. 出现“不受信任服务器的证书”对话框时，单击“接受”  。 稍后可管理证书，请参阅 [Server Certificates](https://www.jetbrains.com/help/idea/settings-tools-server-certificates.html)（服务器证书）。

6. 链接的群集列表位于“SQL Server 大数据群集”下  。 可以通过打开 spark 历史记录 UI 和 Yarn UI 来监视 spark 作业，也可以通过右键单击该群集以取消链接。

    ![链接大数据群集 - 上下文菜单](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-contextmenu.png)

## <a name="create-a-spark-scala-application-from-spark-template"></a>使用 Spark 模板创建 Spark Scala 应用程序

1. 启动 IntelliJ IDEA，然后创建一个项目。 在“新建项目”对话框中，按照下面的步骤操作  ： 

   a. 选择“Azure Spark/HDInsight” >  带有示例的 Spark 项目 (Scala)   。

   b. 在“生成工具”列表中，根据需要选择以下某项  ：

      * **Maven**，用于 Scala 项目创建向导支持
      * **SBT**，用于管理依赖项和生成 Scala 项目

    ![“新建项目”对话框](./media/spark-submit-job-intellij-tool-plugin/create-hdi-scala-app.png)

2. 选择“**下一页**”。

3. Scala 项目创建向导自动检测是否安装了 Scala 插件。 选择“安装”  。

   ![Scala 插件检查](./media/spark-submit-job-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. 若要下载 Scala 插件，请选择“确定”  。 请按照说明重启 IntelliJ。 

   ![“Scala 插件安装”对话框](./media/spark-submit-job-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. 在“新建项目”窗口中，执行以下操作  ：  

    ![选择 Spark SDK](./media/spark-submit-job-intellij-tool-plugin/hdi-new-project.png)

   a. 输入项目名称和位置。

   b. 在“项目 SDK”下拉列表中，选择“Java 1.8”（适用于 Spark 2.x 群集），或选择“Java 1.7”（适用于 Spark 1.x 群集）    。

   c. 在“Spark 版本”下拉列表中，Scala 项目创建向导集成了适用于 Spark SDK 和 Scala SDK 的适当版本  。 如果 Spark 群集版本低于 2.0，请选择“Spark 1.x”  。 否则，请选择“Spark 2.x”  。 此示例使用“Spark 2.0.2 (Scala 2.11.8)”  。

6. 选择“完成”  。

7. Spark 项目自动为你创建项目。 要查看项目，请执行以下步骤：

   a. 在“文件”菜单中，选择“项目结构”   。

   b. 在“项目结构”对话框中，选择“项目”以查看创建的默认项目   。 也可以通过选择加号 (+) 来创建自己的项目  。

      ![对话框中的项目信息](./media/spark-submit-job-intellij-tool-plugin/default-artifact.png)
      

## <a name="submit-application-to-sql-server-big-data-cluster"></a>将应用程序提交到 SQL Server 大数据群集
在链接 SQL Server 大数据群集之后，可以将应用程序提交到该群集。

1. 在“运行/调试配置”窗口中设置配置，单击 +->“SQL Server 上的 Apache Spark”，选择“在群集中远程运行”选项卡，并按以下所示设置参数，然后单击“确定”    。

    ![交互式控制台添加配置条目](./media/spark-submit-job-intellij-tool-plugin/interactive-console-add-config-entry.png)

    ![链接大数据群集 - 配置](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-config.png)

    * 对于“Spark 群集(仅限 Linux)”  ，请选择要在其上运行应用程序的群集。

    * 从 IntelliJ 项目中选择一个项目，或从硬盘中选择一个项目。

    * “主类名”  字段：默认值是所选文件中的主类。 可以通过选择省略号 ( **...** ) 并选择其他类来更改类。   

    * “作业配置”  字段：默认值按如上图所示设置。 可以更改值或为作业提交添加新的键/值。 更多相关信息：[Apache Livy REST API](http://livy.incubator.apache.org./docs/latest/rest-api.html)

      ![“Spark 提交”对话框作业配置的含义](./media/spark-submit-job-intellij-tool-plugin/submit-job-configurations.png)

    * “命令行参数”  字段：如有需要，可以输入按主类的空间拆分的参数值。

    * “引用的 Jar”  和“引用的文件”  字段：可以输入引用的 Jar 和引用的文件的路径（如果有）。 更多相关信息：[Apache Spark 配置](https://spark.apache.org/docs/latest/configuration.html#runtime-environment) 

      ![“Spark 提交”对话框 jar 文件的含义](./media/spark-submit-job-intellij-tool-plugin/jar-files-meaning.png)

       > [!NOTE]  
       > 若要上传引用的 JAR 和引用的文件，请参阅：[如何将资源上传到群集](/azure/storage/blobs/storage-quickstart-blobs-storage-explorer)
                         
    * **上传路径**：可以指示 Jar 或 Scala 项目资源提交的存储位置。 支持多种存储类型：“使用 Spark 交互式会话上传”  和“使用 WebHDFS 上传” 
    
2. 单击“SparkJobRun”以将项目提交到所选的群集  。 底部的“群集中的远程 Spark 任务”选项卡显示作业执行进度  。 通过单击红色按钮，即可停止应用程序。  

    ![链接大数据群集 - 运行](./media/spark-submit-job-intellij-tool-plugin/link-ariscluster-run.png)

## <a name="spark-console"></a>Spark 控制台
可以运行 Spark 本地控制台 (Scala) 或运行 Spark Livy 交互式会话控制台 (Scala)。

### <a name="spark-local-consolescala"></a>Spark 本地控制台 (Scala)
确定自己是否满足 WINUTILS.EXE 先决条件。

1. 从菜单栏中，导航到“运行” > “编辑配置...”   。

2. 在“运行/调试配置”窗口的左窗格中，导航到“SQL Server 大数据群集上的 Apache Spark” > “[Spark on SQL] myApp”    。

3. 在主窗口中，选择“在本地运行”选项卡  。

4. 提供以下值，然后选择“确定”  ：

    |properties |值 |
    |----|----|
    |作业主类|默认值是所选文件中的主类。 可以通过选择省略号 ( **...** ) 并选择其他类来更改类。|
    |环境变量|请确认 HADOOP_HOME 的值是否正确。|
    |WINUTILS.exe 位置|请确保路径正确。|

    ![本地控制台设置配置](./media/spark-submit-job-intellij-tool-plugin/console-set-configuration.png)

5. 从项目中，导航到“myApp” > “src” > “main” > “scala” > “myApp”      。  

6. 从菜单栏中，导航到“工具” > “Spark 控制台” > “运行 Spark 本地控制台(Scala)”    。

7. 然后，系统可能会显示两个对话框，询问你是否要自动修复依赖项。 如果出现对话框，请选择“自动修复”  。

    ![Spark 自动修复 1](./media/spark-submit-job-intellij-tool-plugin/console-auto-fix1.png)

    ![Spark 自动修复 2](./media/spark-submit-job-intellij-tool-plugin/console-auto-fix2.png)

8. 控制台应如下图所示。 在“控制台”窗口中键入 `sc.appName`，然后按 Ctrl+Enter。  系统将显示结果。 可以通过单击红色按钮终止本地控制台。

    ![本地控制台结果](./media/spark-submit-job-intellij-tool-plugin/local-console-result.png)


### <a name="spark-livy-interactive-session-consolescala"></a>Spark Livy 交互式会话控制台 (Scala)
Spark Livy 交互式会话控制台 (Scala) 仅在 IntelliJ 2018.2 和 2018.3 上受支持。

1. 从菜单栏中，导航到“运行” > “编辑配置...”   。

2. 在“运行/调试配置”窗口的左窗格中，导航到“SQL Server 大数据群集上的 Apache Spark” > “[Spark on SQL] myApp”    。

3. 在主窗口中，选择“在群集中远程运行”选项卡  。

4. 提供以下值，然后选择“确定”  ：

    |properties |值 |
    |----|----|
    |Spark 群集（仅限 Linux）|选择要在其上运行应用程序 SQL Server 大数据群集。|
    |主类名|默认值是所选文件中的主类。 可以通过选择省略号 ( **...** ) 并选择其他类来更改类。|

    ![交互式控制台设置配置](./media/spark-submit-job-intellij-tool-plugin/interactive-console-configuration.png)

5. 从项目中，导航到“myApp” > “src” > “main” > “scala” > “myApp”      。  

6. 从菜单栏中，导航到“工具” > “Spark 控制台” > “运行 Spark Livy 交互式会话控制台(Scala)”    。

7. 控制台应如下图所示。 在“控制台”窗口中键入 `sc.appName`，然后按 Ctrl+Enter。  系统将显示结果。 可以通过单击红色按钮终止本地控制台。

    ![交互式控制台结果](./media/spark-submit-job-intellij-tool-plugin/interactive-console-result.png)

### <a name="send-selection-to-spark-console"></a>向 Spark 控制台发送所选内容

为方便起见，通过向本地控制台或 Livy 交互式控制台 (Scala) 发送一些代码，即可查看脚本结果。 可以在 Scala 文件中突出显示一些代码，然后右键单击“向 Spark 控制台发送所选内容”  。 所选代码将被发送到控制台并在其中执行。 结果将显示在控制台中的代码后面。 控制台将检查是否存在错误。  

   ![向 Spark 控制台发送所选内容](./media/spark-submit-job-intellij-tool-plugin/send-selection-to-console.png)

## <a name="next-steps"></a>后续步骤
有关 SQL Server 大数据群集和相关方案的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md)？