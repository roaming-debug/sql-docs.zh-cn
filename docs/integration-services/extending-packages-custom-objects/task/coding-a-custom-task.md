---
description: 编写自定义任务代码
title: 编写自定义任务代码 | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- Validate method
- custom tasks [Integration Services], validating
- validation [Integration Services], design-time tasks
- SSIS custom tasks, validating
ms.assetid: dc224f4f-b339-4eb6-a008-1b4fe0ea4fd2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f336a5969e608d1f2647b02679e87da0a65c1bad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348220"
---
# <a name="coding-a-custom-task"></a>编写自定义任务代码

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  创建继承自 [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task) 基类的类并将 <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> 属性应用于该类后，必须重写基类的属性和方法的实现才能提供自定义功能。  
  
## <a name="configuring-the-task"></a>配置任务  
  
### <a name="validating-the-task"></a>验证任务  
 设计 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 包时，可以使用验证来检查每个任务的设置，以便及时发现不正确或不恰当的设置，而不是只在运行时才发现所有错误。 验证的目的是确定任务是否包含将阻止任务成功运行的无效设置或连接。 这可确保包所包含的任务首次运行能够更为顺利。  
  
 在自定义代码中使用 Validate 方法可实现验证。 运行时引擎通过对任务调用 Validate 方法来验证任务。 任务开发人员应负责定义任务验证成功或失败的标准并通知运行时引擎此评估的结果。  
  
#### <a name="task-abstract-base-class"></a>任务抽象基类  
 [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task) 抽象基类提供 Validate 方法，每个任务应重写该方法以定义其验证标准。 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器将在包设计过程中多次自动调用 Validate 方法，并在警告或错误发生时向用户提供可视化提示，以帮助识别任务的配置问题。 任务返回 <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> 枚举的值并引发警告和错误事件，从而提供验证结果。 这些事件包含将在 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器中向用户显示的信息。  
  
 下面是一些验证示例：  
  
-   连接管理器验证特定文件名。  
  
-   连接管理器验证输入类型是否为预期类型，如 XML 文件。  
  
-   要求数据库输入的任务确定自己收不到来自非数据库连接的数据。  
  
-   任务确保自己的所有属性都不会相互冲突。  
  
-   任务确保在执行时使用的所有必需资源都可用。  
  
 在确定验证对象时，有时需要考虑性能因素。 例如，任务的输入可能是一个低带宽或通信量过大的网络连接。 若要验证该资源是否可用，可能需要几秒钟的处理时间。 别的验证可能需要往返于需要量很大的服务器，低验证例程的速度可能就会降低。 虽然许多属性和设置都可以进行验证，但并不是所有属性和设置都应该进行验证。  
  
-   在任务运行前，<xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> 还会调用 Validate 方法中的代码；如果验证失败，<xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> 将取消执行。  
  
#### <a name="user-interface-considerations-during-validation"></a>验证过程中用户界面的注意事项  
 [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task) 包含 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents> 接口作为 Validate 方法的参数。 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents> 接口包含一些方法，任务可调用这些方法以向运行时引擎引发事件。 在验证期间出现警告或错误情况时会调用 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireWarning%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireError%2A> 方法。 这两个警告方法需要相同的参数，包括错误代码、源组件、说明、帮助文件以及帮助上下文信息。 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器使用这些信息在设计图面上显示可视化提示。 设计器提供的可视化提示包含感叹号图标，该图标显示在设计图面上的任务旁。 此可视化提示通知用户：任务需要其他配置才能继续执行。  
  
 感叹号图标还显示一个包含错误消息的工具提示。 错误消息由任务在事件的说明参数中提供。 错误消息还显示在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] 的“任务列表”窗格中，该窗格可供用户集中查看所有验证错误。  
  
#### <a name="validation-example"></a>验证示例  
 下面的代码示例演示一个具有 UserName 属性的任务。 此属性已被指定为验证成功所必需的。 如果未设置此属性，任务将会发布错误，并从 <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Failure> 枚举返回 <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult>。 Validate 方法包装在 try/catch 块中，如果出现异常，验证将失败。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
public class SampleTask : Task  
{  
  private string userName = "";  
  
