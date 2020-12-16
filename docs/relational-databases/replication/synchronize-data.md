---
title: 同步数据 | Microsoft Docs
description: 在复制中同步数据是指在 SQL Server 中的发布服务器和订阅服务器之间传播数据和架构更改。
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- synchronization [SQL Server replication], about synchronization
- merge replication synchronization [SQL Server replication]
- scripts [SQL Server replication], synchronization and
- synchronization [SQL Server replication]
- snapshot replication [SQL Server], synchronization
- transactional replication, synchronization
- subscriptions [SQL Server replication], synchronizing
- on demand script execution
- replication [SQL Server], synchronization
- scripts [SQL Server replication]
ms.assetid: 724802f7-7d69-46d3-a330-bd8aa7f53114
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: c75b15ddd296d85ba7df44e97aa33dfca33ae8f2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460140"
---
# <a name="synchronize-data"></a>同步数据
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  同步数据是指在订阅服务器上应用初始快照后，在发布服务器和订阅服务器之间传播数据和架构更改的过程。 同步可按下列方式发生：  
  
-   连续，这是事务复制的典型方式。  
  
-   按需，这是合并复制的典型方式。  
  
-   根据计划，这是快照复制的典型方式。  
  
 同步订阅时，根据您所使用的复制类型的不同，将发生不同的过程。  
  
-   快照复制。 同步是指分发代理在订阅服务器上重新应用快照，以便订阅数据库与发布数据库上的架构和数据一致。  
  
     如果在发布服务器上修改了数据或架构，则必须生成一个新快照，以便将修改传播到订阅服务器。  
  
-   事务复制。 同步是指分发代理将更新、插入、删除及其他更改从分发数据库传输到订阅服务器。  
  
-   合并复制。 同步是指合并代理从订阅服务器向发布服务器上载更改，然后再从发布服务器向订阅服务器下载更改。 将检测并解决冲突（如果有）。 数据被收敛，发布服务器和所有订阅服务器将最终达到相同的数据值。 如果检测到冲突并解决了冲突，则一些用户已提交的工作将更改为根据您定义的策略来解决冲突。  
  
 每次发生同步时，快照发布都会彻底刷新订阅服务器上的架构，因此所有架构更改都会应用到订阅服务器。 事务复制和合并复制还支持最常见的架构更改。 有关详细信息，请参阅[对发布数据库进行架构更改](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)。  
  
 若要同步推送订阅，请参阅 [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md)。  
  
 若要同步请求订阅，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md)。  
  
 若要设置同步计划，请参阅 [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md)。  
  
 **查看和解决同步冲突**  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]：[查看和解决合并发布的数据冲突 &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/view-and-resolve-data-conflicts-for-merge-publications.md)  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]：[查看事务发布的数据冲突 &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/view-data-conflicts-for-transactional-publications-sql-server-management-studio.md)  
  
## <a name="executing-code-during-synchronization"></a>在同步过程中执行代码  
 复制支持两种在同步过程中执行代码的方法  
  
-   事务复制和合并复制支持按需脚本执行。 使用按需脚本执行，可以指定要在同步过程中运行的 SQL 脚本。 将该脚本复制到订阅服务器，并在同步进程开始时用 **sqlcmd** 执行该脚本。 在将复制的更改应用到订阅服务器时，该脚本不能访问这些复制的更改。 有关详细信息，请参阅[在同步期间执行脚本（复制 Transact-SQL 编程）](../../relational-databases/replication/execute-scripts-during-synchronization-replication-transact-sql-programming.md)。  
  
-   合并复制支持业务逻辑处理程序。 使用业务逻辑处理程序框架，可以编写一个在合并同步过程中调用的托管代码程序集。 程序集包括可以响应同步过程中的许多状况的业务逻辑：数据更改、冲突和错误。 有关详细信息，请参阅[合并同步期间执行业务逻辑](../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md)。  
  
## <a name="see-also"></a>另请参阅  
 [检测并解决合并复制冲突](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)  
  
  
