---
description: 部署 Integration Services (SSIS) 项目和包
title: 部署 Integration Services (SSIS) 项目和包 | Microsoft Docs
ms.custom: ''
ms.date: 09/26/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: vanto
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.bids.converttolegacydeployment.f1
- sql13.ssis.deploymentwizard.f1
- sql13.ssis.ssms.isenvprop.permissions.f1
- sql13.ssis.ssms.isenvprop.general.f1
- sql13.ssis.ssms.iscreateenv.f1
- sql13.ssis.ssms.isenvprop.variables.f1
- sql13.ssis.migrationwizard.f1
ms.assetid: bea8ce8d-cf63-4257-840a-fc9adceade8c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5ee14ee789174ad12db2cea297202fc63ee68d66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346942"
---
# <a name="deploy-integration-services-ssis-projects-and-packages"></a>部署 Integration Services (SSIS) 项目和包

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 支持两种部署模型：项目部署模型和旧的包部署模型。 项目部署模型使您可以将项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器。  
  
有关早期包部署模型的详细信息，请参阅[包部署 (SSIS)](../../integration-services/packages/legacy-package-deployment-ssis.md)。  
  
> [!NOTE]  
>  项目部署模型是在 [!INCLUDE[ssISversion11](../../includes/ssisversion11-md.md)]中引入的。 使用此部署模型时，不能在未部署整个项目的情况下部署一个或多个包。 [!INCLUDE[ssISversion13](../../includes/ssisversion13-md.md)] 引入的增量包部署功能能够让你部署一个或多个包，无需部署整个项目。

> [!NOTE]
> 本文介绍如何在一般情况下部署 SSIS 包以及如何在本地部署包。 还可将 SSIS 包部署到以下平台：
> - **Microsoft Azure 云**。 有关详细信息，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md)。
> - **Linux**。 有关详细信息，请参阅[使用 SSIS 在 Linux 上提取、转换和加载数据](../../linux/sql-server-linux-migrate-ssis.md)。

## <a name="compare-project-deployment-model-and-legacy-package-deployment-model"></a>比较项目部署模型和旧的包部署模型

 您为项目选择的部署模型的类型将决定可用于该项目的开发和管理选项。 下表显示使用项目部署模型和使用包部署模型之间的差异和相似之处。  
  
|在使用项目部署模型时|使用旧的包部署模型时|  
|---------------------------------------------|----------------------------------------------------|  
|项目是部署单元。|包是部署单元。|  
|参数用于向包属性赋值。|配置用于向包属性赋值。|  
|将包含包和参数的项目生成为一个项目部署文件（.ispac 扩展名）。|包（.dtsx 扩展名）和配置（.dtsConfig 扩展名）单独保存到文件系统中。|  
|将包含包和参数的项目部署到 SQL Server 实例上的 SSISDB 目录中。|包和配置复制到另一台计算机上的文件系统中。 包也可以保存到 SQL Server 实例上的 MSDB 数据库中。|  
|数据库引擎需要 CLR 集成。|数据库引擎不需要 CLR 集成。|  
|特定于环境的参数值存储于环境变量中。|特定于环境的配置值存储于配置文件中。|  
|可在执行前在服务器上验证目录中的项目和包。 可以使用 SQL Server Management Studio、存储过程或托管代码执行该验证。|恰好在执行之前对包进行验证。 还可以使用 dtExec 或托管代码验证包。|  
|通过对数据库引擎启动执行，来执行包。 在开始执行前，将项目标识符、显式参数值（可选）和环境引用（可选）分配给某一执行。<br /><br /> 还可以使用 **dtExec** 执行包。|使用 **dtExec** 和 **DTExecUI** 执行实用工具执行包。 适用配置是通过命令提示符参数（可选）来标识的。|  
|在执行过程中，包生成的事件将自动捕获并保存到目录中。 您可以使用 TRANSACT-SQL 视图查询这些事件。|在执行过程中，包生成的事件不自动捕获。 日志提供程序必须添加到包以便捕获事件。|  
|包在单独的 Windows 进程中运行。|包在单独的 Windows 进程中运行。|  
|SQL Server 代理用于计划包执行。|SQL Server 代理用于计划包执行。|  


## <a name="features-of-project-deployment-model"></a>项目部署模型的功能  
 下表列出的功能仅可用于为项目部署模型开发的项目。  
  
|功能|说明|  
|-------------|-----------------|  
|参数|参数指定包将使用的数据。 您可以分别使用包参数和项目参数将参数范围限定在包级别或项目级别。 参数可用于表达式或任务中。 在将项目部署到目录时，可为每个参数分配文字值，或者使用在设计时分配的默认值。 还可以引用环境变量来代替文字值。 在包执行时解析环境变量值。|  
|环境|环境是可由 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目引用的变量的容器。 每个项目可以具有多个环境引用，但包执行的单个实例只能引用来自单个环境的变量。 环境允许您对分配给包的值进行组织。 例如，您可以具有名为“开发”、“测试”和“生产”的环境。|  
|环境变量|环境变量定义可在包执行过程中赋给参数的文字值。 若要使用某一环境变量，请创建环境引用（在与具有参数的环境相对应的项目中），向该环境变量的名称分配某一参数值，并且在配置执行实例时指定相应的环境引用。|  
|SSISDB 目录|所有 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象都在某一 SQL Server 实例上称作 SSISDB 目录的数据库中进行存储和管理。 通过该目录，您可以使用文件夹组织您的项目和环境。 每个 SQL Server 实例可具有一个目录。 每个目录中可具有零个或多个文件夹。 每个文件夹可具有零个或多个项目以及零个或多个环境。 该目录中的文件夹也可以用作针对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象的权限的边界。|  
|目录存储过程和视图|可以使用大量存储过程和视图来管理该目录中的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象。 例如，您可以指定参数和环境变量值，创建和启动执行，以及监视目录操作。 您甚至可以精确看到在执行开始前将由包使用的值。|  
  
