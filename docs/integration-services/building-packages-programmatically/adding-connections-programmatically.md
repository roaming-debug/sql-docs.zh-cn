---
description: 以编程方式添加连接
title: 以编程方式添加连接 | Microsoft Docs
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
- connection managers [Integration Services], packages
- Integration Services packages, connections
- connections [Integration Services], packages
- SSIS packages, connections
- packages [Integration Services], connections
- intrinsic properties [Integration Services]
- SQL Server Integration Services packages, connections
- ConnectionManager class
- SSIS connection managers
- adding package connections
ms.assetid: d90716d1-4c65-466c-b82c-4aabbee1e3e5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3d15ad56ceb2568f2071e4a4170e95afac24e886
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344918"
---
# <a name="adding-connections-programmatically"></a>以编程方式添加连接

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 类表示与外部数据源的物理连接。 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 类可将连接的实现细节与运行时隔离。 这可使运行时以一致且可预测的方式与每个连接管理器进行交互。 连接管理器包含一组所有连接通用的常用属性，如 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Name%2A>、<xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ID%2A>、<xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Description%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ConnectionString%2A>。 但是，通常配置连接管理器只需 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ConnectionString%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Name%2A> 属性。 在其他编程范例中，连接类会提供一些方法（如 Open 或 Connect ）以物理方式建立与数据源的连接，而不同的是，运行时引擎会在包运行时管理包的所有连接。  
  
 <xref:Microsoft.SqlServer.Dts.Runtime.Connections> 类是一个已添加到该包并且可在运行时使用的连接管理器的集合。 可以使用该集合的 <xref:Microsoft.SqlServer.Dts.Runtime.Connections.Add%2A> 方法，并提供一个用于指示连接管理器类型的字符串，以向该集合添加更多的连接管理器。 <xref:Microsoft.SqlServer.Dts.Runtime.Connections.Add%2A> 方法返回已添加到该包的 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 实例。  
  
## <a name="intrinsic-properties"></a>内部属性  
 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 类公开一组所有连接通用的属性。 但是，有时您需要访问特定连接类型特有的属性。 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Properties%2A> 类的 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 集合提供对这些属性的访问。 可以使用索引器或属性名称以及 GetValue 方法从集合检索这些属性，并使用 SetValue 方法设置属性的值。 设置基础连接对象属性的属性的另一种方法是：获取该对象的实际实例并直接设置其属性。 若要获取基础连接，请使用连接管理器的 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.InnerObject%2A> 属性。 下面的代码行显示了一行 C# 代码，该代码行创建一个具有基础类 <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.ConnectionManagerAdoNetClass> 的 ADO.NET 连接管理器。  
  
 `ConnectionManagerAdoNetClass cmado = cm.InnerObject as ConnectionManagerAdoNet;`  
  
 这可将托管连接管理器对象转换为其基础连接对象。 如果使用的是 C++，则需要调用 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 对象的 QueryInterface 方法，并请求基础连接对象的接口。  
  
 下表列出了随 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供的连接管理器。 此外，还有 `package.Connections.Add("xxx")` 语句中使用的字符串。 若要获取所有连接管理器的列表，请参阅 [Integration Services (SSIS) 连接](../../integration-services/connection-manager/integration-services-ssis-connections.md)。  
  
