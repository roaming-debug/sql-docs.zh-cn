---
description: 脚本任务的编码和调试
title: 脚本任务的编码和调试 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Script task [Integration Services], debugging
- SSIS Script task, development environment
- SSIS Script task, debugging
- Script task [Integration Services], development environment
- Script task [Integration Services], coding
- debugging [Integration Services], scripts
- VSTA
- SSIS Script task, coding
ms.assetid: 687c262f-fcab-42e8-92ae-e956f3d92d69
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6f5cc9132e7a8f67d2c4636eeaeaa0cb7db5ca0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351580"
---
# <a name="coding-and-debugging-the-script-task"></a>脚本任务的编码和调试

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  在“脚本任务编辑器”中配置完脚本任务后，即可在脚本任务开发环境中编写自己的自定义代码。  
  
## <a name="script-task-development-environment"></a>脚本任务开发环境  
 脚本任务将 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA) 用作脚本自身的开发环境。  
  
 脚本代码以 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic 或 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C# 编写。 在“脚本任务编辑器”中设置 **ScriptLanguage** 属性可指定脚本语言。 如果您倾向于使用其他编程语言，您可以用您选择的语言开发自定义程序集，然后通过脚本任务中的代码调用其功能。  
  
 您在脚本任务中创建的脚本存储在包定义中。 由于没有单独的脚本文件， 因此，使用脚本任务不会影响包部署。  
  
> [!NOTE]  
>  在设计包和调试脚本时，脚本代码将临时写入项目文件。 在文件中存储敏感信息会埋下安全隐患，因此建议您不要在脚本代码中包含敏感信息，如密码。  
  
 默认情况下，Option Strict 在 IDE 中处于禁用状态  。  
  
## <a name="script-task-project-structure"></a>脚本任务项目结构  
 创建或修改包含在脚本任务中的脚本时，VSTA 会打开一个空的新项目或重新打开现有项目。 创建此 VSTA 项目不会影响包的部署，因为项目保存在包文件内；脚本任务不会创建其他文件。  
  
### <a name="project-items-and-classes-in-the-script-task-project"></a>脚本任务项目中的项目项和类  
 默认情况下，显示在 VSTA 项目资源管理器窗口中的脚本任务项目包含单个项：**ScriptMain**。 **ScriptMain** 项又包含单个类，名称也为 **ScriptMain**。 该类中的代码元素根据您选择的脚本任务编程语言而有所不同：  
  
-   如果脚本任务配置为 [!INCLUDE[vb_orcas_long](../../../includes/vb-orcas-long-md.md)] 编程语言，则 **ScriptMain** 类有一个公共子例程 **Main**。 **ScriptMain.Main** 子例程是运行脚本任务时运行时所调用的方法。  
  
     默认情况下，新脚本的 **Main** 子例程中只有一行代码：`Dts.TaskResult = ScriptResults.Success`。 此代码行通知运行库任务运行成功。 [从脚本任务返回结果](../../../integration-services/extending-packages-scripting/task/returning-results-from-the-script-task.md)中介绍了 **Dts.TaskResult** 属性。  
  
-   如果脚本任务配置为 Visual C# 编程语言，则 **ScriptMain** 类有一个公共方法：**Main**。 此方法在脚本任务运行时调用。  
  
     默认情况下，**Main** 方法包含一行代码：`Dts.TaskResult = (int)ScriptResults.Success`。 此代码行通知运行库任务运行成功。  
  
 **ScriptMain** 项可包含 **ScriptMain** 类以外的类。 这些类仅在它们所在的脚本任务中可用。  
  
 默认情况下，**ScriptMain** 项目项包含以下自动生成的代码。 该代码模板还提供脚本任务的概览以及有关如何检索和操作 SSIS 对象（如变量、事件和连接）的其他信息。  
  
