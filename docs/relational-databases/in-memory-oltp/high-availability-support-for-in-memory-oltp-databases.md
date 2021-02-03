---
title: 高可用性 - 内存中 OLTP 数据库
description: 包含内存优化表的 SQL Server 数据库（无论是否使用本机编译的存储过程）完全支持用于 Always On 可用性组。
ms.custom: seo-dt-2019
ms.date: 08/31/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 2113a916-3b1e-496c-8650-7f495e492510
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 96686e379f9d10058ebcedffd44405b6c2f8a18a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237576"
---
# <a name="high-availability-support-for-in-memory-oltp-databases"></a>对内存中 OLTP 数据库的高可用性支持
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  包含内存优化表的数据库（无论是否使用本机编译的存储过程）完全支持用于 AlwaysOn 可用性组。  对于包含 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 对象的数据库和不包含这些对象的数据库，在配置和支持上没有任何区别。  

 对主要副本上的内存优化表所做的更改会在重做过程中应用于次要副本上的表。 这样可以快速故障转移到次要副本，因为数据已在内存中。 这些表可用于已针对读取访问配置的辅助副本上的读取查询。  

  
## <a name="always-on-availability-groups-and-in-memory-oltp-databases"></a>AlwaysOn 可用性组和内存中 OLTP 数据库  
 使用 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 组件来配置数据库具有以下好处：  
  
-   **完全集成的体验**   
    你可以使用相同的向导配置包含内存优化表的数据库，该向导具有对同步和异步次要副本的相同级别的支持。 此外，在 SQL Server Management Studio 中使用熟悉的 AlwaysOn 仪表板提供运行状况监视。  
  
-   **可比较的故障转移时间**   
    次要副本维持持久内存优化表的内存中状态。 在发生自动或强制故障转移时，由于不需要恢复，因此故障转移到新的主要副本的时间相当于故障转移到基于磁盘的表的时间。 创建为 SCHEMA_ONLY 的内存优化表在此配置中受支持。 但是，由于未记录对这些表进行的更改，因此辅助副本上的这些表中不存在任何数据。  
  
-   **可读取辅助角色**   
    你可以访问和查询次要副本上的内存优化表（如果已针对读取访问进行配置）。 在 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]中，次要副本上的读取时间戳与主要副本上的读取时间戳紧密同步，这意味着在主要副本上的更改很快在次要副本上变得可见。 这种紧密同步行为不同于 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 内存中 OLTP。  

### <a name="considerations"></a>注意事项

- SQL Server 2019 为内存优化可用性组数据库引入了并行重做。 在 SQL Server 2016 和 2017 中，如果可用性组中的数据库也是内存优化的，则基于磁盘的表不会使用并行重做。 
  
## <a name="failover-clustering-instance-fci-and-in-memory-oltp-databases"></a>故障转移群集实例 (FCI) 和内存中 OLTP 数据库  
 若要在共享存储配置中实现高可用性，则可以使用内存优化表设置带有数据库的故障转移群集实例。 设置 FCI 时，请考虑以下因素：  
  
-   **恢复时间目标**   
    故障转移时间可能会更长，因为内存优化表必须在数据库变为可用之前加载到内存中。  
  
-   **SCHEMA_ONLY 表**   
    请注意，SCHEMA_ONLY 表将在故障转移后为空，并且没有行。 这是由应用程序设计和定义的。 这与你重启带有一个或多个 SCHEMA_ONLY 表的 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 数据库时的行为完全相同。  
  
## <a name="support-for-transaction-replication-in-in-memory-oltp"></a>对内存中 OLTP 中的事务复制的支持  
 充当事务复制订阅服务器的表（不包括对等事务复制）可以配置为内存优化表。 其他复制配置与内存优化表不兼容。  有关详细信息，请参阅 [复制到内存优化表订阅服务器](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md)。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [活动次要副本：可读次要副本（Always On 可用性组）](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [复制到内存优化表订阅服务器](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md)  
  
  
