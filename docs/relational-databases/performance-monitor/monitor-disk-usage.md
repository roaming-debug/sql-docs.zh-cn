---
title: 监视磁盘使用情况 | Microsoft Docs
description: 监视 SQL Server 的磁盘活动包括监视磁盘 I/O 和检测过度分页，以及隔离 SQL Server 创建的磁盘活动。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- database monitoring [SQL Server], disk usage
- disks [SQL Server]
- monitoring performance [SQL Server], disk usage
- server performance [SQL Server], disk usage
- monitoring [SQL Server], disk activity
- excess paging [SQL Server]
- tuning databases [SQL Server], disk usage
- I/O [SQL Server], monitoring
- disks [SQL Server], monitoring activity
- isolating disk activity [SQL Server]
- database performance [SQL Server], disk usage
- monitoring server performance [SQL Server], disk usage
ms.assetid: 1525449c-ea7d-4222-b294-1ba1fe99c9ac
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: de77b88d4b82e0fd7360e76d090dd13468ef2a53
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505971"
---
# <a name="monitor-disk-usage"></a>监视磁盘使用情况
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 Microsoft Windows 操作系统输入/输出 (I/O) 调用来对您的磁盘执行读和写操作。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理何时以及如何执行磁盘 I/O，但是 Windows 操作系统执行基础 I/O 操作。 I/O 子系统包括系统总线、磁盘控制卡、磁盘、磁带驱动器、CD-ROM 驱动器以及许多其他 I/O 设备。 磁盘 I/O 是导致系统瓶颈的最常见原因。  
  
 监视磁盘活动涉及两个主要方面：  
  
-   监视磁盘 I/O 及检测过度换页  
  
-   隔离 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产生的磁盘活动  
  
 有关详细信息，请参阅[监视磁盘使用情况](https://social.technet.microsoft.com/wiki/contents/articles/monitoring-disk-usage.aspx)。 
 
 有关如何在 SQL Server 中排查 I/O 问题的详细信息，请参阅[低 I/O 性能 - SQL Server 和磁盘 I/O 性能](https://techcommunity.microsoft.com/t5/SQL-Server-Support/Slow-I-O-SQL-Server-and-disk-I-O-performance/ba-p/333983)。
  
  
