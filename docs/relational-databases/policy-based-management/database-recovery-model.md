---
title: 数据库恢复模式
description: 了解如何启用策略来检查用户数据库的备份恢复模式，以减少数据丢失。
ms.custom: seo-lt-2019
ms.date: 08/31/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 8c68b60243ec033fff3735901cc4e35134222704
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345506"
---
# <a name="database-recovery-model"></a>数据库恢复模式
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  对于 SQL Server Enterprise Edition 和 Standard Edition，此规则检查将恢复模式设置为简单模式的非只读用户数据库。 对于生产数据库，建议您使用完整恢复模式，而不使用简单恢复模式。 完整恢复模式会启用时间点恢复。 如果有灾难恢复，这将有助于减少数据丢失。
  
## <a name="best-practices-recommendations"></a>最佳做法建议  
 生产数据库应使用完整恢复模式，应经常备份事务日志以确保可恢复性并最大程度地减少数据丢失。
  
## <a name="see-also"></a>另请参阅 
  
 [还原与恢复概述](../backup-restore/restore-and-recovery-overview-sql-server.md)   
  
 [完整数据库还原（完整恢复模式）](../backup-restore/complete-database-restores-full-recovery-model.md)  

 [事务日志备份](../backup-restore/transaction-log-backups-sql-server.md)   
  
