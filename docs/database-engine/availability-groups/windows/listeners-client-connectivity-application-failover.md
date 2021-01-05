---
title: 连接到可用性组侦听器
description: 介绍了如何连接到 AlwaysOn 可用性组侦听程序，包括如何连接到主要副本和只读次要副本、如何使用 TLS/SSL 和 Kerberos。
ms.custom: contperf-fy21q1
ms.date: 02/27/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- read-only routing
- read-intent connections [AlwaysOn Availability Groups]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 76fb3eca-6b08-4610-8d79-64019dd56c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: c0578d5522ca998e7243fd20b77daf8d3705bd85
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878905"
---
# <a name="connect-to-an-always-on-availability-group-listener"></a>连接到 Always On 可用性组侦听器 
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  
[配置可用性组侦听器](create-or-configure-an-availability-group-listener-sql-server.md)后，需要更新连接字符串以连接到 Always On 可用性组侦听器。 这会将来自应用程序的流量自动路由到预期副本，而无需在每次故障转移后手动更新连接字符串。 
  

##  <a name="connect-to-the-primary-replica"></a><a name="ConnectToPrimary"></a> 连接到主要副本  

在连接字符串中指定可用性组侦听器 DNS 名称，可连接到主要副本以进行读写访问。 

例如，若要通过侦听器连接到 SQL Server Management Studio 中的主要副本，请在服务器名称字段中输入侦听器 DNS 名称： 

![在 SSMS 中连接到侦听器](media/listeners-client-connectivity-application-failover/connect-to-listener-in-ssms.png)


在故障转移期间，当主要副本变化时，与侦听器的现有连接将会断开，新的连接将路由到新的主要副本。  

下面是 ADO.NET 提供程序 (System.Data.SqlClient) 的基本连接字符串的一个示例： 
  
```  
Server=tcp: AGListener,1433;Database=MyDB;Integrated Security=SSPI  
```  

可以通过运行以下 Transact-SQL (T-SQL) 命令来验证当前通过侦听器连接到的副本：

```sql
SELECT @@SERVERNAME
```

例如，SQLVM1 是主要副本时： 

![检查副本连接](media/listeners-client-connectivity-application-failover/replica-server-name.png)


仍可使用主要副本或次要副本的实例名称（而不使用可用性组侦听器）直接连接到 SQL Server 的实例。 但是，这样就无法享受将新连接自动路由到当前新主要副本的好处。 此外，还无法享受只读路由的好处，即让用 `read-intent` 指定的连接自动路由到可读次要副本。 

##  <a name="connect-to-a-read-only-replica"></a><a name="ConnectToSecondary"></a> 连接到只读副本 

“只读路由”是指将传入的侦听器连接自动路由到配置为允许只读工作负荷的可读次要副本  。 

如果满足以下条件，则连接将自动路由到只读副本： 
 
-   至少一个次要副本设置为只读访问，并且每个只读次要副本和主要副本都[配置为支持只读路由](configure-read-only-routing-for-an-availability-group-sql-server.md)。 

-   连接字符串引用可用性组中涉及的数据库。 替代方法是连接中使用的登录帐户将该数据库配置为其默认数据库。 有关详细信息，请参阅[此有关如何使用算法处理只读路由的文章](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)。

-   连接字符串引用某一可用性组侦听器，并且将传入连接的应用程序意向设置为只读（例如，使用 ODBC 或 OLEDB 连接字符串或连接特性或属性中的 **Application Intent=ReadOnly** 关键字）。 

在登录期间，应用程序意向属性存储在客户端的会话中，然后 SQL Server 实例将处理该意向，并按照可用性组的配置和辅助副本中目标数据库的当前读写状态来确定执行什么操作。  

例如，若要使用 SQL Server Management Studio 连接到只读副本，请选择“连接到服务器”对话框上的“选项”，选择“附加连接参数”选项卡，然后在文本框中指定 `ApplicationIntent=ReadOnly`   ：

![SSMS 中的只读连接](media/listeners-client-connectivity-application-failover/read-only-intent-in-ssms.png)
  
针对指定只读应用程序意向的 ADO.NET 访问接口 (System.Data.SqlClient) 的连接字符串的一个示例： 

```  
Server=tcp:AGListener;Database=AdventureWorks;Integrated Security=SSPI;ApplicationIntent=ReadOnly  
```  
  
