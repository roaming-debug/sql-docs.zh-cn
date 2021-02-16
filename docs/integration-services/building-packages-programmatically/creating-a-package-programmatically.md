---
description: 以编程方式创建包
title: 以编程方式创建包 | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- SSIS packages, creating
- Integration Services packages, creating
- packages [Integration Services], creating
- SQL Server Integration Services packages, creating
ms.assetid: e44bcc70-32d3-43e8-a84b-29aef819d5d3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6f7d955978a089a3a9c68420658dcc27ae3f12dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355085"
---
# <a name="creating-a-package-programmatically"></a>以编程方式创建包

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  <xref:Microsoft.SqlServer.Dts.Runtime.Package> 对象是 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 项目解决方案中的所有其他对象的顶级容器。 作为顶级容器，包是第一个创建的对象，并将后续对象添加到该包，然后在包的上下文中执行这些对象。 包本身并不移动或转换数据。 包依靠自己包含的任务执行工作。 任务执行大多数由包执行的工作，并定义包的功能。 只需三行代码即可完成包的创建和执行，但需要向包中添加各种任务和 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 对象以增加其功能。 本节讨论如何以编程方式创建包。 不提供有关如何创建任务或 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 的信息。 有关信息，请参阅后续相关章节。  
  
## <a name="example"></a>示例  
 若要使用 Visual Studio IDE 编写代码，需要对 Microsoft.SqlServer.ManagedDTS.DLL 的引用，以便创建针对 Microsoft.SqlServer.Dts.Runtime 的 `using` 语句（在 Visual Basic .NET 中为 `Imports`）。 下面的代码示例演示如何创建一个空包。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package package;  
      package = new Package();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim package As Package  
    package = New Package  
  
  End Sub  
  
End Module  
```  
  
 若要编译并运行该示例，请在 Visual Studio 中按 F5。 若要使用 C# 编译器 csc.exe 生成代码，并在命令提示符处进行编译，请使用下面的命令和文件引用将 \<filename> 替换为 .cs 或 .vb 文件的名称，并选择 \<outputfilename> 。  
  
 csc /target:library /out: \<outputfilename>.dll \<filename>.cs /r:Microsoft.SqlServer.Managed DTS.dll" /r:System.dll  
  
 若要使用 Visual Basic .NET 编译器 (vbc.exe) 生成代码，并在命令提示符处进行编译，请使用下面的命令和文件引用  。  
  
 vbc /target:library /out: \<outputfilename>.dll \<filename>.vb /r:Microsoft.SqlServer.Managed DTS.dll" /r:System.dll  
  
 还可以通过将存储在磁盘中的现有包加载到文件系统或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中来创建包。 区别是要先创建 <xref:Microsoft.SqlServer.Dts.Runtime.Application> 对象，然后由任一应用程序已重载方法填充包对象：LoadPackage 用于平面文件，LoadFromSQLServer 用于保存到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的包，<xref:Microsoft.SqlServer.Dts.Runtime.Application.LoadFromDtsServer%2A> 用于保存到文件系统的包。 下面的示例从磁盘加载一个现有包，然后查看该包的多个属性。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class ApplicationTests  
  {  
    static void Main(string[] args)  
    {  
      // The variable pkg points to the location of the  
      // ExecuteProcess package sample that was installed with  
      // the SSIS samples.  
      string pkg = @"C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" +  
        @"\Package Samples\ExecuteProcess Sample\ExecuteProcess\UsingExecuteProcess.dtsx";  
  
      Application app = new Application();  
      Package p = app.LoadPackage(pkg, null);  
  
      // Now that the package is loaded, we can query on  
      // its properties.  
      int n = p.Configurations.Count;  
      DtsProperty p2 = p.Properties["VersionGUID"];  
      DTSProtectionLevel pl = p.ProtectionLevel;  
  
      Console.WriteLine("Number of configurations = " + n.ToString());  
      Console.WriteLine("VersionGUID = " + (string)p2.GetValue(p));  
      Console.WriteLine("ProtectionLevel = " + pl.ToString());  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module ApplicationTests  
  
  Sub Main()  
  
    ' The variable pkg points to the location of the  
    ' ExecuteProcess package sample that was installed with  
    ' the SSIS samples.  
    Dim pkg As String = _  
      "C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" & _  
      "\Package Samples\ExecuteProcess Sample\ExecuteProcess\UsingExecuteProcess.dtsx"  
  
    Dim app As Application = New Application()  
    Dim p As Package = app.LoadPackage(pkg, Nothing)  
  
    ' Now that the package is loaded, we can query on  
    ' its properties.  
    Dim n As Integer = p.Configurations.Count  
    Dim p2 As DtsProperty = p.Properties("VersionGUID")  
    Dim pl As DTSProtectionLevel = p.ProtectionLevel  
  
    Console.WriteLine("Number of configurations = " & n.ToString())  
    Console.WriteLine("VersionGUID = " & CType(p2.GetValue(p), String))  
    Console.WriteLine("ProtectionLevel = " & pl.ToString())  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **示例输出：**  
  
 `Number of configurations = 2`  
  
 `VersionGUID = {09016682-89B8-4406-AAC9-AF1E527FF50F}`  
  
 `ProtectionLevel = DontSaveSensitive`  
  
## <a name="external-resources"></a>外部资源  
  
-   blogs.msdn.com 上的博客文章 [API Sample - OleDB source and OleDB destination](https://techcommunity.microsoft.com/t5/sql-server-integration-services/api-sample-oledb-source-and-oledb-destination/ba-p/387553)（API 示例 - OleDB 源和 OleDB 目标）。  
  
-   blogs.msdn.com 上的博客文章 [EzAPI - 为 SQL Server 2012 更新](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ezapi-updated-for-sql-server-2012/ba-p/388042)。  
  
## <a name="see-also"></a>另请参阅  
 [以编程方式添加任务](../../integration-services/building-packages-programmatically/adding-tasks-programmatically.md)  
  
  