## <a name="project-deployment"></a>项目部署  
 项目部署模型的中心是项目部署文件（.ispac 扩展名）。 该项目部署文件是自包含的部署单元，仅包含与项目中的包和参数有关的基本信息。 该项目部署文件不捕获在 Integration Services 项目文件（.dtproj 扩展名）中包含的所有信息。 例如，您用于撰写备注的附加文本文件不存储于该项目部署文件中，因此不部署到目录中。  

## <a name="permissions-required-to-deploy-ssis-projects-and-packages"></a>部署 SSIS 项目和包时所需的权限

如果在默认情况下更改 SSIS 服务帐户，可能须向非默认服务账户授予其他权限，才能成功部署包。 如果非默认服务帐户不具有所需权限，可能会收到以下错误消息。

`A .NET Framework error occurred during execution of user-defined routine or aggregate "deploy_project_internal":
System.ComponentModel.Win32Exception: A required privilege is not held by the client.`

此错误通常是因为缺少 DCOM 权限造成的。 若要修复此错误，请执行以下操作：

1.  打开“组件服务”控制台（或运行 Dcomcnfg.exe）。
2.  在“组件服务”控制台中，展开“组件服务” > “计算机” > “我的电脑” > “DCOM Config”。
3.  在列表中，查找适用于所使用的 SQL Server 版本的 Microsoft SQL Server Integration Services xx.0。 例如，SQL Server 2016 的版本为 13。
4.  右键单击并选择 **“属性”**。
5.  在“Microsoft SQL Server Integration Services 13.0 属性”对话框中，选择“安全性”选项卡。
6.  针对三组权限中的每一组 — 启动与激活、访问和配置 — 选择“自定义”，然后选择“编辑”以打开“权限”对话框。
7.  在“权限”对话框中，添加非默认服务帐户并根据需要授予“允许”权限。 通常来说，一个帐户具有“本地启动”和“本地激活”权限。
8.  单击“确定”两次，然后关闭“组件服务”控制台。

有关本部分中描述的错误和 SSIS 服务帐户所需权限的详细信息，请参阅以下博客文章：
 
- [System.ComponentModel.Win32Exception：部署 SSIS 项目时，客户端不具有所需权限](/archive/blogs/dataaccesstechnologies/system-componentmodel-win32exception-a-required-privilege-is-not-held-by-the-client-while-deploying-ssis-project)

## <a name="deploy-projects-to-integration-services-server"></a>Deploy Projects to Integration Services Server
  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的当前版本中，您可以将您的项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器。 通过 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器，您可以使用环境来管理包、运行包以及为包配置运行时值。  
  
> [!NOTE]  
>  与 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的早期版本一样，在当前版本中，您也可以将您的包部署到 SQL Server 实例并且使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务运行和管理包。 您使用包部署模型。 有关详细信息，请参阅[早期包部署 (SSIS)](../../integration-services/packages/legacy-package-deployment-ssis.md)。  
  
 若要将项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器，请完成以下任务：  
  
1.  创建一个 SSISDB 目录（如果尚未创建）。 有关详细信息，请参阅 [SSIS 目录](../../integration-services/catalog/ssis-catalog.md)。  
  
2.  通过运行“Integration Services 项目转换向导”可以将项目转换为项目部署模型。 有关详细信息，请参阅以下说明：[将项目转换为项目部署模型](#convert)  
  
    -   如果在 [!INCLUDE[ssISversion12](../../includes/ssisversion12-md.md)] 中或更高版本中创建了项目，则默认情况下该项目使用项目部署模型。  
  
    -   如果您在早期版本的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]中创建了项目，则在 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]中打开项目文件之后，将该项目转换为项目部署模型。  
  
        > [!NOTE]  
        >  如果项目包含一个或多个数据源，则在项目转换完成时删除数据源。 若要创建到项目中的包可共享的数据源的连接，请在项目级别添加连接管理器。 有关详细信息，请参阅 [在包中添加、删除或共享连接管理器](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130))。  
  
         根据您是从 **还是从** 运行 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] “Integration Services 项目转换向导” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]，该向导将执行不同的转换任务。  
  
        -   如果您是从 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]运行该向导的，则在项目中包含的包将从 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 2005、2008 或 2008 R2 转换为当前版本的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]使用的格式。 原始项目 (.dtproj) 和包 (.dtsx) 文件将升级。  
  
        -   如果你是从 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]运行该向导的，则该向导将根据项目中包含的包和配置生成一个项目部署文件 (.ispac)。 原始包 (.dtsx) 文件不升级。  
  
             您可以在该向导的 **“选择目标”** 页中选择一个现有文件或创建一个新文件。  
  
             若要在转换项目时升级包文件，请从 **运行** “Integration Services 项目转换向导” [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]。 若要单独从项目转换中升级包文件，请从 **中运行** “Integration Services 项目转换向导” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，然后运行 **“SSIS 包升级向导”**。 如果单独升级包文件，请确保您保存了这些更改。 否则，在您将项目转换为项目部署模型时，将不会转换对包的任何未保存的更改。  
  
     有关包升级的详细信息，请参阅 [升级 Integration Services 包](../../integration-services/install-windows/upgrade-integration-services-packages.md) 和 [使用 SSIS 包升级向导升级 Integration Services 包](../../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md)。  
  