有关详细信息，请参阅[配置对可用性副本的只读访问 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  

## <a name="non-default-port"></a>非默认端口

创建侦听器时，指定一个供侦听器使用的端口。 如果端口是默认端口 1433，则连接到侦听器时不必指定端口号。 但是，如果端口不是 1433，则必须在连接字符串中以 `listenername,port` 格式指定端口，例如：

![使用非默认端口进行连接](media/listeners-client-connectivity-application-failover/specify-port-in-ssms.png)

为侦听器指定非默认端口的 ADO.NET 提供程序 (System.Data.SqlClient) 的连接字符串示例： 

```  
Server=tcp:AGListener,1445;Database=AdventureWorks;Integrated Security=SSPI 
```  

##  <a name="bypass-listeners"></a><a name="BypassAGl"></a> 绕过侦听器  

当可用性组侦听器支持故障转移重定向和只读路由时，客户端连接无需使用这两个功能。 客户端连接还可以直接引用 SQL Server 实例，而不是连接到可用性组侦听器。  
  
对于 SQL Server 实例，无论连接是使用可用性组侦听程序还是使用其他实例终结点登录都无关紧要。  SQL Server 实例将验证目标数据库的状态，并允许或禁止基于可用性组配置和当前实例的数据库状态进行连接。  例如，如果客户端应用程序直接连接到 SQL Server 实例的端口并连接到可用性组中承载的目标数据库，且该目标数据库处于主状态和联机状态，则连接将获得成功。  如果目标数据库处于脱机或过渡状态，将无法连接到数据库。  
  
此外，当从数据库镜像迁移到 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]时，只要只有一个辅助副本存在且该副本禁止用户连接，应用程序就可以指定数据库镜像连接字符串。 

##  <a name="database-mirroring-connection-strings"></a><a name="DbmConnectionString"></a> 数据库镜像连接字符串 
 如果可用性组仅拥有一个次要副本并且使用次要副本的 ALLOW_CONNECTIONS = READ_ONLY 或 ALLOW_CONNECTIONS = NONE 进行配置，则客户端可以通过使用数据库镜像连接字符串连接到主要副本。 在将现有应用程序从数据库镜像迁移到可用性组时，此方法可能会很有用，只要您将可用性组限制为两个可用性副本（一个主副本和一个辅助副本）。 如果添加其他辅助副本，您需要为该可用性组创建一个可用性组侦听器，并更新您的应用程序以使用该可用性组侦听器 DNS 名称。  
  
 在使用数据库镜像连接字符串时，客户端可使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 或用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的 .NET Framework 数据访问接口。 客户端提供的连接字符串必须至少提供一个服务器实例的名称，即“初始伙伴名称”  ，以便标识最初承载您想要连接到的可用性副本的服务器实例。 此外，连接字符串还可以提供另一个服务器实例的名称，即“故障转移伙伴名称”  ，以便标识最初将辅助副本作为故障转移伙伴名称承载的服务器实例。  
  
 有关数据库镜像连接字符串详细信息，请参阅 [将客户端连接到数据库镜像会话 (SQL Server)](../../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md)。  
  
  
##  <a name="multi-subnet-failovers"></a><a name="SupportAgMultiSubnetFailover"></a> 多子网故障转移  
 
 如果使用支持连接字符串中 MultiSubnetFailover 连接选项的客户端库，则可通过将 MultiSubnetFailover 设置为“True”或“Yes”（根据使用的提供程序的语法），优化到不同子网的可用性组故障转移。  
  
> [!NOTE]  
>  对于到可用性组侦听器以及到 SQL Server 故障转移群集实例名称的单个和多子网连接，建议使用此设置。  启用此选项将添加额外的优化，即使对于单子网方案也不例外。  
  
 **MultiSubnetFailover** 连接选项仅使用 TCP 网络协议，并且仅当连接到可用性组侦听程序时且针对任何连接到 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]的虚拟网络名称才支持此选项。  
  
 如下所示是针对实现多子网故障转移的 ADO.NET 访问接口 (System.Data.SqlClient) 连接字符串的一个示例：  
  
