---
title: 扩展数据库项目部署以修改部署计划
description: 创建一个 DeploymentPlanModifier 类型的部署参与者，该参与者对部署脚本批进行编程，使其在执行过程中出现错误时重新运行。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 22b077b1-fa25-49ff-94f6-6d0d196d870a
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: bdb9831caf2145b43340fdca9af3108ba20d3c34
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064952"
---
# <a name="walkthrough-extend-database-project-deployment-to-modify-the-deployment-plan"></a>演练：扩展数据库项目部署以修改部署计划

可以创建部署参与者以便在部署 SQL 项目时执行自定义操作。 可以创建 [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) 或 [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor)。 使用 [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) 可在执行计划前更改计划，使用 [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor) 可在执行计划时执行操作。 在本演练中，你将创建一个名为 SqlRestartableScriptContributor 的 [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier)，用于将 IF 语句添加到部署脚本中的批处理，以便脚本能在执行期间出错时重新运行直至批处理完成。  
  
在本演练中，您将完成以下主要任务：  
  
-   [创建 DeploymentPlanModifier 类型的部署参与者](#CreateDeploymentContributor)  
  
-   [安装部署参与者](#InstallDeploymentContributor)  
  
-   [运行或测试部署参与者](#TestDeploymentContributor)  
  
## <a name="prerequisites"></a>先决条件  
您需要满足以下条件才能完成本演练：  
  
-   必须已安装包含 SQL Server Data Tools 且支持 C# 或 VB 开发的 Visual Studio 版本。  
  
-   必须具有包含 SQL 对象的 SQL 项目。  
  
-   可以向其部署数据库项目的 SQL Server 实例。  
  
> [!NOTE]  
> 本演练面向已熟悉 SQL Server Data Tools 的 SQL 功能的用户。 您还应熟悉 Visual Studio 的基本概念，例如，如何创建类库以及如何使用代码编辑器向类添加代码。  
  
## <a name="create-a-deployment-contributor"></a><a name="CreateDeploymentContributor"></a>创建部署参与者  
若要创建部署参与者，您必须执行以下任务：  
  
-   创建类库项目并添加所需的引用。  
  
-   定义从 [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) 继承的名为 SqlRestartableScriptContributor 的类。  
  
-   重写 [OnExecute](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributor.onexecute) 方法。  
  
-   添加私有 Helper 方法。  
  
-   生成结果程序集。  
  
#### <a name="to-create-a-class-library-project"></a>创建类库项目  
  
1.  创建一个名为 MyOtherDeploymentContributor 的 Visual C# 或 Visual Basic 类库项目。  
  
2.  将文件“Class1.cs”重命名为“SqlRestartableScriptContributor.cs”。  
  
3.  在解决方案资源管理器中，右键单击项目节点，然后单击“添加引用”。  
  
4.  在“框架”选项卡上，选择“System.ComponentModel.Composition”。  
  
5.  单击“浏览”并导航到 C:\Program Files (x86)\Microsoft SQL Server\110\SDK\Assemblies 目录，选择 Microsoft.SqlServer.TransactSql.ScriptDom.dll，然后单击“确定”。  
  
6.  添加所需的 SQL 引用：右键单击项目节点，然后单击“添加引用”。 单击“浏览”，并导航到 C:\Program Files (x86)\Microsoft SQL Server\110\DAC\Bin 文件夹。 选择“Microsoft.SqlServer.Dac.dll”、“Microsoft.SqlServer.Dac.Extensions.dll”和“Microsoft.Data.Tools.Schema.Sql.dll”条目，并单击“添加”，然后单击“确定”。  
  
接下来，开始向类中添加代码。  
  
#### <a name="to-define-the-sqlrestartablescriptcontributor-class"></a>定义 SqlRestartableScriptContributor 类  
  
1.  在代码编辑器中，更新 class1.cs 文件以匹配以下 using 语句：  
  
    ```csharp  
    using System;  
    using System.Collections.Generic;  
    using System.Globalization;  
    using System.Text;  
    using Microsoft.SqlServer.Dac.Deployment;  
    using Microsoft.SqlServer.Dac.Model;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    ```  
  
2.  更新类定义以匹配以下示例：  
  
    ```csharp  
        /// <summary>  
    /// This deployment contributor modifies a deployment plan by adding if statements  
    /// to the existing batches in order to make a deployment script able to be rerun to completion  
    /// if an error is encountered during execution  
    /// </summary>  
    [ExportDeploymentPlanModifier("MyOtherDeploymentContributor.RestartableScriptContributor", "1.0.0.0")]  
    public class SqlRestartableScriptContributor : DeploymentPlanModifier  
    {  
    }  
  
    ```  
  
    现在，你已定义继承自 [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) 的部署参与者。 在生成和部署过程中，将从标准扩展目录中加载自定义参与者。 部署计划修改参与者将由 [ExportDeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.exportdeploymentplanmodifierattribute) 属性标识。 必须使用该属性才能发现参与者。 此属性应与下面类似：  
  
    ```csharp  
    [ExportDeploymentPlanModifier("MyOtherDeploymentContributor.RestartableScriptContributor", "1.0.0.0")]  
  
    ```  
  
3.  添加以下成员声明：  
  
    ```vb  
         private const string BatchIdColumnName = "BatchId";  
            private const string DescriptionColumnName = "Description";  
  
            private const string CompletedBatchesVariableName = "CompletedBatches";  
            private const string CompletedBatchesVariable = "$(CompletedBatches)";  
            private const string CompletedBatchesSqlCmd = @":setvar " + CompletedBatchesVariableName + " __completedBatches_{0}_{1}";  
            private const string TotalBatchCountSqlCmd = @":setvar TotalBatchCount {0}";  
            private const string CreateCompletedBatchesTable = @"  
    if OBJECT_ID(N'tempdb.dbo." + CompletedBatchesVariable + @"', N'U') is null  
    begin  
    use tempdb  
    create table [dbo].[$(CompletedBatches)]  
    (  
    BatchId int primary key,  
    Description nvarchar(300)  
    )  
    use [$(DatabaseName)]  
    end  
    ";  
  
    ```  
  
    接下来，您将重写 OnExecute 方法以添加要在部署数据库项目时运行的代码。  
  
#### <a name="to-override-onexecute"></a>重写 OnExecute  
  
1.  将下面的方法添加到 SqlRestartableScriptContributor 类：  
  
    ```csharp  
    /// <summary>  
    /// You override the OnExecute method to do the real work of the contributor.  
    /// </summary>  
    /// <param name="context"></param>  
    protected override void OnExecute(DeploymentPlanContributorContext context)  
    {  
         // Replace this with the method body  
    }  
  
    ```  
  
    你将从基类 [DeploymentPlanContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributor) 重写 [OnExecute](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributor.onexecute) 方法，该基类是 [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) 和 [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor) 的基类。 将向 OnExecute 方法传递一个 [DeploymentPlanContributorContext](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributorcontext) 对象，该对象提供对任何指定参数、源和目标数据库模型、部署计划以及部署选项的访问权。 在此示例中，我们获取了部署计划和目标数据库名称。  
  
2.  现在，将正文的开头添加到 OnExecute 方法：  
  
    ```vb  
    // Obtain the first step in the Plan from the provided context  
    DeploymentStep nextStep = context.PlanHandle.Head;  
    int batchId = 0;  
    BeginPreDeploymentScriptStep beforePreDeploy = null;  
  
    // Loop through all steps in the deployment plan  
    while (nextStep != null)  
    {  
        // Increment the step pointer, saving both the current and next steps  
        DeploymentStep currentStep = nextStep;  
        nextStep = currentStep.Next;  
  
        // Add additional step processing here  
    }  
  
    // if we found steps that required processing, set up a temporary table to track the work that you are doing  
    if (beforePreDeploy != null)  
    {  
        // Add additional post-processing here  
    }  
  
    // Cleanup and drop the table   
    DeploymentScriptStep dropStep = new DeploymentScriptStep(DropCompletedBatchesTable);  
    base.AddAfter(context.PlanHandle, context.PlanHandle.Tail, dropStep);  
  
    ```  
  
    在此代码中，我们定义几个局部变量，并设置将处理部署计划中所有步骤的处理过程的循环。 循环完成后，我们必须进行一些后续处理，然后删除部署过程中创建的临时表以便在执行计划时跟踪进度。 主要类型为：[DeploymentStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentstep) 和 [DeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptstep)。 主要方法是 AddAfter。  
  
3.  现在，添加其他步骤处理来替换显示为“在此处添加其他步骤处理”的注释：  
  
    ```csharp  
    // Look for steps that mark the pre/post deployment scripts  
    // These steps will always be in the deployment plan even if the  
    // user's project does not have a pre/post deployment script  
    if (currentStep is BeginPreDeploymentScriptStep)  
    {  
        // This step marks the beginning of the predeployment script.  
        // Save the step and move on.  
        beforePreDeploy = (BeginPreDeploymentScriptStep)currentStep;  
        continue;  
    }  
    if (currentStep is BeginPostDeploymentScriptStep)  
    {  
        // This is the step that marks the beginning of the post deployment script.    
        // We do not continue processing after this point.  
        break;  
    }  
    if (currentStep is SqlPrintStep)  
    {  
        // We do not need to put if statements around these  
        continue;  
    }  
  
    // if we have not yet found the beginning of the pre-deployment script steps,   
    // skip to the next step.  
    if (beforePreDeploy == null)  
    {  
        // We only surround the "main" statement block with conditional  
        // statements  
        continue;  
    }  
  
    // Determine if this is a step that we need to surround with a conditional statement  
    DeploymentScriptDomStep domStep = currentStep as DeploymentScriptDomStep;  
    if (domStep == null)  
    {  
        // This step is not a step that we know how to modify,  
        // so skip to the next step.  
        continue;  
    }  
  
    TSqlScript script = domStep.Script as TSqlScript;  
    if (script == null)  
    {  
        // The script dom step does not have a script with batches - skip  
        continue;  
    }  
  
        // Loop through all the batches in the script for this step.  All the statements  
        // in the batch will be enclosed in an if statement that will check the  
        // table to ensure that the batch has not already been executed  
        TSqlObject sqlObject;  
        string stepDescription;  
        GetStepInfo(domStep, out stepDescription, out sqlObject);  
        int batchCount = script.Batches.Count;  
  
    for (int batchIndex = 0; batchIndex < batchCount; batchIndex++)  
    {  
        // Add batch processing here  
    }  
  
    ```  
  
    代码注释用于对处理过程进行说明。 粗略来说，此代码将查找您关注的步骤，跳过其他步骤并在您即将开始后期部署步骤时停止。 如果该步骤包含必须用条件句环绕的语句，则我们将执行其他处理。 主要类型、方法和属性包括：[BeginPreDeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.beginpredeploymentscriptstep)、[BeginPostDeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.beginpostdeploymentscriptstep)、[TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject)、[TSqlScript](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlscript)、脚本、[DeploymentScriptDomStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptdomstep) 和 [SqlPrintStep](/dotnet/api/microsoft.sqlserver.dac.deployment.sqlprintstep)。  
  
4.  现在，通过替换显示为“在此处添加批处理”的注释来添加批处理代码：  
  
    ```csharp  
        // Create the if statement that will contain the batch's contents  
        IfStatement ifBatchNotExecutedStatement = CreateIfNotExecutedStatement(batchId);  
        BeginEndBlockStatement statementBlock = new BeginEndBlockStatement();  
        ifBatchNotExecutedStatement.ThenStatement = statementBlock;  
        statementBlock.StatementList = new StatementList();  
  
        TSqlBatch batch = script.Batches[batchIndex];  
        int statementCount = batch.Statements.Count;  
  
        // Loop through all statements in the batch, embedding those in an sp_execsql  
        // statement that must be handled this way (schemas, stored procedures,   
        // views, functions, and triggers).  
        for (int statementIndex = 0; statementIndex < statementCount; statementIndex++)  
        {  
            // Add additional statement processing here  
        }  
  
        // Add an insert statement to track that all the statements in this  
        // batch were executed.  Turn on nocount to improve performance by  
        // avoiding row inserted messages from the server  
        string batchDescription = string.Format(CultureInfo.InvariantCulture,  
            "{0} batch {1}", stepDescription, batchIndex);  
  
        PredicateSetStatement noCountOff = new PredicateSetStatement();  
        noCountOff.IsOn = false;  
        noCountOff.Options = SetOptions.NoCount;  
  
        PredicateSetStatement noCountOn = new PredicateSetStatement();  
        noCountOn.IsOn = true;  
        noCountOn.Options = SetOptions.NoCount;   
        InsertStatement batchCompleteInsert = CreateBatchCompleteInsert(batchId, batchDescription);  
        statementBlock.StatementList.Statements.Add(noCountOn);  
    statementBlock.StatementList.Statements.Add(batchCompleteInsert);  
        statementBlock.StatementList.Statements.Add(noCountOff);  
  
        // Remove all the statements from the batch (they are now in the if block) and add the if statement  
        // as the sole statement in the batch  
        batch.Statements.Clear();  
        batch.Statements.Add(ifBatchNotExecutedStatement);  
  
        // Next batch  
        batchId++;  
  
    ```  
  
    此代码将创建一个 IF 语句和一个 BEGIN/END 块。 随后我们将对批处理中的语句进行其他处理。 完成此操作后，我们将添加一个 INSERT 语句以将信息添加到用于跟踪脚本执行进度的临时表。 最后，更新批处理，并将过去位于该处的语句替换为包含这些语句的新 IF 语句。主要类型、方法和属性包括：[IfStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.ifstatement)、[BeginEndBlockStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.beginendblockstatement)、[StatementList](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.statementlist)、[TSqlBatch](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlbatch)、[PredicateSetStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.predicatesetstatement)、[SetOptions](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.setoptions) 和 [InsertStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.insertstatement)。  
  
5.  现在，添加语句处理循环的正文。 替换显示为“在此处添加其他语句处理”的注释：  
  
    ```csharp  
    TSqlStatement smnt = batch.Statements[statementIndex];  
  
    if (IsStatementEscaped(sqlObject))  
    {  
        // "escape" this statement by embedding it in a sp_executesql statement  
        string statementScript;  
        domStep.ScriptGenerator.GenerateScript(smnt, out statementScript);  
        ExecuteStatement spExecuteSql = CreateExecuteSql(statementScript);  
        smnt = spExecuteSql;  
    }  
  
    statementBlock.StatementList.Statements.Add(smnt);  
  
    ```  
  
    对于批处理中的每个语句，如果语句是必须用 sp_executesql 语句环绕的类型，请相应地修改语句。 随后，代码会将语句添加到您创建的 BEGIN/END 块的语句列表中。 主要类型、方法和属性包括 [TSqlStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlstatement) 和 [ExecuteStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executestatement)。  
  
6.  最后，添加后续处理部分来替代显示为“在此处添加其他后续处理”的注释：  
  
    ```csharp  
    // Declare a SqlCmd variables.  
    //  
    // CompletedBatches variable - defines the name of the table in tempdb that will track  
    // all the completed batches.  The temporary table's name has the target database name and  
    // a guid embedded in it so that:  
    // * Multiple deployment scripts targeting different DBs on the same server  
    // * Failed deployments with old tables do not conflict with more recent deployments  
    //  
    // TotalBatchCount variable - the total number of batches surrounded by if statements.  Using this  
    // variable pre/post deployment scripts can also use the CompletedBatches table to make their  
    // script rerunnable if there is an error during execution  
    StringBuilder sqlcmdVars = new StringBuilder();  
    sqlcmdVars.AppendFormat(CultureInfo.InvariantCulture, CompletedBatchesSqlCmd,  
        context.Options.TargetDatabaseName, Guid.NewGuid().ToString("D"));  
    sqlcmdVars.AppendLine();  
    sqlcmdVars.AppendFormat(CultureInfo.InvariantCulture, TotalBatchCountSqlCmd, batchId);  
  
    DeploymentScriptStep completedBatchesSetVarStep = new DeploymentScriptStep(sqlcmdVars.ToString());  
    base.AddBefore(context.PlanHandle, beforePreDeploy, completedBatchesSetVarStep);  
  
    // Create the temporary table we will use to track the work that we are doing  
    DeploymentScriptStep createStatusTableStep = new DeploymentScriptStep(CreateCompletedBatchesTable);  
    base.AddBefore(context.PlanHandle, beforePreDeploy, createStatusTableStep);  
  
    ```  
  
    如果处理过程发现了一个或多个用条件语句环绕的步骤，则必须向部署脚本添加语句以定义 SQLCMD 变量。 第一个变量 CompletedBatches 包含临时表的唯一名称，部署脚本在执行时将使用此表跟踪已成功完成的批处理。 第二个变量 TotalBatchCount 包含部署脚本中的批处理的总数。  
  
    其他相关的类型、属性和方法包括：  
  
    StringBuilder、[DeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptstep) 和 AddBefore。  
  
    接下来，您将定义由此方法调用的 Helper 方法。  
  
#### <a name="to-add-the-helper-methods"></a>添加 Helper 方法  
  
-   必须定义多种 Helper 方法。 重要方法包括：  
  
    |**方法**|**说明**|  
    |--------------|-------------------|  
    |CreateExecuteSQL|定义 CreateExecuteSQL 方法以使用 EXEC sp_executesql 语句环绕提供的语句。 主要类型、方法和属性包括：[ExecuteStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executestatement)、[ExecutableProcedureReference](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executableprocedurereference)、[SchemaObjectName](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.schemaobjectname)、[ProcedureReference](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.procedurereference) 和 [ExecuteParameter](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executeparameter)。|  
    |CreateCompletedBatchesName|定义 CreateCompletedBatchesName 方法。 此方法创建将插入批处理的临时表中的名称。主要类型、方法和属性包括：[SchemaObjectName](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.schemaobjectname)。|  
    |IsStatementEscaped|定义 IsStatementEscaped 方法。 此方法确定此类模型元素是否需要先将语句包装在 EXEC sp_executesql 语句中，然后才能将其包含在 IF 语句中。 主要类型、方法和属性包括：TSqlObject.ObjectType、ModelTypeClass 和以下模型类型的 TypeClass 属性：Schema、Procedure、View、TableValuedFunction、ScalarFunction、DatabaseDdlTrigger、DmlTrigger、ServerDdlTrigger。|  
    |CreateBatchCompleteInsert|定义 CreateBatchCompleteInsert 方法。 此方法创建将添加到部署脚本以便跟踪脚本执行进度的 INSERT 语句。 主要类型、方法和属性包括：InsertStatement、NamedTableReference、ColumnReferenceExpression、ValuesInsertSource 和 RowValue。|  
    |CreateIfNotExecutedStatement|定义 CreateIfNotExecutedStatement 方法。 此方法生成一个 IF 语句，该语句可检查临时批处理是否执行指示此批处理已执行的表。 主要类型、方法和属性包括：IfStatement、ExistsPredicate、ScalarSubquery、NamedTableReference、WhereClause、ColumnReferenceExpression、IntegerLiteral、BooleanComparisonExpression 和 BooleanNotExpression。|  
    |GetStepInfo|定义 GetStepInfo 方法。 此方法提取有关用于创建步骤的脚本的模型元素的信息以及步骤名称。 相关的类型和方法包括：[DeploymentPlanContributorContext](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributorcontext)、[DeploymentScriptDomStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptdomstep)、[TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject)、[CreateElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.createelementstep)、[AlterElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.alterelementstep) 和 [DropElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.dropelementstep)。|  
    |GetElementName|为 TSqlObject 创建格式化名称。|  
  
1.  添加下列代码可定义 Helper 方法：  
  
    ```csharp  
  
    /// <summary>  
    /// The CreateExecuteSql method "wraps" the provided statement script in an "sp_executesql" statement  
    /// Examples of statements that must be so wrapped include: stored procedures, views, and functions  
    /// </summary>  
    private static ExecuteStatement CreateExecuteSql(string statementScript)  
    {  
        // define a new Exec statement  
        ExecuteStatement executeSp = new ExecuteStatement();  
        ExecutableProcedureReference spExecute = new ExecutableProcedureReference();  
        executeSp.ExecuteSpecification = new ExecuteSpecification { ExecutableEntity = spExecute };  
  
        // define the name of the procedure that you want to execute, in this case sp_executesql  
        SchemaObjectName procName = new SchemaObjectName();  
        procName.Identifiers.Add(CreateIdentifier("sp_executesql", QuoteType.NotQuoted));  
        ProcedureReference procRef = new ProcedureReference { Name = procName };  
  
        spExecute.ProcedureReference = new ProcedureReferenceName { ProcedureReference = procRef };  
  
        // add the script parameter, constructed from the provided statement script  
        ExecuteParameter scriptParam = new ExecuteParameter();  
        spExecute.Parameters.Add(scriptParam);  
        scriptParam.ParameterValue = new StringLiteral { Value = statementScript };  
        scriptParam.Variable = new VariableReference { Name = "@stmt" };  
        return executeSp;  
    }  
  
    /// <summary>  
    /// The CreateIdentifier method returns a Identifier with the specified value and quoting type  
    /// </summary>  
    private static Identifier CreateIdentifier(string value, QuoteType quoteType)  
    {  
        return new Identifier { Value = value, QuoteType = quoteType };  
    }  
  
    /// <summary>  
    /// The CreateCompletedBatchesName method creates the name that will be inserted  
    /// into the temporary table for a batch.  
    /// </summary>  
    private static SchemaObjectName CreateCompletedBatchesName()  
    {  
        SchemaObjectName name = new SchemaObjectName();  
        name.Identifiers.Add(CreateIdentifier("tempdb", QuoteType.SquareBracket));  
        name.Identifiers.Add(CreateIdentifier("dbo", QuoteType.SquareBracket));  
        name.Identifiers.Add(CreateIdentifier(CompletedBatchesVariable, QuoteType.SquareBracket));  
        return name;  
    }  
  
    /// <summary>  
    /// Helper method that determins whether the specified statement needs to  
    /// be escaped  
    /// </summary>  
    /// <param name="sqlObject"></param>  
    /// <returns></returns>  
    private static bool IsStatementEscaped(TSqlObject sqlObject)  
    {  
        HashSet<ModelTypeClass> escapedTypes = new HashSet<ModelTypeClass>  
        {  
            Schema.TypeClass,  
            Procedure.TypeClass,  
            View.TypeClass,  
            TableValuedFunction.TypeClass,  
            ScalarFunction.TypeClass,  
            DatabaseDdlTrigger.TypeClass,  
            DmlTrigger.TypeClass,  
            ServerDdlTrigger.TypeClass  
        };  
        return escapedTypes.Contains(sqlObject.ObjectType);  
    }  
  
    /// <summary>  
    /// Helper method that creates an INSERT statement to track a batch being completed  
    /// </summary>  
    /// <param name="batchId"></param>  
    /// <param name="batchDescription"></param>  
    /// <returns></returns>  
    private static InsertStatement CreateBatchCompleteInsert(int batchId, string batchDescription)  
    {  
        InsertStatement insert = new InsertStatement();  
        NamedTableReference batchesCompleted = new NamedTableReference();  
        insert.InsertSpecification = new InsertSpecification();  
        insert.InsertSpecification.Target = batchesCompleted;  
        batchesCompleted.SchemaObject = CreateCompletedBatchesName();  
  
        // Build the columns inserted into  
        ColumnReferenceExpression batchIdColumn = new ColumnReferenceExpression();  
        batchIdColumn.MultiPartIdentifier = new MultiPartIdentifier();  
        batchIdColumn.MultiPartIdentifier.Identifiers.Add(CreateIdentifier(BatchIdColumnName, QuoteType.NotQuoted));  
  
        ColumnReferenceExpression descriptionColumn = new ColumnReferenceExpression();  
        descriptionColumn.MultiPartIdentifier = new MultiPartIdentifier();  
        descriptionColumn.MultiPartIdentifier.Identifiers.Add(CreateIdentifier(DescriptionColumnName, QuoteType.NotQuoted));  
  
        insert.InsertSpecification.Columns.Add(batchIdColumn);  
        insert.InsertSpecification.Columns.Add(descriptionColumn);  
  
        // Build the values inserted  
        ValuesInsertSource valueSource = new ValuesInsertSource();  
        insert.InsertSpecification.InsertSource = valueSource;  
  
        RowValue values = new RowValue();  
        values.ColumnValues.Add(new IntegerLiteral { Value = batchId.ToString() });  
        values.ColumnValues.Add(new StringLiteral { Value = batchDescription });  
        valueSource.RowValues.Add(values);  
  
        return insert;  
    }  
  
    /// <summary>  
    /// This is a helper method that generates an if statement that checks the batches executed  
    /// table to see if the current batch has been executed.  The if statement will look like this  
    ///   
    /// if not exists(select 1 from [tempdb].[dbo].[$(CompletedBatches)]   
    ///                where BatchId = batchId)  
    /// begin  
    /// end  
    /// </summary>  
    /// <param name="batchId"></param>  
    /// <returns></returns>  
    private static IfStatement CreateIfNotExecutedStatement(int batchId)  
    {  
        // Create the exists/select statement  
        ExistsPredicate existsExp = new ExistsPredicate();  
        ScalarSubquery subQuery = new ScalarSubquery();  
        existsExp.Subquery = subQuery;  
  
        subQuery.QueryExpression = new QuerySpecification  
        {  
            SelectElements =  
            {  
                new SelectScalarExpression  { Expression = new IntegerLiteral { Value ="1" } }  
            },  
            FromClause = new FromClause  
            {  
                TableReferences =  
                    {  
                        new NamedTableReference() { SchemaObject = CreateCompletedBatchesName() }  
                    }  
            },  
            WhereClause = new WhereClause  
            {  
                SearchCondition = new BooleanComparisonExpression  
                {  
                    ComparisonType = BooleanComparisonType.Equals,  
                    FirstExpression = new ColumnReferenceExpression  
                    {  
                        MultiPartIdentifier = new MultiPartIdentifier  
                        {  
                            Identifiers = { CreateIdentifier(BatchIdColumnName, QuoteType.SquareBracket) }  
                        }  
                    },  
                    SecondExpression = new IntegerLiteral { Value = batchId.ToString() }  
                }  
            }  
        };  
  
        // Put together the rest of the statement  
        IfStatement ifNotExists = new IfStatement  
        {  
            Predicate = new BooleanNotExpression  
            {  
                Expression = existsExp  
            }  
        };  
  
        return ifNotExists;  
    }  
  
    /// <summary>  
    /// Helper method that generates a useful description of the step.  
    /// </summary>  
    private static void GetStepInfo(  
        DeploymentScriptDomStep domStep,  
        out string stepDescription,  
        out TSqlObject element)  
    {  
        element = null;  
  
        // figure out what type of step we've got, and retrieve  
        // either the source or target element.  
        if (domStep is CreateElementStep)  
        {  
            element = ((CreateElementStep)domStep).SourceElement;  
        }  
        else if (domStep is AlterElementStep)  
        {  
            element = ((AlterElementStep)domStep).SourceElement;  
        }  
        else if (domStep is DropElementStep)  
        {  
            element = ((DropElementStep)domStep).TargetElement;  
        }  
  
        // construct the step description by concatenating the type and the fully qualified  
        // name of the associated element.  
        string stepTypeName = domStep.GetType().Name;  
        if (element != null)  
        {  
            string elementName = GetElementName(element);  
  
            stepDescription = string.Format(CultureInfo.InvariantCulture, "{0} {1}",  
                stepTypeName, elementName);  
        }  
        else  
        {  
            // if the step has no associated element, just use the step type as the description  
            stepDescription = stepTypeName;  
        }  
    }  
  
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
  
    ```  
  
2.  保存对 SqlRestartableScriptContributor.cs 的更改。  
  
接下来，你将生成类库。  
  
#### <a name="to-sign-and-build-the-assembly"></a>生成程序集并对其进行签名  
  
1.  在“项目”菜单上，单击“MyOtherDeploymentContributor 属性”。  
  
2.  单击“签名”  选项卡。  
  
3.  单击“对程序集签名” 。  
  
4.  在“选择强名称密钥文件”中，单击 **<New>** 。  
  
5.  在“创建强名称密钥”  对话框的“密钥文件名称” 中，键入“MyRefKey” 。  
  
6.  （可选）可以为强名称密钥文件指定密码。  
  
7.  单击“确定”。  
  
8.  在“文件”  菜单上，单击“全部保存” 。  
  
9. 在“生成”菜单中，单击“生成解决方案”。  
  
    接下来，您必须安装程序集，以便在部署 SQL 项目时加载该程序集。  
  
## <a name="install-a-deployment-contributor"></a><a name="InstallDeploymentContributor"></a>安装部署参与者  
若要安装部署参与者，您必须将程序集与关联的 .pdb 文件复制到 Extensions 文件夹。  
  
#### <a name="to-install-the-myotherdeploymentcontributor-assembly"></a>安装 MyOtherDeploymentContributor 程序集  
  
1.  接下来，您要将程序集信息复制到 Extensions 目录中。 Visual Studio 在启动后将识别 %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions 目录和子目录中的任何扩展文件，并使其可供使用。  
  
2.  将 MyOtherDeploymentContributor.dll 程序集文件从输出目录复制到 %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions 目录。 默认情况下，已编译的 .dll 文件的路径为 YourSolutionPath\YourProjectPath\bin\Debug 或 YourSolutionPath\YourProjectPath\bin\Release。  
  
## <a name="run-or-test-your-deployment-contributor"></a><a name="TestDeploymentContributor"></a>运行或测试部署参与者  
若要运行或测试部署参与者，您必须执行以下任务：  
  
-   向计划生成的 .sqlproj 文件添加属性。  
  
-   通过使用 MSBuild 并提供适当的参数来部署数据库项目。  
  
### <a name="add-properties-to-the-sql-project-sqlproj-file"></a>向 SQL 项目 (.sqlproj) 文件添加属性  
您必须始终更新 SQL 项目文件以指定要运行的参与者的 ID。 可以通过两种方法执行此操作：  
  
1.  您可以手动修改 .sqlproj 文件来添加必需的参数。 如果您的参与者不具有配置所需的任何参与者参数或者您不打算跨大量项目重用该生成参与者，则可以选择执行此操作。 如果您选择该选项，请将以下语句添加到 .sqlproj 文件中的第一个 Import 节点的后面：  
  
    ```  
    <PropertyGroup>  
      <DeploymentContributors>  
        $(DeploymentContributors); MyOtherDeploymentContributor.RestartableScriptContributor  
      </DeploymentContributors>  
    </PropertyGroup>  
    ```  
  
2.  第二种方法是创建包含所需的参与者参数的目标文件。 如果您要对多个项目使用相同的参与者并且您具有必需的参与者参数，则此方法会很有用，因为它将包含默认值。 在此情况下，请在 MSBuild 扩展路径中创建目标文件：  
  
    1.  导航到 %Program Files%\MSBuild。  
  
    2.  创建一个将存储目标文件的新文件夹“MyContributors”。  
  
    3.  在该目录中创建一个新文件“MyContributors.targets”，将下列文本添加到该文件中并保存该文件：  
  
        ```  
        <?xml version="1.0" encoding="utf-8"?>  
  
        <Project xmlns="https://schemas.microsoft.com/developer/msbuild/2003">  
          <PropertyGroup>  
            <DeploymentContributors>$(DeploymentContributors);MyOtherDeploymentContributor.RestartableScriptContributor</DeploymentContributors>  
          </PropertyGroup>  
        </Project>  
  
        ```  
  
    4.  在要运行参与者的任何项目的 .sqlproj 文件中，通过在 .sqlproj 文件中的 \<Import Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets" \/> 节点之后添加以下语句来导入目标文件：  
  
        ```  
        <Import Project="$(MSBuildExtensionsPath)\MyContributors\MyContributors.targets " />  
  
        ```  
  
在使用了这些方法之一后，您可以使用 MSBuild 来传入命令行生成的参数。  
  
> [!NOTE]  
> 您必须始终更新“DeploymentContributors”属性以指定您的参与者 ID。 此 ID 与参与者源文件中的“ExportDeploymentPlanModifier”属性中使用的 ID 相同。 如果没有此 ID，你的参与者在生成项目时将不会运行。 仅在具有运行参与者所需的参数时需要更新“ContributorArguments”属性。  
  
## <a name="deploy-the-database-project"></a>部署数据库项目  
  
#### <a name="to-deploy-your-sql-project-and-generate-a-deployment-report"></a>部署 SQL 项目并生成部署报告  
  
-   可以像在 Visual Studio 中一样正常发布或部署您的项目。 只需打开包含 SQL 项目的解决方案，并从该项目的右键单击上下文菜单中选择“发布…”选项，或使用 F5 以对 LocalDB 进行调试部署即可。 在此示例中，我们将使用“发布…”对话框来生成部署脚本。  
  
    1.  打开 Visual Studio 并打开包含您的 SQL 项目的解决方案。  
  
    2.  在解决方案资源管理器中右键单击项目，并选择“发布…”选项。  
  
    3.  设置要发布到的服务器和数据库的名称。  
  
    4.  从对话框底部的选项中选择“生成脚本”。 这将创建可用于部署的脚本。 我们将检查此脚本以验证是否已添加 IF 语句以使脚本可重新启动。  
  
    5.  查看生成的部署脚本。 在标记为“预先部署脚本模板”的节的前面，您将看到类似于下列 Transact-SQL 语法的内容：  
  
        ```  
        :setvar CompletedBatches __completedBatches_CompareProjectDB_cd1e348a-8f92-44e0-9a96-d25d65900fca  
        :setvar TotalBatchCount 17  
        GO  
  
        if OBJECT_ID(N'tempdb.dbo.$(CompletedBatches)', N'U') is null  
        begin  
        use tempdb  
        create table [dbo].[$(CompletedBatches)]  
        (  
        BatchId int primary key,  
        Description nvarchar(300)  
        )  
        use [$(DatabaseName)]  
        end  
  
        ```  
  
        随后，在部署脚本中，您将在每个批处理的周围看到一个环绕原始语句的 IF 语句。 例如，可能为 CREATE SCHEMA 语句显示以下内容：  
  
        ```  
        IF NOT EXISTS (SELECT 1  
                       FROM   [tempdb].[dbo].[$(CompletedBatches)]  
                       WHERE  [BatchId] = 0)  
            BEGIN  
                EXECUTE sp_executesql @stmt = N'CREATE SCHEMA [Sales]  
            AUTHORIZATION [dbo]';  
                SET NOCOUNT ON;  
                INSERT  [tempdb].[dbo].[$(CompletedBatches)] (BatchId, Description)  
                VALUES                                      (0, N'CreateElementStep Sales batch 0');  
                SET NOCOUNT OFF;  
            END  
  
        ```  
  
        请注意，CREATE SCHEMA 是必须包含在 IF 语句中的 EXECUTE sp_executesql 语句中的语句之一。 CREATE TABLE 等语句不需要 EXECUTE sp_executesql 语句，并且将与下面的示例类似：  
  
        ```  
        IF NOT EXISTS (SELECT 1  
                       FROM   [tempdb].[dbo].[$(CompletedBatches)]  
                       WHERE  [BatchId] = 1)  
            BEGIN  
                CREATE TABLE [Sales].[Customer] (  
                    [CustomerID]   INT           IDENTITY (1, 1) NOT NULL,  
                    [CustomerName] NVARCHAR (40) NOT NULL,  
                    [YTDOrders]    INT           NOT NULL,  
                    [YTDSales]     INT           NOT NULL  
                );  
                SET NOCOUNT ON;  
                INSERT  [tempdb].[dbo].[$(CompletedBatches)] (BatchId, Description)  
                VALUES                                      (1, N'CreateElementStep Sales.Customer batch 0');  
                SET NOCOUNT OFF;  
            END  
  
        ```  
  
        > [!NOTE]  
        > 如果您部署的数据库项目与目标数据库相同，则结果报告将没有太大的用处。 若要获得更有用的结果，请将更改部署到数据库或部署新的数据库。  
  
## <a name="command-line-deployment-using-generated-dacpac-file"></a>使用生成的 dacpac 文件进行的命令行部署  
在生成 SQL 项目之后，将创建一个 dacpac 文件，该文件可用于从命令行部署架构并且可支持从其他计算机（如生成计算机）进行部署。 SqlPackage 是一个命令行实用工具，它支持通过各种选项部署 dacpacs，用户可使用这些选项部署 dacpac 或生成部署脚本以及执行其他操作。 有关详细信息，请参阅 [SqlPackage.exe](../tools/sqlpackage/sqlpackage.md)。  
  
> [!NOTE]  
> 若要使用定义的 DeploymentContributors 属性成功部署从项目生成的 dacpacs，则必须在将使用的计算机上安装包含部署参与者的 DLL。 这是因为这些 DLL 已标记为成功完成部署所需的项。  
>   
> 若要避开此要求，请从 .sqlproj 文件中排除部署参与者。 相反，请将 SqlPackage 与 AdditionalDeploymentContributors 参数结合使用来指定参与者在部署期间运行。 这在您仅希望在特定情况下（如部署到特定服务器）使用参与者时会很有用。  
  
## <a name="next-steps"></a>后续步骤  
您可以在执行部署计划前尝试对该计划进行其他类型的修改。 您可能需要进行的其他类型的修改包括：  
  
-   向与某个版本号关联的所有数据库对象添加扩展属性。  
  
-   在部署脚本中添加或删除其他诊断输出语句或注释。  
  
## <a name="see-also"></a>另请参阅  
[使用生成参与者和部署参与者来自定义数据库生成和部署](../ssdt/use-deployment-contributors-to-customize-database-build-and-deployment.md)  
[演练：扩展数据库项目生成以生成模型统计信息](../ssdt/walkthrough-extend-database-project-build-to-generate-model-statistics.md)  
[演练：扩展数据库项目部署以分析部署计划](../ssdt/walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan.md)  
