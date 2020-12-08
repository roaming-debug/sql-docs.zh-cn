---
title: SQL Server - Backup Device 对象 | Microsoft Docs
description: 了解 Backup Device 对象，该对象提供的计数器可监视用于备份和还原操作的 Microsoft SQL Server 备份设备。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Backup Device
- Backup Device object
ms.assetid: 52e7febf-d5e0-4674-945b-aacc40a9ad6e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 383e1cd4ccca7a466817ac5ca226785fa56560ce
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505890"
---
# <a name="sql-server-backup-device-object"></a>SQL Server Backup Device 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **Backup Device** 对象提供的计数器可监视用于备份和还原操作的 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份设备。 在希望基于每个设备确定吞吐量或备份和还原操作的进度及性能时，可以监视备份设备。 若要监视整个数据库备份或还原操作的吞吐量，请使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Databases 对象的 Backup/Restore Throughput/sec 计数器 。 有关详细信息，请参阅 [SQL Server, Databases Object](../../relational-databases/performance-monitor/sql-server-databases-object.md)。  
  
 下表介绍 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份设备计数器。  
  
|SQL Server Backup Device 计数器|说明|  
|---------------------------------------|-----------------|  
|**Device Throughput Bytes/sec**|一个备份设备在备份或还原数据库时所用的读写操作的吞吐量（以每秒字节数表示）。 这一计数器只有在备份或还原操作执行时才存在。|  
  
## <a name="see-also"></a>另请参阅  
 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