  public override DTSExecResult Validate(Connections connections,  
     VariableDispenser variableDispenser, IDTSComponentEvents events,  
     IDTSLogging log)  
  {  
    try  
    {  
      if (this.userName == "")  
      {  
        //   Raise an OnError event.  
        events.FireError(0, "SampleTask", "The UserName property must be configured.", "", 0);  
        //   Fail validation.  
        return DTSExecResult.Failure;  
      }  
      //   Return success.  
      return DTSExecResult.Success;  
    }  
    catch (System.Exception exception)  
    {  
      //   Capture exceptions, post an error, and fail validation.  
      events.FireError(0, "Sampletask", exception.Message, "", 0);  
      return DTSExecResult.Failure;  
    }  
  }  
  public string UserName  
  {  
    get  
    {  
      return this.userName;  
    }  
    set  
    {  
      this.userName = value;  
    }  
  }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Public Class SampleTask  
  Inherits Task  
  
  Private _userName As String = ""  
  
  Public Overrides Function Validate(ByVal connections As Connections, _  
     ByVal variableDispenser As VariableDispenser, _  
     ByVal events As IDTSComponentEvents, _  
     ByVal log As IDTSLogging) As DTSExecResult  
  
    Try  
      If Me._userName = "" Then  
        '   Raise an OnError event.  
        events.FireError(0, "SampleTask", "The UserName property must be configured.", "", 0)  
        '   Fail validation.  
        Return DTSExecResult.Failure  
      End If  
      '   Return success.  
      Return DTSExecResult.Success  
    Catch exception As System.Exception  
      '   Capture exceptions, post an error, and fail validation.  
      events.FireError(0, "Sampletask", exception.Message, "", 0)  
      Return DTSExecResult.Failure  
    End Try  
  
  End Function  
  
