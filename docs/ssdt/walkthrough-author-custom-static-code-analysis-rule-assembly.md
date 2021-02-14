---
title: 创建适用于 SQL Server 的自定义静态代码分析规则程序集
description: 了解如何创建 SQL Server Code Analysis 规则。 设置规则以在存储过程、触发器和函数中避免使用 WAITFOR DELAY 语句。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f7b6ed8c-a4e0-4e33-9858-a8aa40aef309
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 3cae3183bef3d6bd40a9d30805f3923df8d70679
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082338"
---
# <a name="walkthrough-authoring-a-custom-static-code-analysis-rule-assembly-for-sql-server"></a>有关创建适用于 SQL Server 的自定义静态代码分析规则程序集的演练

此演练演示创建 SQL Server 代码分析规则的步骤。 在此演练中创建的规则用于避免存储过程、触发器和函数中的 WAITFOR DELAY 语句。  
  
在此演练中，将使用以下过程创建 Transact\-SQL 静态代码分析的自定义规则：  
  
1. 创建类库、启用该项目的签名并添加必要的引用。  
  
2. 创建 Visual C\# 自定义规则类。  
  
3. 创建两个帮助器 Visual C\# 类。  
  
4. 为了安装你创建的已生成 DLL，请将其复制到 Extensions 目录中。  
  
5. 请验证新代码分析规则是否已到位。  
  
**先决条件**
  
您需要满足以下条件才能完成本演练：  
  
- 必须已安装包含 SQL Server Data Tools 且支持 Visual C\# 或 Visual Basic 开发的 Visual Studio 版本。  
  
- 必须具有包含 SQL Server 对象的 SQL Server 项目。  
  
- 可以向其部署数据库项目的 SQL Server 实例。  
  
> [!NOTE]  
> 本演练面向已熟悉 SQL Server Data Tools 的 SQL Server 功能的用户。 你还应熟悉 Visual Studio 概念，例如，如何创建类库以及如何使用代码编辑器向类添加代码。  
  
## <a name="creating-a-custom-code-analysis-rule-for-sql-server"></a>创建 SQL Server 的自定义代码分析规则  

首先创建类库。 若要创建类库项目：  
  
1. 创建一个名为 SampleRules 的 Visual C\# 或 Visual Basic 类库项目。  
  
2. 将文件 Class1.cs 重命名为 AvoidWaitForDelayRule.cs。  
  
3. 在解决方案资源管理器中，右键单击项目节点，然后单击“添加引用”。  
  
4. 在“框架”选项卡上，选择“System.ComponentModel.Composition”。  
  
5. 单击“浏览”并导航到 C:\Program Files (x86)\\MicrosoftSQL Server\120\SDK\Assemblies 目录，选择 Microsoft.SqlServer.TransactSql.ScriptDom.dll，然后单击“确定”。  
  
6. 接下来，安装所需的 DACFx 引用。 单击“浏览”并导航到 <Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120 目录。 选择“Microsoft.SqlServer.Dac.dll”、“Microsoft.SqlServer.Dac.Extensions.dll”和“Microsoft.Data.Tools.Schema.Sql.dll”条目，并单击“添加”，然后单击“确定”。  
  
    现在，DACFx 二进制文件已安装到 Visual Studio 安装目录中。 对于 Visual Studio 2012，<Visual Studio Install Dir> 通常位于 C:\Program Files (x86)\\MicrosoftVisual Studio 11.0 中。 对于 Visual Studio 2013，该目录通常位于 C:\Program Files (x86)\\MicrosoftVisual Studio 12.0 中。  
  
接下来，将添加将由规则使用的支持类。  
  
## <a name="creating-the-custom-code-analysis-rule-supporting-classes"></a>创建自定义代码分析规则支持类。

在创建规则本身的类之前，将为项目添加访问者类和属性类。 这些类对于创建其他自定义规则可能会很有用。  
  
第一种必须定义的类为从 [TSqlConcreteFragmentVisitor](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlconcretefragmentvisitor) 派生的 WaitForDelayVisitor 类。 此类可访问模型中的 WAITFOR DELAY 语句。 访问者类使用 SQL Server 提供的 [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom) API。 在此 API 中，Transact\-SQL 代码表示为抽象语法树 (AST)，如果希望查找特定语法对象（例如 WAITFORDELAY 语句），访问者类会很有用。 使用对象模型查找语法对象可能会很困难，因为它们没有与特定对象属性或关系关联，而使用访问者模式和 [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom) API 查找它们就很容易。  
  
