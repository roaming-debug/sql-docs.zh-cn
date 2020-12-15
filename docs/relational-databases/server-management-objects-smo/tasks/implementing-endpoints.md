---
description: 实现端点
title: 实现端点
ms.custom: seo-dt-2019
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- endpoints [SMO]
ms.assetid: f8674dbb-9bc0-488f-9def-e9e0ce1ddf86
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 33827a35c7cf0c2c7a4717c63c002b5d61b34685
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475498"
---
# <a name="implementing-endpoints"></a>实现端点
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  端点是一种可以本机方式侦听请求的服务。 SMO 通过使用 <xref:Microsoft.SqlServer.Management.Smo.Endpoint> 对象支持各种类型的端点。 您可以创建用于处理特定类型负载且使用特定协议的端点服务，方法是创建一个 <xref:Microsoft.SqlServer.Management.Smo.Endpoint> 对象的实例并设置其属性。  
  
 <xref:Microsoft.SqlServer.Management.Smo.Endpoint.EndpointType%2A> 对象的 <xref:Microsoft.SqlServer.Management.Smo.Endpoint> 属性可用于指定下列负载类型之一：  
  
-   数据库镜像  
  
-   SOAP（ [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] 和更低的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本中支持 SOAP 端点）  
  
-   Service Broker  
  
-   [!INCLUDE[tsql](../../../includes/tsql-md.md)]  
  
 <xref:Microsoft.SqlServer.Management.Smo.Endpoint.ProtocolType%2A> 属性还可用于指定以下两种支持的协议：  
  
-   HTTP 协议  
  
-   TCP 协议  
  
 如果指定了负载的类型，则可以通过使用 <xref:Microsoft.SqlServer.Management.Smo.Endpoint.Payload%2A> 对象属性设置实际负载。 <xref:Microsoft.SqlServer.Management.Smo.Payload> 对象属性提供了对指定类型的负载对象的引用，可以修改该负载对象的属性。  
  
 对于 <xref:Microsoft.SqlServer.Management.Smo.DatabaseMirroringPayload> 对象，必须指定镜像角色和是否启用加密。 <xref:Microsoft.SqlServer.Management.Smo.ServiceBrokerPayload> 对象要求提供与消息转发、允许的最大连接数和身份验证模式有关的信息。 <xref:Microsoft.SqlServer.Management.Smo.SoapPayloadMethod.%23ctor%2A> 对象需要设置各种属性，包括用来指定可用于客户端（存储过程和用户定义函数）的 SOAP 负载方法的 <xref:Microsoft.SqlServer.Management.Smo.SoapPayloadMethodCollection.Add%2A> 对象属性。  
  
 与此类似，通过使用 <xref:Microsoft.SqlServer.Management.Smo.Endpoint.Protocol%2A> 对象属性可以设置实际协议，该对象属性引用由 <xref:Microsoft.SqlServer.Management.Smo.Endpoint.ProtocolType%2A> 属性所指定类型的协议对象。 <xref:Microsoft.SqlServer.Management.Smo.HttpProtocol> 对象要求提供受限 IP 地址、端口、网站和身份验证信息的列表。 <xref:Microsoft.SqlServer.Management.Smo.TcpProtocol> 对象也需要受限 IP 地址和端口信息的列表。  
  
 创建端点并完全定义之后，便可授予、撤消及拒绝数据库用户、组、角色和登录名的访问权限。  
  
## <a name="example"></a>示例  
 对于下面的代码示例，您必须选择编程环境、编程模板和编程语言才能创建应用程序。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  
  
## <a name="creating-a-database-mirroring-endpoint-service-in-visual-basic"></a>在 Visual Basic 中创建数据库镜像端点服务  
 代码示例演示了如何在 SMO 中创建数据库镜像端点。 这是创建数据库镜像之前的必要步骤。 请使用 <xref:Microsoft.SqlServer.Management.Smo.Database.IsMirroringEnabled%2A> 和 <xref:Microsoft.SqlServer.Management.Smo.Database> 对象上的其他属性创建数据库镜像。  
  
