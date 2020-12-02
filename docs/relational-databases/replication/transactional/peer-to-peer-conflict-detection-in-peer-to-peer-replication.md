---
description: 对等 - 对等复制中的冲突检测
title: 对等复制中的冲突检测 | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, peer-to-peer replication
- peer-to-peer transactional replication, conflict detection
ms.assetid: 754a1070-59bc-438d-998b-97fdd77d45ca
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: eacc75c7f2a5a5ba7993e7f3d574b749b86acd2e
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130976"
---
# <a name="peer-to-peer---conflict-detection-in-peer-to-peer-replication"></a>对等 - 对等复制中的冲突检测
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  通过对等事务复制，可以在拓扑中的任何节点插入、更新或删除数据并将数据更改传播到其他节点。 由于可在任何节点上更改数据，因此在不同节点上进行的数据更改可能会相互冲突。 如果在多个节点上修改了某一行，则将该行传播给其他节点时会导致冲突甚至丢失更新。  
  
 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 和更高版本中的对等复制提供了在对等拓扑中启用冲突检测的选项。 此选项将有助于防止出现因未检测到的冲突而引发的各种问题，包括不一致的应用程序行为和丢失的更新。 启用该选项后，默认情况下，发生冲突的更改将被视为导致分发代理失败的关键错误。 发生冲突时，拓扑将始终处于不一致的状态，直至冲突得以解决而且拓扑中的数据保持一致。  
  
> [!NOTE]  
>  为了避免潜在的数据不一致性，即便已经启用了冲突检测功能，也应尽力避免对等拓扑中发生冲突。 为了确保仅在某一个节点上执行特定行的写入操作，访问并更改数据的应用程序必须对其插入、更新和删除操作进行分区。 分区可确保在一个节点上对给定行进行的修改可以在其他节点修改该行之前，与拓扑中的所有其他节点同步。 如果应用程序需要完善的冲突检测与解决功能，请使用合并复制。 有关详细信息，请参阅[合并复制](../../../relational-databases/replication/merge/merge-replication.md)和[检测并解决合并复制冲突](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)。  
  
## <a name="understanding-conflicts-and-conflict-detection"></a>了解冲突和冲突检测  
 在单个数据库中，不同应用程序对同一行进行的更改不会导致冲突。 原因在于，事务已经过序列化，并使用锁定来处理并发更改。 在异步分布式系统（如对等复制）中，事务独立作用于每个节点，而且没有用来跨多个节点对事务进行序列化的机制。 可以使用两阶段提交之类的协议，但是这会显著影响性能。  
  
 在对等复制之类的系统中，在单个对等节点上提交更改之后将检测不到冲突。 相反，在复制这些更改并将其应用于其他对等节点之后，才能够检测到这些更改。 在对等复制中，用来将更改应用于每个节点的存储过程会基于每个已发布表中的某个隐藏列来检测冲突。 该隐藏列所存储的 ID 将您为每个节点指定的“  发起方 ID”与行版本结合起来。 在同步期间，分发代理会针对每个表执行同步过程。 这些过程会应用来自其他对等节点的插入、更新和删除操作。 如果某个过程在读取该隐藏列的值时检测到冲突，将会引发严重级别为 16 的错误 22815：  
  
 `A conflict of type '%s' was detected at peer %d between peer %d (incoming), transaction id %s  and peer %d (on disk), transaction id %s`  
  
 默认情况下，此错误会导致分发代理停止向该节点应用所做的更改。 有关如何处理所检测到冲突的信息，请参阅本主题稍后部分中的“处理冲突”。  
  
> [!NOTE]  
>  隐藏列只能由通过专用管理员连接 (DAC) 登录的用户来访问。 有关 DAC 的信息，请参阅[用于数据库管理员的诊断连接](../../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)。  
  
 对等复制检测到下列类型的冲突：  
  
-   插入-插入  
  
     每个表中所有参与对等复制的行都使用主键值进行唯一标识。 在将具有相同键值的行插入到多个节点时，会发生插入-插入冲突。  
  
