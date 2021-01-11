---
title: SqlClient 中的性能计数器
description: 使用用于 SQL Server 的 Microsoft SqlClient 数据提供程序性能计数器，通过使用 Windows 性能监视器或以编程方式监视应用程序状态及其连接资源。
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 0b121b71-78f8-4ae2-9aa1-0b2e15778e57
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e9d8c2edb88a9ed50b47c761d3af8aec8016065a
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804290"
---
# <a name="performance-counters-in-sqlclient"></a>SqlClient 中的性能计数器

[!INCLUDE[appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

可以使用 <xref:Microsoft.Data.SqlClient> 性能计数器来监视应用程序的状态及其使用的连接资源。 可以使用 Windows 性能监视器来监视性能计数器，或使用 <xref:System.Diagnostics.PerformanceCounter> 命名空间中的 <xref:System.Diagnostics> 类以编程方式访问性能计数器。

## <a name="available-performance-counters"></a>可用的性能计数器

当前有 14 个不同的性能计数器可用于 <xref:Microsoft.Data.SqlClient>，如下表所述。

|性能计数器|说明|  
|-------------------------|-----------------|  
|`HardConnectsPerSecond`|每秒与数据库服务器连接的数量。|  
|`HardDisconnectsPerSecond`|每秒与数据库服务器断开连接的数量。|  
|`NumberOfActiveConnectionPoolGroups`|处于活动状态的唯一连接池组的数量。 此计数器由 AppDomain 中唯一连接字符串的数量控制。|  
|`NumberOfActiveConnectionPools`|连接池的总数。|  
|`NumberOfActiveConnections`|当前正在使用的活动连接的数量。 **注意：** 默认情况下不启用此性能计数器。 若要启用此性能计数器，请参见[激活默认关闭的计数器](#ActivatingOffByDefault)。|  
|`NumberOfFreeConnections`|连接池中可用连接的数量。 **注意：** 默认情况下不启用此性能计数器。 若要启用此性能计数器，请参见[激活默认关闭的计数器](#ActivatingOffByDefault)。|  
|`NumberOfInactiveConnectionPoolGroups`|标记为删除的唯一连接池组的数量。 此计数器由 AppDomain 中唯一连接字符串的数量控制。|  
|`NumberOfInactiveConnectionPools`|最近未进行任何活动且等待被释放的非活动连接池的数量。|  
|`NumberOfNonPooledConnections`|未存入池中的活动连接的数量。|  
|`NumberOfPooledConnections`|由连接池基础结构管理的活动连接的数量。|  
|`NumberOfReclaimedConnections`|通过垃圾回收而回收的连接的数量，其中应用程序未调用 `Close` 或 `Dispose`。 注意：不显式关闭或处理连接会影响性能。|  
|`NumberOfStasisConnections`|当前正在等待完成某项操作并因此无法供应用程序使用的连接的数量。|  
|`SoftConnectsPerSecond`|从连接池中拉出的活动连接的数量。 **注意：** 默认情况下不启用此性能计数器。 若要启用此性能计数器，请参见[激活默认关闭的计数器](#ActivatingOffByDefault)。|  
|`SoftDisconnectsPerSecond`|被返回连接池的活动连接的数量。 **注意：** 默认情况下不启用此性能计数器。 若要启用此性能计数器，请参见[激活默认关闭的计数器](#ActivatingOffByDefault)。|  

### <a name="connection-pool-groups-and-connection-pools"></a>连接池组和连接池

在使用 Windows 身份验证（集成安全性）时，必须监视 `NumberOfActiveConnectionPoolGroups` 和 `NumberOfActiveConnectionPools` 性能计数器。 这样做的原因是连接池组会映射为唯一连接字符串。 在使用集成安全性时，连接池会映射为连接字符串，此外，连接池还会为各个 Windows 标识创建单独的池。 例如，如果 Fred 和 Julie 在同一 AppDomain 中，并且二者都使用连接字符串 `"Data Source=MySqlServer;Integrated Security=true"`，则将为连接字符串创建一个连接池组，还将为 Fred 和 Julie 分别创建一个其他池。 如果 John 和 Martha 将某个连接字符串用于相同的 SQL Server 登录 `"Data Source=MySqlServer;User Id=<myUserID>;Password=<myPassword>"`，则仅会为 <myUserID> 标识创建一个池。

<a name="ActivatingOffByDefault"></a>

### <a name="activate-off-by-default-counters"></a>激活默认关闭的计数器

性能计数器 `NumberOfFreeConnections`、`NumberOfActiveConnections`、`SoftDisconnectsPerSecond` 和 `SoftConnectsPerSecond` 默认情况下为关。 将下面的信息添加到应用程序配置文件中，以启用这些信息：

```xml  
<system.diagnostics>  
  <switches>  
    <add name="ConnectionPoolPerformanceCounterDetail"  
         value="4"/>  
  </switches>  
</system.diagnostics>  
```  

## <a name="retrieve-performance-counter-values"></a>检索性能计数器值

下面的控制台应用程序演示如何在应用程序中检索性能计数器的值。 若要为所有用于 SQL Server 的 Microsoft SqlClient 数据提供程序性能计数器返回信息，连接必须处于打开状态并保持连接。

> [!NOTE]
> 本示例使用 [AdventureWorks 示例数据库](../../samples/adventureworks-install-configure.md)。 示例代码中提供的连接字符串假定本地计算机已安装数据库且可用，并且你创建的登录名与连接字符串中提供的登录名匹配。 如果使用仅允许 Windows 身份验证的默认安全设置来配置服务器，则需要启用 SQL Server 登录。 可以根据您的环境需要修改连接字符串。

### <a name="example"></a>示例

[!code-csharp[SqlClient_PerformanceCounter#1](~/../sqlclient/doc/samples/SqlClient_PerformanceCounter.cs#1)]

## <a name="see-also"></a>另请参阅

- [连接到数据源](connecting-to-data-source.md)
- [运行时分析](/dotnet/framework/debug-trace-profile/runtime-profiling)
- [监视性能阈值简介](/previous-versions/visualstudio/visual-studio-2008/bd20x32d(v=vs.90))
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