```vb  
' Microsoft SQL Server Integration Services Script Task  
' Write scripts using Microsoft Visual Basic 2008.  
' The ScriptMain is the entry point class of the script.  
  
Imports System  
Imports System.Data  
Imports System.Math  
Imports Microsoft.SqlServer.Dts.Runtime.VSTAProxy  
  
<System.AddIn.AddIn("ScriptMain", Version:="1.0", Publisher:="", Description:="")> _  
Partial Class ScriptMain  
  
Private Sub ScriptMain_Startup(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Startup  
  
End Sub  
  
Private Sub ScriptMain_Shutdown(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Shutdown  
Try  
' Unlock variables from the read-only and read-write variable collection properties  
If (Dts.Variables.Count <> 0) Then  
Dts.Variables.Unlock()  
End If  
Catch ex As Exception  
        End Try  
End Sub  
  
Enum ScriptResults  
Success = DTSExecResult.Success  
Failure = DTSExecResult.Failure  
End Enum  
  
' The execution engine calls this method when the task executes.  
' To access the object model, use the Dts property. Connections, variables, events,  
' and logging features are available as members of the Dts property as shown in the following examples.  
'  
' To reference a variable, call Dts.Variables("MyCaseSensitiveVariableName").Value  
' To post a log entry, call Dts.Log("This is my log text", 999, Nothing)  
' To fire an event, call Dts.Events.FireInformation(99, "test", "hit the help message", "", 0, True)  
'  
' To use the connections collection use something like the following:  
' ConnectionManager cm = Dts.Connections.Add("OLEDB")  
' cm.ConnectionString = "Data Source=localhost;Initial Catalog=AdventureWorks;Provider=SQLNCLI10;Integrated Security=SSPI;Auto Translate=False;"  
'  
' Before returning from this method, set the value of Dts.TaskResult to indicate success or failure.  
'   
' To open Help, press F1.  
  
Public Sub Main()  
'  
' Add your code here  
'  
Dts.TaskResult = ScriptResults.Success  
End Sub  
  
End Class  
```  
  
```csharp  
/*  
   Microsoft SQL Server Integration Services Script Task  
   Write scripts using Microsoft Visual C# 2008.  
   The ScriptMain is the entry point class of the script.  
*/  
  
using System;  
using System.Data;  
using Microsoft.SqlServer.Dts.Runtime.VSTAProxy;  
using System.Windows.Forms;  
  
namespace ST_1bcfdbad36d94f8ba9f23a10375abe53.csproj  
{  
    [System.AddIn.AddIn("ScriptMain", Version = "1.0", Publisher = "", Description = "")]  
    public partial class ScriptMain  
    {  
        private void ScriptMain_Startup(object sender, EventArgs e)  
        {  
  
        }  
  
        private void ScriptMain_Shutdown(object sender, EventArgs e)  
        {  
            try  
            {  
                // Unlock variables from the read-only and read-write variable collection properties  
                if (Dts.Variables.Count != 0)  
                {  
                    Dts.Variables.Unlock();  
                }  
            }  
            catch  
            {  
            }  
        }  
  
        #region VSTA generated code  
        private void InternalStartup()  
        {  
            this.Startup += new System.EventHandler(ScriptMain_Startup);  
            this.Shutdown += new System.EventHandler(ScriptMain_Shutdown);  
        }  
        enum ScriptResults  
        {  
            Success = DTSExecResult.Success,  
            Failure = DTSExecResult.Failure  
        };  
  
        #endregion  
  
        /*  
The execution engine calls this method when the task executes.  
To access the object model, use the Dts property. Connections, variables, events,  
and logging features are available as members of the Dts property as shown in the following examples.  
  
To reference a variable, call Dts.Variables["MyCaseSensitiveVariableName"].Value;  
To post a log entry, call Dts.Log("This is my log text", 999, null);  
To fire an event, call Dts.Events.FireInformation(99, "test", "hit the help message", "", 0, true);  
  
To use the connections collection use something like the following:  
ConnectionManager cm = Dts.Connections.Add("OLEDB");  
cm.ConnectionString = "Data Source=localhost;Initial Catalog=AdventureWorks;Provider=SQLNCLI10;Integrated Security=SSPI;Auto Translate=False;";  
  
Before returning from this method, set the value of Dts.TaskResult to indicate success or failure.  
  
To open Help, press F1.  
*/  
  
        public void Main()  
        {  
            // TODO: Add your code here  
            Dts.TaskResult = (int)ScriptResults.Success;  
        }  
    }  
```  
  
### <a name="additional-project-items-in-the-script-task-project"></a>脚本任务项目中的其他项目项  
 脚本任务项目可包含默认的 **ScriptMain** 项以外的项。 您可以向项目添加类、模块和代码。 您还可以使用文件夹来组织项组。 您添加的所有项在包中都将持久化。  
  
