---
description: 事务 - AlwaysOn 可用性组和数据库镜像
title: 事务：可用性组和数据库镜像
descripton: Learn about cross-database and distributed transaction support for SQL Server Always On availability groups and database mirroring.
ms.custom: seo-lt-2019
ms.date: 12/11/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- cross-database transactions [SQL Server]
- transactions [database mirroring]
- Availability Groups [SQL Server], interoperability
- troubleshooting [SQL Server], cross-database transactions
ms.assetid: 9f7ed895-ad65-43e3-ba08-00d7bff1456d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 79e04e64a4fc89bb3e745d67b1cdbe929850b6ec
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642951"
---
# <a name="transactions---availability-groups-and-database-mirroring"></a>事务 - AlwaysOn 可用性组和数据库镜像
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

本文介绍了用于 AlwaysOn 可用性组和数据库镜像的跨数据库事务和分布式事务。  

## <a name="support-for-distributed-transactions"></a>对分布式事务的支持

SQL Server 2017 支持用于可用性组中数据库的分布式事务。 包括支持相同 SQL Server 实例上的数据库或不同 SQL Server 实例上的数据库。 为数据库镜像配置的数据库不支持分布式事务。

> [!NOTE]
> [!INCLUDE[SQL Server 2016](../../../includes/sssql15-md.md)] 服务包 2 及更高版本完全支持可用性组中的分布式事务。 
> 
> 在服务包 2 之前的 [!INCLUDE[SQL Server 2016](../../../includes/sssql15-md.md)]!INCLUDE[SQL2016] 版本中，不支持涉及可用性组中的数据库的跨数据库分布式事务（即，使用同一 SQL Server 实例上的数据库的事务）。

若要为分布式事务配置可用性组，请参阅[为分布式事务配置可用性组](configure-availability-group-for-distributed-transactions.md)。

有关详细信息，请参阅：

- [DTC 管理指南](/previous-versions/windows/desktop/ms681291(v=vs.85))
- [DTC 开发人员指南](/previous-versions/windows/desktop/ms679938(v=vs.85))
- [DTC 程序员参考](/previous-versions/windows/desktop/ms686108(v=vs.85))

## <a name="sql-server-2016-sp1-and-before-support-for-cross-database-transactions-within-the-same-sql-server-instance"></a>SQL Server 2016 SP1 及以前版本：支持同一个 SQL Server 实例中的跨数据库事务  

在 SQL Server 2016 SP1 及以前版本中，可用性组不支持同一个 SQL Server 实例中的跨数据库事务。 如果其中一个或两个数据库位于可用性组中，同一 SQL Server 实例无法托管跨数据库事务中的两个数据库。 即使这些数据库属于同一个可用性组，这些限制也适用。  
  
数据库镜像也不支持跨数据库事务。  
  
##  <a name="sql-server-2016-sp1-and-before-support-for-distributed-transactions"></a><a name="dtcsupport"></a> SSQL Server 2016 SP1 及较早版本：支持分布式事务  
当数据库由不同的 SQL Server 实例托管时，可用性组支持分布式事务。 它也适用于 SQL Server 实例和其他兼容 DTC 的服务器之间的分布式事务。  
 
Microsoft 分布式事务处理协调器（MSDTC 或 DTC）是一项 Windows 服务，用于为分布式系统提供事务基础结构。 MSDTC 允许客户端应用程序在一个事务中包含多个数据源，然后跨该事务包含的所有服务器提交该事务。 例如，可以使用 MSDTC 来协调跨不同服务器上多个数据库的事务。

SQL Server 2016 引入了使用分布式事务的功能，且该事务中的一个或多个数据库位于某个可用性组中。 在 SQL Server 2016 之前，可用性组中的数据库不支持分布式事务。 SQL Server 2016 可以为每个数据库注册一个资源管理器。 这项新功能正是分布式事务可以包含可用性组中的数据库的原因。
  
 必须满足以下要求：  
  
-   可用性组必须在 Windows Server 2012 R2 或更高版本上运行。 对于 Windows Server 2012 R2，必须安装 KB3090973 中的更新，网址：[https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973)。  
  
-   必须使用 CREATE AVAILABILITY GROUP 命令和 WITH DTC\_SUPPORT = PER_DB 子句创建可用性组。 当前不可更改现有可用性组。  

- 将加入可用性组的所有 SQL Server 实例都必须为 SQL Server 2016 或更高版本。
 
 ## <a name="non-support-for-distributed-transactions"></a>不支持分布式事务
 不支持分布式事务的特定情况包括：
 
 - 在 SQL Server 2016 SP1 及以前版本中，事务包含的多个数据库位于同一个可用性组中。
 
 - 在 SQL Server 2016 SP1 及以前版本中，每个可用性组中至少有一个数据库，并且其他数据库位于相同的 SQL Server 实例。 
 
 - 其中未通过启用分布式事务创建可用性组。
 
 - 数据库镜像。
 
 > [!IMPORTANT]
 > 确定 DTC 不能为你的环境解决的事务的适当默认结果。  有关如何配置默认结果的信息，请参阅 [in-doubt xact resolution 服务器配置选项](../../../database-engine/configure-windows/in-doubt-xact-resolution-server-configuration-option.md)。
  
## <a name="example-scenario-with-database-mirroring"></a>使用数据库镜像的示例方案  
 以下数据库镜像示例说明了如何可能出现逻辑上的不一致。 在此示例中，应用程序使用跨数据库事务插入两行数据：将其中一行插入镜像数据库 A 中的表，将另一行插入另一个数据库 B 中的表。数据库 A 在具有自动故障转移功能的高安全性模式下进行镜像。 提交事务时，数据库 A 不可用，镜像会话将故障自动转移到数据库 A 的镜像数据库。  
  
 故障转移之后，跨数据库事务可能会在数据库 B 上成功提交，但不可能会在故障转移的数据库中成功提交。 如果在发生故障之前，数据库 A 的原始主体服务器未能将跨数据库事务的日志发送到镜像服务器，则可能会出现这种情况。 故障转移之后，该事务将不存在于新的主体服务器上。 数据库 A 和数据库 B 出现不一致，因为在数据库 B 中插入的数据保持完好无损，而在数据库 A 中插入的数据已经丢失。  
  
 使用 MS DTC 事务时可能会出现类似的情况。 例如，故障转移后，新主体将访问 MS DTC。 但是 MS DTC 不能识别新的主体服务器，因而会终止被认为已在其他数据库中提交了的、所有“准备提交”的事务。  
  
> [!NOTE]  
>  通过本文未批准的方式配合 DTC 使用数据库镜像或配合 DTC 使用可用性组不受支持。  这并不意味着不支持与 DTC 不相关的产品方面；而是不支持因错误使用分布式事务而引发的任何问题。  
  
## <a name="next-steps"></a>后续步骤  
 [Always On 可用性组：互操作性 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)  
  
