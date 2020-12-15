---
description: 连接到 SQL Server 实例
title: 连接到 SQL Server 的实例 |Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- SQL Server Management Objects, connections
- connections [SMO]
- instances of SQL Server, connections
- SMO [SQL Server], connections
ms.assetid: ad3cf354-b2e3-468b-b986-1232e375fd84
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7de50d6040e53bffe18e4e13e8458c55c32ac46c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463048"
---
# <a name="connecting-to-an-instance-of-sql-server"></a>连接到 SQL Server 实例
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]管理对象 (SMO) 应用程序中的第一个编程步骤是创建对象的实例 <xref:Microsoft.SqlServer.Management.Smo.Server> ，并建立与实例的连接 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。  
  
 您可以创建 <xref:Microsoft.SqlServer.Management.Smo.Server> 对象的实例并且以三种方式建立与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例的连接。 第一种方式是使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象变量提供连接信息。 第二种方式是通过显式设置 <xref:Microsoft.SqlServer.Management.Smo.Server> 对象属性，提供连接信息。 第三种方式是将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称传递到 <xref:Microsoft.SqlServer.Management.Smo.Server> 对象构造函数中。 
  
 **使用 ServerConnection 对象**  
  
 使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象变量的优点在于可以重复使用连接信息。 声明一个 <xref:Microsoft.SqlServer.Management.Smo.Server> 对象变量。 然后，声明一个 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象，并且设置与连接信息（例如 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称）和身份验证模式有关的属性。 接下来，将 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象变量作为参数传递到 <xref:Microsoft.SqlServer.Management.Smo.Server> 对象构造函数中。 建议不要同时在不同的服务器对象之间共享连接。 使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection.Copy%2A> 方法可以获取现有连接设置的副本。  
  
 **显式设置服务器对象属性**  
  
 或者，您可以声明 <xref:Microsoft.SqlServer.Management.Smo.Server> 对象变量并调用默认构造函数。 照原样，<xref:Microsoft.SqlServer.Management.Smo.Server> 对象将尝试通过所有默认连接设置连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的默认实例。  
  
 **在服务器对象构造函数中提供 SQL Server 实例名称**  
  
 声明 <xref:Microsoft.SqlServer.Management.Smo.Server> 对象变量并将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例名称作为字符串参数传递到构造函数中。 <xref:Microsoft.SqlServer.Management.Smo.Server> 对象使用默认的连接设置建立与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例的连接。  
  
## <a name="connection-pooling"></a>连接池  
 通常不要求调用 <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Connect%2A> 对象的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 方法。 在需要时，SMO 会自动建立连接，并在操作完成后，将连接发布到连接池。 在调用 <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Connect%2A> 方法时，不释放与池的连接。 若要释放与池的连接，需要显式调用 <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Disconnect%2A> 方法。 此外，您可以通过设置 <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.NonPooledConnection%2A> 对象的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 属性，请求非池的连接。  
  
## <a name="multithreaded-applications"></a>多线程应用程序  
 对于多线程应用程序，每个线程应使用单独的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象。  
  
## <a name="connecting-to-an-instance-of-sql-server-for-rmo"></a>连接到用于 RMO 的 SQL Server 实例  
 复制管理对象 (RMO) 用于连接到复制服务器的方法与 SMO 稍有不同。  
  
 RMO 编程对象要求 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通过使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 由命名空间实现的对象建立与实例的连接。 与服务器的这一连接独立于 RMO 编程对象建立。 然后，在实例创建期间或通过指派该对象的 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性，将它传递到 RMO 对象。 采用这种方式，RMO 编程对象实例和连接对象实例可以分别创建和管理，而多个 RMO 编程对象可以重用一个连接对象。 连接复制服务器时适用下列规则：  
  
-   为指定的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象定义连接的所有属性。  
  
-   与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的每个连接都必须具有自己的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象。  
  
-   执行连接并成功登录服务器所需的所有验证信息由 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象提供。  
  
-   默认情况下，使用 Microsoft Windows 身份验证进行连接。 若要使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证，<xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.LoginSecure%2A> 必须设置为 False，并且 <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.Login%2A> 和 <xref:Microsoft.SqlServer.Management.Common.ConnectionSettings.Password%2A> 必须设置为有效的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名和密码。 安全凭据必须始终以安全的方式存储和处理，并且尽可能在运行时提供。  
  
