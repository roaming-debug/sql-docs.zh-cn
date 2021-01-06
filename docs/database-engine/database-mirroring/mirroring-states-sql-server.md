---
title: 镜像状态 (SQL Server) | Microsoft Docs
description: 了解 SQL Server 的数据库镜像会话中的数据库状态。 该状态反映了通信状态、数据流和数据之间的差异。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- states [SQL Server], database mirroring
- PENDING_FAILOVER state
- mirroring states [SQL Server]
- DISCONNECTED state
- SYNCHRONIZING state
- SYNCHRONIZED state
- SUSPENDED state
- database mirroring [SQL Server], states
ms.assetid: 90062917-74f9-471b-b49e-bc153ae1a468
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 7b371edc49168a847c9b9b02c92f1fc1a61425c6
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643657"
---
# <a name="mirroring-states-sql-server"></a>镜像状态 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  数据库镜像会话期间，镜像数据库一直处于一种特定状态（镜像状态）。 数据库的状态反映了通信状态、数据流和伙伴之间数据的差异。 数据库镜像会话采用的状态与主体数据库相同。  
  
 在整个数据库镜像会话期间，服务器实例相互监视。 伙伴使用镜像状态监视数据库。 除 PENDING_FAILOVER 状态外，主体数据库和镜像数据库始终处于同一状态。 如果为会话设置一个见证服务器，则每个伙伴都将使用其连接状态（CONNECTED 或 DISCONNECTED）监视该见证服务器。  
  
 可能的数据库镜像状态如下所示：  
  
|镜像状态|说明|  
|---------------------|-----------------|  
|SYNCHRONIZING|镜像数据库的内容滞后于主体数据库的内容。 主体服务器正在将日志记录发送到镜像服务器（正在将更改应用于镜像数据库以使其前滚）。<br /><br /> 在数据库镜像会话开始时，数据库处于 SYNCHRONIZING 状态。 主体服务器为数据库提供服务，同时镜像服务器尽量与主体服务器保持同步。|  
|SYNCHRONIZED|当镜像服务器与主体服务器几乎保持同步时，镜像状态将更改为 SYNCHRONIZED。 只要主体服务器继续向镜像服务器发送更改，并且镜像服务器继续将更改应用于镜像数据库，数据库就会保持此状态。<br /><br /> 如果将事务安全性设置为 FULL，则 SYNCHRONIZED 状态同时支持自动故障转移和手动故障转移，并且在故障转移后不会丢失数据。<br /><br /> 如果关闭事务安全性，则即使处于 SYNCHRONIZED 状态，也总可能丢失某些数据。<br /><br /> 在 SQL Server Management Studio 中，数据库的状态将显示为“正在还原”。 如需获取实际状态，请在 [sys.database_mirroring](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md) 中查询 `mirroring_state_desc` 列 |  
|SUSPENDED|数据库的镜像副本不可用。 主体数据库运行时不向镜像服务器发送任何日志，这种情况称为“运行已公开” 。 这是故障转移后的状态。<br /><br /> 如果发生重做错误或管理员暂停会话，则会话也可能会变为 SUSPENDED 状态。<br /><br /> SUSPENDED 是在伙伴关闭和启动时都能存在的持久性状态。|  
|PENDING_FAILOVER|此状态只在故障转移开始之后的主体服务器中存在，但此时服务器尚未转换到镜像角色。<br /><br /> 当故障转移开始时，主体数据库将进入 PENDING_FAILOVER 状态，快速终止任何用户连接，并在此后不久便接管镜像角色。|  
|DISCONNECTED|伙伴已失去与其他伙伴的通信。|  
  
## <a name="see-also"></a>另请参阅  
 [监视数据库镜像 (SQL Server)](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
  
