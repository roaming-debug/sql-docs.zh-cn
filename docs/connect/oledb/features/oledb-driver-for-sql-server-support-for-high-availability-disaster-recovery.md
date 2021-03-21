---
title: 适用于 SQL Server 的 OLE DB 驱动程序对高可用性和灾难恢复的支持
description: 了解在连接到配置了高可用性和灾难恢复的数据库时 OLE DB Driver for SQL Server 对这些功能的支持。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 819e81cd1d1b09d866901b54cc02341ef3e200cf
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742567"
---
# <a name="ole-db-driver-for-sql-server-support-for-high-availability-disaster-recovery"></a>适用于 SQL Server 的 OLE DB 驱动程序对高可用性和灾难恢复的支持
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  本文介绍 OLE DB Driver for SQL Server  对 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的支持。 有关 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的详细信息，请参阅[可用性组侦听器、客户端连接和应用程序故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)、[创建和配置可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)、[故障转移群集和 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md) 和[活动次要副本：可读次要副本（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)。  
  
 您可以在连接字符串中指定给定可用性组的可用性组侦听器。 如果某一 OLE DB Driver for SQL Server 应用程序连接到进行故障转移的可用性组中的某个数据库，原始连接则将被断开，并且应用程序必须打开一个新的连接才能在故障转移后继续工作。  
  
 如果未连接到某一可用性组侦听程序，并且如果多个 IP 地址与一个主机名相关联，OLE DB Driver for SQL Server 则将按顺序循环访问与 DNS 条目相关联的所有 IP 地址。 如果 DNS 服务器返回的第一个 IP 地址未绑定到任何网络接口卡 (NIC)，则上述遍历操作可能会用时较长。 在连接到可用性组侦听程序时，OLE DB Driver for SQL Server 将尝试建立与所有 IP 地址的并行连接，如果连接尝试成功，此驱动程序将放弃所有挂起的连接尝试。  
  
> [!NOTE]  
> 增大连接超时值和实现连接重试逻辑将增加应用程序连接到可用性组的概率。 此外，由于可用性组进行故障转移而可能使连接失败，您应实现连接重试逻辑，重试失败的连接，直至重新连接。  
  
## <a name="connecting-with-multisubnetfailover"></a>使用 MultiSubnetFailover 进行连接  
 在连接到 SQL Server AlwaysOn 可用性组侦听程序或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例时，应始终指定 MultiSubnetFailover=Yes。 MultiSubnetFailover 可加快 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中所有 AlwaysOn 可用性组和故障转移群集实例的故障转移速度，并且将显著缩短单子网和多子网 AlwaysOn 拓扑的故障转移时间。 在多子网故障转移过程中，客户端将尝试并行进行连接。 在子网故障转移过程中，OLE DB Driver for SQL Server 将重试 TCP 连接。  
  
 MultiSubnetFailover 连接属性指示应用程序正部署在某一可用性组或故障转移群集实例中，并且该 OLE DB Driver for SQL Server 将通过试图连接到所有的 IP 地址来尝试连接到主 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上的数据库。 如果为连接指定的是 MultiSubnetFailover=Yes  ，客户端重试 TCP 连接的时间短于操作系统的默认 TCP 重传间隔。 这样，就可以在对 AlwaysOn 可用性组或故障转移群集实例执行故障转移之后更快地进行重新连接，这一点同时适用于单子网和多子网可用性组和故障转移群集实例。  
  
 有关连接字符串关键字的详细信息，请参阅[将连接字符串关键字用于 OLE DB Driver for SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)。  
  
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
  
如果将当前使用数据库镜像的 OLE DB Driver for SQL Server 应用程序升级到多子网方案，则应删除 Failover_Partner 连接属性并替换成设置为“是”的 MultiSubnetFailover，并且还应使用可用性组侦听程序替换连接字符串中的服务器名称    。 如果连接字符串使用 **Failover_Partner** 和 **MultiSubnetFailover=Yes**，驱动程序将生成一个错误。 但是，如果连接字符串使用 **Failover_Partner** 和 **MultiSubnetFailover=No**（或 **ApplicationIntent=ReadWrite**），则该应用程序将使用数据库镜像。  
  
如果数据库镜像用于可用性组中的主数据库，并且 **MultiSubnetFailover=Yes** 用于连接到主数据库（而非连接到可用性组侦听程序）的连接字符串中，则驱动程序将返回错误。  