```  
Server=tcp:AGListener,1433;Database=AdventureWorks;Integrated Security=SSPI; MultiSubnetFailover=True  
```  
  
 **MultiSubnetFailover** 连接选项应设置为 **True** ，即使可用性组仅跨单子网也不例外。  这允许您预先配置新的客户端以支持进一步跨多个子网，而无需进一步更改客户端连接字符串，还可以优化单子网故障转移的故障转移性能。  在不需要 **MultiSubnetFailover** 连接选项时，它将提供更快进行子网故障转移的优势。  这是因为，客户端驱动程序将尝试为每个与可用性组关联的 IP 地址并行打开 TCP 套接字。  客户端驱动程序将等待第一个 IP 响应成功，一旦成功，就将其用于连接。  
  
##  <a name="listeners--tlsssl-certificates"></a><a name="SSLcertificates"></a> 侦听程序和 TLS/SSL 证书  

在连接到可用性组侦听程序时，如果参与的 SQL Server 实例将 TLS/SSL 证书与会话加密结合使用，连接的客户端驱动程序必须在 TLS/SSL 证书中支持“使用者替代名称”，才能强制执行加密。  SQL Server 驱动程序对于证书“使用者可选名称”的支持计划用于 ADO.NET (SqlClient)、Microsoft JDBC 和 SQL Native Client (SNAC)。  
  
必须为故障转移群集中每个参与的服务器节点配置 X.509 证书，并在证书“使用者可选名称”中设置所有可用性组侦听程序的列表。 

证书值的格式为： 

```  
CN = Server.FQDN  
SAN = Server.FQDN,Listener1.FQDN,Listener2.FQDN
```

例如，你有以下值： 

```
Servername: Win2019   
Instance: SQL2019   
AG: AG2019   
Listener: Listener2019   
Domain: contoso.com  (which is also the FQDN)
```

对于有一个可用性组的 WSFC，证书应同时有服务器和侦听程序的完全限定的域名 (FQDN)： 

```
CN: Win2019.contoso.com
SAN: Win2019.contoso.com, Listener2019.contoso.com 
```

如果使用此配置，连接会在连接到实例 (`WIN2019\SQL2019`) 或侦听程序 (`Listener2019`) 时进行加密。 

有一小部分客户可能也需要将 NetBIOS 添加到 SAN，具体视网络的配置方式而定。 在这种情况下，证书值应为： 

```
CN: Win2019.contoso.com
SAN: Win2019,Win2019.contoso.com,Listener2019,Listener2019.contoso.com
```

如果 WSFC 有三个可用性组侦听程序，如：Listener1、Listener2、Listener3

则证书值应为： 

```
CN: Win2019.contoso.com
SAN: Win2019.contoso.com,Listener1.contoso.com,Listener2.contoso.com,Listener3.contoso.com
```
  
  
##  <a name="listeners-and-kerberos-spns"></a><a name="SPNs"></a> 侦听器和 Kerberos (SPN) 

域管理员必须在 Active Directory 中为每个可用性组侦听器配置服务主体名称 (SPN)，以便为到侦听器的客户端连接启用 Kerberos。 注册 SPN 时，必须使用托管可用性副本的服务器实例的服务帐户。 对于跨所有副本工作的 SPN，必须为承载可用性组的 WSFC 群集中的所有实例使用相同的服务帐户。  
  
 使用 Windows 命令行工具 **setspn** 配置 SPN。  例如，要为一组 SQL Server 实例上承载的名为 `AG1listener.Adventure-Works.com` 的可用性组配置 SPN，所有实例都应被配置为在域帐户 `corp\svclogin2`下运行：  
  
```  
setspn -A MSSQLSvc/AG1listener.Adventure-Works.com:1433 corp\svclogin2  
```  
  
 有关为 SQL Server 手动注册 SPN 的详细信息，请参阅 [为 Kerberos 连接注册服务主体名称](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)。  
  


## <a name="next-steps"></a>后续步骤

成功连接到侦听器后，请考虑将[只读工作负荷](overview-of-always-on-availability-groups-sql-server.md)和[备份](configure-backup-on-availability-replicas-sql-server.md)转移到次要副本以提高性能。 还可以查看各种[可用性组监视策略](monitoring-of-availability-groups-sql-server.md)，以确保可用性组正常运行。 

有关可用性组的详细信息，请参阅 [AlwaysOn 可用性组 &#40;SQL Server&#41; 概述](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。