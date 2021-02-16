---
description: 以编程方式使用变量
title: 以编程方式使用变量 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- System namespace
- scope [Integration Services]
- variables [Integration Services], programmatically
- configuration files [Integration Services]
- Variables collection
- User namespace
- custom variables [Integration Services]
- variables [Integration Services], customizing
ms.assetid: c4b76a3d-94ca-4a8e-bb45-cb8bd0ea3ec1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8258d74d7f81b17d1e3e7973011a036031bd78da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355012"
---
# <a name="working-with-variables-programmatically"></a>以编程方式使用变量

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  变量提供一种在包、容器、任务和事件处理程序中动态设置值和控制进程的方式。 优先约束还可使用变量来控制数据流流向不同任务的方向。 变量具有多种用途：  
  
-   在运行时更新包的属性。  
  
-   在运行时填充 Transact-SQL 语句的参数值。  
  
-   控制 Foreach 循环流。 有关详细信息，请参阅[将枚举添加到控制流](../control-flow/foreach-loop-container.md)。  
  
-   通过在表达式中使用优先约束对其进行控制。 优先约束可在约束定义中包含变量。 有关详细信息，请参阅 [将表达式添加到优先约束](../control-flow/precedence-constraints.md)。  
  
-   控制 For 循环容器的条件重复。 有关详细信息，请参阅[将迭代添加到控制流](../control-flow/for-loop-container.md)。  
  
-   生成包含变量值的表达式。  
  
-   可以为所有容器类型创建自定义变量，这些容器类型包括：包、Foreach 循环容器、For 循环容器、Sequence 容器、TaskHost 和事件处理程序。 有关详细信息，请参阅 [Integration Services (SSIS) 变量](../../integration-services/integration-services-ssis-variables.md)和[在包中使用变量](../integration-services-ssis-variables.md)。  
  
