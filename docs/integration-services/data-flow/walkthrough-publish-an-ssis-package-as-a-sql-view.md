---
description: 演练：将 SSIS 包作为 SQL 视图发布
title: 演练：将 SSIS 包作为 SQL 视图发布 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.packagepublishwizard.f1
ms.assetid: d32d9761-93fb-4020-bf82-231439c6f3ac
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0a9c522bf292147852203fb9ac740e7103195b35
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835986"
---
# <a name="walkthrough-publish-an-ssis-package-as-a-sql-view"></a>演练：将 SSIS 包作为 SQL 视图发布

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  本演练提供在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中将 SSIS 包作为 SQL 视图发布的详细步骤。  
  
## <a name="prerequisites"></a>必备条件  
 若要执行本演练，必须先在计算机上安装以下软件。  
  
1.  包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的 [!INCLUDE[ssNoVersion ](../../includes/ssnoversion-md.md)]。  
  
2.  [SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md)。  
  
## <a name="step-1-build-and-deploy-ssis-project-to-the-ssis-catalog"></a>步骤 1：生成 SSIS 项目并将其部署到 SSIS 目录  
 在此步骤中，创建一个从 SSIS 支持的数据源（在本示例中，我们使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库）中提取数据并使用数据流目标组件输出数据的 SSIS 包。 然后，将生成 SSIS 项目并将其部署到 SSIS 目录。  
  
1.  启动 **SQL Server Data Tools**。 在 **“开始”** 菜单上，依次指向 **“所有程序”** 、 **Microsoft SQL Server**，再单击 **SQL Server Data Tools**。  
  
2.  创建一个新的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
    1.  在菜单栏上单击“文件”  ，指向“新建”  ，然后单击“项目”  。  
  
    2.  在左窗格中展开“商业智能”  ，然后在树视图中单击“Integration Services”  。  
  
    3.  选择“Integration Services 项目”（如果尚未选中）。  
  
    4.  在“项目名称”中指定 **SSISPackagePublishing** 。  
  
    5.  指定项目的位置。  
  
    6.  单击“确定”  关闭“新建项目”  对话框。  
  
3.  将“数据流”  组件从“SSIS 工具箱”  拖到“控制流”  选项卡的设计图面。  
  
4.  在“控制流”中双击“数据流”组件打开“数据流设计器”。  
  
5.  将 **源组件** 从工具箱拖到“数据流设计器”，然后并将它配置为从数据源提取数据。  
  
    1.  为了进行演练，请创建一个包含表 **Employee** 的测试数据库 **TestDB**。 创建包含三列： **ID**、 **FirstName** 和 **LastName** 的表。  
  
    2.  将 **ID** 设为主键。  
  
    3.  插入两条包含以下数据的记录。  
  
        |ID|FIRSTNAME|LASTNAME|  
        |--------|---------------|--------------|  
        |1|John|Doe|  
        |2|Jane|Doe|  
  
    4.  将“OLE DB 源”  组件从“SSIS 工具箱”  拖到“数据流设计器”  。  
  
    5.  将组件配置为从 **TestDB** 数据库中的 **Employee** 表提取数据。 为“OLE DB 连接管理器”选择“(local).TestDB”，为“数据访问模式”选择“表或视图”，为“表或视图的名称”选择“[dbo].[Employee]”。  
  
         ![数据流目标 - OLE DB 连接](../../integration-services/data-flow/media/dsd-oledbconnectionmanager.jpg "数据流目标 - OLE DB 连接")  
  
6.  现在，将“数据流目标”从工具箱拖到数据流。 应该可以在工具箱的“常用”部分中找到此组件。  
  
7.  将数据流中的“OLE DB 源”  组件连接到“数据流目标”  组件。  
  
8.  生成 SSIS 项目并将其部署到 SSIS 目录。  
  
    1.  在菜单栏上单击“项目”  ，然后单击“部署”  。  
  
    2.  按照向导中的说明将项目部署到本地数据库服务器中的 SSIS 目录。 以下示例使用 **Power BI** 作为文件夹名称，使用 **SSISPackagePublishing** 作为 SSIS 目录中的项目名称。  
  