-   更新-更新  
  
     在多个节点上更新了同一行时发生。  
  
-   插入-更新  
  
     在以下情况下发生：在一个节点上更新了某行，但是在另一个节点上删除了该行，然后又重新插入该行。  
  
-   插入-删除  
  
     在以下情况下发生：在一个节点上删除了某行，但是在另一个节点上删除了该行，然后又重新插入该行。  
  
-   更新-删除  
  
     在以下情况下发生：在一个节点上更新了某行，但是在另一个节点上删除了该行。  
  
-   删除-删除  
  
     在多个节点上删除了同一行时发生。  
  
## <a name="enabling-conflict-detection"></a>启用冲突检测  
 若要使用冲突检测，所有节点都必须运行 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 或更高版本；且必须为所有节点启用检测。 在 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 和更高版本中，默认情况下，冲突检测在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]中处于启用状态。 我们建议您启用冲突检测，即使在应当不会有任何冲突时也是如此。 可以使用 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] 或 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 存储过程来启用和禁用冲突检测：  
  
-   在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] 中，可以使用 **“发布属性”** 对话框的 **“订阅选项”** 页或配置对等拓扑向导的 **“配置拓扑”** 页来启用和禁用冲突检测。 有关详细信息，请参阅 [Conflict Detection in Peer-to-Peer Replication](../../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)。  
  
     如果您使用 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]来配置冲突检测，则分发代理将配置为在检测到冲突时停止应用所做的更改。  
  
-   还可以使用 [sp_addpublication](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) 或 [sp_configure_peerconflictdetection](../../../relational-databases/system-stored-procedures/sp-configure-peerconflictdetection-transact-sql.md)存储过程来启用和禁用冲突检测。  
  
     如果您使用存储过程来配置冲突检测，则可以指定在检测到冲突时分发代理是否应当停止应用所做的更改。 默认值是让分发代理停止。 我们建议您使用默认设置。  
  
## <a name="handling-conflicts"></a>处理冲突  
 对等复制中发生冲突时，会引发对等冲突检测警报。 我们建议您配置该警报，以便在发生冲突时获得通知。 有关警报的详细信息，请参阅[对复制代理事件使用警报](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md)。  
  
 在分发代理停止并且引发警报之后，请使用下列方法之一来处理所发生的冲突：  
  
-   从包含必需数据的节点的备份中重新初始化检测到冲突的节点（建议的方法）。 此方法可确保数据保持一致状态。  
  
-   尝试通过允许分发代理继续应用所做的更改，再次同步节点：  
  
    1.  执行 [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)：为 @property 参数指定“p2p_continue_onconflict”，为 @value 参数指定 **true**。  
  
    2.  重新启动分发代理。  
  
    3.  使用冲突查看器验证检测到的冲突，并确定所涉及的行、冲突类型以及入选方。 冲突可基于您在配置过程中指定的发起方 ID 值来解决：源自具有最高 ID 的节点的行将在冲突中入选。 有关详细信息，请参阅[查看事务发布的数据冲突 (SQL Server Management Studio)](../../../relational-databases/replication/view-data-conflicts-for-transactional-publications-sql-server-management-studio.md)。  
  
    4.  运行验证步骤以确保发生冲突的行能够正确收敛。 有关详细信息，请参阅[验证已复制的数据](../../../relational-databases/replication/validate-data-at-the-subscriber.md)。  
  
        > [!NOTE]  
        >  如果在执行该步骤之后数据不一致，则必须在具有最高优先级的节点上手动更新行，然后允许从该节点传播所做的更改。 如果拓扑中不再有发生冲突的更改，则所有节点将保持一致状态。  
  
    5.  执行 [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)：为 @property 参数指定“p2p_continue_onconflict”，为 @value 参数指定 **false**。  
  
## <a name="see-also"></a>另请参阅  
 [@loopback_detection](../../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)  
  
  