3.  将项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器。 有关详细信息，请参阅下面的说明：[将项目部署到 Integration Services 服务器](#deploy)。  
  
4.  （可选）创建已部署项目的环境。 
  
###  <a name="to-convert-a-project-to-the-project-deployment-model"></a><a name="convert"></a> 将项目转换为项目部署模型  
  
1.  在 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 中打开该项目，然后在解决方案资源管理器中，右键单击该项目并单击“转换为项目部署模型”。  
  
     - 或 -  
  
     从 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 的对象资源管理器中，右键单击“项目”节点并选择“导入包”。  
  
2.  完成向导。
  
###  <a name="to-deploy-a-project-to-the-integration-services-server"></a><a name="deploy"></a> 将项目部署到 Integration Services 服务器  
  
1.  在 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]并打开项目，然后从 **“项目”** 菜单，选择 **“部署”** 以便启动 **“Integration Services 部署向导”**。  
  
     或  
  
     在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 的对象资源管理器中，展开 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] > SSISDB 节点，并查找想要部署的项目的项目文件夹。 右键单击“项目”文件夹，然后单击“部署项目”。  
  
     或  
  
     在命令提示符下，从 **%ProgramFiles%\Microsoft SQL Server\130\DTS\Binn** 运行 **isdeploymentwizard.exe**。 在 64 位计算机上， **%ProgramFiles(x86)%\Microsoft SQL Server\130\DTS\Binn** 中还有 32 位版本的工具。  
  
2.  在 **“选择源”** 页上，单击 **“项目部署文件”** 以便选择项目的部署文件。  
  
     或  
  
     单击 **“Integration Services 目录”** 以便选择已部署到 SSISDB 目录的项目。  
  
3.  完成向导。 

## <a name="deploy-packages-to-integration-services-server"></a>将包部署到 Integration Services 服务器
  [!INCLUDE[ssISversion13](../../includes/ssisversion13-md.md)] 引入了增量包部署功能，能够让你将一个或多个包部署到现有或新的项目，而无需部署整个项目。  
  
###  <a name="deploy-packages-by-using-the-integration-services-deployment-wizard"></a><a name="DeployWizard"></a> 通过使用 Integration Services 部署向导部署包  
  
1.  在命令提示符下，从 **%ProgramFiles%\Microsoft SQL Server\130\DTS\Binn** 运行 **isdeploymentwizard.exe**。 在 64 位计算机上， **%ProgramFiles(x86)%\Microsoft SQL Server\130\DTS\Binn** 中还有 32 位版本的工具。  
  
2.  在“选择源”  页上，切换到“包部署模型” 。 然后，选择包含源包的文件夹，并配置包。  
  
