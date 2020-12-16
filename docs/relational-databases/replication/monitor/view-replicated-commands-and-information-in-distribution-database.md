---
title: 查看分发数据库中的复制命令和信息
description: 了解如何在 SQL Server 分发数据库中查看复制命令及其他与复制相关的信息。
ms.custom: seo-lt-2019
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- sp_browsereplcmds
- transactional replication, monitoring
- distribution databases [SQL Server replication], viewing replicated commands
- viewing replicated commands
ms.assetid: 9c20acec-8fab-4483-b9c1-dfe3768f85dd
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 222c902eae5b38a634f382ac0d393dc741db61fe
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463228"
---
# <a name="view-replicated-commands-and-information-in-distribution-database"></a>查看分发数据库中的复制命令和信息
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  在使用事务复制时，事务命令在分发代理将其传播到所有订阅服务器或订阅服务器中的分发代理请求更改之前存储在分发数据库中。 使用复制存储过程可以编程方式查看分发数据库中的这些挂起的命令。 有关详细信息，请参阅[复制存储过程 (Transact-SQL)](../../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)。  
  
### <a name="to-view-replicated-commands-from-all-transactional-publications-in-the-distribution-database"></a>查看分发数据库中来自所有事务发布的复制命令  
  
1.  在分发服务器的分发数据库中，执行 [sp_browsereplcmds](../../../relational-databases/system-stored-procedures/sp-browsereplcmds-transact-sql.md)。  
  
### <a name="to-view-replicated-commands-in-the-distribution-database-from-a-specific-article-or-from-a-specific-database-published-using-transactional-replication"></a>查看分发数据库中来自使用事务复制发布的某个特定项目或特定数据库的复制命令  
  
1.  （可选）在发布服务器的发布数据库中，执行 [sp_helparticle](../../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)。 指定 `@publication` 和 `@article`。 请记录结果集中 **article id** 的值。  
  
2.  在分发服务器的分发数据库中，执行 [sp_browsereplcmds](../../../relational-databases/system-stored-procedures/sp-browsereplcmds-transact-sql.md)。 （可选）为 `@article_id` 指定从步骤 2 中得到的项目 ID。 （可选）为 `@publisher_database_id`指定发布数据库的 ID，此 ID 可以从 [sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图的 database_id 列获得。  
  
## <a name="see-also"></a>另请参阅  
 [以编程方式监视复制](../../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