## <a name="step-2-use-the-ssis-data-feed-publishing-wizard-to-publish-ssis-package-as-a-sql-view"></a>步骤 2：使用 SSIS 数据馈送发布向导将 SSIS 包发布为 SQL 视图  
 在此步骤中，你将使用 SQL Server Integration Services (SSIS) 数据馈送发布向导将 SSIS 包发布为 SQL Server 数据库中的视图。 可通过查询此视图来使用包的输出数据。  
  
 SSIS 数据馈送向导创建一个使用 OLE DB Provider for SSIS (SSISOLEDB) 的链接服务器，然后在链接服务器上创建一个包含查询的 SQL 视图。 该查询包括 SSIS 目录中的文件夹名称、项目名称和包名称。  
  
 在运行时，该视图通过你创建的链接服务器将查询发送到 OLE DB Provider for SSIS。 OLE DB Provider for SSIS 执行你在查询中指定的包，然后向查询返回表格结果集。  
  
1.  通过运行 C:\Program Files\Microsoft SQL Server\130\DTS\Binn 中的 ISDataFeedPublishingWizard.exe，或者单击“开始\所有程序”下的 Microsoft SQL Server 2016\SQL Server 2016 Data Feed Publishing Wizard，来启动 **SSIS 数据馈送发布向导** 。  
  
2.  在“简介”页上，单击“下一步”。  
  
     ![数据馈送发布向导 -“简介”页](../../integration-services/data-flow/media/dsd-feedpublishingwizard-introductionpage.jpg "数据馈送发布向导 -“简介”页")  
  
3.  在“包设置”页上执行以下任务：  
  
    1.  键入包含 SSIS 目录的的 SQL Server 实例的 **名称** ，或单击“浏览”选择服务器。  
  
         ![数据馈送发布向导 -“包设置”页](../../integration-services/data-flow/media/dsd-feedpublishingwizard-packagesettingspage.jpg "数据馈送发布向导 -“包设置”页")  
  
    2.  单击“路径”字段旁边的“浏览”  ，浏览 SSIS 目录，选择要发布的 SSIS 包（例如：**SSISDB**->**SSISPackagePublishing**->**Package.dtsx**），然后单击“确定”  。  
  
         ![数据馈送发布向导 - 浏览包](../../integration-services/data-flow/media/dsd-feedpublishingwizard-browseforpackage.jpg "数据馈送发布向导 - 浏览包")  
  
    3.  使用页面底部的“包参数”、“项目参数”和“连接管理器”选项卡，输入包的任何包参数、项目参数或连接管理器设置的值。 还可以指定要用于包执行的环境引用，并将项目/包参数绑定到环境变量。  
  
         我们建议你将敏感参数绑定到环境变量。 这是为了确保不会以纯文本格式将敏感参数的值存储在向导创建的 SQL 视图中。  
  
    4.  单击“下一步”  切换“发布设置”页  。  
  
4.  在“发布设置”页上执行以下任务：  
  
    1.  选择要创建的视图的 **数据库** 。  
  
         ![数据馈送发布向导 -“发布设置”页](../../integration-services/data-flow/media/dsd-feedpublishingwizard-publishsettingspage.jpg "数据馈送发布向导 -“发布设置”页")  
  
    2.  输入 **视图** 的 **名称**。 也可以从下拉列表中选择一个现有视图。  
  
    3.  在“设置”列表中，指定要与视图关联的 **链接服务器** 的 **名称** 。 如果链接服务器尚不存在，向导将在创建视图之前创建链接服务器。 还可以在此处设置 **User32BitRuntime** 和 **Timeout** 的值。  
  
    4.  单击“高级”按钮。 你应会看到“高级设置”对话框。  
  
    5.  在“高级设置”对话框中执行以下操作：  
  
        1.  指定要在其中创建视图的数据库架构（“架构”字段）。  
  
        2.  指定在通过网络发送数据之前是否应当将其加密（“加密”字段）。 有关此设置和 TrustServerCertificate 设置的更多详细信息，请参阅 [Using Encryption Without Validation](../../relational-databases/native-client/features/using-encryption-without-validation.md) （在不验证的情况下使用加密）主题。  
  
        3.  指定在启用加密设置时是否可以使用自签名服务器证书（**TrustServerCertificate** 字段）。  
  
        4.  单击“确定”  关闭“高级设置”  对话框。  
  
    6.  单击“下一步”  切换到“验证”页  。  
  