-   必须在将连接传递到任何 RMO 编程对象前调用 <xref:Microsoft.SqlServer.Management.Common.ConnectionManager.Connect%2A> 方法。  
  
## <a name="examples"></a>示例  
若要使用所提供的任何代码示例，您必须选择创建应用程序所需的编程环境、编程模板和编程语言。 有关详细信息，请参阅  [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  
  
## <a name="connecting-to-the-local-instance-of-sql-server-by-using-windows-authentication-in-visual-basic"></a>通过在 Visual Basic 中使用 Windows 身份验证连接到 SQL Server 的本地实例  
 连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的本地实例并不要求大量代码。 而是依赖于身份验证方法和服务器的默认设置。 要求检索数据的第一个操作将导致创建连接。  
 
此示例 Visual Basic 通过使用 Windows 身份验证连接到 SQL Server 的本地实例的 .NET 代码。 

```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'The connection is established when a property is requested.
Console.WriteLine(srv.Information.Version)
'The connection is automatically disconnected when the Server variable goes out of scope.
```
  
## <a name="connecting-to-the-local-instance-of-sql-server-by-using-windows-authentication-in-visual-c"></a>通过在 Visual C# 中使用 Windows 身份验证连接到 SQL Server 的本地实例  
 连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的本地实例并不要求大量代码。 而是依赖于身份验证方法和服务器的默认设置。 要求检索数据的第一个操作将导致创建连接。  
  
 此示例是 Visual C# .NET 代码，它通过使用 Windows 身份验证连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的本地实例。  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//The connection is established when a property is requested.   
Console.WriteLine(srv.Information.Version);   
}   
//The connection is automatically disconnected when the Server variable goes out of scope.  
```  
  
## <a name="connecting-to-a-remote-instance-of-sql-server-by-using-windows-authentication-in-visual-basic"></a>通过在 Visual Basic 中使用 Windows 身份验证连接到 SQL Server 的远程实例  
 在您通过使用 Windows 身份验证连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例时，不必指定身份验证类型。 默认情况下使用 Windows 身份验证。  
  
 此示例是 [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通过使用 Windows 身份验证连接到远程实例的 .net 代码。 字符串变量 *strServer* 包含远程实例的名称。  
  
```VBNET   
'Connect to a remote instance of SQL Server.
Dim srv As Server
'The strServer string variable contains the name of a remote instance of SQL Server.
srv = New Server(strServer)
'The actual connection is made when a property is retrieved. 
Console.WriteLine(srv.Information.Version)
'The connection is automatically disconnected when the Server variable goes out of scope.
```
  
## <a name="connecting-to-a-remote-instance-of-sql-server-by-using-windows-authentication-in-visual-c"></a>通过在 Visual C# 中使用 Windows 身份验证连接到 SQL Server 的远程实例  
 在您通过使用 Windows 身份验证连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例时，不必指定身份验证类型。 默认情况下使用 Windows 身份验证。  
  
 此示例是 Visual C# .NET 代码，它通过使用 Windows 身份验证连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的远程实例。 字符串变量 *strServer* 包含远程实例的名称。  
  
```csharp  
{   
//Connect to a remote instance of SQL Server.   
Server srv;   
//The strServer string variable contains the name of a remote instance of SQL Server.   
srv = new Server(strServer);   
//The actual connection is made when a property is retrieved.   
Console.WriteLine(srv.Information.Version);   
}   
//The connection is automatically disconnected when the Server variable goes out of scope.  
```  
  
## <a name="connecting-to-an-instance-of-sql-server-by-using-sql-server-authentication-in-visual-basic"></a>通过在 Visual Basic 中使用 SQL Server 身份验证连接到 SQL Server 的实例  
 在您通过使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例时，必须指定身份验证类型。 此示例演示声明 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象变量的替代方法，从而使连接信息可以重复使用。  
  
 示例是 [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] .net 代码，演示如何连接到远程，并且 *vPassword* 包含登录名和密码。  
  
```VBNET  
' compile with:   
' /r:Microsoft.SqlServer.Smo.dll  
' /r:Microsoft.SqlServer.ConnectionInfo.dll  
' /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll   
  