### <a name="references-in-the-script-task-project"></a>脚本任务项目中的引用  
 在“项目资源管理器”中右键单击脚本任务项目，然后单击“添加引用”，可以添加对托管程序集的引用   。 有关详细信息，请参阅[引用脚本解决方案中的其他程序集](../../../integration-services/extending-packages-scripting/referencing-other-assemblies-in-scripting-solutions.md)。  
  
> [!NOTE]  
>  可以在“类视图”或“项目资源管理器”中查看 VSTA IDE 中的项目引用   。 这些窗口都可以从“视图”菜单中打开  。 可以从“项目”菜单、“项目资源管理器”或“类视图”添加新引用    。  
  
## <a name="interacting-with-the-package-in-the-script-task"></a>在脚本任务中与包进行交互  
 脚本任务使用全局 **Dts** 对象（<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel> 类的一个实例）及其成员与包含包和 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 运行时进行交互。  
  
 下表列出了 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel> 类的主要公共成员，该类通过全局 **Dts** 对象向脚本任务代码公开。 本节中的主题详细讨论这些成员的使用。  
  
|成员|目的|  
|------------|-------------|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Connections%2A>|提供对包中定义的连接管理器的访问。|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A>|提供事件接口，使脚本任务能够引发错误、警告和信息性消息。|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.ExecutionValue%2A>|提供一种向运行时返回单个对象（除 **TaskResult** 之外）的简单方法，该对象还可用于工作流分支跳转。|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A>|向已启用的日志提供程序记录信息，如任务进度和结果。|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.TaskResult%2A>|报告任务成功或失败。|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Transaction%2A>|提供任务的容器运行于的事务（如果有）。|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A>|提供对 **ReadOnlyVariables** 和 **ReadWriteVariables** 任务属性中列出的、在脚本中使用的变量的访问。|  
  
 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel> 类还包含一些您可能不会使用的公共成员。  
  
|成员|说明|  
|------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A>|使用 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A> 属性可更加方便地访问变量。 虽然您可以使用 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A>，但您必须为读和写显式调用锁定和解锁变量的方法。 使用 <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A> 属性时，脚本任务会为您处理锁定语义。|  
  
## <a name="debugging-the-script-task"></a>调试脚本任务  
 若要调试脚本任务中的代码，请在代码中设置至少一个断点，然后关闭 VSTA IDE 以在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] 中运行包。 当包执行进入脚本任务时，VSTA IDE 会在只读模式下重新打开并显示您的代码。 包执行到达您的断点后，您可以检查变量值，并单步执行剩余代码。  
  
> [!WARNING]  
>  在 64 位模式下运行包时不能调试脚本任务。  
  
> [!NOTE]  
>  您必须执行包，以调试您的脚本任务。 如果您只执行单个任务，则脚本任务代码中的断点将被忽略。  
  
> [!NOTE]  
>  如果将脚本任务作为从执行包任务运行的子包的一部分来执行，则不能调试该脚本任务。 在这些情况下，您在子包中的脚本任务中设置的断点将被忽略。 您可以通过单独运行子包来正常调试子包。  
  
> [!NOTE]  
>  当调试包含多个脚本任务的包时，调试器将调试一个脚本任务。 系统可以在调试器完成调试后调试另一个脚本任务，就像在 Foreach 循环容器或 For 循环容器中那样。  
  
## <a name="external-resources"></a>外部资源  
  
-   blogs.msdn.com 上的博客文章：[VSTA setup and configuration troubles for SSIS 2008 and R2 installations（针对 SSIS 2008 和 R2 安装的 VSTA 安装和配置难题）](/archive/blogs/jason_howell/vsta-setup-and-configuration-troubles-for-ssis-2008-and-r2-installations)。  
  
## <a name="see-also"></a>另请参阅  
 [引用脚本解决方案中的其他程序集](../../../integration-services/extending-packages-scripting/referencing-other-assemblies-in-scripting-solutions.md)   
 [在脚本任务编辑器中配置脚本任务](../../../integration-services/extending-packages-scripting/task/configuring-the-script-task-in-the-script-task-editor.md)  
  
