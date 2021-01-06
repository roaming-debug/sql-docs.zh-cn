---
title: 强制数据库镜像服务
description: 如果主体服务器在镜像服务器可用时出现故障，请强制将服务故障转移到镜像数据库，以使数据库可用。
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- forced service [SQL Server]
- database mirroring [SQL Server], forcing service
ms.assetid: 8b6ffe77-35f3-4e2a-a658-8a38a8e1c794
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 453238fa4fe07bb90d10b502195bf9b8e5ad8805
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641629"
---
# <a name="force-service-in-a-database-mirroring-session-transact-sql"></a>在数据库镜像会话中强制服务 (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  在高性能模式和不带自动故障转移功能的高安全性模式下，如果主体服务器失败而镜像服务器可用，则数据库所有者可以强制将服务故障转移到镜像数据库（可能造成数据丢失），从而使数据库可用。 此选项仅在以下情况中可用：  
  
-   主体服务器已关闭。  
  
-   WITNESS 设置为 OFF 或连接到镜像服务器。  
  
> [!CAUTION]  
>  严格讲来，强制服务是一种灾难恢复方法。 强制服务可能会导致一些数据丢失。 因此，只有在为了立即恢复数据库服务而不惜丢失某些数据时，才可强制执行服务。 如果强制服务面临丢失重要数据的风险，则建议您停止镜像并手动重新同步数据库。 有关强制服务所面临风险的详细信息，请参阅 [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)。  
  
 强制服务会挂起会话并启动新的恢复分叉。 强制服务的结果相当于删除镜像并恢复以前的主体数据库。 但是，强制服务便于在恢复镜像时重新同步数据库（可能造成数据丢失）。  
  
### <a name="to-force-service-in-a-database-mirroring-session"></a>在数据库镜像会话中强制执行服务  
  
1.  连接到镜像服务器。  
  
2.  发出以下语句：  
  
     ALTER DATABASE *<database_name>* SET PARTNER FORCE_SERVICE_ALLOW_DATA_LOSS  
  
     其中， *<database_name>* 为镜像数据库。  
  
     镜像服务器将立即转换为主体服务器，并且镜像挂起。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [数据库镜像运行模式](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)  
  
  
