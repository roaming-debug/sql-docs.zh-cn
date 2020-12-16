---
title: 在同步期间执行脚本（复制 SP）
description: 了解如何在事务发布或合并发布的同步过程中使用复制存储过程执行按需脚本。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- synchronization [SQL Server replication], scripts
- scripts [SQL Server replication], synchronization and
- sp_addscriptexec
ms.assetid: b58a0877-4e43-4fab-a281-24e6022d3fb1
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: da811d3a0df948ec8687b4a9c7f3fc07e65cc689
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464768"
---
# <a name="execute-scripts-during-synchronization-replication-transact-sql-programming"></a>在同步期间执行脚本（复制 Transact-SQL 编程）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  复制支持事务发布和合并发布的订阅服务器的按需脚本执行。 此功能可将脚本复制到复制工作目录，然后使用 **sqlcmd** 将脚本应用到订阅服务器。 默认情况下，如果在对事务发布的订阅应用脚本时发生故障，分发代理将停止。 您可以使用复制存储过程以编程的方式指定要执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。  
  
### <a name="to-specify-a-script-to-run-for-all-subscribers-to-a-snapshot-transactional-or-merge-publication"></a>为快照发布、事务发布或合并发布的所有订阅服务器指定要运行的脚本  
  
1.  编写并测试将按需执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。  
  
2.  将此脚本文件保存到此发布的快照代理能访问的位置。  
  
3.  在发布服务器上，对发布数据库执行 [sp_addscriptexec (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addscriptexec-transact-sql.md)。 指定 `@publication`，为 `@scriptfile` 指定步骤 2 中创建的具有完整 UNC 路径的脚本文件名称，并为 `@skiperror` 指定下列值之一：  
  
    -   **0** - 如果遇到错误，代理将停止执行脚本。  
  
    -   **1** - 遇到错误时，代理将记录错误并继续执行脚本。  
  
4.  在代理下次运行以同步订阅时，指定的脚本将在每个订阅服务器上执行。  
  
## <a name="see-also"></a>另请参阅  
 [同步数据](../../relational-databases/replication/synchronize-data.md)  
  
  