|String|“ODBC 源编辑器”|  
|------------|------------------------|  
|"OLEDB"|OLE DB 连接的连接管理器。|  
|"ODBC"|ODBC 连接的连接管理器。|  
|"ADO"|ADO 连接的连接管理器。|  
|"ADO.NET:SQL"|ADO.NET（SQL 数据访问接口）连接的连接管理器。|  
|"ADO.NET:OLEDB"|ADO.NET（OLE DB 数据访问接口）连接的连接管理器。|  
|"FLATFILE"|平面文件连接的连接管理器。|  
|"FILE"|文件连接的连接管理器。|  
|"MULTIFLATFILE"|多平面文件连接的连接管理器。|  
|"MULTIFILE"|多文件连接的连接管理器。|  
|"SQLMOBILE"|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 连接的连接管理器。|  
|"MSOLAP100"|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接的连接管理器。|  
|"FTP"|FTP 连接的连接管理器。|  
|"HTTP"|HTTP 连接的连接管理器。|  
|"MSMQ"|消息队列（也称为 MSMQ）连接的连接管理器。|  
|"SMTP"|SMTP 连接的连接管理器。|  
|"WMI"|Microsoft Windows Management Instrumentation (WMI) 连接的连接管理器。|  
  
 下面的代码示例演示如何向 <xref:Microsoft.SqlServer.Dts.Runtime.Package.Connections%2A> 的 <xref:Microsoft.SqlServer.Dts.Runtime.Package> 集合添加 OLE DB 和 FILE 连接。 然后，该示例还设置了 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.ConnectionString%2A>、<xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Name%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager.Description%2A> 属性。  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      // Create a package, and retrieve its connections.  
      Package pkg = new Package();  
      Connections pkgConns = pkg.Connections;  
  
      // Add an OLE DB connection to the package, using the   
      // method defined in the AddConnection class.  
      CreateConnection myOLEDBConn = new CreateConnection();  
      myOLEDBConn.CreateOLEDBConnection(pkg);  
  
      // View the new connection in the package.  
      Console.WriteLine("Connection description: {0}",  
         pkg.Connections["SSIS Connection Manager for OLE DB"].Description);  
  
      // Add a second connection to the package.  
      CreateConnection myFileConn = new CreateConnection();  
      myFileConn.CreateFileConnection(pkg);  
  
      // View the second connection in the package.  
      Console.WriteLine("Connection description: {0}",  
        pkg.Connections["SSIS Connection Manager for Files"].Description);  
  
      Console.WriteLine();  
      Console.WriteLine("Number of connections in package: {0}", pkg.Connections.Count);  
  
      Console.Read();  
    }  
  }  
  // <summary>  
  // This class contains the definitions for multiple  
  // connection managers.  
  // </summary>  
  public class CreateConnection  
  {  
    // Private data.  
    private ConnectionManager ConMgr;  
  
    // Class definition for OLE DB Provider.  
    public void CreateOLEDBConnection(Package p)  
    {  
      ConMgr = p.Connections.Add("OLEDB");  
      ConMgr.ConnectionString = "Provider=SQLOLEDB.1;" +  
        "Integrated Security=SSPI;Initial Catalog=AdventureWorks;" +  
        "Data Source=(local);";  
      ConMgr.Name = "SSIS Connection Manager for OLE DB";  
      ConMgr.Description = "OLE DB connection to the AdventureWorks database.";  
    }  
    public void CreateFileConnection(Package p)  
    {  
      ConMgr = p.Connections.Add("File");  
      ConMgr.ConnectionString = @"\\<yourserver>\<yourfolder>\books.xml";  
      ConMgr.Name = "SSIS Connection Manager for Files";  
      ConMgr.Description = "Flat File connection";  
    }  
  }  
  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    ' Create a package, and retrieve its connections.  
    Dim pkg As New Package()  
    Dim pkgConns As Connections = pkg.Connections  
  
    ' Add an OLE DB connection to the package, using the   
    ' method defined in the AddConnection class.  
    Dim myOLEDBConn As New CreateConnection()  
    myOLEDBConn.CreateOLEDBConnection(pkg)  
  
    ' View the new connection in the package.  
    Console.WriteLine("Connection description: {0}", _  
      pkg.Connections("SSIS Connection Manager for OLE DB").Description)  
  
    ' Add a second connection to the package.  
    Dim myFileConn As New CreateConnection()  
    myFileConn.CreateFileConnection(pkg)  
  
    ' View the second connection in the package.  
    Console.WriteLine("Connection description: {0}", _  
      pkg.Connections("SSIS Connection Manager for Files").Description)  
  
    Console.WriteLine()  
    Console.WriteLine("Number of connections in package: {0}", pkg.Connections.Count)  
  
    Console.Read()  
  
  End Sub  
  
End Module  
  
' This class contains the definitions for multiple  
' connection managers.  
  
Public Class CreateConnection  
  ' Private data.  
  Private ConMgr As ConnectionManager  
  
  ' Class definition for OLE DB provider.  
  Public Sub CreateOLEDBConnection(ByVal p As Package)  
    ConMgr = p.Connections.Add("OLEDB")  
    ConMgr.ConnectionString = "Provider=SQLOLEDB.1;" & _  
      "Integrated Security=SSPI;Initial Catalog=AdventureWorks;" & _  
      "Data Source=(local);"  
    ConMgr.Name = "SSIS Connection Manager for OLE DB"  
    ConMgr.Description = "OLE DB connection to the AdventureWorks database."  
  End Sub  
  
  Public Sub CreateFileConnection(ByVal p As Package)  
    ConMgr = p.Connections.Add("File")  
    ConMgr.ConnectionString = "\\<yourserver>\<yourfolder>\books.xml"  
    ConMgr.Name = "SSIS Connection Manager for Files"  
    ConMgr.Description = "Flat File connection"  
  End Sub  
  
End Class  
```  
  
 **示例输出：**  
  
 `Connection description: OLE DB connection to the AdventureWorks database.`  
  
 `Connection description: Flat File connection.`  
  
 `Number of connections in package: 2`  
  
## <a name="external-resources"></a>外部资源  
 carlprothman.net 上的技术文章 [Connection Strings](https://go.microsoft.com/fwlink/?LinkId=220743)（连接字符串）。  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services (SSIS) 连接](../../integration-services/connection-manager/integration-services-ssis-connections.md)   
 [创建连接管理器](../connection-manager/integration-services-ssis-connections.md)  
  
