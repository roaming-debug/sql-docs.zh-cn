---
title: 高可用性，恢复
description: 了解 SQL Server 2012 中添加的 Always On 可用性组 SQL Server Native Client 支持。
ms.custom: ''
ms.date: 04/04/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 2b06186b-4090-4728-b96b-90d6ebd9f66f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 22a707d2a373045666b8eae0ad6f9d48d23c88ba
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462038"
---
# <a name="sql-server-native-client-support-for-high-availability-disaster-recovery"></a>对高可用性、灾难恢复的 SQL Server Native Client 支持
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本主题讨论 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 对于 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]的支持（[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 新增功能）。 有关 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的详细信息，请参阅[可用性组侦听器、客户端连接和应用程序故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)、[创建和配置可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)、[故障转移群集和 AlwaysOn 可用性组 (SQL Server)](~/database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md) 和[活动次要副本：可读次要副本（AlwaysOn 可用性组）](~/database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)。  
  
 您可以在连接字符串中指定给定可用性组的可用性组侦听器。 如果某一 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 应用程序连接到故障转移的可用性组中的某个数据库，则原始连接将被断开，并且应用程序必须打开一个新连接，以便在故障转移后继续工作。  
  
 如果您未连接到某一可用性组侦听器，并且多个 IP 地址与主机名相关联，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将按顺序遍历与 DNS 条目关联的所有 IP 地址。 如果 DNS 服务器返回的第一个 IP 地址未绑定到任何网络接口卡 (NIC)，则上述遍历操作可能会用时较长。 在连接到某一可用性组侦听器时，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将尝试并行建立与所有 IP 地址的连接；如果某一连接尝试成功，则驱动程序将放弃所有挂起的连接尝试。  
  
> [!NOTE]  
>  增大连接超时值和实现连接重试逻辑将增加应用程序连接到可用性组的概率。 此外，由于可用性组进行故障转移而可能使连接失败，您应实现连接重试逻辑，重试失败的连接，直至重新连接。  
  
## <a name="connecting-with-multisubnetfailover"></a>使用 MultiSubnetFailover 进行连接  
 在连接到 SQL Server 2012 可用性组侦听程序或 SQL Server 2012 故障转移群集实例时，应始终指定 **MultiSubnetFailover=Yes**。 **MultiSubnetFailover** 为 SQL Server 2012 中的所有可用性组和故障转移群集实例启用更快的故障转移，并且将显著缩短单个和多子网 Always On 拓扑的故障转移时间。 在多子网故障转移过程中，客户端将尝试并行进行连接。 在子网故障转移过程中，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将会主动重试 TCP 连接。  
  
 **MultiSubnetFailover** 连接属性指示应用程序正部署在某一可用性组或故障转移群集实例中，并且 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 将尝试通过连接到所有 IP 地址来连接到主 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上的数据库。 如果为连接指定的是 MultiSubnetFailover=Yes  ，客户端重试 TCP 连接的时间短于操作系统的默认 TCP 重传间隔。 这样，在 Always On 可用性组或 Always On 故障转移群集实例进行故障转移后，就可以更快地重新连接，并且适用于单个和多子网可用性组和故障转移群集实例。  
  
 有关连接字符串关键字的详细信息，请参阅 [将连接字符串关键字用于 SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
 如果在连接到非可用性组侦听程序或非故障转移群集实例时指定了 **MultiSubnetFailover=Yes**，可能会对性能造成负面影响，因此不支持这样做。  
  
 使用以下准则可以连接到可用性组或故障转移群集实例中的服务器：  
  
-   连接到单子网或多子网时使用 **MultiSubnetFailover** 连接属性，这二者的性能都会得到改进。  
  
-   若要连接到某一可用性组，请在您的连接字符串中将该可用性组的可用性组侦听器指定为服务器。  
  
-   连接到配置有超过 64 个 IP 地址的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例将导致连接失败。  
  
-   基于以下身份验证类型，使用 **MultiSubnetFailover** 连接属性的应用程序的行为不受影响：[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证、Kerberos 身份验证或 Windows 身份验证。  
  
-   你可以增加 **loginTimeout** 的值，以延长故障转移时间并减少应用程序连接重试次数。  
  
-   不支持分布式事务。  
  
 如果只读路由不起作用，则连接到可用性组中的辅助副本位置在以下情况下将失败：  
  
1.  如果未将辅助副本位置配置为接受连接。  
  
2.  如果应用程序使用 **ApplicationIntent=ReadWrite**（在下文中介绍）且将次要副本位置配置为只读访问。  
  
 如果将主副本配置为拒绝只读工作负荷且连接字符串包含 **ApplicationIntent=ReadOnly**，连接将失败。  
  
## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>升级以便使用来自数据库镜像的多子网群集  
 如果连接字符串中已存在 **MultiSubnetFailover** 和 **Failover_Partner** 连接关键字，将出现连接错误。 如果使用 **MultiSubnetFailover** 且 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 返回一个故障转移伙伴响应指示它是数据库镜像对的一部分，也将出现错误。  
  
 如果将当前使用数据库镜像的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 应用程序升级到多子网方案，则应删除 **Failover_Partner** 连接属性并使用设置为 **Yes** 的 **MultiSubnetFailover** 替换它，并且还应使用可用性组侦听程序替换连接字符串中的服务器名称。 如果连接字符串使用 **Failover_Partner** 和 **MultiSubnetFailover=Yes**，驱动程序将生成一个错误。 但是，如果连接字符串使用 **Failover_Partner** 和 **MultiSubnetFailover=No**（或 **ApplicationIntent=ReadWrite**），则该应用程序将使用数据库镜像。  
  
 如果数据库镜像用于可用性组中的主数据库，并且 **MultiSubnetFailover=Yes** 用于连接到主数据库（而非连接到可用性组侦听程序）的连接字符串中，则驱动程序将返回错误。  


[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="odbc"></a>ODBC  
 添加了两个 ODBC 连接字符串关键字以在 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] Native Client 中支持 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]：  
  
-   **ApplicationIntent**  
  
-   **MultiSubnetFailover**  
  
 有关 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 中 ODBC 连接字符串关键字的详细信息，请参阅[将连接字符串关键字用于 SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
 等效的连接属性为：  
  
-   **SQL_COPT_SS_APPLICATION_INTENT**  
  
-   **SQL_COPT_SS_MULTISUBNET_FAILOVER**  
  
 有关 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 中的 ODBC 连接属性的详细信息，请参阅 [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md)。  
  
 从 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 开始，将在使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 驱动程序的 DSN 的 ODBC 数据源管理器中公开 **ApplicationIntent** 和 **MultiSubnetFailover** 关键字的功能。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 应用程序可以使用以下三个函数之一进行连接：  
  
|函数|说明|  
|--------------|-----------------|  
|[SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)|**SQLBrowseConnect** 返回的服务器列表将不包括 VNN。 如果服务器是独立服务器，或是 Windows Server Failover Clustering (WSFC) 群集中的主服务器或辅助服务器，该群集包含已为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 启用的两个或多个 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 实例，您将只能看到不带任何指示信息的服务器列表。 如果连接到服务器失败，可能是因为你已连接到某服务器而 **ApplicationIntent** 设置与该服务器配置不兼容。<br /><br /> 由于 **SQLBrowseConnect** 无法识别 Windows Server Failover Clustering (WSFC) 群集（该群集包含已为 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 启用的两个或多个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例）中的服务器，**SQLBrowseConnect** 将忽略 **MultiSubnetFailover** 连接字符串关键字。|  
|[SQLConnect](../../../relational-databases/native-client-odbc-api/sqlconnect.md)|**SQLConnect** 通过数据源名称 (DSN) 或连接属性同时支持 **ApplicationIntent** 和 **MultiSubnetFailover** 。|  
|[SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)|**SQLDriverConnect** 通过连接字符串关键字、连接属性或 DSN 支持 **ApplicationIntent** 和 **MultiSubnetFailover** 。|  
  
## <a name="ole-db"></a>OLE DB  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 中的 OLE DB 不支持 **MultiSubnetFailover** 关键字。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 中的 OLE DB 将支持应用程序意向。 对于 OLE DB 应用程序，应用程序意向的行为将与 ODBC 应用程序的行为相同（请参阅上文）。  
  
 添加了一个 OLE DB 连接字符串关键字以在 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] Native Client 中支持 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]：  
  
-   **Application Intent**  
  
 有关 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 中连接字符串关键字的详细信息，请参阅[将连接字符串关键字用于 SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
 等效的连接属性为：  
  
-   **SSPROP_INIT_APPLICATIONINTENT**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 应用程序可以使用以下方法之一来指定应用程序意向：  
  
 **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** 使用以前配置的属性集来初始化数据源并创建数据源对象。 将应用程序意向指定为访问接口属性或作为扩展属性字符串的一部分。  
  
 **IDataInitialize::GetDataSource**  
 **IDataInitialize::GetDataSource** 使用可包含 **Application Intent** 关键字的输入连接字符串。  
  
 **IDBProperties::GetProperties**  
 **IDBProperties::GetProperties** 检索当前为数据源设置的属性值。  可以通过 DBPROP_INIT_PROVIDERSTRING 属性和 SSPROP_INIT_APPLICATIONINTENT 属性检索 **Application Intent** 值。  
  
 **IDBProperties::SetProperties**  
 若要设置 **ApplicationIntent** 属性值，请调用在带有“**ReadWrite**”或“**ReadOnly**”值的 **SSPROP_INIT_APPLICATIONINTENT** 属性或含有“**ApplicationIntent=ReadOnly**”或“**ApplicationIntent=ReadWrite**”值的 **DBPROP_INIT_PROVIDERSTRING** 属性中传递的 **IDBProperties::SetProperties**。  
  
 可以在“数据链接属性”  对话框的“全部”选项卡的“应用程序意向属性”字段中指定应用程序意向。  
  
 建立隐式连接时，隐式连接将使用父连接的应用程序意向设置。 同样，从同一数据源创建的多个会话将继承数据源的应用程序意向设置。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client 功能](../../../relational-databases/native-client/features/sql-server-native-client-features.md)   
 [将连接字符串关键字用于 SQL Server 本机客户端](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
  
  