3.  完成向导。 按照 [Package Deployment Model](#PackageModel)中所述的后续步骤进行操作。  
  
###  <a name="deploy-packages-by-using-sql-server-management-studio"></a><a name="SSMS"></a> 使用 SQL Server Management Studio 部署包  
  
1.  在 SQL Server Management Studio 中，展开对象资源管理器中的“Integration Services 目录” > **SSISDB** 节点。  
  
2.  右键单击“项目”文件夹，然后单击“部署项目”。  
  
3.  如果看到“简介”  页，单击“下一步”  以继续。  
  
4.  在“选择源”  页上，切换到“包部署模型” 。 然后，选择包含源包的文件夹，并配置包。  
  
5.  完成向导。 按照 [Package Deployment Model](#PackageModel)中所述的后续步骤进行操作。  
  
###  <a name="deploy-packages-by-using-sql-server-data-tools-visual-studio"></a><a name="SSDT"></a> 使用 SQL Server Data Tools (Visual Studio) 部署包  
  
1.  在 Visual Studio 中，在 Integration Services 项目处于打开状态时，选择一个或多个要部署的包。  
  
2.  右键单击并选择“部署包”。 部署向导将打开，并且所选的包已配置为源包。  
  
3.  完成向导。 按照 [Package Deployment Model](#PackageModel)中所述的后续步骤进行操作。  
  
###  <a name="deploy-packages-by-using-the-deploy_packages-stored-procedure"></a><a name="StoredProcedure"></a> 使用 deploy_packages 存储过程部署包  
 可以使用 **[catalog].[deploy_packages]** 存储过程将一个或多个 SSIS 包部署到 SSIS 目录。 下面的代码示例演示如何通过此存储过程将包部署到 SSIS 服务器。 有关详细信息，请参阅 [catalog.deploy_packages](../../integration-services/system-stored-procedures/catalog-deploy-packages.md)。  
  
```cs
  
private static void Main(string[] args)  
{  
    // Connection string to SSISDB  
    var connectionString = "Data Source=.;Initial Catalog=SSISDB;Integrated Security=True;MultipleActiveResultSets=false";  
  
    using (var sqlConnection = new SqlConnection(connectionString))  
    {  
        sqlConnection.Open();  
  
        var sqlCommand = new SqlCommand  
        {  
            Connection = sqlConnection,  
            CommandType = CommandType.StoredProcedure,  
            CommandText = "[catalog].[deploy_packages]"  
        };  
  
        var packageData = Encoding.UTF8.GetBytes(File.ReadAllText(@"C:\Test\Package.dtsx"));  
  
        // DataTable: name is the package name without extension and package_data is byte array of package.  
        var packageTable = new DataTable();  
        packageTable.Columns.Add("name", typeof(string));  
        packageTable.Columns.Add("package_data", typeof(byte[]));  
        packageTable.Rows.Add("Package", packageData);  
  
        // Set the destination project and folder which is named Folder and Project.  
        sqlCommand.Parameters.Add(new SqlParameter("@folder_name", SqlDbType.NVarChar, ParameterDirection.Input, "Folder", -1));  
        sqlCommand.Parameters.Add(new SqlParameter("@project_name", SqlDbType.NVarChar, ParameterDirection.Input, "Project", -1));  
        sqlCommand.Parameters.Add(new SqlParameter("@packages_table", SqlDbType.Structured, ParameterDirection.Input, packageTable, -1));  
  
        var result = sqlCommand.Parameters.Add("RetVal", SqlDbType.Int);  
        result.Direction = ParameterDirection.ReturnValue;  
  
        sqlCommand.ExecuteNonQuery();  
    }  
}  
  
```  
  
###  <a name="deploy-packages-using-the-management-object-model-api"></a><a name="MOMApi"></a> 使用管理对象模型 API 部署包  
 下面的代码示例演示如何通过管理对象模型 API 将包部署到服务器。  
  
```cs 
  
static void Main()  
 {  
     // Before deploying packages, make sure the destination project exists in SSISDB.  
     var connectionString = "Data Source=.;Integrated Security=True;MultipleActiveResultSets=false";  
     var catalogName = "SSISDB";  
     var folderName = "Folder";  
     var projectName = "Project";  
  
     // Get the folder instance.  
     var sqlConnection = new SqlConnection(connectionString);  
     var store = new Microsoft.SqlServer.Management.IntegrationServices.IntegrationServices(sqlConnection);  
     var folder = store.Catalogs[catalogName].Folders[folderName];  
  
     // Key is package name without extension and value is package binaries.  
     var packageDict = new Dictionary<string, string>();  
  
     var packageData = File.ReadAllText(@"C:\Folder\Package.dtsx");  
     packageDict.Add("Package", packageData);  
  
     // Deploy package to the destination project.  
     folder.DeployPackages(projectName, packageDict);  
 }  
  
```

## <a name="convert-to-package-deployment-model-dialog-box"></a>“转换为包部署模型”对话框
  使用 **“转换为包部署模型”** 命令可以在检查项目和项目中的每个包与包部署模型是否兼容后将包转换为该模型。 如果包使用项目部署模型特有的功能（如参数），则不能转换该包。  

 将包转换为包部署模型需要两个步骤。  
  
1.  从 **“项目”** 菜单选择 **“转换为包部署模型”** 命令时，检查项目和其中的每个包是否与此模型兼容。 结果显示在 **“结果”** 表中。  
  
     如果项目或其中的某个包未通过兼容测试，单击 **“结果”** 列中的 **“失败”** 可以获取详细信息。 单击 **“保存报告”** ，以将此信息的副本保存到文本文件。  
  
2.  如果项目和所有包都通过了兼容测试，则单击 **“确定”** 以转换包。  
  
> [!NOTE]
> 若要将项目转换为项目部署模型，请使用 **“Integration Services 项目转换向导”**。 有关详细信息，请参阅 [Integration Services Project Conversion Wizard](deploy-integration-services-ssis-projects-and-packages.md)。  

## <a name="integration-services-deployment-wizard"></a>Integration Services 部署向导
  **Integration Services 部署向导** 支持两种部署模型：
   - 项目部署模型
   - 包部署模型 
   
 **项目部署模型** 允许将 SQL Server Integration Services (SSIS) 项目作为一个单元部署到 SSIS 目录中。
 
 **包部署模型** 允许将已更新的包部署到 SSIS 目录中，而无需部署整个项目。 
 
 > [!NOTE]
 > 向导默认部署是项目部署模型。  
  
### <a name="launch-the-wizard"></a>启动向导
通过以下方式之一启动向导：

 - 在 Windows Search 中键入 **“SQL Server 部署向导”** 

 或

 - 在 SQL Server 安装文件夹下搜索可执行文件 ISDeploymentWizard.exe；例如：“C:\Program Files (x86)\Microsoft SQL Server\130\DTS\Binn”。 
 
 > **注意：** 如果显示“简介”页，则单击“下一步”切换到“选择源”页。 
 
 对于每个部署模型，此页上的设置会有所不同。 基于你在此页中选择的模型，按照 [Project Deployment Model](#ProjectModel) 部分或 [Package Deployment Model](#PackageModel) 部分的步骤进行操作。  
  
###  <a name="project-deployment-model"></a><a name="ProjectModel"></a> Project Deployment Model  
  
#### <a name="select-source"></a>选择源

 若要部署你所创建的项目部署文件，请选择“项目部署文件”  并输入 .ispac 文件的路径。 若要部署位于 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的项目，请选择 **“Integration Services 目录”**，然后输入目录中指向该项目的服务器名称和路径。 单击“下一步”  以查看“选择目标”  页。  
  
#### <a name="select-destination"></a>选择目标

 若要在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中选择项目的目标文件夹，请输入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例或单击 **“浏览”** 从服务器列表中选择。 然后输入 SSISDB 中的项目路径或单击 **“浏览”** 以选择此路径。 单击“下一步”  以查看“检查”  页。  
  
#### <a name="review-and-deploy"></a>查看（和部署）

 该页使你能够查看你所选择的设置。 可以通过单击 **“上一步”** 或单击左窗格中的任意步骤来更改所做的选择。 单击“部署”  以启动部署过程。  
  
#### <a name="results"></a>结果

 部署过程完成之后，将看到“结果”  页。 该页显示每个操作是成功了还是失败了。 如果操作失败，单击 **“结果”** 列中的 **“失败”** 可以显示错误说明。 单击“保存报表...”以将结果保存到 XML 文件，或单击“关闭”以退出向导。
  
###  <a name="package-deployment-model"></a><a name="PackageModel"></a> Package Deployment Model  
  
#### <a name="select-source"></a>选择源

 在“部署模型”  中选择“包部署”  选项后，“Integration Services 部署向导”  中的“选择源” 页会显示包部署模型的特定设置。  
  
 若要选择源包，请单击“浏览…”按钮以选择包含包的“文件夹”，或在“包文件夹路径”文本框中键入文件夹路径，然后单击页面底部的“刷新”按钮。 现在，将可以在列表框中看见特定文件夹中的所有包。 默认情况下，所有的包都会被选中。 单击第一列中的“复选框”  以选择要部署到服务器的包。  
  
 请参阅“状态”  和“消息”  列以验证包的状态。 如果将状态设置为“就绪”  或“警告” ，部署向导则不会妨碍部署过程。 如果状态设置为“错误”，则向导将不会继续部署所选的包。 若要查看详细的警告或错误消息，请单击“消息”列中的链接。  
  
 如果使用密码加密敏感数据或包数据，请在“密码”  列中键入密码，然后单击“刷新”  按钮以验证是否接受此密码。 如果密码正确无误，状态将会更改为“就绪”  ，而且警告消息将会消失。 如果有多个包具有相同的密码，请选择具有相同加密密码的包，在“密码”文本框中输入密码，选择“应用”按钮。 该密码将应用到所选包。  
  
 如果所有选定包的状态未设置为“错误” ，则可使用“下一步”  按钮以继续进行包部署过程。  
  
#### <a name="select-destination"></a>选择目标

 在选择包源后，单击“下一步”按钮以切换到“选择目标”页。 必须将包部署到 SSIS 目录 (SSISDB) 中的项目。 在部署包之前，请确保 SSIS 目录中已存在该目标项目。 如果项目不存在，请创建一个空项目。 在“选择目标”页中的“服务器名称”文本框中键入服务器名称，或单击“浏览…”按钮以选择服务器实例。 然后单击“路径”文本框旁的“浏览…”按钮以指定目标项目。 如果项目不存在，请单击“新建项目…”按钮以创建空项目作为目标项目。 必须在文件夹下创建项目。  
  
#### <a name="review-and-deploy"></a>查看和部署

 在“选择目标”  页上单击“下一步”  以切换到“Integration Services 部署向导”  中的“审阅” 页。 在审阅页中，审阅有关部署操作的摘要报表。 验证之后，单击“部署”按钮以执行部署操作。  
  
#### <a name="results"></a>结果

 部署完成之后，将看到“结果”  页。 在“结果”页中，查看部署过程中每个步骤的结果。 单击“保存报表”以保存部署报表，或单击“关闭”以关闭该向导。  

## <a name="create-and-map-a-server-environment"></a>创建和映射服务器环境

  创建服务器环境来指定已部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的项目中所含包的运行时值。 您可以随后针对特定包、入口点包或给定项目中的所有包，将环境变量映射到参数。 入口点包通常是执行子包的父包。  
  
> [!IMPORTANT]  
>  对于给定的执行，只能使用单个服务器环境中包含的值来执行包。  
  
 您可以查询视图以获得服务器环境、环境引用和环境变量的列表。 你也可以调用存储过程来添加、删除和修改环境、环境引用和环境变量。 有关详细信息，请参阅 [SSIS Catalog](../../integration-services/catalog/ssis-catalog.md) 中的“服务器环境、服务器变量和服务器环境引用”一节。  
  
### <a name="to-create-and-use-a-server-environment"></a>创建和使用服务器环境  
  
1.  在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中，在对象资源管理器中依次展开“[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录”> SSISDB 节点，找到要为其创建环境的项目的“环境”文件夹。  
  
2.  右键单击“环境”文件夹，然后单击“创建环境”。  
  
3.  为环境键入名称，并添加说明（可选）。 单击“确定”。  
  
4.  右键单击该新环境，然后单击“属性”。  
  
5.  在 **“变量”** 页上，执行以下操作以便添加变量。  
  
    1.  选择变量的 **“类型”** 。 变量的名称无需匹配你将映射到该变量的项目参数的名称。  
  
    2.  输入变量的可选 **“说明”** 。  
  
    3.  输入环境变量的 **“值”** 。  
  
         有关环境变量名称的规则的信息，请参阅 [SSIS Catalog](../../integration-services/catalog/ssis-catalog.md) 中的“环境变量”一节。  
  
    4.  通过选中或取消选中 **“敏感”** 复选框，指示该变量是否包含敏感值。  
  
         如果您选择 **“敏感”**，则变量值不会显示在 **“值”** 字段中。  
  
         敏感值在 SSISDB 目录中进行加密。 有关加密的详细信息，请参阅 [SSIS Catalog](../../integration-services/catalog/ssis-catalog.md)。  
  
6.  在 **“权限”** 页上，通过执行以下操作，授予或拒绝所选用户和角色的权限。  
  
    1.  单击 **“浏览”**，然后在 **“浏览所有主体”** 对话框中选择一个或多个用户和角色。  
  
    2.  在 **“登录名或角色”** 区域中，选择您要为其授予或拒绝权限的用户或角色。  
  
    3.  在“显式”区域中，选择每个权限旁边的“授予”或“拒绝”。  
  
7.  若要编写环境的脚本，请单击 **“脚本”**。 默认情况下，该脚本显示在一个新的查询编辑器窗口中。  
  
    > [!TIP]  
    >  对环境属性进行了更改（例如添加变量）后，并且在“环境属性”对话框中单击“确定”前，需要单击“脚本”。 否则，将不会生成脚本。  
  
8.  单击 **“确定”** 保存对环境属性的更改。  
  
9. 在对象资源管理器的 **SSISDB** 节点下，展开“项目”文件夹，右键单击项目，然后单击“配置”。  
  
10. 在 **“引用”** 页上，单击 **“添加”** 以便添加一个环境，然后单击 **“确定”** 以便保存对该环境的引用。  
  
11. 再次右键单击该项目，然后单击“配置”。  
  
12. 若要将环境变量映射到在设计时添加到包的参数，或映射到将 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目转换为项目部署模型时生成的参数，请执行以下操作：
  
    1.  在 **“参数”** 页上的 **“参数”** 选项卡中，单击 **“值”** 字段旁边的浏览按钮。  
  
    2.  单击 **“使用环境变量”**，然后选择已创建的环境变量。  
  
13. 若要将环境变量映射到连接管理器属性，请执行以下操作。 将在 SSIS 服务器上为连接管理器属性自动生成参数。  
  
    1.  在“参数”页上的“连接管理器”选项卡中，单击“值”字段旁边的浏览按钮   。  
  
    2.  单击 **“使用环境变量”**，然后选择已创建的环境变量。  
  
14. 单击“确定”两次以保存更改。  

## <a name="deploy-and-execute-ssis-packages-using-stored-procedures"></a>使用存储过程部署和执行 SSIS 包

  在您配置一个 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目以便使用项目部署模型时，可以使用 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 目录中的存储过程部署该项目并且执行包。 有关项目部署模型的信息，请参阅 [Deployment of Projects and Packages](deploy-integration-services-ssis-projects-and-packages.md#compare-project-deployment-model-and-legacy-package-deployment-model)。  
  
 您还可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 部署项目和执行包。 有关详细信息，请参阅“另请参见”  部分中的主题。  
  
> [!TIP]
>  您可以通过执行以下操作为下面的过程中列出的存储过程轻松地生成 Transact-SQL 语句，但 catalog.deploy_project 除外：
> 
>  1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，在对象资源管理器中展开“Integration Services 目录”  节点，然后导航到您要执行的包。  
> 2.  右键单击该包，然后单击“执行”  。  
> 3.  根据需要，设置参数值、连接管理器属性和 **“高级”** 选项卡中的选项（例如，日志记录级别）。  
> 
>      有关日志记录级别的详细信息，请参阅 [启用日志记录在 SSIS 服务器上的包执行的](../../integration-services/performance/integration-services-ssis-logging.md#server_logging)。  
> 4.  在单击 **“确定”** 以便执行该包之前，单击 **“脚本”**。 Transact-SQL 将出现在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]的“查询编辑器”窗口中。  
  
### <a name="to-deploy-and-execute-a-package-using-stored-procedures"></a>使用存储过程部署和执行包  
  
1.  调用 [catalog.deploy_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md) 将包含包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器。  
  
     若要检索 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目部署文件的二进制内容，对于 \@project_stream 参数，请将 SELECT 语句与 OPENROWSET 函数和 BULK 行集提供程序一起使用。 通过 BULK 行集提供程序，您可以从文件读取数据。 BULK 行集提供程序的 SINGLE_BLOB 参数将该数据文件的内容以 varbinary(max) 类型的单行、单列行集的形式返回。 有关详细信息，请参阅 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)。  
  
     在下面的示例中，SSISPackages_ProjectDeployment 项目将部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器上的“SSIS 包”文件夹。 二进制数据从项目文件 (SSISPackage_ProjectDeployment.ispac) 读取并且存储于 varbinary(max) 类型的 _\@ProjectBinary 参数中。 将 \@ProjectBinary 参数值赋给 \@project_stream 参数 。  
  
    ```sql
    DECLARE @ProjectBinary as varbinary(max)  
    DECLARE @operation_id as bigint  
    Set @ProjectBinary = (SELECT * FROM OPENROWSET(BULK 'C:\MyProjects\ SSISPackage_ProjectDeployment.ispac', SINGLE_BLOB) as BinaryData)  
  
    Exec catalog.deploy_project @folder_name = 'SSIS Packages', @project_name = 'DeployViaStoredProc_SSIS', @Project_Stream = @ProjectBinary, @operation_id = @operation_id out  
  
    ```  
  
2.  调用 [catalog.create_execution（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md) 来创建包执行的实例，并根据需要调用 [catalog.set_execution_parameter_value（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md) 来设置运行时参数值。  
  
     在下面的示例中，catalog.create_execution 为在 SSISPackage_ProjectDeployment 项目中包含的 package.dtsx 创建一个执行实例。 该项目位于“SSIS 包”文件夹中。 存储过程返回的 execution_id 用于对 catalog.set_execution_parameter_value 的调用中。 此第二个存储过程将 LOGGING_LEVEL 参数设置为 3（详细日志记录），并且将名为 Parameter1 的包参数设置为值 1。  
  
     对于 LOGGING_LEVEL 之类的参数，object_type 值为 50。 对于包参数，object_type 值为 30。  
  
    ```sql
    Declare @execution_id bigint  
    EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'SSIS Packages', @project_name=N'SSISPackage_ProjectDeployment', @use32bitruntime=False, @reference_id=1  
  
    Select @execution_id  
    DECLARE @var0 smallint = 3  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0  
  
    DECLARE @var1 int = 1  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=30, @parameter_name=N'Parameter1', @parameter_value=@var1  
  
    GO  
  
    ```  
  
3.  调用 [catalog.start_execution（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md)执行包。  
  
     在下面的示例中，将对 catalog.start_execution 的调用添加到 Transact-SQL 以便开始包执行。 将使用 catalog.create_execution 存储过程返回的 execution_id。  
  
    ```sql
    Declare @execution_id bigint  
    EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'SSIS Packages', @project_name=N'SSISPackage_ProjectDeployment', @use32bitruntime=False, @reference_id=1  
  
    Select @execution_id  
    DECLARE @var0 smallint = 3  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0  
  
    DECLARE @var1 int = 1  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=30, @parameter_name=N'Parameter1', @parameter_value=@var1  
  
    EXEC [SSISDB].[catalog].[start_execution] @execution_id  
    GO  
  
    ```  
  
### <a name="to-deploy-a-project-from-server-to-server-using-stored-procedures"></a>使用存储过程将项目从一个服务器部署到另一个服务器

 可以通过使用 [catalog.get_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-get-project-ssisdb-database.md)和 [catalog.deploy_project（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md)存储过程在服务器之间部署项目。  
  
 在运行存储过程之前，您需要执行以下操作。  
  
-   创建一个链接服务器对象。 有关详细信息，请参阅[创建链接服务器（SQL Server 数据库引擎）](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md)。  
  
     在 **“链接服务器属性”** 的 **“服务器选项”** 页上，将 **RPC** 和 **RPC Out** 设置为 **True**。 此外，将 **“为 RPC 启用针对分布式事务的升级”** 设置为 **False**。  
  
-   通过在对象资源管理器中展开“链接服务器”下的“提供程序”节点，右键单击该提供程序，然后单击“属性”，对为链接服务器选择的提供程序启用动态参数。 选择 **“动态参数”** 旁边的 **“启用”**。  
  
-   确认分布式事务处理协调器 (DTC) 在两个服务器上均启动。  
  
 调用 catalog.get_project 以便返回该项目的二进制文件，然后调用 catalog.deploy_project。 将 catalog.get_project 返回的值插入 varbinary(max) 类型的表变量中。 链接服务器无法返回类型为 varbinary(max) 的结果。  
  
 在下面的示例中，catalog.get_project 为链接服务器上的 SSISPackages 项目返回二进制文件。 catalog.deploy_project 将该项目部署到本地服务器上名为 DestFolder 的文件夹中。  
  
```sql
declare @resultsTableVar table (  
project_binary varbinary(max)  
)  
  
INSERT @resultsTableVar (project_binary)  
EXECUTE [MyLinkedServer].[SSISDB].[catalog].[get_project] 'Packages', 'SSISPackages'  
  
declare @project_binary varbinary(max)  
select @project_binary = project_binary from @resultsTableVar  
  
exec [SSISDB].[CATALOG].[deploy_project] 'DestFolder', 'SSISPackages', @project_binary  
  
```  

## <a name="integration-services-project-conversion-wizard"></a>Integration Services 项目转换向导
  **“Integration Services 项目转换向导”** 可以将项目转换为项目部署模型。  
  
> [!NOTE]  
>  如果项目包含一个或多个数据源，则在项目转换完成时删除数据源。 若要创建到可由项目中的包共享的数据源的连接，请在项目级别添加连接管理器。 有关详细信息，请参阅 [在包中添加、删除或共享连接管理器](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130))。  
  
 **您希望做什么？**  
  
-   [打开“Integration Services 项目转换向导”](#open_dialog)  
  
-   [设置“查找包”页上的选项](#locate)  
  
-   [设置“选择包”页上的选项](#selectPackages)  
  
-   [设置“选择目标”页上的选项](#destination)  
  
-   [设置“指定项目属性”页上的选项](#projectProperties)  
  
-   [设置“更新执行包任务”页上的选项](#executePackage)  
  
-   [设置“选择配置”页上的选项](#configurations)  
  
-   [设置“创建参数”页上的选项](#createParameters)  
  
-   [设置“配置参数”页上的选项](#configureParameters)  
  
-   [设置“检查”页上的选项](#review)  
  
-   [设置执行转换的选项](#conversion)  
  
###  <a name="open-the-integration-services-project-conversion-wizard"></a><a name="open_dialog"></a> 打开“Integration Services 项目转换向导”  
 执行下列操作之一以打开 **“Integration Services 项目转换”** 向导。  
  
-   在 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 中打开该项目，然后在解决方案资源管理器中，右键单击该项目并单击“转换为项目部署模型”。  
  
-   从 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 的对象资源管理器中，右键单击“Integration Services 目录”中的“项目”节点，然后选择“导入包”。  
  
 根据您是从 **还是从** 运行 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] “Integration Services 项目转换向导” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]，该向导将执行不同的转换任务。   
  
###  <a name="set-options-on-the-locate-packages-page"></a><a name="locate"></a> 设置“查找包”页上的选项  
  
> [!NOTE]  
>   只有在您从 **运行该向导时，** “查找包” [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]页才可用。  
  
 在“源”下拉列表中选择“文件系统”时，该页显示以下选项。 当包驻留在文件系统中时选择此选项。  
  
 **文件夹**  
 键入包路径，或通过单击“浏览”导航到该包。  
  
 在“源”下拉列表中选择“SSIS 包存储区”时，该页显示以下选项。 有关包存储区的详细信息，请参阅[包管理（SSIS 服务）](../../integration-services/service/package-management-ssis-service.md)。  
  
 **Server**  
 键入服务器名称或选择该服务器。  
  
 **文件夹**  
 键入包路径，或通过单击“浏览”导航到该包。  
  
 在“源”下拉列表中选择“Microsoft SQL Server”时，该页显示以下选项。 当包驻留在 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中时选择此选项。  
  
 **Server**  
 键入服务器名称或选择该服务器。  
  
 **使用 Windows 身份验证**  
 Microsoft Windows 身份验证模式允许用户通过 Windows 用户帐户进行连接。 如果使用 Windows 身份验证，则不需要提供用户名或密码。  
  
 **使用 SQL Server 身份验证**  
 当户使用指定的登录名和密码从不可信连接进行连接时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将通过检查是否已设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户以及指定的密码是否与以前记录的密码匹配，对该连接进行身份验证。 如果未设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户，则身份验证失败，并且用户会收到一条错误消息。  
  
 **用户名**  
 使用 SQL Server 身份验证时，指定用户名。  
  
 **密码**  
 使用 SQL Server 身份验证时，提供密码。  
  
 **文件夹**  
 键入包路径，或通过单击“浏览”导航到该包。  
  
###  <a name="set-options-on-the-select-packages-page"></a><a name="selectPackages"></a> 设置“选择包”页上的选项  
 **包名称**  
 列出包文件。  
  
 **Status**  
 指示包是否已准备好转换为项目部署模型。  
  
 **消息**  
 显示与包关联的消息。  
  
 **密码**  
 显示与包关联的密码。 密码文本将被隐藏。  
  
 **应用于所选内容**  
 单击以将“密码”文本框中的密码应用于一个或多个所选包。  
  
 **“刷新”**  
 刷新包的列表。  
  
###  <a name="set-options-on-the-select-destination-page"></a><a name="destination"></a> 设置“选择目标”页上的选项  
 在此页上，指定新的项目部署文件 (.ispac) 的名称和路径或者选择一个现有文件。  
  
> [!NOTE]  
>   只有在您从 **运行该向导时，** “选择目标” [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]页才可用。  
  
 “输出路径”   
 键入部署文件的路径，或通过单击“浏览”导航到该文件。  
  
 **项目名称**  
 键入项目名称。  
  
 **保护级别**  
 选择保护级别。 有关详细信息，请参阅 [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md)。  
  
 **项目说明**  
 键入项目的可选说明。  
  
###  <a name="set-options-on-the-specify-project-properties-page"></a><a name="projectProperties"></a> 设置“指定项目属性”页上的选项  
  
> [!NOTE]  
>   只有在您从 **运行该向导时，** “指定项目属性” [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]页才可用。  
  
 **项目名称**  
 列出项目名称。  
  
 **保护级别**  
 为项目中所含的包选择保护级别。 有关保护级别的详细信息，请参阅 [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md)。  
  
 **项目说明**  
 键入可选的项目说明。  
  
###  <a name="set-options-on-the-update-execute-package-task-page"></a><a name="executePackage"></a> 设置“更新执行包任务”页上的选项  
 更新包中所含的执行包任务，以使用基于项目的引用。 有关详细信息，请参阅 [Execute Package Task Editor](../control-flow/execute-package-task.md)。  
  
 **父包**  
 列出使用执行包任务执行子包的包名称。  
  
 **任务名称**  
 列出执行包任务的名称。  
  
 **原始引用**  
 列出子包的当前路径。  
  
 **分配引用**  
 选择存储在项目中的子包。  
  
###  <a name="set-options-on-the-select-configurations-page"></a><a name="configurations"></a> 设置“选择配置”页上的选项  
 选择您要用参数替换的包配置。  
  
 **包**  
 列出包文件。  
  
 **类型**  
 列出配置类型，如 XML 配置文件。  
  
 **配置字符串**  
 列出配置文件的路径。  
  
 **Status**  
 显示配置的状态消息。 单击该消息可以查看整个消息文本。  
  
 **添加配置**  
 将在其他项目中包含的包配置添加到要用参数替换的可用配置的列表中。 您可以选择存储在文件系统或 SQL Server 中的配置。  
  
 **“刷新”**  
 单击以刷新配置列表。  
  
 **在转换后删除所有包的配置**  
 建议通过选择此选项从项目中删除所有配置。  
  
 如果没有选择此选项，将只删除已选择用参数替换的配置。  
  
###  <a name="set-options-on-the-create-parameters-page"></a><a name="createParameters"></a> 设置“创建参数”页上的选项  
 选择每个配置属性的参数名称和作用域。  
  
 **包**  
 列出包文件。  
  
 **参数名称**  
 列出参数名称。  
  
 **范围**  
 选择参数的作用域（包或项目）。  
  
###  <a name="set-options-on-the-configure-parameters-page"></a><a name="configureParameters"></a> 设置“配置参数”页上的选项  
 **名称**  
 列出参数名称。  
  
 **范围**  
 列出参数的作用域。  
  
 **值**  
 列出参数值。  
  
 单击值字段旁边的省略号按钮以配置参数属性。  
  
 在 **“设置参数详细信息”** 对话框中，可以编辑参数值。 还可以指定在运行包时是否必须提供参数值。  
  
 您可以通过单击参数旁的“浏览”按钮，在 **的** “配置” **对话框的** “参数” [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]页中修改值。 将显示 **“设置参数值”** 对话框。  
  
 **“设置参数详细信息”** 对话框还列出参数值的数据类型和参数的来源。  
  
###  <a name="set-the-options-on-the-review-page"></a><a name="review"></a> 设置“检查”页上的选项  
 使用“检查”页可以确认为项目转换选择的选项。  
  
 **“上一步”**  
 单击以更改选项。  
  
 **转换**  
 单击以将项目转换为项目部署模型。  
  
###  <a name="set-the-options-on-the-perform-conversion"></a><a name="conversion"></a> 设置执行转换的选项  
 “执行转换”页显示项目转换的状态。  
  
 **操作**  
 列出特定的转换步骤。  
  
 **结果**  
 列出每个转换步骤的状态。 单击状态消息可获取详细信息。  
  
 直到在 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]中保存项目后，才保存项目转换。  
  
 **保存报告**  
 单击以在 .xml 文件中保存项目转换的摘要。