[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="ole-db"></a>OLE DB  
OLE DB Driver for SQL Server 同时支持 ApplicationIntent  和 MultiSubnetFailover  关键字。   
  
添加了两个 OLE DB 连接字符串关键字，以支持 OLE DB Driver for SQL Server 中的 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]：  
  
-   **ApplicationIntent** 
-   **MultiSubnetFailover**  
  
 有关适用于 SQL Server 的 OLE DB 驱动程序中连接字符串关键字的详细信息，请参阅[结合使用连接字符串关键字与适用于 SQL Server 的 OLE DB 驱动程序](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)。  

### <a name="application-intent"></a>应用程序意向 

等效的连接属性为：  
  
-   **SSPROP_INIT_APPLICATIONINTENT**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  
  
OLE DB Driver for SQL Server 应用程序可以使用以下方法之一来指定应用程序意向：  
  
 -   **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** 使用以前配置的属性集来初始化数据源并创建数据源对象。 将应用程序意向指定为访问接口属性或作为扩展属性字符串的一部分。  
  
 -   **IDataInitialize::GetDataSource**  
 **IDataInitialize::GetDataSource** 使用可包含 **Application Intent** 关键字的输入连接字符串。  
  
 -   **IDBProperties::SetProperties**  
 若要设置 **ApplicationIntent** 属性值，请调用在带有“**ReadWrite**”或“**ReadOnly**”值的 **SSPROP_INIT_APPLICATIONINTENT** 属性或含有“**ApplicationIntent=ReadOnly**”或“**ApplicationIntent=ReadWrite**”值的 **DBPROP_INIT_PROVIDERSTRING** 属性中传递的 **IDBProperties::SetProperties**。  
  
可以在“数据链接属性”  对话框的“全部”选项卡的“应用程序意向属性”字段中指定应用程序意向。  
  
建立隐式连接时，隐式连接将使用父连接的应用程序意向设置。 同样，从同一数据源创建的多个会话将继承数据源的应用程序意向设置。  
  
### <a name="multisubnetfailover"></a>MultiSubnetFailover

等效的连接属性为：  
  
-   **SSPROP_INIT_MULTISUBNETFAILOVER**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  

OLE DB Driver for SQL Server 应用程序可以使用以下方法之一来设置 MultiSubnetFailover 选项：  

 -   **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** 使用以前配置的属性集来初始化数据源并创建数据源对象。 将应用程序意向指定为访问接口属性或作为扩展属性字符串的一部分。  
  
 -   **IDataInitialize::GetDataSource**  
 IDataInitialize::GetDataSource 使用可包含 MultiSubnetFailover 关键字的输入连接字符串   。  

-   **IDBProperties::SetProperties**  
若要设置 MultiSubnetFailover 属性值，请调用 IDBProperties::SetProperties，从而传入值为 VARIANT_TRUE 或 VARIANT_FALSE 的 SSPROP_INIT_MULTISUBNETFAILOVER 属性，或值包含“MultiSubnetFailover=Yes”或“MultiSubnetFailover=No”的 DBPROP_INIT_PROVIDERSTRING 属性。

#### <a name="example"></a>示例

```
DBPROP rgPropMultisubnet;

rgPropMultisubnet.dwPropertyID = SSPROP_INIT_MULTISUBNETFAILOVER;
rgPropMultisubnet.dwOptions = DBPROPOPTIONS_REQUIRED;
rgPropMultisubnet.dwStatus = DBPROPSTATUS_OK;
rgPropMultisubnet.colid = DB_NULLID;
V_VT(&(rgPropMultisubnet.vValue)) = VT_BOOL;
V_BOOL(&(rgPropMultisubnet.vValue)) = VARIANT_TRUE;

DBPROPSET PropSet;

PropSet.rgProperties = &rgPropMultisubnet;
PropSet.cProperties = 1;
PropSet.guidPropertySet = DBPROPSET_SQLSERVERDBINIT;
IDBProperties* pIDBProperties = NULL;
hr = pIDBInitialize->QueryInterface(IID_IDBProperties, (void **)&pIDBProperties);
pIDBProperties->SetProperties(1, &PropSet);
```

## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序功能](../../oledb/features/oledb-driver-for-sql-server-features.md)    
 [将连接字符串关键字与适用于 SQL Server 的 OLE DB 驱动程序结合使用](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)  
  
  
