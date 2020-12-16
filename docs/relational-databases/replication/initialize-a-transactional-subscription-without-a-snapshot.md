---
title: 在没有快照的情况下初始化订阅（事务性）
description: 了解如何在不使用 SQL Server 快照的情况下初始化事务复制。
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, initializing
- replication [SQL Server], initializing
- initializing subscriptions [SQL Server replication], without snapshots
ms.assetid: 75c8c1f8-60bc-44a8-944b-d18d1f6bda11
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 5d987a05c863b08de4fc74ddcb6c63064a8e57ce
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483078"
---
# <a name="initialize-a-transactional-subscription-without-a-snapshot"></a>Initialize a Transactional Subscription Without a Snapshot
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  默认情况下，使用快照初始化对事务发布的订阅，此快照由快照代理生成并由分发代理应用。 在某些方案中，例如涉及大型初始数据集的方案中，最好用其他方法初始化订阅。 初始化订阅服务器的其他方法包括：  
  
-   指定备份。 还原订阅服务器上的备份，然后分发代理将复制任何所需的复制元数据和系统过程。 使用备份进行初始化是向订阅服务器传递数据最快的方法，而且也很方便，因为如果在启用发布以使用备份进行初始化之后取得备份，则可以使用任何最近的备份。  
  
-   通过用其他机制（如附加数据库）向订阅服务器复制初始数据集。 必须确保订阅服务器上的数据和架构是正确的，然后分发代理将复制任何所需的元数据和系统过程。  
  
## <a name="initializing-a-subscription-with-a-backup"></a>用备份初始化订阅  
 备份包含整个数据库；因此，初始化每个订阅数据库时，它将包含发布数据库的完整副本：  
  
-   备份包括未指定为发布项目的表。  
  
-   备份包括所有数据，即使表中指定了行或列筛选器。  
  
 管理员或应用程序负责在还原备份后删除任何不需要的对象或数据。 在后续同步中，仅当数据更改应用于指定为项目的表时才被复制，而且这些更改满足您指定的任何筛选条件。  
  
> [!NOTE]  
>  还原备份时，如果要让订阅服务器自动同步，必须确保备份来自发布服务器。 备份中的日志序列号 (LSN) 值（用于设置开始同步的点）是发布服务器所特有的。  
  
 **用备份初始化订阅**  
  
 若要使用备份初始化订阅，首先必须在创建发布时启用此选项，然后在创建订阅时为多个选项指定值。 可以通过新建发布向导或以编程方式启用发布。 但是，订阅选项所需的值只能用编程方式指定。  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]：[使用备份来初始化事务发布 (SQL Server Management Studio)](../../relational-databases/replication/enable-initialization-with-backup-for-transactional-publications.md)  
  
-   复制 Transact-SQL 编程：[从备份初始化事务订阅（复制 Transact-SQL 编程）](../../relational-databases/replication/initialize-a-transactional-subscription-from-a-backup.md)  
  
> [!NOTE]  
>  如果初始化订阅时不使用快照，在发布服务器上运行 SQL Server 服务的帐户必须具有分发服务器上快照文件夹的写权限。 有关权限的详细信息，请参阅 [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
### <a name="ensuring-the-suitability-of-a-backup"></a>确保备份的适用性  
 如果在执行备份后发生的所有事务都存储在分发服务器上，则备份适用于初始化订阅服务器。 如果备份不适用，复制将显示一条错误消息。  
  
 若要帮助确保备份适用，请按下列规则执行：  
  
-   使用最新的可用备份，如果最新备份的保留时间超过了最大分发保持期，则在尝试用备份初始化订阅之前创建新的备份。 有关保持期的详细信息，请参阅 [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md)。  
  
-   默认情况下，分发清除作业将从分发数据库中清除保留时间超过 72 小时的事务。 清除根据发布的保持期设置进行。 用保留时间较长的备份进行同步时，请考虑在想要还原的备份进行之前临时禁用此作业，并在订阅成功创建后再重新启用它。 这可以防止从分发数据库中删除那些从备份成功同步时可能需要的事务。 有关运行清除作业的信息，请参阅[运行复制维护作业 (SQL Server Management Studio)](../../relational-databases/replication/administration/run-replication-maintenance-jobs-sql-server-management-studio.md)。  
  
 在某些情况下，设置使用备份初始化的订阅后，必须在还原的订阅服务器数据库中手动执行自定义。 通常，如果用以下方式定义发布服务器：希望订阅服务器数据库的内容与发布服务器数据库的内容不同，则需要对还原的订阅服务器数据库进行手动修改。  
  
-   如果将还原数据库中的索引视图作为基于日志的索引视图到表的项目发布，则必须将其转换为表  
  
-   必须将还原数据库中的订阅时间戳列转换为 **binary(8)** 列：将包含时间戳列的表的内容复制到具有匹配架构（除了用 **binary(8)** 列替代时间戳列之外）的新表，删除原始表，然后使用与原始表相同的名称重命名新表。  
  
## <a name="initializing-a-subscription-with-an-alternative-method"></a>使用其他方法初始化订阅  
 可以使用任何能够将发布数据库架构和数据复制到订阅服务器的方法初始化订阅，例如 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]。 用其他方法初始化订阅服务器时，复制支持将对象复制到订阅服务器。  
  
 与使用备份进行初始化不同，您或您的应用程序必须确保数据和架构在添加订阅时正确同步。 例如，如果从将数据和架构复制到订阅服务器到添加订阅这一时间段内，发布服务器上进行了某一活动，则此活动导致的更改可能不会复制到订阅服务器。  
  
 若要使用其他方法初始化订阅，请参阅 [Initialize a Subscription Manually](../../relational-databases/replication/initialize-a-subscription-manually.md)。  
  
## <a name="see-also"></a>另请参阅  
 [初始化订阅](../../relational-databases/replication/initialize-a-subscription.md)  
  
  