```VBNET
'Set up a database mirroring endpoint on the server before setting up a database mirror.
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Define an Endpoint object variable for database mirroring.
Dim ep As Endpoint
ep = New Endpoint(srv, "Mirroring_Endpoint")
ep.ProtocolType = ProtocolType.Tcp
ep.EndpointType = EndpointType.DatabaseMirroring
'Specify the protocol ports.
ep.Protocol.Http.SslPort = 5024
ep.Protocol.Tcp.ListenerPort = 6666
'Specify the role of the payload.
ep.Payload.DatabaseMirroring.ServerMirroringRole = ServerMirroringRole.All
'Create the endpoint on the instance of SQL Server.
ep.Create()
'Start the endpoint.
ep.Start()
Console.WriteLine(ep.EndpointState)
``` 
  
## <a name="creating-a-database-mirroring-endpoint-service-in-visual-c"></a>在 Visual C# 中创建数据库镜像端点服务  
 代码示例演示了如何在 SMO 中创建数据库镜像端点。 这是创建数据库镜像之前的必要步骤。 请使用 <xref:Microsoft.SqlServer.Management.Smo.Database.IsMirroringEnabled%2A> 和 <xref:Microsoft.SqlServer.Management.Smo.Database> 对象上的其他属性创建数据库镜像。  
  
```csharp  
{  
            //Set up a database mirroring endpoint on the server before   
        //setting up a database mirror.   
        //Connect to the local, default instance of SQL Server.   
            Server srv = new Server();  
            //Define an Endpoint object variable for database mirroring.   
            Endpoint ep = default(Endpoint);  
            ep = new Endpoint(srv, "Mirroring_Endpoint");  
            ep.ProtocolType = ProtocolType.Tcp;  
            ep.EndpointType = EndpointType.DatabaseMirroring;  
            //Specify the protocol ports.   
            ep.Protocol.Http.SslPort = 5024;  
            ep.Protocol.Tcp.ListenerPort = 6666;  
            //Specify the role of the payload.   
            ep.Payload.DatabaseMirroring.ServerMirroringRole = ServerMirroringRole.All;  
            //Create the endpoint on the instance of SQL Server.   
            ep.Create();  
            //Start the endpoint.   
            ep.Start();  
            Console.WriteLine(ep.EndpointState);  
        }  
```  
  
## <a name="creating-a-database-mirroring-endpoint-service-in-powershell"></a>在 PowerShell 中创建数据库镜像端点服务  
 代码示例演示了如何在 SMO 中创建数据库镜像端点。 这是创建数据库镜像之前的必要步骤。 请使用 <xref:Microsoft.SqlServer.Management.Smo.Database.IsMirroringEnabled%2A> 和 <xref:Microsoft.SqlServer.Management.Smo.Database> 对象上的其他属性创建数据库镜像。  
  
```powershell  
# Set the path context to the local, default instance of SQL Server.  
CD \sql\localhost\  
$srv = get-item default  
  
#Get a new endpoint to congure and add  
$ep = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Endpoint -argumentlist $srv,"Mirroring_Endpoint"  
  
#Set some properties  
$ep.ProtocolType = [Microsoft.SqlServer.Management.SMO.ProtocolType]::Tcp  
$ep.EndpointType = [Microsoft.SqlServer.Management.SMO.EndpointType]::DatabaseMirroring  
$ep.Protocol.Http.SslPort = 5024  
$ep.Protocol.Tcp.ListenerPort = 6666 #inline comment  
$ep.Payload.DatabaseMirroring.ServerMirroringRole = [Microsoft.SqlServer.Management.SMO.ServerMirroringRole]::All  
  
# Create the endpoint on the instance  
$ep.Create()  
  
# Start the endpoint  
$ep.Start()  
  
# Report its state  
$ep.EndpointState;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像终结点 (SQL Server)](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  