### <a name="defining-the-waitfordelayvisitor-class"></a>定义 WaitForDelayVisitor 类  
  
1. 在“解决方案资源管理器”中，选择 SampleRules 项目。  
  
2. 在“项目”菜单上，选择“添加类”。 此时会显示“添加新项”对话框。  
  
3. 在“名称”文本框中，键入 WaitForDelayVisitor.cs，然后单击“添加”按钮。 WaitForDelayVisitor.cs 文件将添加到“解决方案资源管理器”中的项目。  
  
4. 打开 WaitForDelayVisitor.cs 文件并更新内容，以与以下代码相匹配：  
  
    ```  
    using System.Collections.Generic;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    namespace SampleRules {  
        class WaitForDelayVistor {}  
    }  
    ```  
  
5. 在类声明中，将访问修饰符更改为内部并从 TSqlConcreteFragmentVisitor 派生类：  
  
    ```  
    internal class WaitForDelayVisitor : TSqlConcreteFragmentVisitor {}  
    ```  
  
6. 添加以下代码以定义列表成员变量：  
  
    ```  
    public IList<WaitForStatement> WaitForDelayStatements { get; private set; }  
    ```  
  
7. 通过添加以下代码，定义类构造函数：  
  
    ```  
    public WaitForDelayVisitor() {  
       WaitForDelayStatments = new List<WaitForStatement>();  
    }  
    ```  
  
8. 通过添加以下代码，重写 ExplicitVisit 方法：  
  
    ```  
    public override void ExplicitVisit(WaitForStatement node) {  
       // We are only interested in WAITFOR DELAY occurrences  
       if (node.WaitForOption == WaitForOption.Delay)  
          WaitForDelayStatments.Add(node);  
    }  
    ```  
  
    此方法访问模型中的 WAITFOR 语句，并且将指定了 DELAY 选项的语句添加到 WAITFOR DELAY 语句列表中。 此处引用的关键类为 [WaitForStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.waitforstatement)。  
  
9. 在“文件”菜单上，单击“保存”。  
  
第二种类为 LocalizedExportCodeAnalysisRuleAttribute.cs。 这是框架提供的内置 Microsoft.SqlServer.Dac.CodeAnalysis.ExportCodeAnalysisRuleAttribute 扩展，它支持从资源文件中读取由你的规则所使用的 DisplayName 和 Description。 如果之前计划在多种语言中使用规则，此类会很有用。  
  
### <a name="defining-the-localizedexportcodeanalysisruleattribute-class"></a>定义 LocalizedExportCodeAnalysisRuleAttribute 类  
  
1. 在“解决方案资源管理器”中，选择 SampleRules 项目。  
  
2. 在“项目”菜单上，选择“添加类”。 此时会显示“添加新项”对话框。  
  
3. 在“名称”文本框中，键入 LocalizedExportCodeAnalysisRuleAttribute.cs，然后单击“添加”按钮。 该文件将添加到“解决方案资源管理器”中的项目。  
  
