---
description: 编写自定义连接管理器代码
title: 编写自定义连接管理器代码 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom connection managers [Integration Services], coding
ms.assetid: b12b6778-1f01-4a7d-984d-73f2f7630aa5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 352735b75d99f87b741959b6c53f66937860588f
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123174"
---
# <a name="coding-a-custom-connection-manager"></a>编写自定义连接管理器代码

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  创建继承自 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase> 基类的类并将 <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute> 属性应用于该类后，必须重写基类的属性和方法的实现以提供自定义功能。  
  
 有关自定义连接管理器的示例，请参阅[为自定义连接管理器开发用户界面](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md)。 本主题中演示的代码示例来自 SQL Server 自定义连接管理器示例。  
  
> [!NOTE]  
>  内置于 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 的大多数任务、源和目标都只能与特定类型的内置连接管理器一起工作。 因此，不能使用内置任务和组件测试这些示例。  
  
## <a name="configuring-the-connection-manager"></a>配置连接管理器  
  
### <a name="setting-the-connectionstring-property"></a>设置 ConnectionString 属性  
 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A> 属性是一个重要的属性，并且是自定义连接管理器独有且唯一的属性。 连接管理器使用此属性的值连接到外部数据源。 如果组合多个其他属性（如服务器名称和数据库名称）来创建连接字符串，则可以使用 Helper 函数来组合字符串，方法是使用用户提供的新值来替换连接字符串模板中的某些值。 下面的代码示例演示了 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A> 属性的实现，该属性依赖 Helper 函数来组合字符串。  
  
```vb  
' Default values.  
Private _serverName As String = "(local)"  
Private _databaseName As String = "AdventureWorks"  
Private _connectionString As String = String.Empty  
  
Private Const CONNECTIONSTRING_TEMPLATE As String = _  
  "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=SSPI"  
  
Public Property ServerName() As String  
  Get  
    Return _serverName  
  End Get  
  Set(ByVal value As String)  
    _serverName = value  
  End Set  
End Property  
  
Public Property DatabaseName() As String  
  Get  
    Return _databaseName  
  End Get  
  Set(ByVal value As String)  
    _databaseName = value  
  End Set  
End Property  
  
Public Overrides Property ConnectionString() As String  
  Get  
    UpdateConnectionString()  
    Return _connectionString  
  End Get  
  Set(ByVal value As String)  
    _connectionString = value  
  End Set  
End Property  
  
Private Sub UpdateConnectionString()  
  
  Dim temporaryString As String = CONNECTIONSTRING_TEMPLATE  
  
  If Not String.IsNullOrEmpty(_serverName) Then  
    temporaryString = temporaryString.Replace("<servername>", _serverName)  
  End If  
  If Not String.IsNullOrEmpty(_databaseName) Then  
    temporaryString = temporaryString.Replace("<databasename>", _databaseName)  
  End If  
  
  _connectionString = temporaryString  
  
End Sub  
```  
  
```csharp  
// Default values.  
private string _serverName = "(local)";  
private string _databaseName = "AdventureWorks";  
private string _connectionString = String.Empty;  
  
private const string CONNECTIONSTRING_TEMPLATE = "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=SSPI";  
  
public string ServerName  
{  
  get  
  {  
    return _serverName;  
  }  
  set  
  {  
    _serverName = value;  
  }  
}  
  
public string DatabaseName  
{  
  get  
  {  
    return _databaseName;  
  }  
  set  
  {  
    _databaseName = value;  
  }  
}  
  
public override string ConnectionString  
{  
  get  
  {  
    UpdateConnectionString();  
    return _connectionString;  
  }  
  set  
  {  
    _connectionString = value;  
  }  
}  
  
private void UpdateConnectionString()  
{  
  
  string temporaryString = CONNECTIONSTRING_TEMPLATE;  
  
  if (!String.IsNullOrEmpty(_serverName))  
  {  
    temporaryString = temporaryString.Replace("<servername>", _serverName);  
  }  
  
  if (!String.IsNullOrEmpty(_databaseName))  
  {  
    temporaryString = temporaryString.Replace("<databasename>", _databaseName);  
  }  
  
  _connectionString = temporaryString;  
  
}  
```  
  
### <a name="validating-the-connection-manager"></a>验证连接管理器  
 重写 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.Validate%2A> 方法以确保连接管理器已正确配置。 您至少应验证连接字符串的格式并确保已为所有参数提供值。 在连接管理器从 <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult.Success> 方法返回 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.Validate%2A> 之前，执行不能继续。  
  
 下面的代码示例演示用于确保用户已为连接指定服务器名称的 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.Validate%2A> 的实现。  
  