Imports Microsoft.SqlServer.Management.Smo  
Imports Microsoft.SqlServer.Management.Common  
  
Public Class A  
   Public Shared Sub Main()  
      Dim sqlServerLogin As [String] = "user_id"  
      Dim password As [String] = "pwd"  
      Dim instanceName As [String] = "instance_name"  
      Dim remoteSvrName As [String] = "remote_server_name"  
  
      ' Connecting to an instance of SQL Server using SQL Server Authentication  
      Dim srv1 As New Server()   ' connects to default instance  
      srv1.ConnectionContext.LoginSecure = False   ' set to true for Windows Authentication  
      srv1.ConnectionContext.Login = sqlServerLogin  
      srv1.ConnectionContext.Password = password  
      Console.WriteLine(srv1.Information.Version)   ' connection is established  
  
      ' Connecting to a named instance of SQL Server with SQL Server Authentication using ServerConnection  
      Dim srvConn As New ServerConnection()  
      srvConn.ServerInstance = ".\" & instanceName   ' connects to named instance  
      srvConn.LoginSecure = False   ' set to true for Windows Authentication  
      srvConn.Login = sqlServerLogin  
      srvConn.Password = password  
      Dim srv2 As New Server(srvConn)  
      Console.WriteLine(srv2.Information.Version)   ' connection is established  
  
      ' For remote connection, remote server name / ServerInstance needs to be specified  
      Dim srvConn2 As New ServerConnection(remoteSvrName)  
      srvConn2.LoginSecure = False  
      srvConn2.Login = sqlServerLogin  
      srvConn2.Password = password  
      Dim srv3 As New Server(srvConn2)  
      Console.WriteLine(srv3.Information.Version)   ' connection is established  
   End Sub  
End Class  
```  
  
## <a name="connecting-to-an-instance-of-sql-server-by-using-sql-server-authentication-in-visual-c"></a>通过在 Visual C# 中使用 SQL Server 身份验证连接到 SQL Server 的实例  
 在您通过使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例时，必须指定身份验证类型。 此示例演示声明 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象变量的替代方法，从而使连接信息可以重复使用。  
  
 该示例是 Visual c # .NET 代码，演示如何连接到远程，并且 *vPassword* 包含登录名和密码。  
  
```csharp  
// compile with:   
// /r:Microsoft.SqlServer.Smo.dll  
// /r:Microsoft.SqlServer.ConnectionInfo.dll  
// /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll   
  
using System;  
using Microsoft.SqlServer.Management.Smo;  
using Microsoft.SqlServer.Management.Common;  
  
public class A {  
   public static void Main() {   
      String sqlServerLogin = "user_id";  
      String password = "pwd";  
      String instanceName = "instance_name";  
      String remoteSvrName = "remote_server_name";  
  
      // Connecting to an instance of SQL Server using SQL Server Authentication  
      Server srv1 = new Server();   // connects to default instance  
      srv1.ConnectionContext.LoginSecure = false;   // set to true for Windows Authentication  
      srv1.ConnectionContext.Login = sqlServerLogin;  
      srv1.ConnectionContext.Password = password;  
      Console.WriteLine(srv1.Information.Version);   // connection is established  
  
      // Connecting to a named instance of SQL Server with SQL Server Authentication using ServerConnection  
      ServerConnection srvConn = new ServerConnection();  
      srvConn.ServerInstance = @".\" + instanceName;   // connects to named instance  
      srvConn.LoginSecure = false;   // set to true for Windows Authentication  
      srvConn.Login = sqlServerLogin;  
      srvConn.Password = password;  
      Server srv2 = new Server(srvConn);  
      Console.WriteLine(srv2.Information.Version);   // connection is established  
  
      // For remote connection, remote server name / ServerInstance needs to be specified  
      ServerConnection srvConn2 = new ServerConnection(remoteSvrName);  
      srvConn2.LoginSecure = false;  
      srvConn2.Login = sqlServerLogin;  
      srvConn2.Password = password;  
      Server srv3 = new Server(srvConn2);  
      Console.WriteLine(srv3.Information.Version);   // connection is established  
   }  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 <xref:Microsoft.SqlServer.Management.Smo.Server>   
 <xref:Microsoft.SqlServer.Management.Common.ServerConnection>  
  
  