4. 打开该文件并更新内容，以与以下代码相匹配：  
  
    ```  
    using Microsoft.SqlServer.Dac.CodeAnalysis;  
    using System;  
    using System.Globalization;  
    using System.Reflection;  
    using System.Resources;  
  
    namespace SampleRules  
    {  
  
        internal class LocalizedExportCodeAnalysisRuleAttribute : ExportCodeAnalysisRuleAttribute  
        {  
            private readonly string _resourceBaseName;  
            private readonly string _displayNameResourceId;  
            private readonly string _descriptionResourceId;  
  
            private ResourceManager _resourceManager;  
            private string _displayName;  
            private string _descriptionValue;  
  
            /// <summary>  
            /// Creates the attribute, with the specified rule ID, the fully qualified  
            /// name of the resource file that will be used for looking up display name  
            /// and description, and the Ids of those resources inside the resource file.  
            /// </summary>  
            public LocalizedExportCodeAnalysisRuleAttribute(  
                string id,  
                string resourceBaseName,  
                string displayNameResourceId,  
                string descriptionResourceId)  
                : base(id, null)  
            {  
                _resourceBaseName = resourceBaseName;  
                _displayNameResourceId = displayNameResourceId;  
                _descriptionResourceId = descriptionResourceId;  
            }  
  
            /// <summary>  
            /// Rules in a different assembly would need to overwrite this  
            /// </summary>  
            /// <returns></returns>  
            protected virtual Assembly GetAssembly()  
            {  
                return GetType().Assembly;  
            }  
  
            private void EnsureResourceManagerInitialized()  
            {  
                var resourceAssembly = GetAssembly();  
  
                try  
                {  
                    _resourceManager = new ResourceManager(_resourceBaseName, resourceAssembly);  
                }  
                catch (Exception ex)  
                {  
                    var msg = String.Format(CultureInfo.CurrentCulture, RuleResources.CannotCreateResourceManager, _resourceBaseName, resourceAssembly);  
                    throw new RuleException(msg, ex);  
                }  
            }  
  
            private string GetResourceString(string resourceId)  
            {  
                EnsureResourceManagerInitialized();  
                return _resourceManager.GetString(resourceId, CultureInfo.CurrentUICulture);  
            }  
  
            /// <summary>  
            /// Overrides the standard DisplayName and looks up its value inside a resources file  
            /// </summary>  
            public override string DisplayName  
            {  
                get  
                {  
                    if (_displayName == null)  
                    {  
                        _displayName = GetResourceString(_displayNameResourceId);  
                    }  
                    return _displayName;  
                }  
            }  
  
            /// <summary>  
            /// Overrides the standard Description and looks up its value inside a resources file  
            /// </summary>  
            public override string Description  
            {  
                get  
                {  
                    if (_descriptionValue == null)  
                    {   
                        _descriptionValue = GetResourceString(_descriptionResourceId);  
                    }  
                    return _descriptionValue;  
                }  
            }  
        }  
    }  
    ```  
  
接下来，添加一个资源文件，该文件将定义规则名称、规则描述以及类别（规则将在规则配置界面的该类别中显示）。  
  
### <a name="to-add-a-resource-file-and-three-resource-strings"></a>添加一个资源文件和三个资源字符串  
  
1. 在“解决方案资源管理器”中，选择 SampleRules 项目。  
  
2. 在“项目”菜单上，选择“添加新项”。 此时会显示“添加新项”对话框。  
  
3. 在“已安装的模板”列表中，单击“常规”。  
  
4. 在详细信息窗格中，单击“资源文件”。  
  
5. 在“名称”中，键入 RuleResources.resx。 资源编辑器随即出现，其中未定义任何资源。  
  
6. 定义 4 个资源字符串，如下所示：  
  
    |名称|值|  
    |--------|---------|  
    |AvoidWaitForDelay_ProblemDescription|在 {0} 中找到 WAITFOR DELAY 语句。|  
    |AvoidWaitForDelay_RuleName|避免在存储过程、函数和触发器中使用 WaitFor Delay 语句。|  
    |CategorySamples|SamplesCategory|  
    |CannotCreateResourceManager|无法从 {1} 创建 {0} 的 ResourceManager。|  
  
7. 在“文件”菜单上，单击“保存RuleResources.resx”。  
  
接下来，定义某个类（该类引用资源文件中由 Visual Studio 使用的资源），以在用户界面中显示有关规则的信息。  
  
### <a name="defining-the-sampleconstants-class"></a>定义 SampleConstants 类  
  
1. 在“解决方案资源管理器”中，选择 SampleRules 项目。  
  
2. 在“项目”菜单上，选择“添加类”。 此时会显示“添加新项”对话框。  
  
3. 在“名称”文本框中，键入 SampleRuleConstants.cs，然后单击“添加”按钮。 SampleRuleConstants.cs 文件将添加到“解决方案资源管理器”中的项目。  
  
4. 打开 SampleRuleConstants.cs 文件并且将以下 using 语句添加到该文件：  
  
    ```  
    namespace SampleRules  
    {   
        internal static class RuleConstants  
        {  
            /// <summary>  
            /// The name of the resources file to use when looking up rule resources  
            /// </summary>  
            public const string ResourceBaseName = "Public.Dac.Samples.Rules.RuleResources";  
  
            /// <summary>  
            /// Lookup name inside the resources file for the select asterisk rule name  
            /// </summary>  
            public const string AvoidWaitForDelay_RuleName = "AvoidWaitForDelay_RuleName";  
            /// <summary>  
            /// Lookup ID inside the resources file for the select asterisk description  
            /// </summary>  
            public const string AvoidWaitForDelay_ProblemDescription = "AvoidWaitForDelay_ProblemDescription";  
  
            /// <summary>  
            /// The design category (should not be localized)  
            /// </summary>  
            public const string CategoryDesign = "Design";  
  
            /// <summary>  
            /// The performance category (should not be localized)  
            /// </summary>  
            public const string CategoryPerformance = "Design";  
        }  
    }  
    ```  
  