  Public Property UserName() As String  
    Get  
      Return Me._userName  
    End Get  
    Set(ByVal Value As String)  
      Me._userName = Value  
    End Set  
  End Property  
  
End Class  
```  
  
### <a name="persisting-the-task"></a>使任务持久化  
 通常不需要对任务实现自定义持久性。 自定义持久性仅在对象的属性使用复杂数据类型时才需要。 有关详细信息，请参阅[开发用于 Integration Services 的自定义对象](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)。  
  
## <a name="executing-the-task"></a>执行任务  
 本节介绍如何使用任务继承和重写的 Execute 方法。 本节还介绍用于提供任务执行结果信息的各种方法。  
  
### <a name="execute-method"></a>Execute 方法  
 当 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 运行时调用包中包含的任务的 Execute 方法时，该任务将会运行。 任务在此方法中实现其核心业务逻辑和功能，并通过发布消息、返回 <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> 枚举的值以及重写 ExecutionValue 属性的 get 属性来提供执行结果 。  
  
 [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task) 基类提供了 <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A> 方法的默认实现。 自定义任务重写此方法以定义其运行时功能。 <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> 对象包装任务，从而将该任务与运行时引擎和包中的其他对象隔离。 由于这种隔离，任务不知道它在包中所处的执行顺序位置，所以仅在由运行库调用时才运行。 这种体系结构可防止任务在执行期间修改包时可能发生的问题。 只有通过向任务提供作为 <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A> 方法参数的对象，该任务才能访问包中的其他对象。 这些参数允许任务引发事件、向事件日志写入条目、访问变量集合以及在事务中登记与数据源的连接，同时还保持确保包的稳定性和可靠性所需的隔离。  
  
 下表列出了提供给任务的 <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A> 方法的参数。  
  
|参数|说明|  
|---------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Connections>|包含任务可用的 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 对象的集合。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.VariableDispenser>|包含任务可用的变量。 任务通过 VariableDispenser 使用变量，而不直接使用变量。 变量分配器可锁定变量和解除变量锁定，并防止死锁或覆盖。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents>|包含任务向运行时引擎引发事件而调用的方法。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.IDTSLogging>|包含任务向事件日志写入条目所使用的方法和属性。|  
|对象|包含容器（如果有）所属的事务对象。 此值作为参数传递给 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.AcquireConnection%2A> 对象的 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 方法。|  
  
### <a name="providing-execution-feedback"></a>提供执行反馈  
 任务将其代码包装到 try/catch 块中，以防止向运行时引擎引发异常。 这样可确保包会完成执行，不会意外停止。 但是，运行时引擎提供了其他机制，可处理任务执行期间可能会出现的错误情况。 这些机制包括发布错误和警告信息、从 <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> 结构返回值、发布消息、返回 <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> 值以及通过 <xref:Microsoft.SqlServer.Dts.Runtime.Task.ExecutionValue%2A> 属性提供任务执行结果的信息。  
  
 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents> 接口包含 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireWarning%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireError%2A> 方法，任务可调用这些方法向运行时引擎发布错误和警告消息。 这两个方法需要一些参数，如错误代码、源组件、说明、帮助文件以及帮助上下文信息。 根据任务的配置，运行库会通过引发事件和断点或向事件日志写入信息来响应这些消息。  
  
 <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> 还提供 <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecutionValue%2A> 属性，该属性可用于提供有关执行结果的其他信息。 例如，如果任务要在其 Execute 方法中删除表中的行，则该任务可能会将所删除的行数作为 <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecutionValue%2A> 属性的值返回。 此外，<xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> 还提供 <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecValueVariable%2A> 属性。 此属性允许用户将从任务返回的 <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecutionValue%2A> 映射到任何任务可见的变量。 随后，指定的变量即可用于建立任务之间的优先约束。  
  
### <a name="execution-example"></a>执行示例  
 下面的代码示例演示 Execute 方法的实现，并演示了一个重写的 ExecutionValue 属性 。 该任务删除其 fileName 属性指定的文件。 如果该文件不存在或 fileName 属性为空字符串，任务将发布一条警告。 该任务在 <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.ExecutionValue%2A> 属性中返回一个布尔值，以指示该文件是否已删除。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
public class SampleTask : Task  
{  
  private string fileName = "";  
  private bool fileDeleted = false;  
  
  public override DTSExecResult Execute(Connections cons,  
     VariableDispenser vars, IDTSComponentEvents events,  
     IDTSLogging log, Object txn)  
  {  
    try  
    {  
      if (this.fileName == "")  
      {  
        events.FireWarning(0, "SampleTask", "No file specified.", "", 0);  
        this.fileDeleted = false;  
      }  
      else  
      {  
        if (System.IO.File.Exists(this.fileName))  
        {  
          System.IO.File.Delete(this.fileName);  
          this.fileDeleted = true;  
        }  
        else  
          this.fileDeleted = false;  
      }  
      return DTSExecResult.Success;  
    }  
    catch (System.Exception exception)  
    {  
      //   Capture the exception and post an error.  
      events.FireError(0, "Sampletask", exception.Message, "", 0);  
      return DTSExecResult.Failure;  
    }  
  }  
  public string FileName  
  {  
    get { return this.fileName; }  
    set { this.fileName = value; }  
  }  
  public override object ExecutionValue  
  {  
    get { return this.fileDeleted; }  
  }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Public Class SampleTask  
  Inherits Task  
  
  Private _fileName As String = ""  
  Private _fileDeleted As Boolean = False  
  
  Public Overrides Function Execute(ByVal cons As Connections, _  
     ByVal vars As VariableDispenser, ByVal events As IDTSComponentEvents, _  
     ByVal log As IDTSLogging, ByVal txn As Object) As DTSExecResult  
  
    Try  
      If Me._fileName = "" Then  
        events.FireWarning(0, "SampleTask", "No file specified.", "", 0)  
        Me._fileDeleted = False  
      Else  
        If System.IO.File.Exists(Me._fileName) Then  
          System.IO.File.Delete(Me._fileName)  
          Me._fileDeleted = True  
        Else  
          Me._fileDeleted = False  
        End If  
      End If  
      Return DTSExecResult.Success  
    Catch exception As System.Exception  
      '   Capture the exception and post an error.  
      events.FireError(0, "Sampletask", exception.Message, "", 0)  
      Return DTSExecResult.Failure  
    End Try  
  
  End Function  
  
  Public Property FileName() As String  
    Get  
      Return Me._fileName  
    End Get  
    Set(ByVal Value As String)  
      Me._fileName = Value  
    End Set  
  End Property  
  
  Public Overrides ReadOnly Property ExecutionValue() As Object  
    Get  
      Return Me._fileDeleted  
    End Get  
  End Property  
  
End Class  
```  
  
## <a name="see-also"></a>另请参阅  
 [创建自定义任务](../../../integration-services/extending-packages-custom-objects/task/creating-a-custom-task.md)   
 [编写自定义任务代码](../../../integration-services/extending-packages-custom-objects/task/coding-a-custom-task.md)   
 [为自定义任务开发用户界面](../../../integration-services/extending-packages-custom-objects/task/developing-a-user-interface-for-a-custom-task.md)  
  
  
