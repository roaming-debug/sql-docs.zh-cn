---
title: Always On 可用性组：互操作性
description: 介绍可以和不可以与 Always On 可用性组一起正常运行的不同功能。
ms.custom: seodec18
ms.date: 04/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
helpviewer_keywords:
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], interoperability
ms.assetid: daf87f90-2623-42ca-912c-b8f07d210510
author: cawrites
ms.author: chadam
ms.openlocfilehash: fea5426d48b9808612062fccae4cc7c1228a2e1d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343940"
---
# <a name="always-on-availability-groups-interoperability-sql-server"></a>Always On 可用性组：互操作性 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

本主题说明 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 与其他 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能的互操作性。

## <a name="features-that-interoperate-with-always-on-availability-groups"></a><a name="Interop"></a> 可与 AlwaysOn 可用性组互操作的功能

下表列出了可与 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 互操作的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能。  “详细信息”列中的链接表示给定功能的互操作性注意事项。

|Feature|更多信息|
|:------|:---------------|
|更改数据捕获|[复制、更改跟踪、更改数据捕获和 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)|
|更改跟踪|[复制、更改跟踪、更改数据捕获和 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)|
|包含的数据库|[含有 AlwaysOn 可用性组的包含数据库 (SQL Server)](../../../database-engine/availability-groups/windows/contained-databases-with-always-on-availability-groups-sql-server.md)|
|数据库加密|[含有 AlwaysOn 可用性组的加密数据库 (SQL Server)](../../../database-engine/availability-groups/windows/encrypted-databases-with-always-on-availability-groups-sql-server.md)|
|数据库快照|[含有 AlwaysOn 可用性组的数据库快照 (SQL Server)](../../../database-engine/availability-groups/windows/database-snapshots-with-always-on-availability-groups-sql-server.md)|
|FILESTREAM 和 FileTable|[含有 AlwaysOn 可用性组的 FILESTREAM 和 FileTable (SQL Server)](../../../database-engine/availability-groups/windows/filestream-and-filetable-with-always-on-availability-groups-sql-server.md)|
|全文搜索|注意：全文索引已与 AlwaysOn 辅助数据库同步。|
|日志传送|[从日志传送迁移到 AlwaysOn 可用性组的先决条件 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)|
|远程 Blob 存储区 (RBS)|[远程 Blob 存储区 (RBS) 和 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/remote-blob-store-rbs-and-always-on-availability-groups-sql-server.md)|
|复制|[为 AlwaysOn 可用性组配置复制 (SQL Server)](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)<br /><br /> [维护 AlwaysOn 发布数据库 (SQL Server)](../../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)<br /><br /> [复制、更改跟踪、更改数据捕获和 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)<br /><br /> [复制订阅服务器和 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)|
|Analysis Services|[含有 AlwaysOn 可用性组的 Analysis Services](../../../database-engine/availability-groups/windows/analysis-services-with-always-on-availability-groups.md)|
|Reporting Services|使用只读辅助副本作为报告数据源，减少您的读写主副本上的负载。<br /><br /> [含有 AlwaysOn 可用性组的 Reporting Services (SQL Server)](../../../database-engine/availability-groups/windows/reporting-services-with-always-on-availability-groups-sql-server.md)|
|Service Broker|[含有 AlwaysOn 可用性组的 Service Broker (SQL Server)](../../../database-engine/availability-groups/windows/service-broker-with-always-on-availability-groups-sql-server.md)|
|SQL Server 代理|&nbsp;|

## <a name="features-that-interoperate-with-always-on-availability-groups-with-restrictions"></a><a name="restrictions"></a> 可与 AlwaysOn 可用性组互操作，但具有限制的功能

以下功能可与 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 互操作，但存在某些限制。 有关详细信息，请参阅链接的主题。

- 跨数据库事务/分布式事务（[!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] 和 Windows Server 2016）。 有关详细信息，请参阅[用于 AlwaysOn 可用性组和数据库镜像的跨数据库事务和分布式事务 (SQL Server)](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)
- [查询统计信息系统数据收集器](../../../relational-databases/data-collection/system-data-collection-set-reports.md#Query)无法在具有不可读的辅助副本的环境中可靠运行。 若要使用查询统计信息系统数据收集器，请将所有辅助可用性组副本设置为允许[读取访问](configure-read-only-access-on-an-availability-replica-sql-server.md)。 

## <a name="features-that-do-not-interoperate-with-always-on-availability-groups"></a><a name="NoInterop"></a> 不能与 AlwaysOn 可用性组互操作的功能

[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 不能与以下功能互操作：

- 数据库镜像。 有关详细信息，请参阅[用于 AlwaysOn 可用性组和数据库镜像的跨数据库事务和分布式事务 (SQL Server)](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)。

## <a name="related-content"></a><a name="RelatedContent"></a> 相关内容

- **博客：**

  [迁移指南：从之前群集和镜像部署迁移到 SQL Server 2012 故障转移群集和可用性组](/archive/blogs/sqlalwayson/now-available-migration-guide-migrating-to-sql-server-2012-failover-clustering-and-availability-groups-from-prior-clustering-and-mirroring-deployments)
  [SQL Server Always On 团队博客：SQL Server AlwaysOn 团队官方博客](/archive/blogs/sqlalwayson/)
  [CSS SQL Server 工程师博客](/archive/blogs/psssql/)

- **白皮书：**

  [迁移指南：从结合数据库镜像和日志传送的先前部署迁移到 Always On 可用性组](/previous-versions/sql/sql-server-2012/jj635217(v=msdn.10))
  [Microsoft SQL Server Always On 解决方案高可用性和灾难恢复指南](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))
  [Microsoft SQL Server 2012 白皮书](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)
  [SQL Server 客户咨询团队白皮书](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)

## <a name="see-also"></a>另请参阅

[Always On 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
[针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)