5. 单击“文件” > “保持”。  
  
## <a name="creating-the-custom-code-analysis-rule-class"></a>创建自定义代码分析规则类。

现在，你已添加自定义代码分析规则将使用的帮助器类，你将创建一个自定义规则类并将其命名为 AvoidWaitForDelayRule。 AvoidWaitForDelayRule 自定义规则将用于帮助数据库开发人员避免在存储过程、触发器和函数中使用 WAITFOR DELAY 语句。  
  
### <a name="creating-the-avoidwaitfordelayrule-class"></a>创建 AvoidWaitForDelayRule 类  
  
1. 在“解决方案资源管理器”中，选择 SampleRules 项目。  
  
2. 在“项目”菜单上，选择“添加类”。 此时会显示“添加新项”对话框。  
  
3. 在“名称”文本框中，键入 AvoidWaitForDelayRule.cs，然后单击“添加”。 AvoidWaitForDelayRule.cs 文件将添加到“解决方案资源管理器”中的项目。  
  
4. 打开 AvoidWaitForDelayRule.cs 文件并且将以下 using 语句添加到该文件：  
  
    ```  
    using Microsoft.SqlServer.Dac.CodeAnalysis;  
    using Microsoft.SqlServer.Dac.Model;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    using System;  
    using System.Collections.Generic;  
    using System.Globalization;   
    namespace SampleRules {  
        class AvoidWaitForDelayRule {}  
    }  
    ```  
  
5. 在 AvoidWaitForDelayRule 类声明中，将访问修饰符更改为公共：  
  
    ```  
    /// <summary>  
    /// This is a rule that returns a warning message   
    /// whenever there is a WAITFOR DELAY statement appears inside a subroutine body.  
    /// This rule only applies to stored procedures, functions and triggers.  
    /// </summary>  
    public sealed class AvoidWaitForDelayRule  
    ```  
  
6. 从 Microsoft.SqlServer.Dac.CodeAnalysis.SqlCodeAnalysisRule 基类派生 AvoidWaitForDelayRule 类：  
  
    ```  
    public sealed class AvoidWaitForDelayRule : SqlCodeAnalysisRule  
    ```  
  
7. 将 LocalizedExportCodeAnalysisRuleAttribute 添加到你的类。  
  
    LocalizedExportCodeAnalysisRuleAttribute 允许代码分析服务发现自定义代码分析规则。 只有使用 ExportCodeAnalysisRuleAttribute 标记的类（或派生自此类的属性）可以用于代码分析中。  
  
    LocalizedExportCodeAnalysisRuleAttribute 提供了一些该服务所使用的必需元数据。 包括：此规则的唯一 ID、将在 Visual Studio 用户界面显示的显示名称以及标识问题时可由规则使用的说明。  
  
    ```  
    [LocalizedExportCodeAnalysisRule(AvoidWaitForDelayRule.RuleId,  
        RuleConstants.ResourceBaseName,  
        RuleConstants.AvoidWaitForDelay_RuleName,   
        RuleConstants.AvoidWaitForDelay_ProblemDescription  
        Category = RuleConstants.CategoryPerformance,   
        RuleScope = SqlRuleScope.Element)]   
    public sealed class AvoidWaitForDelayRule : SqlCodeAnalysisRule  
    {  
       /// <summary>  
       /// The Rule ID should resemble a fully-qualified class name. In the Visual Studio UI  
       /// rules are grouped by "Namespace + Category", and each rule is shown using "Short ID: DisplayName".  
       /// For this rule, that means the grouping will be "Public.Dac.Samples.Performance", with the rule  
       /// shown as "SR1004: Avoid using WaitFor Delay statements in stored procedures, functions and triggers."  
       /// </summary>  
       public const string RuleId = "RuleSamples.SR1004";  
    }  
    ```  
  
    RuleScope 属性应为 Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleScope.Element，因为此规则将分析特定元素。 该规则将为模型中的每个匹配元素调用一次。 如果希望分析整个模型，则可改用 Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleScope.Model。  
  
