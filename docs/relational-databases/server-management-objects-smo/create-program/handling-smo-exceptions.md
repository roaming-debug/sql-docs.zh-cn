---
description: 处理 SMO 异常
title: 处理 SMO 异常 |Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- SMO [SQL Server], exceptions
- exceptions [SMO]
- SQL Server Management Objects, exceptions
- inner exceptions [SMO]
ms.assetid: 4c725ff2-6588-44ca-b86a-87979e164153
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 66956ff6d6b8d904ba20cb68ce012194d6069e75
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463028"
---
# <a name="handling-smo-exceptions"></a>处理 SMO 异常
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  在托管代码中，如果出现错误，便会引发异常。 SMO 方法和属性不在返回值中报告成功或失败信息。 相反，可以通过异常处理程序捕获和处理异常。  
  
 SMO 中存在多种不同的异常类。 可以从提供与异常相关的文字信息的异常属性（例如 **Message** 属性）中提取出关于异常的信息。  
  
 异常处理语句是特定于编程语言的。 例如，在 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic 中，该语句为 **Catch** 语句。  
  
## <a name="inner-exceptions"></a>内部异常  
 异常可能是常规异常，也可能是特定异常。 常规异常包含一组特定异常。 可使用几条 **Catch** 语句处理预计可能出现的错误，并使用常规异常处理代码处理其余错误。 通常，异常按照级联顺序发生。 很多情况下，SMO 异常可能是由 SQL 异常导致的。 检测是否为这种情况的方法便是连续使用 **InnerException** 属性确定导致最终顶级异常的原始异常。  
  
> [!NOTE]  
>  **SQLException** 异常在 **SqlClient** 命名空间中声明。  
  
 ![显示级别的关系图](../../../relational-databases/server-management-objects-smo/create-program/media/exception-flow.gif "显示级别的关系图")  
  
 此图显示了异常通过各应用程序层的流程。  
  
## <a name="example"></a>示例  
 若要使用所提供的任何代码示例，您必须选择创建应用程序所需的编程环境、编程模板和编程语言。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。
  
## <a name="catching-an-exception-in-visual-basic"></a>在 Visual Basic 中捕获异常  
 此代码示例演示如何使用 **Try .。。Catch .。。Finally** [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] 语句来捕获 SMO 异常。 所有 SMO 异常的类型均为 SmoException，并且均列出在 SMO 引用中。 显示内部异常的顺序的目的在于揭示错误的根源。 有关详细信息，请参阅 [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] .NET 文档。  
  
```VBNET
'This sample requires the Microsoft.SqlServer.Management.Smo.Agent namespace is included.
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Define an Operator object variable by supplying the parent SQL Agent and the name arguments in the constructor.
'Note that the Operator type requires [] parenthesis to differentiate it from a Visual Basic key word.
Dim op As [Operator]
op = New [Operator](srv.JobServer, "Test_Operator")
op.Create()
'Start exception handling.
Try
    'Create the operator again to cause an SMO exception.
    Dim opx As OperatorCategory
    opx = New OperatorCategory(srv.JobServer, "Test_Operator")
    opx.Create()
    'Catch the SMO exception
Catch smoex As SmoException
    Console.WriteLine("This is an SMO Exception")
    'Display the SMO exception message.
    Console.WriteLine(smoex.Message)
    'Display the sequence of non-SMO exceptions that caused the SMO exception.
    Dim ex As Exception
    ex = smoex.InnerException
    Do While ex.InnerException IsNot (Nothing)
        Console.WriteLine(ex.InnerException.Message)
        ex = ex.InnerException
    Loop
    'Catch other non-SMO exceptions.
Catch ex As Exception
    Console.WriteLine("This is not an SMO exception.")
End Try
``` 
  
## <a name="catching-an-exception-in-visual-c"></a>在 Visual C# 中捕获异常  
 此代码示例演示如何使用 **Try .。。Catch .。。Finally** Visual c # 语句来捕获 SMO 异常。 所有 SMO 异常的类型均为 SmoException，并且均列出在 SMO 引用中。 显示内部异常的顺序的目的在于揭示错误的根源。 有关详细信息，请参阅 Visual C# 文档。  
  
```csharp  
{   
//This sample requires the Microsoft.SqlServer.Management.Smo.Agent namespace to be included.   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Define an Operator object variable by supplying the parent SQL Agent and the name arguments in the constructor.   
//Note that the Operator type requires [] parenthesis to differentiate it from a Visual Basic key word.   
op = new Operator(srv.JobServer, "Test_Operator");   
op.Create();   
//Start exception handling.   
try {   
    //Create the operator again to cause an SMO exception.   
    OperatorCategory opx;   
    opx = new OperatorCategory(srv.JobServer, "Test_Operator");   
    opx.Create();   
}   
//Catch the SMO exception   
catch (SmoException smoex) {   
    Console.WriteLine("This is an SMO Exception");   
   //Display the SMO exception message.   
   Console.WriteLine(smoex.Message);   
   //Display the sequence of non-SMO exceptions that caused the SMO exception.   
   Exception ex;   
   ex = smoex.InnerException;   
   while (!object.ReferenceEquals(ex.InnerException, (null))) {   
      Console.WriteLine(ex.InnerException.Message);   
      ex = ex.InnerException;   
    }   
    }   
   //Catch other non-SMO exceptions.   
   catch (Exception ex) {   
      Console.WriteLine("This is not an SMO exception.");   
}   
}  
```  
  
  
