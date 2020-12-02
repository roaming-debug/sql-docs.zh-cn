---
description: 以编程方式连接任务
title: 以编程方式连接任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- tasks [Integration Services], precedence constraints
- precedence constraints [Integration Services], connecting tasks
- constraints [Integration Services]
ms.assetid: 23668e88-cef4-4009-a9cf-38e607eab7a2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8fbe6beb6556db9e5dfddc6f63e6922a34ae6fe5
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123796"
---
# <a name="connecting-tasks-programmatically"></a>以编程方式连接任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  优先约束在对象模型中由 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> 类表示，它确立 <xref:Microsoft.SqlServer.Dts.Runtime.Executable> 对象在包中的运行顺序。 优先约束允许包中容器和任务的执行取决于前一任务或容器的执行结果。 优先约束是通过调用容器对象中的 <xref:Microsoft.SqlServer.Dts.Runtime.Executable> 集合的 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraints.Add%2A> 方法，在成对的 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraints> 对象之间建立的。 在两个可执行对象之间创建约束后，设置 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Value%2A> 属性可建立约束中定义的第二个可执行对象的执行条件。  
  
 在一个优先约束中既可以使用约束，又可以使用表达式，具体取决于为 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.EvalOp%2A> 属性指定的值，如下表中所述：  
  
|EvalOp 属性的值|说明|  
|----------------------------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DTSPrecedenceEvalOp.Constraint>|指定执行结果确定受约束的容器或任务是否运行。 请将 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Value%2A> 的 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> 属性设置为 <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> 枚举中的所需值。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DTSPrecedenceEvalOp.Expression>|指定表达式的值确定受约束的容器或任务是否运行。 请设置 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Expression%2A> 的 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> 属性。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DTSPrecedenceEvalOp.ExpressionAndConstraint>|指定约束结果必须发生且表达式必须计算，受约束的容器或任务才能运行。 设置 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> 的 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Value%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Expression%2A> 属性，并将其 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.LogicalAnd%2A> 属性设置为“true”。|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DTSPrecedenceEvalOp.ExpressionOrConstraint>|指定约束结果必须发生，或者表达式必须计算，受约束的容器或任务才能运行。 设置 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> 的 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Value%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.Expression%2A> 属性，并将其 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint.LogicalAnd%2A> 属性设置为“false”。|  
  
 下面的代码示例演示将两个任务添加到包中。 在它们之间创建 <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint>，以防止第二个任务在第一个任务完成之前运行。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
  
      // Add a File System task.  
      Executable eFileTask1 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileHost1 = eFileTask1 as TaskHost;  
  
      // Add a second File System task.  
      Executable eFileTask2 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileHost2 = eFileTask2 as TaskHost;  
  
      // Put a precedence constraint between the tasks.  
      // Set the constraint to specify that the second File System task cannot run  
      // until the first File System task finishes.  
      PrecedenceConstraint pcFileTasks =   
        p.PrecedenceConstraints.Add((Executable)thFileHost1, (Executable)thFileHost2);  
      pcFileTasks.Value = DTSExecResult.Completion;  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    ' Add a File System task.  
    Dim eFileTask1 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileHost1 As TaskHost = CType(eFileTask1, TaskHost)  
  
    ' Add a second File System task.  
    Dim eFileTask2 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileHost2 As TaskHost = CType(eFileTask2, TaskHost)  
  
    ' Put a precedence constraint between the tasks.  
    ' Set the constraint to specify that the second File System task cannot run  
    ' until the first File System task finishes.  
    Dim pcFileTasks As PrecedenceConstraint = _  
      p.PrecedenceConstraints.Add(CType(thFileHost1, Executable), CType(thFileHost2, Executable))  
    pcFileTasks.Value = DTSExecResult.Completion  
  
  End Sub  
  
End Module  
```
  
## <a name="see-also"></a>另请参阅  
 [以编程方式添加数据流任务](../../integration-services/building-packages-programmatically/adding-the-data-flow-task-programmatically.md)  
  
  
