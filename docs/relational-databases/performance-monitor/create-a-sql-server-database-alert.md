---
title: 创建 SQL Server 数据库警报 | Microsoft Docs
description: 可以使用系统监视器创建一个在达到系统监视器计数器的阈值时发出的 SQL Server 数据库警报。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- database performance [SQL Server], alerts
- alerts [SQL Server], creating
- thresholds [SQL Server]
- database alerts [SQL Server]
- tuning databases [SQL Server], alerts
- monitoring performance [SQL Server], alerts
- monitoring server performance [SQL Server], alerts
- database monitoring [SQL Server], alerts
- server performance [SQL Server], alerts
ms.assetid: 0511136a-1b6b-4095-aa45-39e77b67aba2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 512c902e0b35e15b65866464500b4928e105d4ad
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506031"
---
# <a name="create-a-sql-server-database-alert"></a>创建 SQL Server 数据库警报
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  可以使用系统监视器创建一个在达到系统监视器计数器的阈值时发出的警报。 系统监视器将启动一个应用程序（例如，为处理警报情况而编写的自定义应用程序）来响应警报。 例如，可以创建一个在死锁数超过特定值时发出的警报。  
  
 此外，还可以通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理来定义警报。 有关详细信息，请参阅 [“警报”](../../ssms/agent/alerts.md)。  
  
 有关使用系统监视器设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库警报的详细信息，请参阅[设置 SQL Server 数据库警报 (Windows)](../../relational-databases/performance/set-up-a-sql-server-database-alert-windows.md)。  
  
## <a name="see-also"></a>另请参阅  
 [sp_add_alert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)   
 [sys.sysperfinfo (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)  
  
  
