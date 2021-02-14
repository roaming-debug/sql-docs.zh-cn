---
title: 通过 Team Foundation Build 运行 SQL Server 单元测试
description: 了解如何通过 Team Foundation Build 运行 SQL Server 单元测试。 查看如何在自动测试运行中创建生成定义并运行单元测试。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 24f5b85d-d6f9-415f-b09f-933b78dc0b67
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2b87720ba7a4cbdb531cd19fe23be7d48d9a07ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018127"
---
# <a name="how-to-run-sql-server-unit-tests-from-team-foundation-build"></a>如何：通过 Team Foundation Build 运行 SQL Server 单元测试

可以使用 Team Foundation Build 将 SQL Server 单元测试作为生成验证测试 (BVT) 的一部分运行。 可以配置单元测试以部署数据库、生成测试数据，然后运行选定测试。 如果您不熟悉 Team Foundation Build，则应该在执行本主题中的过程之前查看以下信息：  
  
-   [创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
  
-   [如何：在生成应用程序后配置和运行计划的测试](/previous-versions/visualstudio/visual-studio-2010/ms182465(v=vs.100))  
  
-   [创建基本生成定义](/previous-versions/visualstudio/visual-studio-2010/ms181716(v=vs.100))  
  
在使用这些过程之前，必须首先通过执行以下任务来配置工作环境：  
  
-   安装 Team Foundation Build 和 Team Foundation 版本控制。 您可能必须在不同计算机上安装 Team Foundation Build 和 Team Foundation 版本控制。  
  
-   将 Microsoft SQL Server Data Tools 生成实用工具安装在 Team Foundation Build 所在的计算机上。 要安装 SQL Server Data Tools 生成实用工具，请首先执行一个管理安装点。 有关管理安装点的更多信息，请参阅[安装 SQL Server Data Tools](./download-sql-server-data-tools-ssdt.md)。 然后从用于管理安装点的位置 (/location) 将 SSDTBuildUtilties.msi 安装到生成服务器。  
  
-   连接到 Visual Studio Team Foundation Server 的实例。  
  
在配置工作环境后，必须执行以下步骤：  
  
1.  创建一个数据库项目。  
  
2.  为该数据库项目导入或创建架构和对象。  
  
3.  配置数据库项目属性以进行生成和部署。  
  
4.  创建一个或多个单元测试。  
  
5.  将包含数据库项目和单元测试项目的解决方案添加到版本控制中并签入所有文件。  
  
本主题中的过程介绍如何创建生成定义以作为自动化测试运行的一部分运行单元测试：  
  
1.  [配置测试设置以在 x64 生成代理上运行数据库单元测试](#ConfigureX64)  
  
2.  [为测试指定测试类别（可选）](#CreateATestList)  
  
3.  [修改测试项目](#ModifyTestProject)  
  
4.  [签入解决方案](#CheckInTheTestList)  
  
5.  [创建生成定义](#CreateBuildDef)  
  
6.  [运行新生成定义](#RunBuild)  
  
在生成计算机上运行 SQL Server 单元测试  
  
在生成计算机上运行单元测试时，单元测试可能无法找到数据库项目文件 (.dbproj)。 出现此问题的原因是 app.config 文件使用相对路径来引用这些文件。 此外，如果单元测试无法找到要用于运行单元测试的 SQL Server 实例，则单元测试可能会失败。 如果生成计算机中存储在 app.config 文件中的连接字符串无效，则会出现此问题。  
  
若要解决这些问题，您必须在 app.config 中指定使用特定于 Team Foundation Build 环境的配置文件替代 app.config 的替代部分。 有关详细信息，请参见本主题后面的[修改测试项目](#ModifyTestProject)。  
  
## <a name="configure-test-settings-to-run-sql-server-unit-tests-on-an-x64-build-agent"></a><a name="ConfigureX64"></a>配置测试设置以在 x64 生成代理上运行 SQL Server 单元测试  
在 x64 生成代理上运行单元测试之前，您必须配置测试设置以更改宿主进程平台。  
  
#### <a name="to-specify-the-host-process-platform"></a>指定宿主进程平台  
  
1.  打开包含要配置其设置的测试项目的解决方案。  
  
2.  在“解决方案资源管理器”中的“解决方案项”文件夹中，双击“Local.testsettings”文件。  
  
    此时将出现“测试设置”对话框。  
  
3.  在列表中，单击“主机”。  
  
4.  在详细信息窗格中的“宿主进程平台”中，单击“MSIL”以将测试配置为在 x64 生成代理上运行。  
  
5.  单击“应用”。  
  
## <a name="assign-tests-to-a-test-category-optional"></a><a name="CreateATestList"></a>为测试指定测试类别（可选）  
通常，在创建生成定义以运行单元测试时，可以指定一个或多个测试类别。 在运行生成时，将运行指定类别中的所有测试。  
  
#### <a name="to-assign-tests-to-a-test-category"></a>为测试指定测试类别  
  
1.  打开“测试视图”窗口。  
  
2.  选择测试。  
  
3.  在属性窗格中，单击“测试类别”，然后单击最右侧列中的省略号 (…)。  
  
4.  在“测试类别”窗口中的“添加新类别”框中，为新测试类别键入名称。  
  
5.  单击“添加” ，然后单击“确定”。  
  
    新建的测试类别将分配给您的测试，并且可通过测试的属性将其用于其他测试。  
  
## <a name="modify-the-test-project"></a><a name="ModifyTestProject"></a>修改测试项目  
默认情况下，Team Foundation Build 在生成单元测试项目时会从项目的 app.config 文件创建配置文件。 在 app.config 文件中，数据库项目的路径是以相对路径的方式存储的。 在 Visual Studio 中有效的相对路径将变得无效，因为 Team Foundation Build 将生成文件放在与运行单元测试的位置不同的位置。 另外，app.config 文件包含指定要测试的数据库的连接字符串。 如果单元测试必须连接到与创建测试项目时使用的数据库不同的数据库，则您还需要为 Team Foundation Build 提供单独的 app.config 文件。 通过在下一过程中进行修改，您可以设置测试项目和生成服务器，以便 Team Foundation Build 使用不同的配置。  
  
> [!IMPORTANT]  
> 您必须为每个测试项目（.vbproj 或 .vsproj）执行此过程。  
  
#### <a name="to-specify-an-appconfig-file-for-team-foundation-build"></a>为 Team Foundation Build 指定 app.config 文件  
  
1.  在“解决方案资源管理器”中，右键单击 app.config 文件，然后单击“复制”。  
  
2.  右键单击测试项目，然后单击“粘贴”。  
  
3.  右键单击名为“app.config 的副本”的文件，然后单击“重命名”。  
  
4.  键入 _BuildComputer_ **.sqlunitttest.config** 并按 ENTER，其中 *BuildComputer* 是你的生成代理运行所在计算机的名称。  
  
5.  双击 BuildComputer.sqlunitttest.config。  
  
    将在编辑器中打开此配置文件。  
  
6.  通过添加 Sources 文件夹的文件夹级别以及与解决方案同名的子文件夹，更改 .dbproj 文件的相对路径。 例如，如果配置文件最初包括以下条目：  
  
    ```  
    <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database3\Database3.sqlproj"      Configuration="Debug" />  
    ```  
  
    按如下所示更新文件：  
  
    ```  
    <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database3\Database3.sqlproj"      Configuration="Debug" />  
    ```  
  
    完成后，BuildComputer.sqlunitttest.config 文件应类似于 Visual Studio 2010 的以下示例：  
  
    ```  
    <SqlUnitTesting_VS2010>  
        <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database4\Database4.sqlproj"  
            Configuration="Debug" />  
        <DataGeneration ClearDatabase="true" />  
        <ExecutionContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
            CommandTimeout="30" />  
        <PrivilegedContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
            CommandTimeout="30" />  
    </SqlUnitTesting_VS2010>  
    ```  
  
    或者，如果你使用的是 Visual Studio 2012：  
  
    ```  
    <SqlUnitTesting_VS2012>  
            <DatabaseDeployment DatabaseProjectFileName="..\..\..\Database4\Database4.sqlproj"  
                Configuration="Debug" />  
            <DataGeneration ClearDatabase="true" />  
            <ExecutionContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
                CommandTimeout="30" />  
            <PrivilegedContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(localdb)\Projects;Initial Catalog=Database4;Integrated Security=True;Pooling=False"  
                CommandTimeout="30" />  
        </SqlUnitTesting_VS2012>  
    ```  
  
7.  更新 ExecutionContext 和 PrivilegedContext 的 ConnectionString 属性，以指定与要部署到的目标数据库的连接。  
  
8.  在“文件”  菜单上，单击“全部保存” 。  
  
9. 在解决方案资源管理器中，双击 app.config。  
  
10. 在编辑器中，为每个 \<SqlUnitTesting_*VSVersion*> 节点添加 `AllowConfigurationOverride="true"`。 例如：  
  
    ```  
    -- Update SqlUnitTesting_VS2010 node to:  
    <SqlUnitTesting_VS2010 AllowConfigurationOverride="true">   
  
    -- Update SqlUnitTesting_VS2012 node to:  
    <SqlUnitTesting_VS2012 AllowConfigurationOverride="true">  
    ```  
  
    进行此更改后，将允许 Team Foundation Build 使用您创建的替换配置文件。  
  
11. 在“文件”  菜单上，单击“全部保存” 。  
  
    接下来，必须更新 Local.testsettings 以包括自定义的配置文件。  
  
#### <a name="to-customize-localtestsettings-to-deploy-the-customized-configuration-file"></a>自定义 Local.testsettings 以部署自定义的配置文件  
  
1.  在解决方案资源管理器中，双击 Local.testsettings。  
  
    此时将出现“测试设置”对话框。  
  
2.  在类别列表中，单击“部署”。  
  
3.  选中“启用部署”复选框。  
  
4.  单击“添加文件”。  
  
5.  在“添加部署文件”对话框中，指定你创建的 BuildComputer.sqlunitttest.config 文件。  
  
6.  单击“应用”。  
  
7.  单击“关闭”。  
  
8.  在“文件”  菜单上，单击“全部保存” 。  
  
    接下来，将您的解决方案签入版本控制中。  
  
## <a name="check-in-the-solution"></a><a name="CheckInTheTestList"></a>签入解决方案  
在此过程中，需要签入您的解决方案的所有文件。 这些文件包括您的解决方案的测试元数据文件，其中包含测试类别关联和测试。 在添加、删除、重新组织或更改测试内容时，会自动更新测试元数据文件以反映这些更改。  
  
> [!NOTE]  
> 此过程介绍的步骤针对的是使用 Team Foundation 版本控制的情形。 如果您使用的是其他的版本控制软件，则必须按照适合您的软件的步骤操作。  
  
#### <a name="to-check-in-the-solution"></a>签入解决方案  
  
1.  连接到运行 Team Foundation Server 的计算机。  
  
    有关详细信息，请参阅[使用源代码管理器](/previous-versions/visualstudio/visual-studio-2010/ms181370(v=vs.100))。  
  
2.  如果您的解决方案尚未包含在源代码管理中，请将它添加到源代码管理中。  
  
    有关更多信息，请参阅[将项目或解决方案添加到版本控制](/previous-versions/visualstudio/visual-studio-2010/ms181374(v=vs.100))。  
  
3.  单击“视图”，然后单击“挂起的签入”。  
  
4.  签入您的解决方案的所有文件。  
  
    有关详细信息，请参阅[签入挂起的更改](/previous-versions/visualstudio/visual-studio-2010/ms181411(v=vs.100))。  
  
    > [!NOTE]  
    > 您可能有特定的团队流程来控制如何创建和管理自动测试。 例如，该流程可能要求您先在本地验证您的生成，然后再将该代码以及在代码上运行的测试一起签入。  
  
    在“解决方案资源管理器”中，挂锁图标显示在每个文件旁边，以指示该文件已签入。 有关详细信息，请参阅[查看版本控制文件和文件夹属性](/previous-versions/visualstudio/visual-studio-2010/ms245468(v=vs.100))。  
  
    您的测试现在已经可用于 Team Foundation Build。 您现在可以创建包含要运行的测试的生成定义。  
  
## <a name="create-a-build-definition"></a><a name="CreateBuildDef"></a>创建生成定义  
  
#### <a name="to-create-a-build-definition"></a>创建生成定义  
  
1.  在团队资源管理器中，单击你的团队项目，右键单击“生成”节点，然后单击“新建生成定义”。  
  
    此时将显示“新建生成定义”窗口。  
  
2.  在“生成定义名称”中，键入要用于此生成定义的名称。  
  
3.  在导航栏中，单击“生成默认值”。  
  
4.  在“将生成输出复制到以下放置文件夹(UNC 路径，如 \\\server\share)”中，指定一个文件夹以包含生成输出。  
  
    您可以指定本地计算机上的共享文件夹或生成过程有权访问的任何网络位置。  
  
5.  在导航栏中，单击“进程”。  
  
6.  在“必需”组中的“要生成的项”中，单击“浏览(…)”按钮 。  
  
7.  在“生成项目列表编辑器”对话框中，单击“添加”。  
  
8.  指定在本演练前面添加到版本控制中的解决方案文件 (.sln)，然后单击“确定”。  
  
    此解决方案将显示在“要生成的项目或解决方案文件”列表中。  
  
9. 单击“确定”。  
  
10. 在“基本”组中的“自动测试”中，指定要运行的测试。 默认情况下，将运行解决方案中名为 \*test\*.dll 的文件中包含的测试。  
  
11. 在“文件”菜单上，单击“保存 ProjectName”。  
  
    现在，您已创建了生成定义。 接下来，需要修改测试项目。  
  
## <a name="run-the-new-build-definition"></a><a name="RunBuild"></a>运行新生成定义  
  
#### <a name="to-run-the-new-build-type"></a>运行新生成类型  
  
1.  在团队资源管理器中，展开团队项目节点，展开“生成”节点，右键单击要运行的生成定义，然后单击“使新生成入队”。  
  
    此时将显示“**将生成 {** _TeamProjectName_ **}** 排队”对话框，其中包含所有现有生成类型的列表。  
  
2.  如果需要，在“生成定义”中单击新生成定义。  
  
3.  确认“生成定义”、“生成代理”和“该生成的放置文件夹”字段中的值全部正确，然后单击“排队”。  
  
    此时将显示“生成资源管理器”的“排队”选项卡。 有关详细信息，请参阅[管理和查看已完成的生成 (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms181730(v=vs.100))或[在生成资源管理器中管理你的生成 (Visual Studio 2012)](/previous-versions/ms181732(v=vs.140))。  
  
## <a name="see-also"></a>另请参阅  
[运行 SQL Server 单元测试](../ssdt/running-sql-server-unit-tests.md)  
[创建基本生成定义](/previous-versions/visualstudio/visual-studio-2010/ms181716(v=vs.100))  
[将生成排队](/previous-versions/visualstudio/visual-studio-2010/ms181722(v=vs.100))  
[监视正在运行的生成的进度](/previous-versions/visualstudio/visual-studio-2010/ms181724(v=vs.100))  