```vb  
Public Overrides Function Validate(ByVal infoEvents As Microsoft.SqlServer.Dts.Runtime.IDTSInfoEvents) As Microsoft.SqlServer.Dts.Runtime.DTSExecResult  
  
  If String.IsNullOrEmpty(_serverName) Then  
    infoEvents.FireError(0, "SqlConnectionManager", "No server name specified", String.Empty, 0)  
    Return DTSExecResult.Failure  
  Else  
    Return DTSExecResult.Success  
  End If  
  
End Function  
```  
  
```csharp  
public override Microsoft.SqlServer.Dts.Runtime.DTSExecResult Validate(Microsoft.SqlServer.Dts.Runtime.IDTSInfoEvents infoEvents)  
{  
  
  if (String.IsNullOrEmpty(_serverName))  
  {  
    infoEvents.FireError(0, "SqlConnectionManager", "No server name specified", String.Empty, 0);  
    return DTSExecResult.Failure;  
  }  
  else  
  {  
    return DTSExecResult.Success;  
  }  
  
}  
```  
  
### <a name="persisting-the-connection-manager"></a>使连接管理器持久化  
 通常不需要对连接管理器实现自定义持久性。 自定义持久性仅在对象的属性使用复杂数据类型时才需要。 有关详细信息，请参阅[开发用于 Integration Services 的自定义对象](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)。  
  
## <a name="working-with-the-external-data-source"></a>使用外部数据源  
 用于支持与外部数据源连接的方法是自定义连接管理器的最重要方法。 会在设计时和运行时期间的不同时间调用 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A> 和 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ReleaseConnection%2A> 方法。  
  
### <a name="acquiring-the-connection"></a>获取连接  
 您需要决定哪种类型的对象适合由 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A> 方法从自定义连接管理器返回。 例如，文件连接管理器只返回包含路径和文件名的字符串，而 ADO.NET 连接管理器则返回已打开的托管连接对象。 OLE DB 连接管理器返回本机 OLE DB 连接对象，这些对象不能从托管代码使用。 自定义 SQL Server 连接管理器（从中获取本主题中的代码段）返回打开的 SqlConnection 对象。  
  
 连接管理器用户需要事先了解所需的对象类型，以便可以将返回的对象转换为合适的类型并访问其方法和属性。  
  
```vb  
Public Overrides Function AcquireConnection(ByVal txn As Object) As Object  
  
  Dim sqlConnection As New SqlConnection  
  
  UpdateConnectionString()  
  
  With sqlConnection  
    .ConnectionString = _connectionString  
    .Open()  
  End With  
  
  Return sqlConnection  
  
End Function  
```  
  
```csharp  
public override object AcquireConnection(object txn)  
{  
  
  SqlConnection sqlConnection = new SqlConnection();  
  
  UpdateConnectionString();  
  
  {  
    sqlConnection.ConnectionString = _connectionString;  
    sqlConnection.Open();  
  }  
  
  return sqlConnection;  
  
}  
```  
  
### <a name="releasing-the-connection"></a>释放连接  
 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ReleaseConnection%2A> 方法中执行的操作取决于从 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A> 方法返回的对象类型。 如果存在已打开的连接对象，则应将其关闭，并释放该对象所使用的所有资源。 如果 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A> 只返回了一个字符串值，则无需执行任何操作。  
  
```vb  
Public Overrides Sub ReleaseConnection(ByVal connection As Object)  
  
  Dim sqlConnection As SqlConnection  
  
  sqlConnection = DirectCast(connection, SqlConnection)  
  
  If sqlConnection.State <> ConnectionState.Closed Then  
    sqlConnection.Close()  
  End If  
  
End Sub  
```  
  
```csharp  
public override void ReleaseConnection(object connection)  
{  
  SqlConnection sqlConnection;  
  sqlConnection = (SqlConnection)connection;  
  if (sqlConnection.State != ConnectionState.Closed)  
    sqlConnection.Close();  
}  
```  
 
## <a name="see-also"></a>另请参阅  
 [创建自定义连接管理器](../../../integration-services/extending-packages-custom-objects/connection-manager/creating-a-custom-connection-manager.md)   
 [为自定义连接管理器开发用户界面](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md)  
  
  
