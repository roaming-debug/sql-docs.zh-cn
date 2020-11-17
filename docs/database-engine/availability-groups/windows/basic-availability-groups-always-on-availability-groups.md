---
title: 单一数据库的基本可用性组
description: '介绍常规和基本 Always On 可用性组之间的差异，以及如何配置基本可用性组。 '
ms.custom: seodec18
ms.date: 02/01/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
ms.assetid: 285adbc7-ac9b-40f6-b4a9-3f1591d3b632
author: cawrites
ms.author: chadam
ms.openlocfilehash: 88704561006f0beff14ae69fff2b79c14ef3114b
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584599"
---
# <a name="basic-always-on-availability-groups-for-a-single-database"></a>单一数据库的基本 Always On 可用性组
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  AlwaysOn Basic 可用性组为 SQL Server 2016 和 SQL Server 2017 Standard Edition 提供高可用性解决方案。 基本可用性组支持单个数据库的故障转移环境。 它的创建和管理与传统（高级）的 [AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md) 的 Enterprise Edition 非常类似。 本文档概述了基本可用性组的差异和限制。  
  
## <a name="features"></a>功能  
 AlwaysOn 基本可用性组取代已弃用的数据库镜像功能，并提供类似级别的功能支持。 基本可用性组使主数据库可以维护单一的副本。 此副本可以使用同步提交模式或异步提交模式。 有关可用性模式的详细信息，请参阅[可用性模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)。 次要副本将保持非活动状态，除非需要进行故障转移。 此故障转移会反转主要和次要角色分配，导致次要副本成为活动的主数据库。 有关故障转移的详细信息，请参阅[故障转移和故障转移模式（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)。 基本可用性组可以在跨本地和 Microsoft Azure 的混合环境中运行。  
  
## <a name="limitations"></a>限制  
 相比 SQL Server 2016 Enterprise Edition 上的高级可用性组，基本可用性组仅使用一部分功能。 基本可用性组包括以下限制：  
  
- 两个副本（主要和次要）的限制。 Linux 上的 SQL Server 2017 的基本可用性组支持附加的仅配置副本。
  
- 对次要副本没有读取访问权限。  
  
- 在次要副本上没有备份。  

- 在次要副本上没有完整性检查。 

- 不支持在运行 SQL Server 2016 社区技术预览版 3 (CTP3) 之前的 SQL Server 版本的服务器上托管的副本。  

- 支持一个可用性数据库。  
  
- 基本可用性组不能升级到高级可用性组。 必须删除该组并重新添加到包含仅运行 SQL Server 2016 Enterprise Edition 的服务器的组中。  
  
- 仅 Standard Edition 服务器支持基本可用性组。 

- 基本可用性组不能为分布式可用性组的一部分。 

- 可能有多个基本可用性组连接到一个 SQL Server 实例。

  
## <a name="configuration"></a>配置  
 可以在任意两个 SQL Server 2016 Standard Edition 服务器上创建 AlwaysOn 基本可用性组。 当创建基本可用性组时，必须在创建期间指定两个副本。  
  
 若要创建基本可用性组，请使用 **CREATE AVAILABILITY GROUP** transact-SQL 命令并指定“WITH BASIC”选项（默认值为“ADVANCED”）。 从版本 17.8 开始，还可以使用 SQL Server Management Studio 中的 UI 创建基本可用性组。 有关详细信息，请参阅 [CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md)。 

请参阅以下有关使用 Transact-SQL (T-SQL) 创建基本可用性组的示例： 

```sql
CREATE AVAILABILITY GROUP [BasicAG]
WITH (AUTOMATED_BACKUP_PREFERENCE = PRIMARY,
BASIC,
DB_FAILOVER = OFF,
DTC_SUPPORT = NONE,
REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = 0)
FOR DATABASE [AdventureWorks]
REPLICA ON N'SQLVM1\MSSQLSERVER' WITH (ENDPOINT_URL = N'TCP://SQLVM1.Contoso.com:5022', FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, SEEDING_MODE = AUTOMATIC, SECONDARY_ROLE(ALLOW_CONNECTIONS = NO)),
    N'SQLVM2\MSSQLSERVER' WITH (ENDPOINT_URL = N'TCP://SQLVM2.Contoso.com:5022', FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, SEEDING_MODE = AUTOMATIC, SECONDARY_ROLE(ALLOW_CONNECTIONS = NO));

GO
```

  
> [!NOTE]  
>  在指定“WITH BASIC”  时，基本可用性组的限制适用于 **CREATE AVAILABILITY GROUP** 命令。 例如，如果你尝试创建一个允许读取访问权限的基本可用性组，将收到错误。 其他限制同样适用。 有关详细信息，请参阅本主题的“限制”部分。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