8. 添加设置 Microsoft.SqlServer.Dac.CodeAnalysis.SqlAnalysisRule.SupportedElementTypes 的构造函数。 这是元素范围内的规则所必需的。 它定义了将应用此规则的元素的类型。 在这种情况下，此规则将应用于存储过程、触发器和函数。 请注意 Microsoft.SqlServer.Dac.Model.ModelSchema 类可列出可以进行分析的所有可用元素类型。  
  
    ```  
    public AvoidWaitForDelayRule()  
    {  
       // This rule supports Procedures, Functions and Triggers. Only those objects will be passed to the Analyze method  
       SupportedElementTypes = new[]  
       {  
          // Note: can use the ModelSchema definitions, or access the TypeClass for any of these types  
          ModelSchema.ExtendedProcedure,  
          ModelSchema.Procedure,  
          ModelSchema.TableValuedFunction,  
          ModelSchema.ScalarFunction,  
  
          ModelSchema.DatabaseDdlTrigger,  
          ModelSchema.DmlTrigger,  
          ModelSchema.ServerDdlTrigger  
       };  
    }  
    ```  
  
9. 为 Microsoft.SqlServer.Dac.CodeAnalysis.SqlAnalysisRule.Analyze(Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleExecutionContext) 方法添加替代，它使用 Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleExecutionContext 作为输入参数。 此方法返回潜在问题列表。  
  
    此方法从上下文参数获取 Microsoft.SqlServer.Dac.Model.TSqlModel、Microsoft.SqlServer.Dac.Model.TSqlObject 和 [TSqlFragment](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlfragment)。 然后使用 WaitForDelayVisitor 类获取包含模型中所有 WAITFOR DELAY 语句的列表。  
  
    将为列表中的每个 [WaitForStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.waitforstatement) 创建一个 Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleProblem。  
  
    ```  
    /// <summary>  
    /// For element-scoped rules the Analyze method is executed once for every matching   
    /// object in the model.   
    /// </summary>  
    /// <param name="ruleExecutionContext">The context object contains the TSqlObject being   
    /// analyzed, a TSqlFragment  
    /// that's the AST representation of the object, the current rule's descriptor, and a   
    /// reference to the model being  
    /// analyzed.  
    /// </param>  
    /// <returns>A list of problems should be returned. These will be displayed in the Visual   
    /// Studio error list</returns>  
    public override IList<SqlRuleProblem> Analyze(  
        SqlRuleExecutionContext ruleExecutionContext)  
    {  
         IList<SqlRuleProblem> problems = new List<SqlRuleProblem>();  
  
         TSqlObject modelElement = ruleExecutionContext.ModelElement;  
  
         // this rule does not apply to inline table-valued function  
         // we simply do not return any problem in that case.  
         if (IsInlineTableValuedFunction(modelElement))  
         {  
             return problems;  
         }  
  
         string elementName = GetElementName(ruleExecutionContext, modelElement);  
  
         // The rule execution context has all the objects we'll need, including the   
         // fragment representing the object,  
         // and a descriptor that lets us access rule metadata  
         TSqlFragment fragment = ruleExecutionContext.ScriptFragment;  
         RuleDescriptor ruleDescriptor = ruleExecutionContext.RuleDescriptor;  
  
         // To process the fragment and identify WAITFOR DELAY statements we will use a   
         // visitor   
         WaitForDelayVisitor visitor = new WaitForDelayVisitor();  
         fragment.Accept(visitor);  
         IList<WaitForStatement> waitforDelayStatements = visitor.WaitForDelayStatements;  
  
         // Create problems for each WAITFOR DELAY statement found   
         // When creating a rule problem, always include the TSqlObject being analyzed. This   
         // is used to determine  
         // the name of the source this problem was found in and a best guess as to the   
         // line/column the problem was found at.  
         //  
         // In addition if you have a specific TSqlFragment that is related to the problem   
         //also include this  
         // since the most accurate source position information (start line and column) will   
         // be read from the fragment  
         foreach (WaitForStatement waitForStatement in waitforDelayStatements)  
         {  
            SqlRuleProblem problem = new SqlRuleProblem(  
                String.Format(CultureInfo.CurrentCulture,   
                    ruleDescriptor.DisplayDescription, elementName),  
                modelElement,  
                waitForStatement);  
            problems.Add(problem);  
        }  
        return problems;  
    }  
  
    private static string GetElementName(  
        SqlRuleExecutionContext ruleExecutionContext,   
        TSqlObject modelElement)  
    {  
        // Get the element name using the built in DisplayServices. This provides a number of   
        // useful formatting options to  
        // make a name user-readable  
        var displayServices = ruleExecutionContext.SchemaModel.DisplayServices;  
        string elementName = displayServices.GetElementName(  
            modelElement, ElementNameStyle.EscapedFullyQualifiedName);  
        return elementName;  
    }  
  
    private static bool IsInlineTableValuedFunction(TSqlObject modelElement)  
    {  
        return TableValuedFunction.TypeClass.Equals(modelElement.ObjectType)  
                       && FunctionType.InlineTableValuedFunction ==   
            modelElement.GetMetadata<FunctionType>(TableValuedFunction.FunctionType);  
    }  
    ```  
  