## <a name="scope"></a>范围  
 每个容器都有自己的 <xref:Microsoft.SqlServer.Dts.Runtime.Variables> 集合。 创建新变量时，新变量的作用域在其父容器的作用域内。 由于包容器位于容器层次结构的顶部，所以包作用域内的变量所起作用类似于全局变量，并且这些变量对包中的所有容器都可见。 容器的变量集合还可由容器的子容器通过 <xref:Microsoft.SqlServer.Dts.Runtime.Variables> 集合进行访问，方法是使用集合中的变量名称或变量的索引。  
  
 由于变量可见性的作用域是自上而下，所以在包级别声明的变量对包中的所有容器都可见。 因此，容器的 <xref:Microsoft.SqlServer.Dts.Runtime.Variables> 集合除了包含自己的变量，还包含所有属于其父容器的变量。  
  
 相反，包含在任务中的变量在作用域和可见性方面都会受到限制，并且仅对该任务可见。  
  
 如果包运行其他包，则在调用包作用域内定义的变量将对被调用的包可用。 唯一的例外是被调用的包中存在同名变量的情况。 发生此类冲突时，被调用的包中的变量值将会重写调用包的值。 在被调用的包的作用域中定义的变量永远不对调用包可用。  
  
 下面的代码示例以编程方式创建一个对包作用域的变量 `myCustomVar`，然后遍历所有对包可见的变量，并打印这些变量的名称、数据类型和值。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Application app = new Application();  
      // Load a sample package that contains a variable that sets the file name.  
      Package pkg = app.LoadPackage(  
        @"C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" +  
        @"\Package Samples\CaptureDataLineage Sample\CaptureDataLineage\CaptureDataLineage.dtsx",  
        null);  
      Variables pkgVars = pkg.Variables;  
      Variable myVar = pkg.Variables.Add("myCustomVar", false, "User", "3");  
      foreach (Variable pkgVar in pkgVars)  
      {  
        Console.WriteLine("Variable: {0}, {1}, {2}", pkgVar.Name,  
          pkgVar.DataType, pkgVar.Value.ToString());  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim app As Application = New Application()  
    ' Load a sample package that contains a variable that sets the file name.  
    Dim pkg As Package = app.LoadPackage( _  
      "C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" & _  
      "\Package Samples\CaptureDataLineage Sample\CaptureDataLineage\CaptureDataLineage.dtsx", _  
      Nothing)  
    Dim pkgVars As Variables = pkg.Variables  
    Dim myVar As Variable = pkg.Variables.Add("myCustomVar", False, "User", "3")  
    Dim pkgVar As Variable  
    For Each pkgVar In pkgVars  
      Console.WriteLine("Variable: {0}, {1}, {2}", pkgVar.Name, _  
        pkgVar.DataType, pkgVar.Value.ToString())  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **示例输出：**  
  
 `Variable: CancelEvent, Int32, 0`  
  
 `Variable: CreationDate, DateTime, 4/18/2003 11:57:00 AM`  
  
 `Variable: CreatorComputerName, String,`  
  
 `Variable: CreatorName, String,`  
  
 `Variable: ExecutionInstanceGUID, String, {237AB5A4-7E59-4FC9-8D61-E8F20363DF25}`  
  
 `Variable: FileName, String, Junk`  
  
 `Variable: InteractiveMode, Boolean, False`  
  
 `Variable: LocaleID, Int32, 1033`  
  
 `Variable: MachineName, String, MYCOMPUTERNAME`  
  
 `Variable: myCustomVar, String, 3`  
  
 `Variable: OfflineMode, Boolean, False`  
  
 `Variable: PackageID, String, {F0D2E396-A6A5-42AE-9467-04CE946A810C}`  
  
 `Variable: PackageName, String, DTSPackage1`  
  
 `Variable: StartTime, DateTime, 1/28/2005 7:55:39 AM`  
  
 `Variable: UserName, String, <domain>\<userid>`  
  
 `Variable: VersionBuild, Int32, 198`  
  
 `Variable: VersionComments, String,`  
  
 `Variable: VersionGUID, String, {90E105B4-B4AF-4263-9CBD-C2050C2D6148}`  
  
 `Variable: VersionMajor, Int32, 1`  
  
 `Variable: VersionMinor, Int32, 0`  
  
 请注意，所有作用域在系统命名空间内的变量都可用于该包。 有关详细信息，请参阅 [System Variables](../../integration-services/system-variables.md)。  
  
## <a name="namespaces"></a>命名空间  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ([!INCLUDE[ssIS](../../includes/ssis-md.md)]) 提供了两个供变量驻留的默认命名空间；User 和 System 命名空间。 默认情况下，开发人员所创建的任何自定义变量都会添加到用户命名空间中。 系统变量驻留在系统命名空间中。 可以创建除用户命名空间以外的其他命名空间以保存自定义变量，可以更改用户命名空间的名称，但是不能在系统命名空间中添加变量或修改其中的变量，也不能向不同的命名空间分配系统变量。  
  
 可用的系统变量互有差异，具体取决于容器类型。 有关包、容器、任务和事件处理器可用的系统变量的列表，请参阅[系统变量](../../integration-services/system-variables.md)。  
  
## <a name="value"></a>值  
 自定义变量的值可以是文字或表达式：  
  
-   如果希望变量包含文字值，请设置其 <xref:Microsoft.SqlServer.Dts.Runtime.Variable.Value%2A> 属性的值。  
  
-   如果希望变量包含表达式，以便可以将表达式的结果用作变量的值，请将变量的 <xref:Microsoft.SqlServer.Dts.Runtime.Variable.EvaluateAsExpression%2A> 属性设置为 true，并在 <xref:Microsoft.SqlServer.Dts.Runtime.Variable.Expression%2A> 属性中提供一个表达式。 在运行时计算表达式，并将表达式的结果用作变量的值。 例如，如果变量的表达式属性为 `"100 * 2""100 * 2"`，则变量的计算结果将为 200。  
  
 对于变量，您不能显式设置其 <xref:Microsoft.SqlServer.Dts.Runtime.Variable.DataType%2A> 的值。 <xref:Microsoft.SqlServer.Dts.Runtime.Variable.DataType%2A> 值是从分配给变量的初始值推断得出的，之后将不可更改。 有关变量数据类型的详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
 下面的代码示例创建一个新变量，将 <xref:Microsoft.SqlServer.Dts.Runtime.Variable.EvaluateAsExpression%2A> 设置为 true，并向该变量的表达式属性分配表达式 `"100 * 2"`，然后输出该变量的值。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package pkg = new Package();  
      Variable v100 = pkg.Variables.Add("myVar", false, "", 1);  
      v100.EvaluateAsExpression = true;  
      v100.Expression = "100 * 2";  
      Console.WriteLine("Expression for myVar: {0}",   
        v100.Properties["Expression"].GetValue(v100));  
      Console.WriteLine("Value of myVar: {0}", v100.Value.ToString());  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim pkg As Package = New Package  
    Dim v100 As Variable = pkg.Variables.Add("myVar", False, "", 1)  
    v100.EvaluateAsExpression = True  
    v100.Expression = "100 * 2"  
    Console.WriteLine("Expression for myVar: {0}", _  
      v100.Properties("Expression").GetValue(v100))  
    Console.WriteLine("Value of myVar: {0}", v100.Value.ToString)  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **示例输出：**  
  
 `Expression for myVar: 100 * 2`  
  
 `Value of myVar: 200`  
  
 表达式必须是使用 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 表达式语法的有效表达式。 除了表达式语法提供的运算符和函数，变量表达式中还可以使用文字，但表达式不能引用其他变量或列。 有关详细信息，请参阅 [Integration Services (SSIS) 表达式](../../integration-services/expressions/integration-services-ssis-expressions.md)。  
  
## <a name="configuration-files"></a>配置文件  
 如果配置文件中包含自定义变量，则该变量可在运行时更新。 这意味着，当包运行时，配置文件中的新值会替换包中的原始变量值。 将包部署到需要不同变量值的多个服务器时，此替换技术将会很有用。 例如，变量可指定 Foreach 循环容器重复其工作流的次数、列出引发错误时接收事件处理程序发送的电子邮件的收件人、或者更改包失败前可能发生的错误的数量。 对于每种环境，这些变量都以编程方式在配置文件中动态提供。 因此，配置文件中只允许读/写变量。 有关详细信息，请参阅 [创建包配置](../packages/legacy-package-deployment-ssis.md)。  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services (SSIS) 变量](../../integration-services/integration-services-ssis-variables.md)   
 [在包中使用变量](../integration-services-ssis-variables.md)  
  