5.  在“验证”页上，检查验证所有设置的值后返回的结果。 在以下示例中，你将看到针对链接服务器存在状态的 **警告** ，因为选定的 SQL Server 实例上不存在链接服务器。 如果你看到“结果”中包含“错误”，请将鼠标悬停在“错误”，这样便可以查看有关该错误的详细信息。 例如，如果你尚未启用“允许对 SSISOLEDB 提供程序使用 inprocess 选项”，则在执行链接服务器配置操作时会收到错误。  
  
     ![数据馈送发布向导 -“验证”页](../../integration-services/data-flow/media/dsd-feedpublishingwizard-validationpage.jpg "数据馈送发布向导 -“验证”页")  
  
6.  若要将此报告保存为 XML 文件，请单击“保存报告”。  
  
7.  在“验证”页上单击“下一步”切换到“摘要”页。  
  
8.  在“摘要”  页中检查所做的选择，然后单击“发布”  启动发布过程。这将会创建链接服务器（如果服务器上没有链接服务器），然后使用链接服务器创建视图。  
  
     ![数据馈送发布向导 -“摘要”页](../../integration-services/data-flow/media/dsd-feedpublishingwizard-summarypage.jpg "数据馈送发布向导 -“摘要”页")  
  
     现在，可以针对 TestDB 数据库执行以下 SQL 语句，来查询包的输出数据：SELECT * FROM [SSISPackageView]。  
  
9. 若要将此报告保存为 XML 文件，请单击“保存报告”。  
  
10. 查看发布过程的结果，然后单击“完成”关闭向导。  
  
    > [!NOTE]  
    >  不支持以下数据类型：text、ntext、image、nvarchar(max)、varchar(max) 和 varbinary(max)。  
  
## <a name="step-3-test-the-sql-view"></a>步骤 3：测试 SQL 视图  
 在此步骤中，你将运行 SSIS 数据馈送发布向导创建的 SQL 视图。  
  
1.  启动 SQL Server Management Studio。  
  
2.  展开 \<**machine name**>、“数据库”、\<**database you selected in the wizard**> 和“视图” 。  
  
3.  右键单击向导创建的 \<**view created by the wizard**>，然后单击“选择前 1000 行”。  
  
4.  确认能够看到 SSIS 包的结果。  
  
## <a name="step-4-verify-the-ssis-package-execution"></a>步骤 4：验证 SSIS 包执行  
 在此步骤中，你将验证是否已执行 SSIS 包。  
  
1.  在 SQL Server Management Studio 中，依次展开“Integration Services 目录”  、“SSISDB”  、包含 SSIS 项目的 **文件夹**、“项目”  、项目节点、“包”  。  
  
2.  右键单击 SSIS 包，指向“报告”  ，指向“标准报告”  ，然后单击“所有执行”  。  
  
3.  你应会报告中看到 SSIS 包的执行状态。  
  
    > [!NOTE]  
    >  在 Windows Vista Service Pack 2 计算机上，你可能会在报告中看到两个 SSIS 包执行状态，一个成功，一个失败。 忽略失败的执行，因为它是由此版本中的已知问题造成的。  
  
## <a name="more-info"></a>更多信息  
 数据馈送发布向导将执行以下重要步骤：  
  
1.  创建链接服务器，并将其配置为使用 OLE DB Provider for SSIS。  
  
2.  在指定的数据库中创建 SQL 视图，该视图将查询包含所选包的目录信息的链接服务器。  
  
 本部分提供在不使用数据馈送发布向导的情况下，创建链接服务器和 SQL 视图的过程。 此外，还提供有关将 OPENQUERY 函数与 OLE DB Provider for SSIS 配合使用的其他信息。  
  
### <a name="create-a-linked-server-using-the-ole-db-provider-for-ssis"></a>使用 OLE DB Provider for SSIS 创建链接服务器  
 在 SQL Server Management Studio 中，通过运行以下查询来使用 OLE DB Provider for SSIS (SSISOLEDB) 创建链接服务器。  
  
```sql 
  
USE [master]  
GO  
  
EXEC sp_addlinkedserver  
@server = N'SSISFeedServer',  
@srvproduct = N'Microsoft',  
@provider = N'SSISOLEDB',  
@datasrc = N'.'  
GO  
  
```  
  
