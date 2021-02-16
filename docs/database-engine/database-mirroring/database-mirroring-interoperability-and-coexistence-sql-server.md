---
title: 数据库镜像：互操作性和共存
description: 了解 SQL Server 数据库镜像及其他 SQL Server 功能（如全文目录和数据库快照）的互操作性和共存性。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- high availability [SQL Server], interoperability and coexistence
- Database Engine [SQL Server], high availability
ms.assetid: 89fef397-e0cf-4e08-b598-25b8d4170523
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 5bd20c8ef36e676ab02430bc0da9d44f2de34768
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353159"
---
# <a name="database-mirroring-interoperability-and-coexistence-sql-server"></a>数据库镜像：互操作性和共存 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  数据库镜像可以与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的下列功能或组件一起使用：  
  
-   [AlwaysOn 故障转移群集实例（SQL Server 故障转移群集）](../../database-engine/database-mirroring/database-mirroring-and-sql-server-failover-cluster-instances.md)  
  
-   [变更数据捕获（和更改跟踪）](../../relational-databases/track-changes/change-data-capture-and-other-sql-server-features.md)  
  
-   [数据库快照](../../database-engine/database-mirroring/database-mirroring-and-database-snapshots-sql-server.md)  
  
-   [全文目录](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md)  
  
-   [日志传送](../../database-engine/database-mirroring/database-mirroring-and-log-shipping-sql-server.md)  
  
-   [复制](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)  
  
 数据库镜像与以下功能不能共存：  
  
-   跨数据库事务/分布式事务  
  
     有关为何不支持此类事务的信息，请参阅[用于 Always On 可用性组和数据库镜像的跨数据库事务和分布式事务 (SQL Server)](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)。  
  
-   [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
