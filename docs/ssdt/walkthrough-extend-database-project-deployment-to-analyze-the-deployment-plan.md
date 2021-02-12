---
title: 扩展数据库项目部署以分析部署计划
description: 创建 DeploymentPlanExecutor 部署参与者。 设置参与者，该参与者会记录部署数据库项目时发生的事件。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 9ead8470-93ba-44e3-8848-b59322e37621
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d389d9f71b08914385cdf3bedef5bf613e6700d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016667"
---
# <a name="walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan"></a>演练：扩展数据库项目部署以分析部署计划

可以创建部署参与者以便在部署 SQL 项目时执行自定义操作。 可以创建 DeploymentPlanModifier 或 DeploymentPlanExecutor。 使用 DeploymentPlanModifier 可在执行计划前更改计划，使用 DeploymentPlanExecutor 可在执行计划时执行操作。 在本演练中，您创建一个名为 DeploymentUpdateReportContributor 的 DeploymentPlanExecutor，它可创建有关在部署数据库项目时执行的操作的报告。 由于此生成参与者接受用于控制是否生成报告的参数，因此您必须额外执行一个必需步骤。  
  
在本演练中，您将完成以下主要任务：  
  
-   [创建 DeploymentPlanExecutor 类型的部署参与者](#CreateDeploymentContributor)  
  
-   [安装部署参与者](#InstallDeploymentContributor)  
  
-   [测试部署参与者](#TestDeploymentContributor)  
  
## <a name="prerequisites"></a>先决条件  
您需要满足以下条件才能完成本演练：  
  
-   必须已安装包含 SQL Server Data Tools (SSDT) 且支持 C# 或 VB 开发的 Visual Studio 版本。  
  
-   必须具有包含 SQL 对象的 SQL 项目。  
  
-   可以向其部署数据库项目的 SQL Server 实例。  
  
> [!NOTE]  
> 本演练面向已熟悉 SSDT 的 SQL 功能的用户。 您还应熟悉 Visual Studio 的基本概念，例如，如何创建类库以及如何使用代码编辑器向类添加代码。  
  
## <a name="create-a-deployment-contributor"></a><a name="CreateDeploymentContributor"></a>创建部署参与者  
若要创建部署参与者，您必须执行以下任务：  
  
-   创建类库项目并添加所需的引用。  
  
-   定义一个名为 DeploymentUpdateReportContributor 的类，该类继承自 [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor)。  
  
-   重写 OnExecute 方法。  
  
-   添加私有帮助器类。  
  
-   生成结果程序集。  
  
#### <a name="to-create-a-class-library-project"></a>创建类库项目  
  
1.  创建一个名为 MyDeploymentContributor 的 Visual Basic 或 Visual C# 类库项目。  
  
2.  将文件“Class1.cs”重命名为“DeploymentUpdateReportContributor.cs”。  
  
3.  在解决方案资源管理器中，右键单击项目节点，然后单击“添加引用”。  
  
4.  在“框架”选项卡上，选择“System.ComponentModel.Composition”。  
  
5.  添加所需的 SQL 引用：右键单击项目节点，然后单击“添加引用”。 单击“浏览”，并导航到 C:\Program Files (x86)\Microsoft SQL Server\110\DAC\Bin 文件夹。 选择“Microsoft.SqlServer.Dac.dll”、“Microsoft.SqlServer.Dac.Extensions.dll”和“Microsoft.Data.Tools.Schema.Sql.dll”条目，单击“添加”，然后单击“确定”。  
  
    接下来，开始向类中添加代码。  
  
#### <a name="to-define-the-deploymentupdatereportcontributor-class"></a>定义 DeploymentUpdateReportContributor 类  
  
1.  在代码编辑器中，更新 DeploymentUpdateReportContributor.cs 文件以匹配以下 using 语句：  
  
    ```csharp  
    using System;  
    using System.IO;  
    using System.Text;  
    using System.Xml;  
    using Microsoft.SqlServer.Dac.Deployment;  
    using Microsoft.SqlServer.Dac.Extensibility;  
    using Microsoft.SqlServer.Dac.Model;  
  
    ```  
  
2.  更新类定义以匹配以下内容：  
  
    ```csharp  
    /// <summary>  
        /// An executor that generates a report detailing the steps in the deployment plan. Will only run  
        /// if a "GenerateUpdateReport=true" contributor argument is set in the project file, in a targets file or  
        /// passed as an additional argument to the DacServices API. To set in a project file, add the following:  
        ///   
        /// <PropertyGroup>  
        ///     <ContributorArguments Condition="'$(Configuration)' == 'Debug'">  
        /// $(ContributorArguments);DeploymentUpdateReportContributor.GenerateUpdateReport=true;  
        ///     </ContributorArguments>  
        /// <PropertyGroup>  
        ///   
        /// </summary>  
        [ExportDeploymentPlanExecutor("MyDeploymentContributor.DeploymentUpdateReportContributor", "1.0.0.0")]  
        public class DeploymentUpdateReportContributor : DeploymentPlanExecutor  
        {  
        }  
  
    ```  
  
    现在，您已定义继承自 DeploymentPlanExecutor 的部署参与者。 在生成和部署过程中，将从标准扩展目录中加载自定义参与者。 部署计划执行器参与者将由 [ExportDeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.exportdeploymentplanexecutorattribute) 属性标识。  
  
    必须使用该属性才能发现参与者。 该属性应与下面类似：  
  
    ```csharp  
    [ExportDeploymentPlanExecutor("MyDeploymentContributor.DeploymentUpdateReportContributor", "1.0.0.0")]  
  
    ```  
  
    在此示例中，该属性的第一个参数应是一个唯一标识符，该标识符将用于在项目文件中标识你的参与者。 最佳做法是将库的命名空间（在本演练中为“MyDeploymentContributor”）与类名称（在本演练中为“DeploymentUpdateReportContributor”）结合使用来生成标识符。  
  
3.  接下来，添加下面的成员，您将使用该成员以使此提供程序能够接受命令行参数：  
  
    ```csharp  
    public const string GenerateUpdateReport = "DeploymentUpdateReportContributor.GenerateUpdateReport";  
    ```  
  
    该成员使用户能够指定是否应使用 GenerateUpdateReport 选项来生成报告。  
  
    接下来，您将重写 OnExecute 方法以添加要在部署数据库项目时运行的代码。  
  
#### <a name="to-override-onexecute"></a>重写 OnExecute  
  
-   将下面的方法添加到您的 DeploymentUpdateReportContributor 类中：  
  
    ```csharp  
    /// <summary>  
            /// Override the OnExecute method to perform actions when you execute the deployment plan for  
            /// a database project.  
            /// </summary>  
            protected override void OnExecute(DeploymentPlanContributorContext context)  
            {  
                // determine whether the user specified a report is to be generated  
                bool generateReport = false;  
                string generateReportValue;  
                if (context.Arguments.TryGetValue(GenerateUpdateReport, out generateReportValue) == false)  
                {  
                    // couldn't find the GenerateUpdateReport argument, so do not generate  
                    generateReport = false;  
                }  
                else  
                {  
                    // GenerateUpdateReport argument was specified, try to parse the value  
                    if (bool.TryParse(generateReportValue, out generateReport))  
                    {  
                        // if we end up here, the value for the argument was not valid.  
                        // default is false, so do nothing.  
                    }  
                }  
  
                if (generateReport == false)  
                {  
                    // if user does not want to generate a report, we are done  
                    return;  
                }  
  
                // We will output to the same directory where the deployment script  
                // is output or to the current directory  
                string reportPrefix = context.Options.TargetDatabaseName;  
                string reportPath;  
                if (string.IsNullOrEmpty(context.DeploymentScriptPath))  
                {  
                    reportPath = Environment.CurrentDirectory;  
                }  
                else  
                {  
                    reportPath = Path.GetDirectoryName(context.DeploymentScriptPath);  
                }  
                FileInfo summaryReportFile = new FileInfo(Path.Combine(reportPath, reportPrefix + ".summary.xml"));  
                FileInfo detailsReportFile = new FileInfo(Path.Combine(reportPath, reportPrefix + ".details.xml"));  
  
                // Generate the reports by using the helper class DeploymentReportWriter  
                DeploymentReportWriter writer = new DeploymentReportWriter(context);  
                writer.WriteReport(summaryReportFile);  
                writer.IncludeScripts = true;  
                writer.WriteReport(detailsReportFile);  
  
                string msg = "Deployment reports ->"  
                    + Environment.NewLine + summaryReportFile.FullName  
                    + Environment.NewLine + detailsReportFile.FullName;  
  
                ExtensibilityError reportMsg = new ExtensibilityError(msg, Severity.Message);  
                base.PublishMessage(reportMsg);  
            }  
        /// <summary>  
        /// Override the OnExecute method to perform actions when you execute the deployment plan for  
        /// a database project.  
        /// </summary>  
            protected override void OnExecute(DeploymentPlanContributorContext context)  
            {  
                // determine whether the user specified a report is to be generated  
                bool generateReport = false;  
                string generateReportValue;  
                if (context.Arguments.TryGetValue(GenerateUpdateReport, out generateReportValue) == false)  
                {  
                    // couldn't find the GenerateUpdateReport argument, so do not generate  
                    generateReport = false;  
                }  
                else  
                {  
                    // GenerateUpdateReport argument was specified, try to parse the value  
                    if (bool.TryParse(generateReportValue, out generateReport))  
                    {  
                        // if we end up here, the value for the argument was not valid.  
                        // default is false, so do nothing.  
                    }  
                }  
  
                if (generateReport == false)  
                {  
                    // if user does not want to generate a report, we are done  
                    return;  
                }  
  
                // We will output to the same directory where the deployment script  
                // is output or to the current directory  
                string reportPrefix = context.Options.TargetDatabaseName;  
                string reportPath;  
                if (string.IsNullOrEmpty(context.DeploymentScriptPath))  
                {  
                    reportPath = Environment.CurrentDirectory;  
                }  
                else  
                {  
                    reportPath = Path.GetDirectoryName(context.DeploymentScriptPath);  
                }  
                FileInfo summaryReportFile = new FileInfo(Path.Combine(reportPath, reportPrefix + ".summary.xml"));  
                FileInfo detailsReportFile = new FileInfo(Path.Combine(reportPath, reportPrefix + ".details.xml"));  
  
                // Generate the reports by using the helper class DeploymentReportWriter  
                DeploymentReportWriter writer = new DeploymentReportWriter(context);  
                writer.WriteReport(summaryReportFile);  
                writer.IncludeScripts = true;  
                writer.WriteReport(detailsReportFile);  
  
                string msg = "Deployment reports ->"  
                    + Environment.NewLine + summaryReportFile.FullName  
                    + Environment.NewLine + detailsReportFile.FullName;  
  
                DataSchemaError reportMsg = new DataSchemaError(msg, ErrorSeverity.Message);  
                base.PublishMessage(reportMsg);  
            }  
    ```  
  
    将向 OnExecute 方法传递一个 [DeploymentPlanContributorContext](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributorcontext) 对象，该对象提供对任何指定参数、源和目标数据库模型、生成属性以及扩展文件的访问权。 在此示例中，我们获取模型，然后调用帮助器函数来输出有关模型的信息。 我们在基类上使用 PublishMessage 帮助器方法报告发生的任何错误。  
  
    其他相关的类型和方法包括：[TSqlModel](/dotnet/api/microsoft.sqlserver.dac.model.tsqlmodel)、[ModelComparisonResult](/dotnet/api/microsoft.sqlserver.dac.deployment.modelcomparisonresult)、[DeploymentPlanHandle](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanhandle) 和 [SqlDeploymentOptions](/dotnet/api/microsoft.sqlserver.dac.deployment.sqldeploymentoptions)。  
  
    接下来，您定义探究部署计划的详细信息的帮助器类。  
  
#### <a name="to-add-the-helper-class-that-generates-the-report-body"></a>添加生成表体的帮助器类  
  
-   通过添加以下代码来添加帮助器类及其方法：  
  
    ```csharp  
    /// <summary>  
            /// This class is used to generate a deployment  
            /// report.   
            /// </summary>  
            private class DeploymentReportWriter  
            {  
                readonly TSqlModel _sourceModel;  
                readonly ModelComparisonResult _diff;  
                readonly DeploymentStep _planHead;  
  
                /// <summary>  
                /// The constructor accepts the same context info  
                /// that was passed to the OnExecute method of the  
                /// deployment contributor.  
                /// </summary>  
                public DeploymentReportWriter(DeploymentPlanContributorContext context)  
                {  
                    if (context == null)  
                    {  
                        throw new ArgumentNullException("context");  
                    }  
  
                    // save the source model, source/target differences,  
                    // and the beginning of the deployment plan.  
                    _sourceModel = context.Source;  
                    _diff = context.ComparisonResult;  
                    _planHead = context.PlanHandle.Head;  
                }  
                /// <summary>  
                /// Property indicating whether script bodies  
                /// should be included in the report.  
                /// </summary>  
                public bool IncludeScripts { get; set; }  
  
                /// <summary>  
                /// Drives the report generation, opening files,   
                /// writing the beginning and ending report elements,  
                /// and calling helper methods to report on the  
                /// plan operations.  
                /// </summary>  
                internal void WriteReport(FileInfo reportFile)  
                {// Assumes that we have a valid report file  
                    if (reportFile == null)  
                    {  
                        throw new ArgumentNullException("reportFile");  
                    }  
  
                    // set up the XML writer  
                    XmlWriterSettings xmlws = new XmlWriterSettings();  
                    // Indentation makes it a bit more readable  
                    xmlws.Indent = true;  
                    FileStream fs = new FileStream(reportFile.FullName, FileMode.Create, FileAccess.Write, FileShare.ReadWrite);  
                    XmlWriter xmlw = XmlWriter.Create(fs, xmlws);  
  
                    try  
                    {  
                        xmlw.WriteStartDocument(true);  
                        xmlw.WriteStartElement("DeploymentReport");  
  
                        // Summary report of the operations that  
                        // are contained in the plan.  
                        ReportPlanOperations(xmlw);  
  
                        // You could add a method call here  
                        // to produce a detailed listing of the   
                        // differences between the source and  
                        // target model.  
                        xmlw.WriteEndElement();  
                        xmlw.WriteEndDocument();  
                        xmlw.Flush();  
                        fs.Flush();  
                    }  
                    finally  
                    {  
                        xmlw.Close();  
                        fs.Dispose();  
                    }  
                }  
  
                /// <summary>  
                /// Writes details for the various operation types  
                /// that could be contained in the deployment plan.  
                /// Optionally writes script bodies, depending on  
                /// the value of the IncludeScripts property.  
                /// </summary>  
                private void ReportPlanOperations(XmlWriter xmlw)  
                {// write the node to indicate the start  
                    // of the list of operations.  
                    xmlw.WriteStartElement("Operations");  
  
                    // Loop through the steps in the plan,  
                    // starting at the beginning.  
                    DeploymentStep currentStep = _planHead;  
                    while (currentStep != null)  
                    {  
                        // Report the type of step  
                        xmlw.WriteStartElement(currentStep.GetType().Name);  
  
                        // based on the type of step, report  
                        // the relevant information.  
                        // Note that this procedure only handles   
                        // a subset of all step types.  
                        if (currentStep is SqlRenameStep)  
                        {  
                            SqlRenameStep renameStep = (SqlRenameStep)currentStep;  
                            xmlw.WriteAttributeString("OriginalName", renameStep.OldName);  
                            xmlw.WriteAttributeString("NewName", renameStep.NewName);  
                            xmlw.WriteAttributeString("Category", GetElementCategory(renameStep.RenamedElement));  
                        }  
                        else if (currentStep is SqlMoveSchemaStep)  
                        {  
                            SqlMoveSchemaStep moveStep = (SqlMoveSchemaStep)currentStep;  
                            xmlw.WriteAttributeString("OrignalName", moveStep.PreviousName);  
                            xmlw.WriteAttributeString("NewSchema", moveStep.NewSchema);  
                            xmlw.WriteAttributeString("Category", GetElementCategory(moveStep.MovedElement));  
                        }  
                        else if (currentStep is SqlTableMigrationStep)  
                        {  
                            SqlTableMigrationStep dmStep = (SqlTableMigrationStep)currentStep;  
                            xmlw.WriteAttributeString("Name", GetElementName(dmStep.SourceTable));  
                            xmlw.WriteAttributeString("Category", GetElementCategory(dmStep.SourceElement));  
                        }  
                        else if (currentStep is CreateElementStep)  
                        {  
                            CreateElementStep createStep = (CreateElementStep)currentStep;  
                            xmlw.WriteAttributeString("Name", GetElementName(createStep.SourceElement));  
                            xmlw.WriteAttributeString("Category", GetElementCategory(createStep.SourceElement));  
                        }  
                        else if (currentStep is AlterElementStep)  
                        {  
                            AlterElementStep alterStep = (AlterElementStep)currentStep;  
                            xmlw.WriteAttributeString("Name", GetElementName(alterStep.SourceElement));  
                            xmlw.WriteAttributeString("Category", GetElementCategory(alterStep.SourceElement));  
                        }  
                        else if (currentStep is DropElementStep)  
                        {  
                            DropElementStep dropStep = (DropElementStep)currentStep;  
                            xmlw.WriteAttributeString("Name", GetElementName(dropStep.TargetElement));  
                            xmlw.WriteAttributeString("Category", GetElementCategory(dropStep.TargetElement));  
                        }  
  
                        // If the script bodies are to be included,  
                        // add them to the report.  
                        if (this.IncludeScripts)  
                        {  
                            using (StringWriter sw = new StringWriter())  
                            {  
                                currentStep.GenerateBatchScript(sw);  
                                string tsqlBody = sw.ToString();  
                                if (string.IsNullOrEmpty(tsqlBody) == false)  
                                {  
                                    xmlw.WriteCData(tsqlBody);  
                                }  
                            }  
                        }  
  
                        // close off the current step  
                        xmlw.WriteEndElement();  
                        currentStep = currentStep.Next;  
                    }  
                    xmlw.WriteEndElement();  
                }  
  
                /// <summary>  
                /// Returns the category of the specified element  
                /// in the source model  
                /// </summary>  
                private string GetElementCategory(TSqlObject element)  
                {  
                    return element.ObjectType.Name;  
                }  
  
                /// <summary>  
                /// Returns the name of the specified element  
                /// in the source model  
                /// </summary>  
                private static string GetElementName(TSqlObject element)  
                {  
                    StringBuilder name = new StringBuilder();  
                    if (element.Name.HasExternalParts)  
                    {  
                        foreach (string part in element.Name.ExternalParts)  
                        {  
                            if (name.Length > 0)  
                            {  
                                name.Append('.');  
                            }  
                            name.AppendFormat("[{0}]", part);  
                        }  
                    }  
  
                    foreach (string part in element.Name.Parts)  
                    {  
                        if (name.Length > 0)  
                        {  
                            name.Append('.');  
                        }  
                        name.AppendFormat("[{0}]", part);  
                    }  
  
                    return name.ToString();  
                }  
            }        /// <summary>  
            /// This class is used to generate a deployment  
            /// report.   
            /// </summary>  
            private class DeploymentReportWriter  
            {  
                /// <summary>  
                /// The constructor accepts the same context info  
                /// that was passed to the OnExecute method of the  
                /// deployment contributor.  
                /// </summary>  
                public DeploymentReportWriter(DeploymentPlanContributorContext context)  
                {  
               }  
                /// <summary>  
                /// Property indicating whether script bodies  
                /// should be included in the report.  
                /// </summary>  
                public bool IncludeScripts { get; set; }  
  
                /// <summary>  
                /// Drives the report generation, opening files,   
                /// writing the beginning and ending report elements,  
                /// and calling helper methods to report on the  
                /// plan operations.  
                /// </summary>  
                internal void WriteReport(FileInfo reportFile)  
                {  
                }  
  
                /// <summary>  
                /// Writes details for the various operation types  
                /// that could be contained in the deployment plan.  
                /// Optionally writes script bodies, depending on  
                /// the value of the IncludeScripts property.  
                /// </summary>  
                private void ReportPlanOperations(XmlWriter xmlw)  
                {  
                }  
  
                /// <summary>  
                /// Returns the category of the specified element  
                /// in the source model  
                /// </summary>  
                private string GetElementCategory(IModelElement element)  
                {  
                }  
  
                /// <summary>  
                /// Returns the name of the specified element  
                /// in the source model  
                /// </summary>  
                private string GetElementName(IModelElement element)  
                {  
                }  
            }  
    ```  
  
-   将更改保存到类文件。 帮助器类中引用了大量有用的类型：  
  
    |代码区域|有用的类型|  
    |-----------------|--------------------|  
    |类成员|[TSqlModel](/dotnet/api/microsoft.sqlserver.dac.model.tsqlmodel)、[ModelComparisonResult](/dotnet/api/microsoft.sqlserver.dac.deployment.modelcomparisonresult)、[DeploymentStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentstep)|  
    |WriteReport 方法|XmlWriter 和 XmlWriterSettings|  
    |ReportPlanOperations 方法|相关类型包括：[DeploymentStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentstep)、[SqlRenameStep](/dotnet/api/microsoft.sqlserver.dac.deployment.sqlrenamestep)、[SqlMoveSchemaStep](/dotnet/api/microsoft.sqlserver.dac.deployment.sqlmoveschemastep)、[SqlTableMigrationStep](/dotnet/api/microsoft.sqlserver.dac.deployment.sqltablemigrationstep)、[CreateElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.createelementstep)、[AlterElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.alterelementstep)、[DropElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.dropelementstep)。<br /><br />有许多其他步骤 - 有关这些步骤的完整列表，请参见 API 文档。|  
    |GetElementCategory|[TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject)|  
    |GetElementName|[TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject)|  
  
    接下来，你将生成类库。  
  
#### <a name="to-sign-and-build-the-assembly"></a>生成程序集并对其进行签名  
  
1.  在“项目”菜单上，单击“MyDeploymentContributor 属性”。  
  
2.  单击“签名”  选项卡。  
  
3.  单击“对程序集签名” 。  
  
4.  在“选择强名称密钥文件”中，单击 **<New>** 。  
  
5.  在“创建强名称密钥”  对话框的“密钥文件名称” 中，键入“MyRefKey” 。  
  
6.  （可选）可以为强名称密钥文件指定密码。  
  
7.  单击“确定”。  
  
8.  在“文件”  菜单上，单击“全部保存” 。  
  
9. 在“生成”菜单中，单击“生成解决方案”。  
  
接下来，您必须安装程序集，以便在生成和部署 SQL 项目时加载该程序集。  
  
## <a name="install-a-deployment-contributor"></a><a name="InstallDeploymentContributor"></a>安装部署参与者  
若要安装部署参与者，您必须将程序集与关联的 .pdb 文件复制到 Extensions 文件夹。  
  
#### <a name="to-install-the-mydeploymentcontributor-assembly"></a>安装 MyDeploymentContributor 程序集  
  
-   接下来，您要将程序集信息复制到 Extensions 目录中。 Visual Studio 在启动后将标识 %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions 目录和子目录中的任何扩展，并使其可供使用：  
  
-   将 MyDeploymentContributor.dll 程序集文件从输出目录复制到 %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions 目录。 默认情况下，已编译的 .dll 文件的路径为 YourSolutionPath\YourProjectPath\bin\Debug 或 YourSolutionPath\YourProjectPath\bin\Release。  
  
## <a name="test-your-deployment-contributor"></a><a name="TestDeploymentContributor"></a>测试部署参与者  
若要测试部署参与者，您必须执行以下任务：  
  
-   向计划部署的 .sqlproj 文件添加属性。  
  
-   通过使用 MSBuild 并提供适当的参数来部署项目。  
  
### <a name="add-properties-to-the-sql-project-sqlproj-file"></a>向 SQL 项目 (.sqlproj) 文件添加属性  
您必须始终更新 SQL 项目文件以指定要运行的参与者的 ID。 此外，由于此参与者需要“GenerateUpdateReport”参数，因此必须将此参数指定为参与者参数。  
  
可以通过两种方法执行此操作。 您可以手动修改 .sqlproj 文件来添加必需的参数。 如果您的参与者不具有配置所需的任何参与者参数或者您不打算跨大量项目重用该参与者，则可以选择执行此操作。 如果您选择该选项，请将以下语句添加到 .sqlproj 文件中的第一个 Import 节点的后面：  
  
```  
<PropertyGroup>  
    <DeploymentContributors>$(DeploymentContributors); MyDeploymentContributor.DeploymentUpdateReportContributor</DeploymentContributors>  
<ContributorArguments Condition="'$(Configuration)' == 'Debug'">$(ContributorArguments);DeploymentUpdateReportContributor.GenerateUpdateReport=true;</ContributorArguments>  
  </PropertyGroup>  
```  
  
第二种方法是创建包含所需的参与者参数的目标文件。 如果您要对多个项目使用相同的参与者并且您具有必需的参与者参数，则此方法会很有用，因为它将包含默认值。 在此情况下，请在 MSBuild 扩展路径中创建目标文件。  
  
1.  导航到 %Program Files%\MSBuild。  
  
2.  创建一个将存储目标文件的新文件夹“MyContributors”。  
  
3.  在该目录中创建一个新文件“MyContributors.targets”，将下列文本添加到该文件中并保存该文件：  
  
    ```  
    <?xml version="1.0" encoding="utf-8"?>  
  
    <Project xmlns="https://schemas.microsoft.com/developer/msbuild/2003">  
      <PropertyGroup>  
    <DeploymentContributors>$(DeploymentContributors);MyDeploymentContributor.DeploymentUpdateReportContributor</DeploymentContributors>  
    <ContributorArguments Condition="'$(Configuration)' == 'Debug'">$(ContributorArguments); DeploymentUpdateReportContributor.GenerateUpdateReport=true;</ContributorArguments>  
      </PropertyGroup>  
    </Project>  
    ```  
  
4.  在要运行参与者角色的任何项目的 .sqlproj 文件中，通过在 .sqlproj 文件中的 \<Import Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets" \/> 节点之后添加以下语句来导入目标文件：  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath)\MyContributors\MyContributors.targets " />  
    ```  
  
在使用了这些方法之一后，您可以使用 MSBuild 来传入命令行生成的参数。  
  
> [!NOTE]  
> 您必须始终更新“DeploymentContributors”属性以指定您的参与者 ID。 此 ID 与参与者源文件中的“ExportDeploymentPlanExecutor”属性中使用的 ID 相同。 如果没有此 ID，你的参与者在生成项目时将不会运行。 仅在具有运行参与者所需的参数时需要更新“ContributorArguments”属性。  
  
### <a name="deploy-the-database-project"></a>部署数据库项目  
可以像在 Visual Studio 中一样正常发布或部署您的项目。 只需打开包含 SQL 项目的解决方案，并从该项目的右键单击上下文菜单中选择“发布…”选项，或使用 F5 以对 LocalDB 进行调试部署即可。 在此示例中，我们将使用“发布…”对话框来生成部署脚本。  
  
##### <a name="to-deploy-your-sql-project-and-generate-a-deployment-report"></a>部署 SQL 项目并生成部署报告  
  
1.  打开 Visual Studio 并打开包含您的 SQL 项目的解决方案。  
  
2.  选择你的项目，并点击 F5 以进行调试部署。 注意：由于 ContributorArguments 元素设置为仅在配置为“调试”时包含，因此现在仅为调试部署生成部署报告。 若要更改此情况，请从 ContributorArguments 定义中删除 Condition="'$(Configuration)' == 'Debug'" 语句。  
  
3.  输出窗口中将显示与下面类似的输出：  
  
    ```  
    ------ Deploy started: Project: Database1, Configuration: Debug Any CPU ------  
    Finished verifying cached model in 00:00:00  
    Deployment reports ->  
  
      C:\Users\UserName\Documents\Visual Studio 2012\Projects\MyDatabaseProject\MyDatabaseProject\sql\debug\MyTargetDatabase.summary.xml  
      C:\Users\UserName\Documents\Visual Studio 2012\Projects\MyDatabaseProject\MyDatabaseProject\sql\debug\MyTargetDatabase.details.xml  
  
      Deployment script generated to:  
      C:\Users\UserName\Documents\Visual Studio 2012\Projects\MyDatabaseProject\MyDatabaseProject\sql\debug\MyDatabaseProject.sql  
  
    ```  
  
4.  打开 MyTargetDatabase.summary.xml 并检查内容。 该文件与演示了新的数据库部署的以下示例类似：  
  
    ```  
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>  
    <DeploymentReport>  
      <Operations>  
        <DeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <BeginPreDeploymentScriptStep />  
        <DeploymentScriptStep />  
        <EndPreDeploymentScriptStep />  
        <SqlBeginPreservationStep />  
        <SqlEndPreservationStep />  
        <SqlBeginDropsStep />  
        <SqlEndDropsStep />  
        <SqlBeginAltersStep />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales" Category="Schema" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Customer" Category="Table" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.PK_Customer_CustID" Category="Primary Key" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Orders" Category="Table" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.PK_Orders_OrderID" Category="Primary Key" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Def_Customer_YTDOrders" Category="Default Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Def_Customer_YTDSales" Category="Default Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Def_Orders_OrderDate" Category="Default Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Def_Orders_Status" Category="Default Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.FK_Orders_Customer_CustID" Category="Foreign Key" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.CK_Orders_FilledDate" Category="Check Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.CK_Orders_OrderDate" Category="Check Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspCancelOrder" Category="Procedure" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspFillOrder" Category="Procedure" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspNewCustomer" Category="Procedure" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspPlaceNewOrder" Category="Procedure" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspShowOrderDetails" Category="Procedure" />  
        <SqlEndAltersStep />  
        <DeploymentScriptStep />  
        <BeginPostDeploymentScriptStep />  
        <DeploymentScriptStep />  
        <EndPostDeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
      </Operations>  
    </DeploymentReport>  
  
    ```  
  
    > [!NOTE]  
    > 如果您部署的数据库项目与目标数据库相同，则结果报告将没有太大的用处。 若要获得更有用的结果，请将更改部署到数据库或部署新的数据库。  
  
5.  打开 MyTargetDatabase.details.xml 并检查内容。 详细信息文件的一小部分内容显示了一些条目和脚本，它们可创建 Sales 架构、输出与创建表有关的信息和创建表：  
  
    ```  
    <CreateElementStep Name="Sales" Category="Schema"><![CDATA[CREATE SCHEMA [Sales]  
        AUTHORIZATION [dbo];  
  
    ]]></CreateElementStep>  
        <SqlPrintStep><![CDATA[PRINT N'Creating [Sales].[Customer]...';  
  
    ]]></SqlPrintStep>  
        <CreateElementStep Name="Sales.Customer" Category="Table"><![CDATA[CREATE TABLE [Sales].[Customer] (  
        [CustomerID]   INT           IDENTITY (1, 1) NOT NULL,  
        [CustomerName] NVARCHAR (40) NOT NULL,  
        [YTDOrders]    INT           NOT NULL,  
        [YTDSales]     INT           NOT NULL  
    );  
  
    ]]></CreateElementStep>  
  
    ```  
  
    通过在执行部署计划时分析该计划，您可以报告部署中包含的任何信息，并可以基于该计划中的步骤执行其他操作。  
  
## <a name="next-steps"></a>后续步骤  
可以创建其他工具来处理输出 XML 文件。 这只是 [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor) 的一个示例。 还可以创建 [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier)，以便在执行部署计划前更改该计划。  
  
## <a name="see-also"></a>另请参阅  
[演练：扩展数据库项目生成以生成模型统计信息](/previous-versions/visualstudio/visual-studio-2010/ee461508(v=vs.100))  
[演练：扩展数据库项目部署以修改部署计划](/previous-versions/visualstudio/visual-studio-2010/ee461507(v=vs.100))  
[使用生成参与者和部署参与者来自定义数据库生成和部署](/previous-versions/visualstudio/visual-studio-2010/ee461505(v=vs.100))  