### <a name="create-a-view-using-linked-server-and-ssis-catalog-information"></a>使用链接服务器和 SSIS 目录信息创建视图  
 在此步骤中，你将创建一个针对你在上一部分中创建的链接服务器运行查询的 SQL 视图。 该查询将包括 SSIS 目录中的文件夹名称、项目名称和包名称。  
  
 在运行时执行该视图时，视图中定义的链接服务器查询将启动查询中指定的 SSIS 包，并接收表格结果集形式的包输出。  
  
1.  在创建视图之前，请在新查询窗口中键入并运行以下查询。 OPENQUERY 是 SQL Server 支持的行集函数。 它使用与链接服务器关联的 OLE DB 提供程序在指定的链接服务器上执行指定的传递查询。 OPENQUERY 可以在查询的 FROM 子句中引用，就好象它是一个表名。 有关详细信息，请参阅 [MSDN 库上的 OPENQUERY 文档](../../t-sql/functions/openquery-transact-sql.md) 。  
  
    ```sql
    SELECT * FROM OPENQUERY(SSISFeedServer,N'Folder=Eldorado;Project=SSISPackagePublishing;Package=Package.dtsx')   
    GO  
    ```  
  
    > [!IMPORTANT]  
    >  如果需要，请更新文件夹名称、项目名称和包名称。 如果 OPENQUERY 函数失败，请在 **SQL Server Management Studio** 中依次展开“服务器对象”  、“链接服务器”  、“提供程序”  ，然后双击 **SSISOLEDB** 提供程序，并确保“允许 inprocess”  选项处于启用状态。  
  
2.  通过运行以下查询，在数据库 **TestDB** 中为本演练创建一个视图。  
  
    ```sql
  
    USE [TestDB]   
    GO   
  
    CREATE VIEW SSISPackageView AS   
    SELECT * FROM OPENQUERY(SSISFeedServer, 'Folder=Eldorado;Project=SSISPackagePublishing;Package=Package.dtsx')   
    GO  
  
    ```  
  
3.  通过运行以下查询来测试该视图。  
  
    ```sql
    SELECT * FROM SSISPackageView  
    ```  
  
### <a name="openquery-function"></a>OPENQUERY 函数  
 OPENQUERY 函数的语法是：  
  
```sql 
SELECT * FROM OPENQUERY(<LinkedServer Name>, N'Folder=<Folder Name from SSIS Catalog>; Project=<SSIS Project Name>; Package=<SSIS Package Name>; Use32BitRuntime=[True | False];Parameters="<parameter_name_1>=<value1>; parameter_name_2=<value2>";Timeout=<Number of Seconds>;')  
```  
  
 Folder、Project 和 Package 参数是必需的。 Use32BitRuntime、Timeout 和 Parameters 是可选的。  
  
 Use32BitRuntime 的值可以是 0、1、true 或 false。 它指示当 SQL Server 的平台是 64 位时，包是否应使用 32 位运行时（1 或 true）运行。  
  
 Timeout 指示在 SSIS 包中的新数据到达之前，OLE DB Provider for SSIS 可以等待的秒数。 默认情况下，超时为 60 秒。 可以指定介于 20 和 32000 之间的整数超时值。  
  
 Parameters 包含包参数和项目参数的值。 参数的规则与 [DTExec](../packages/dtexec-utility.md)中的参数相同。  
  
 以下列表指定了查询子句中允许的特殊字符：  
  
-   单引号 (') - 标准 OPENQUERY 支持此字符。 如果你想要在查询子句中使用单引号，请使用两个单引号 ('')。  
  
-   双引号 (") - 查询的参数部分括在双引号中。 如果参数值本身包含双引号，请使用转义符。 例如：\"。  
  
-   左右方括号（[ 和 ]）- 这些字符用于指示首部/尾部空格。 例如，“[ some spaces ]”表示字符串“ some spaces ”的首部和尾部各有一个空格。 如果在查询子句中使用这些字符本身，必须将其转义。 例如， \\[ 和 \\]。  
  
-   正斜杠 (\\) - 查询子句中使用的每个 \ 必须使用转义符。 例如，查询子句中的 \\\ 将作为 \ 计算。  
  
## <a name="see-also"></a>另请参阅  
 [数据流目标](../../integration-services/data-flow/data-streaming-destination.md)   
 [配置数据流目标](./data-streaming-destination.md)  
  