10. 单击“文件” > “保持”。  
  
### <a name="building-the-class-library"></a>正在生成类库  
  
1. 在“项目”菜单上，单击“SampleRules 属性”。  
  
2. 单击“签名”  选项卡。  
  
3. 单击“对程序集签名” 。  
  
4. 在“选择强名称密钥文件”中，单击 **<New>** 。  
  
5. 在“创建强名称密钥”对话框的“密钥文件名称” 中，键入 MyRefKey。  
  
6. （可选）可以为强名称密钥文件指定密码。  
  
7. 单击“确定”。  
  
8. 在“文件”  菜单上，单击“全部保存” 。  
  
9. 在“生成”菜单中，单击“生成解决方案”。  
  
接下来，你必须安装程序集，以便在生成和部署 SQL Server 项目时加载该程序集。  
  
## <a name="install-a-static-code-analysis-rule"></a>安装静态代码分析规则

若要安装规则，必须将程序集和关联的 .pdb 文件复制到 Extensions 文件夹。  
  
### <a name="to-install-the-samplerules-assembly"></a>安装 SampleRules 程序集

接下来，您要将程序集信息复制到 Extensions 目录中。 当 Visual Studio 启动后，它将标识 <Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions 目录和子目录中的任何扩展，并使其可供使用。  
  
对于 Visual Studio 2012，<Visual Studio Install Dir> 通常位于 C:\Program Files (x86)\\MicrosoftVisual Studio 11.0 中。 对于 Visual Studio 2013，该目录通常位于 C:\Program Files (x86)\\MicrosoftVisual Studio 12.0 中。  
  
将 SampleRules.dll 程序集文件从输出目录复制到 <Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions 目录。 默认情况下，已编译的 .dll 文件的路径为 YourSolutionPath\YourProjectPath\bin\Debug 或 YourSolutionPath\YourProjectPath\bin\Release。  
  
现在规则安装完成，重新启动 Visual Studio 即可显示。 接下来，你将启动 Visual Studio 的一个新会话并且创建一个数据库项目。  
  
### <a name="starting-a-new-visual-studio-session-and-creating-a-database-project"></a>启动新 Visual Studio 会话并且创建数据库项目  
  
1. 启动 Visual Studio 的第二个会话。  
  
2. 单击“文件” > “新建” > “项目”。    
  
3. 在“新建项目”对话框内的“已安装模板”列表中，展开“SQL Server”节点，然后再单击“SQL Server 数据库项目”。  
  
4. 在“名称”文本框中，键入 SampleRulesDB，然后单击“确定”。  
  
最后，将看到在 SQL Server 项目中显示的新规则。 查看新的 AvoidWaitForRule 代码分析规则：  
  
1. 在“解决方案资源管理器”中，选择 SampleRulesDB 项目。  
  
2. 在 **“项目”** 菜单上，单击 **“属性”** 。 SampleRulesDB 属性页随即显示。  
  
3. 单击“代码分析”。 应看到一个名为 RuleSamples.CategorySamples 的新类别。  
  
4. 展开 RuleSamples .CategorySamples。 应看到 SR1004：在存储过程、触发器和函数中避免使用 WAITFOR DELAY 语句。  
  
## <a name="see-also"></a>另请参阅

[数据库代码分析规则的扩展性概述](../ssdt/overview-of-extensibility-for-database-code-analysis-rules.md)