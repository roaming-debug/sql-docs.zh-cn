---
title: 将数据库项目内部版本扩展为生成模型统计信息
description: 了解如何创建、安装和测试生成参与者，该参与者在生成数据库项目时从 SQL 数据库模型中输出统计信息。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: d44935ce-63bf-46df-976a-5a54866c8119
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 5e3905a3ded61ac6ef99f9ada4e94ffaab77a36c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016677"
---
# <a name="walkthrough-extend-database-project-build-to-generate-model-statistics"></a>演练：扩展数据库项目生成以生成模型统计信息

可以创建生成参与者以便在生成数据库项目时执行自定义操作。 在本演练中，您将创建一个名为 ModelStatistics 的生成参与者，该参与者可在生成数据库项目时从 SQL 数据库模型中输出统计信息。 由于此生成参与者在您生成时会使用一些参数，因此需要执行一些额外步骤。  
  
在本演练中，您将完成以下主要任务：  
  
-   [创建生成参与者](#CreateBuildContributor)  
  
-   [安装生成参与者](#InstallBuildContributor)  
  
-   [测试生成参与者](#TestBuildContributor)  
  
## <a name="prerequisites"></a>先决条件  
您需要满足以下条件才能完成本演练：  
  
-   必须已安装包含 SQL Server Data Tools (SSDT) 且支持 C# 或 VB 开发的 Visual Studio 版本。  
  
-   必须具有包含 SQL 对象的 SQL 项目。  
  
> [!NOTE]  
> 本演练面向已熟悉 SSDT 的 SQL 功能的用户。 您还应熟悉 Visual Studio 的基本概念，例如，如何创建类库以及如何使用代码编辑器向类添加代码。  
  
## <a name="build-contributor-background"></a>生成参与者背景  
生成参与者将在项目生成期间运行（在生成表示项目的模型后，但在将项目保存到磁盘之前）。 生成参与者可用于许多方案，例如：  
  
-   验证模型内容并将验证错误报告给调用方。 可通过将错误添加到作为参数传递给 OnExecute 方法的列表来完成此操作。  
  
-   生成模型统计信息并报告给用户。 这是此处显示的示例。  
  
生成参与者的主入口点是 OnExecute 方法。 继承自 BuildContributor 的所有类都必须实现此方法。 BuildContributorContext 对象将传递给此方法 - 这包含生成的所有相关数据，例如，数据库模型、生成属性以及生成参与者将使用的参数/文件。  
  
**TSqlModel 和数据库模型 API**  
  
最有用的对象将是数据库模型，它由 TSqlModel 对象表示。 这是数据库的逻辑表示形式，包括所有表、视图和其他元素以及它们之间的关系。 有一种可用于查询特定类型的元素和遍历相关的关系的强类型架构。 您将在演练代码中看到如何使用此架构的示例。  
  
以下是本演练中的示例参与者使用的一些命令：  
  
|**类**|**方法/属性**|**说明**|  
|-------------|------------------------|-------------------|  
|[TSqlModel](/dotnet/api/microsoft.sqlserver.dac.model.tsqlmodel)|GetObjects()|用于查询对象的模型，并且是模型 API 的主入口点。 只能查询顶级类型，如表或视图 - 诸如列这样的类型只能通过遍历模型来查找。 如果未指定 ModelTypeClass 筛选器，则将返回所有顶级类型。|  
|[TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject)|GetReferencedRelationshipInstances()|查找与当前 TSqlObject 引用的元素的关系。 例如，对于表，这将返回类似表的列的对象。 在此情况下，ModelRelationshipClass 筛选器可用于指定要查询的确切关系（例如，使用“Table.Columns”筛选器将确保仅返回列）。<br /><br />有多种类似的方法，如 GetReferencingRelationshipInstances、GetChildren 和 GetParent。 有关详细信息，请参阅 API 文档。|  
  
**唯一标识你的参与者**  
  
在生成过程中，将从标准扩展目录中加载自定义参与者。 生成参与者由 [ExportBuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.exportbuildcontributorattribute) 属性标识。 必须使用该属性才能发现参与者。 此属性应与下面类似：  
  
```  
[ExportBuildContributor("ExampleContributors.ModelStatistics", "1.0.0.0")]  
  
```  
  
在此示例中，该属性的第一个参数应是一个唯一标识符，该标识符将用于在项目文件中标识你的参与者。 最佳做法是将库的命名空间（在本演练中为 “ExampleContributors”）与类名称（在本演练中为 “ModelStatistics”）结合使用来生成标识符。 您将了解如何使用此命名空间来指定应在演练的后面部分运行参与者。  
  
## <a name="create-a-build-contributor"></a><a name="CreateBuildContributor"></a>创建生成参与者  
若要创建生成参与者，您必须执行以下任务：  
  
-   创建类库项目并添加所需的引用。  
  
-   定义从 [BuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.buildcontributor) 继承的名为 ModelStatistics 的类。  
  
-   重写 OnExecute 方法。  
  
-   添加几个私有 Helper 方法。  
  
-   生成结果程序集。  
  
#### <a name="to-create-a-class-library-project"></a>创建类库项目  
  
1.  创建一个名为 MyBuildContributor 的 Visual Basic 或 Visual C# 类库项目。  
  
2.  将文件“Class1.cs”重命名为“ModelStatistics.cs”。  
  
3.  在解决方案资源管理器中，右键单击项目节点，然后单击“添加引用”。  
  
4.  选择“System.ComponentModel.Composition”  条目，然后单击“确定” 。  
  
5.  添加所需的 SQL 引用：右键单击项目节点，然后单击“添加引用”。 单击“浏览”按钮。  导航到 C:\Program Files (x86)\Microsoft SQL Server\110\DAC\Bin 文件夹。 选择“Microsoft.SqlServer.Dac.dll” 、“Microsoft.SqlServer.Dac.Extensions.dll” 和“Microsoft.Data.Tools.Schema.Sql.dll”  条目，然后单击“确定” 。  
  
    接下来，开始向类中添加代码。  
  
#### <a name="to-define-the-modelstatistics-class"></a>定义 ModelStatistics 类  
  
1.  ModelStatistics 类处理传递给 OnExecute 方法的数据库模型，并生成一个详细介绍该模型的内容的 XML 报告。  
  
    在代码编辑器中，更新 ModelStatistics.cs 文件以匹配：  
  
    ```csharp  
    using System;  
    using System.Collections.Generic;  
    using System.IO;  
    using System.Linq;  
    using System.Xml.Linq;  
    using Microsoft.Data.Schema;  
    using Microsoft.Data.Schema.Build;  
    using Microsoft.Data.Schema.Extensibility;  
    using Microsoft.Data.Schema.SchemaModel;  
    using Microsoft.Data.Schema.Sql;  
  
    namespace ExampleContributors  
    {  
    /// <summary>  
        /// A BuildContributor that generates statistics about a model and saves this to the output directory.  
        /// Will only run if a "GenerateModelStatistics=true" contributor argument is set in the project file, or a targets file.   
        /// Statistics can be sorted by "none, "name" or "value", with "none" being the default sort behavior.  
        ///   
        /// To set contributor arguments in a project file, add the following:  
        ///   
        /// <PropertyGroup>  
        ///     <ContributorArguments Condition="'$(Configuration)' == 'Debug'">  
        /// $(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy="name";  
        ///     </ContributorArguments>  
        /// <PropertyGroup>      
        ///   
        /// This will generate model statistics when building in Debug mode only - remove the condition to generate in all build modes.  
        /// </summary>  
        [ExportBuildContributor("ExampleContributors.ModelStatistics", "1.0.0.0")]  
        public class ModelStatistics : BuildContributor  
        {  
            public const string GenerateModelStatistics = "ModelStatistics.GenerateModelStatistics";  
            public const string SortModelStatisticsBy = "ModelStatistics.SortModelStatisticsBy";  
            public const string OutDir = "ModelStatistics.OutDir";  
            public const string ModelStatisticsFilename = "ModelStatistics.xml";  
            private enum SortBy { None, Name, Value };  
            private static Dictionary<string, SortBy> SortByMap = new Dictionary<string, SortBy>(StringComparer.OrdinalIgnoreCase)  
            {  
                { "none", SortBy.None },  
                { "name", SortBy.Name },  
                { "value", SortBy.Value },  
            };  
  
            private SortBy _sortBy = SortBy.None;  
  
            /// <summary>  
            /// Override the OnExecute method to perform actions when you build a database project.  
            /// </summary>  
            protected override void OnExecute(BuildContributorContext context, IList<ExtensibilityError> errors)  
            {  
                // handle related arguments, passed in as part of  
                // the context information.  
                bool generateModelStatistics;  
                ParseArguments(context.Arguments, errors, out generateModelStatistics);  
  
                // Only generate statistics if requested to do so  
                if (generateModelStatistics)  
                {  
                    // First, output model-wide information, such  
                    // as the type of database schema provider (DSP)  
                    // and the collation.  
                    StringBuilder statisticsMsg = new StringBuilder();  
                    statisticsMsg.AppendLine(" ")  
                                 .AppendLine("Model Statistics:")  
                                 .AppendLine("===")  
                                 .AppendLine(" ");  
                    errors.Add(new ExtensibilityError(statisticsMsg.ToString(), Severity.Message));  
  
                    var model = context.Model;  
  
                    // Start building up the XML that will later  
                    // be serialized.  
                    var xRoot = new XElement("ModelStatistics");  
  
                    SummarizeModelInfo(model, xRoot, errors);  
  
                    // First, count the elements that are contained   
                    // in this model.  
                    IList<TSqlObject> elements = model.GetObjects(DacQueryScopes.UserDefined).ToList();  
                    Summarize(elements, element => element.ObjectType.Name, "UserDefinedElements", xRoot, errors);  
  
                    // Now, count the elements that are defined in  
                    // another model. Examples include built-in types,  
                    // roles, filegroups, assemblies, and any   
                    // referenced objects from another database.  
                    elements = model.GetObjects(DacQueryScopes.BuiltIn | DacQueryScopes.SameDatabase | DacQueryScopes.System).ToList();  
                    Summarize(elements, element => element.ObjectType.Name, "OtherElements", xRoot, errors);  
  
                    // Now, count the number of each type  
                    // of relationship in the model.  
                    SurveyRelationships(model, xRoot, errors);  
  
                    // Determine where the user wants to save  
                    // the serialized XML file.  
                    string outDir;  
                    if (context.Arguments.TryGetValue(OutDir, out outDir) == false)  
                    {  
                        outDir = ".";  
                    }  
                    string filePath = Path.Combine(outDir, ModelStatisticsFilename);  
                    // Save the XML file and tell the user  
                    // where it was saved.  
                    xRoot.Save(filePath);  
                    ExtensibilityError resultArg = new ExtensibilityError("Result was saved to " + filePath, Severity.Message);  
                    errors.Add(resultArg);  
                }  
            }  
  
            /// <summary>  
            /// Examine the arguments provided by the user  
            /// to determine if model statistics should be generated  
            /// and, if so, how the results should be sorted.  
            /// </summary>  
            private void ParseArguments(IDictionary<string, string> arguments, IList<ExtensibilityError> errors, out bool generateModelStatistics)  
            {  
                // By default, we don't generate model statistics  
                generateModelStatistics = false;  
  
                // see if the user provided the GenerateModelStatistics   
                // option and if so, what value was it given.  
                string valueString;  
                arguments.TryGetValue(GenerateModelStatistics, out valueString);  
                if (string.IsNullOrWhiteSpace(valueString) == false)  
                {  
                    if (bool.TryParse(valueString, out generateModelStatistics) == false)  
                    {  
                        generateModelStatistics = false;  
  
                        // The value was not valid from the end user  
                        ExtensibilityError invalidArg = new ExtensibilityError(  
                            GenerateModelStatistics + "=" + valueString + " was not valid.  It can be true or false", Severity.Error);  
                        errors.Add(invalidArg);  
                        return;  
                    }  
                }  
  
                // Only worry about sort order if the user requested  
                // that we generate model statistics.  
                if (generateModelStatistics)  
                {  
                    // see if the user provided the sort option and  
                    // if so, what value was provided.  
                    arguments.TryGetValue(SortModelStatisticsBy, out valueString);  
                    if (string.IsNullOrWhiteSpace(valueString) == false)  
                    {  
                        SortBy sortBy;  
                        if (SortByMap.TryGetValue(valueString, out sortBy))  
                        {  
                            _sortBy = sortBy;  
                        }  
                        else  
                        {  
                            // The value was not valid from the end user  
                            ExtensibilityError invalidArg = new ExtensibilityError(  
                                SortModelStatisticsBy + "=" + valueString + " was not valid.  It can be none, name, or value", Severity.Error);  
                            errors.Add(invalidArg);  
                        }  
                    }  
                }  
            }  
  
            /// <summary>  
            /// Retrieve the database schema provider for the  
            /// model and the collation of that model.  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private static void SummarizeModelInfo(TSqlModel model, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                // use a Dictionary to accumulate the information  
                // that will later be output.  
                var info = new Dictionary<string, string>();  
  
                // Two things of interest: the database schema  
                // provider for the model, and the language id and  
                // case sensitivity of the collation of that  
                // model  
                info.Add("Version", model.Version.ToString());  
  
                TSqlObject options = model.GetObjects(DacQueryScopes.UserDefined, DatabaseOptions.TypeClass).FirstOrDefault();  
                if (options != null)  
                {  
                    info.Add("Collation", options.GetProperty<string>(DatabaseOptions.Collation));  
                }  
  
                // Output the accumulated information and add it to   
                // the XML.  
                OutputResult("Basic model info", info, xContainer, errors);  
            }  
  
            /// <summary>  
            /// For a provided list of model elements, count the number  
            /// of elements for each class name, sorted as specified  
            /// by the user.  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private void Summarize<T>(IList<T> set, Func<T, string> groupValue, string category, XElement xContainer, IList<ExtensibilityError> errors)  
            { // Use a Dictionary to keep all summarized information  
                var statistics = new Dictionary<string, int>();  
  
                // For each element in the provided list,  
                // count items based on the specified grouping  
                var groups =  
                    from item in set  
                    group item by groupValue(item) into g  
                    select new { g.Key, Count = g.Count() };  
  
                // order the groups as requested by the user  
                if (this._sortBy == SortBy.Name)  
                {  
                    groups = groups.OrderBy(group => group.Key);  
                }  
                else if (this._sortBy == SortBy.Value)  
                {  
                    groups = groups.OrderBy(group => group.Count);  
                }  
  
                // build the Dictionary of accumulated statistics  
                // that will be passed along to the OutputResult method.  
                foreach (var item in groups)  
                {  
                    statistics.Add(item.Key, item.Count);  
                }  
  
                statistics.Add("subtotal", set.Count);  
                statistics.Add("total items", groups.Count());  
  
                // output the results, and build up the XML  
                OutputResult(category, statistics, xContainer, errors);  
            }  
  
            /// <summary>  
            /// Iterate over all model elements, counting the  
            /// styles and types for relationships that reference each   
            /// element  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private static void SurveyRelationships(TSqlModel model, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                // get a list that contains all elements in the model  
                var elements = model.GetObjects(DacQueryScopes.All);  
                // We are interested in all relationships that  
                // reference each element.  
                var entries =  
                    from element in elements  
                    from entry in element.GetReferencedRelationshipInstances(DacExternalQueryScopes.All)  
                    select entry;  
  
                // initialize our counting buckets  
                var composing = 0;  
                var hierachical = 0;  
                var peer = 0;  
  
                // process each relationship, adding to the   
                // appropriate bucket for style and type.  
                foreach (var entry in entries)  
                {  
                    switch (entry.Relationship.Type)  
                    {  
                        case RelationshipType.Composing:  
                            ++composing;  
                            break;  
                        case RelationshipType.Hierarchical:  
                            ++hierachical;  
                            break;  
                        case RelationshipType.Peer:  
                            ++peer;  
                            break;  
                        default:  
                            break;  
                    }  
                }  
  
                // build a dictionary of data to pass along  
                // to the OutputResult method.  
                var stat = new Dictionary<string, int>  
                {  
                    {"Composing", composing},  
                    {"Hierarchical", hierachical},  
                    {"Peer", peer},  
                    {"subtotal", entries.Count()}  
                };  
  
                OutputResult("Relationships", stat, xContainer, errors);  
            }  
  
            /// <summary>  
            /// Performs the actual output for this contributor,  
            /// writing the specified set of statistics, and adding any   
            /// output information to the XML being constructed.  
            /// </summary>  
            private static void OutputResult<T>(string category, Dictionary<string, T> statistics, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                var maxLen = statistics.Max(stat => stat.Key.Length) + 2;  
                var format = string.Format("{{0, {0}}}: {{1}}", maxLen);  
  
                StringBuilder resultMessage = new StringBuilder();  
                //List<ExtensibilityError> args = new List<ExtensibilityError>();  
                resultMessage.AppendLine(category);  
                resultMessage.AppendLine("-----------------");  
  
                // Remove any blank spaces from the category name  
                var xCategory = new XElement(category.Replace(" ", ""));  
                xContainer.Add(xCategory);  
  
                foreach (var item in statistics)  
                {  
                    //Console.WriteLine(format, item.Key, item.Value);  
                    var entry = string.Format(format, item.Key, item.Value);  
                    resultMessage.AppendLine(entry);  
                    // Replace any blank spaces in the element key with  
                    // underscores.  
                    xCategory.Add(new XElement(item.Key.Replace(' ', '_'), item.Value));  
                }  
                resultMessage.AppendLine(" ");  
                errors.Add(new ExtensibilityError(resultMessage.ToString(), Severity.Message));  
            }  
        }  
    }  
  
    ```  
  
    接下来，您将生成类库。  
  
### <a name="to-sign-and-build-the-assembly"></a>生成程序集并对其进行签名  
  
1.  在“项目”  菜单上，单击“MyBuildContributor 属性” 。  
  
2.  单击“签名”  选项卡。  
  
3.  单击“对程序集签名” 。  
  
4.  在“选择强名称密钥文件”中，单击 **<New>** 。  
  
5.  在“创建强名称密钥”  对话框的“密钥文件名称” 中，键入“MyRefKey” 。  
  
6.  （可选）可以为强名称密钥文件指定密码。  
  
7.  单击“确定”。  
  
8.  在“文件”  菜单上，单击“全部保存” 。  
  
9. 在“生成”菜单中，单击“生成解决方案”。  
  
    接下来，您必须安装程序集，以便在生成 SQL 项目时加载该程序集。  
  
## <a name="install-a-build-contributor"></a><a name="InstallBuildContributor"></a>安装生成参与者  
若要安装生成参与者，您必须将程序集与关联的 .pdb 文件复制到 Extensions 文件夹。  
  
#### <a name="to-install-the-mybuildcontributor-assembly"></a>安装 MyBuildContributor 程序集  
  
1.  接下来，您要将程序集信息复制到 Extensions 目录中。 Visual Studio 在启动后将识别 %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions 目录和子目录中的任何扩展文件，并使其可供使用。  
  
2.  将 MyBuildContributor.dll 程序集文件从输出目录复制到 %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions 目录。  
  
    > [!NOTE]  
    > 默认情况下，已编译的 .dll 文件的路径为 YourSolutionPath\YourProjectPath\bin\Debug 或 YourSolutionPath\YourProjectPath\bin\Release。  
  
## <a name="run-or-test-your-build-contributor"></a><a name="TestBuildContributor"></a>运行或测试生成参与者  
若要运行或测试生成参与者，您必须执行以下任务：  
  
-   向计划生成的 .sqlproj 文件添加属性。  
  
-   通过使用 MSBuild 并提供适当的参数来生成数据库项目。  
  
### <a name="add-properties-to-the-sql-project-sqlproj-file"></a>向 SQL 项目 (.sqlproj) 文件添加属性  
您必须始终更新 SQL 项目文件以指定要运行的参与者的 ID。 此外，由于此生成参与者接受来自 MSBuild 的命令行参数，因此您必须修改 SQL 项目以允许用户通过 MSBuild 传递这些参数。  
  
可以通过两种方法执行此操作：  
  
-   您可以手动修改 .sqlproj 文件来添加必需的参数。 如果您不想在大量项目中重用生成参与者，则可以选择执行此操作。 如果您选择该选项，请将以下语句添加到 .sqlproj 文件中的第一个 Import 节点的后面  
  
    ```  
    /// <PropertyGroup>  
    ///     <ContributorArguments Condition="'$(Configuration)' == 'Debug'">  
    ///         $(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy="name";  
    ///     </ContributorArguments>  
    /// <PropertyGroup>  
  
    ```  
  
-   第二种方法是创建包含所需的参与者参数的目标文件。 如果您对多个项目使用相同的参与者，则此方法会很有用，因为它包含默认值。  
  
    在此情况下，请在 MSBuild 扩展路径中创建目标文件：  
  
    1.  导航到 %Program Files%\MSBuild\\。  
  
    2.  创建一个将存储目标文件的新文件夹“MyContributors”。  
  
    3.  在该目录中创建一个新文件“MyContributors.targets”，将下列文本添加到该文件中并保存该文件：  
  
        ```  
        <?xml version="1.0" encoding="utf-8"?>  
  
        <Project xmlns="https://schemas.microsoft.com/developer/msbuild/2003">  
          <PropertyGroup>  
            <BuildContributors>$(BuildContributors);ExampleContributors.ModelStatistics</BuildContributors>  
            <ContributorArguments Condition="'$(Configuration)' == 'Debug'">$(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy=name;</ContributorArguments>  
          </PropertyGroup>  
        </Project>  
        ```  
  
    4.  在要运行参与者角色的任何项目的 .sqlproj 文件中，通过在 .sqlproj 文件中的 \<Import Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets" \/> 节点之后添加以下语句来导入目标文件：  
  
        ```  
        <Import Project="$(MSBuildExtensionsPath)\MyContributors\MyContributors.targets " />  
        ```  
  
在使用了这些方法之一后，您可以使用 MSBuild 来传入命令行生成的参数。  
  
> [!NOTE]  
> 您必须始终更新“BuildContributors”属性以指定您的参与者 ID。 此 ID 与参与者源文件中的“ExportBuildContributor”属性中使用的 ID 相同。 如果没有此 ID，则您的参与者在生成项目时将不会运行。 仅在具有运行参与者所需的参数时才必须更新“ContributorArguments”属性。  
  
### <a name="build-the-sql-project"></a>生成 SQL 项目  
  
##### <a name="to-rebuild-your-database-project-by-using-msbuild-and-generate-statistics"></a>使用 MSBuild 重新生成数据库项目并生成统计信息  
  
1.  在 Visual Studio 中，右键单击项目并选择“重新生成”。 这将重新生成项目，您应该看到生成的模型统计信息，其中输出将包含在生成输出中并保存到 ModelStatistics.xml。 请注意，可能需要在解决方案资源管理器中选择“显示所有文件”才能看到 xml 文件。  
  
2.  打开 Visual Studio 命令提示符：在“开始”菜单上，依次单击“所有程序”、“Microsoft Visual Studio <Visual Studio Version>”、“Visual Studio Tools”和“Visual Studio 命令提示符(<Visual Studio Version>)”。  
  
3.  在命令提示符处，导航到包含 SQL 项目的文件夹。  
  
4.  在命令提示符窗口中键入以下命令：  
  
    ```  
    MSBuild /t:Rebuild MyDatabaseProject.sqlproj /p:BuildContributors=$(BuildContributors);ExampleContributors.ModelStatistics /p:ContributorArguments=$(ContributorArguments);GenerateModelStatistics=true;SortModelStatisticsBy=name;OutDir=.\;  
    ```  
  
    将 *MyDatabaseProject* 替换为要生成的数据库项目的名称。 如果您在上次生成项目后更改了该项目，则可使用 /t:Build 代替 /t:Rebuild。  
  
    在输出中，您将看到类似于下面的生成信息：  
  
```  
Model Statistics:  
===  
  
Basic model info  
-----------------  
    Version: Sql110  
  Collation: SQL_Latin1_General_CP1_CI_AS  
  
UserDefinedElements  
-----------------  
  DatabaseOptions: 1  
         subtotal: 1  
      total items: 1  
  
OtherElements  
-----------------  
                Assembly: 1  
       BuiltInServerRole: 9  
           ClrTypeMethod: 218  
  ClrTypeMethodParameter: 197  
         ClrTypeProperty: 20  
                Contract: 6  
                DataType: 34  
                Endpoint: 5  
               Filegroup: 1  
             MessageType: 14  
                   Queue: 3  
                    Role: 10  
                  Schema: 13  
                 Service: 3  
                    User: 4  
         UserDefinedType: 3  
                subtotal: 541  
             total items: 16  
  
Relationships  
-----------------  
     Composing: 477  
  Hierarchical: 6  
          Peer: 19  
      subtotal: 502  
  
```  
  
1.  打开 ModelStatistics.xml 并检查内容。  
  
    以报告的结果也将永久保存到 XML 文件中。  
  
## <a name="next-steps"></a>后续步骤  
可以创建其他工具来处理输出 XML 文件。 这只是生成参与者的一个示例。 例如，您可以创建生成参与者来将数据字典文件作为生成的一部分输出。  
  
## <a name="see-also"></a>另请参阅  
[使用生成参与者和部署参与者来自定义数据库生成和部署](../ssdt/use-deployment-contributors-to-customize-database-build-and-deployment.md)  
[演练：扩展数据库项目部署以分析部署计划](../ssdt/walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan.md)  
