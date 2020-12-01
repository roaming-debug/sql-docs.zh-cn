---
description: 发布属性，订阅选项
title: 发布属性 - 订阅选项 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.pubproperties.subscriptionoptions.f1
ms.assetid: 31abd605-b273-419d-86df-d0ecf539a507
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 8637bafe6433893b73e3dc2a7980c43f73fe19d4
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88420491"
---
# <a name="publication-properties-subscription-options"></a>发布属性，订阅选项
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  可以使用 **“发布属性”** 对话框的 **“订阅选项”** 页，查看和设置与订阅关联的发布级别属性。 属性分为以下类别：  
  
-   适用于所有发布的属性。  
  
-   适用于快照发布和事务发布的属性（包括允许更新订阅的属性）。  
  
-   适用于合并发布的属性。  
  
> [!NOTE]  
>  一些属性是只读的；在本主题的属性说明中讲述了原因。 一些属性更改要求新的发布快照，而一些属性更改还要求重新初始化所有订阅。 有关详细信息，请参阅[更改发布和项目属性](../../relational-databases/replication/publish/change-publication-and-article-properties.md)。  
  
## <a name="options-for-all-publications"></a>用于所有发布的选项  
  
### <a name="creation-and-synchronization"></a>创建和同步  
 **允许匿名订阅**  
 确定是否允许匿名请求订阅。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssEWEd2005](../../includes/ssewed2005-md.md)]、[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssMobileEd2005](../../includes/ssmobileed2005-md.md)] 和 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] for Windows CE 支持匿名订阅。 若要对快照发布和事务发布使用此选项，则必须将选项 **“快照始终可用”** 设置为 **True**。  
  
 **可附加的订阅数据库**  
 确定是否可以通过附加订阅数据库的副本来创建订阅（对于快照发布和事务发布，要求将选项 **“快照始终可用”** 设置为 **True** ）。  
  
> [!IMPORTANT]  
>  在未来的版本中，将不支持可附加订阅。 不推荐使用该功能。  
  
 **允许请求订阅**  
 确定是否允许订阅服务器创建对此发布的请求订阅。 有关详细信息，请参阅[订阅发布](../../relational-databases/replication/subscribe-to-publications.md)。  
  
### <a name="schema-replication"></a>架构复制  
 **复制架构更改**  
 仅限 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本。 确定是否将架构更改（例如向表中添加列或者更改列的数据类型）复制到已发布的对象。 有关详细信息，请参阅[对发布数据库进行架构更改](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)。  
  
## <a name="options-for-snapshot-and-transactional-publications"></a>适用于快照发布和事务发布的选项  
  
### <a name="creation-and-synchronization"></a>创建和同步  
 **独立的分发代理**  
 确定是否使用独立于此数据库的其他发布的代理。 此选项是只读选项；对于用新建发布向导创建的发布，该选项默认设置为 **True** ，并且不能在创建发布后更改。 有关详细信息，请参阅[复制代理管理](../../relational-databases/replication/agents/replication-agent-administration.md)。  
  
 **“快照始终可用”**  
 确定是否在每次运行快照代理时创建快照文件（需要 **“独立的分发代理”**）。 此选项是只读选项；如果在新建发布向导的 **“快照代理”** 页上选择了 **“立即创建快照并使快照保持可用状态，以初始化订阅”** ，此选项将设置为 **True** （默认值）。 有关详细信息，请参阅[创建并应用快照](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
 **允许从备份文件初始化**  
 仅限[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本。 确定是否允许使用备份文件来初始化订阅。 有关详细信息，请参阅 [初始化事务订阅（不使用快照）](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)中手动初始化订阅。  
  
 **允许非 SQL Server 订阅服务器**  
 仅限[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本。 确定发布是否支持非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器。 将此选项设置为 True 可将其他发布属性设置为支持非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器。 如果存在订阅，则此选项是只读的；如果 **“允许立即更新订阅”** 、 **“允许排队更新订阅”** 或 **“允许对等订阅”** 设置为 **True** ，则不能将此选项设置为 **True**。 有关详细信息，请参阅 [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)。  
  
### <a name="data-transformation"></a>数据转换  
 **允许数据转换**  
 确定在将数据分发到订阅服务器之前是否使用 Data Transformation Services (DTS) 转换数据。 此选项是只读的；只有当使用存储过程创建发布时，才能启用数据转换。  
  
> [!IMPORTANT]  
>  在未来的版本中，将不支持可转换订阅。 不推荐使用该功能。  
  
### <a name="peer-to-peer-replication"></a>对等复制  
 **True**  
 仅适用于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本。 确定发布是否支持对等复制。 将此选项设置为 **True** 可将其他发布属性设置为支持对等复制。 如果存在订阅，此选项为只读。 如果“允许立即更新订阅”  、“允许排队更新订阅”  或“允许非 SQL Server 订阅服务器” 设置为 **True** ，则不能将此选项设置为 **True**。 有关详细信息，请参阅 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)。  
  
 **允许对等冲突检测**  
 仅适用于 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。 指定是否为此发布启用冲突检测。 若要使用冲突检测，所有节点都必须运行 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 或更高版本；且必须为所有节点启用检测。 若要使用冲突检测，还必须为“对等发起方 ID” 指定一个值。有关详细信息，请参阅 [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)。  
  
 **对等发起方 ID**  
 仅适用于 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。 指定对等拓扑中某个节点的 ID。 如果 **“允许对等冲突检测”** 设置为 **True**，此 ID 将用于冲突检测。 请指定拓扑中从未使用过的非零、正值 ID。 有关已经使用过的 ID 的列表，请查询 [Mspeer_originatorid_history](../../relational-databases/system-tables/mspeer-originatorid-history-transact-sql.md) 系统表。  
  
### <a name="updatable-subscriptions"></a>可更新订阅  
 **“允许排队更新订阅”**  
 确定是否允许立即将订阅服务器数据更改复制到发布服务器。 此选项是只读选项；只有在创建发布后，才能启用更新订阅。 有关详细信息，请参阅 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)。  
  
 **“允许对等订阅”**  
 确定是否允许将订阅服务器数据更改加入队列并在稍后复制到发布服务器。 此选项是只读选项；只有在创建发布后，才能启用更新订阅。 有关详细信息，请参阅 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)。  
  
 **集中报告冲突**  
 确定是仅报告发布服务器上发生冲突的数据更改，还是同时报告发布服务器和订阅服务器上发生冲突的数据更改（需要选项 **“允许排队更新订阅”**）。 此选项是只读选项；对于用新建发布向导创建的发布，该选项默认设置为 **True** ，并且不能在创建发布后更改。 如果值为 **True** ，则表示只在发布服务器上报告冲突。 只能在报告冲突的位置查看冲突。  
  
 **冲突解决策略**  
 指定当订阅服务器更改与发布服务器更改发生冲突时所执行的操作（需要选项 **“允许排队更新订阅”**）。 有关详细信息，请参阅 [Queued Updating Conflict Detection and Resolution](../../relational-databases/replication/transactional/updatable-subscriptions-queued-updating-conflict-resolution.md)。  
  
 **队列类型**  
 确定在将订阅服务器上的更改应用于发布服务器之前，是使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 队列还是使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 消息队列 (MSMQ) 来排队订阅服务器上的更改（需要选项 **“允许排队更新订阅”**）。 此选项仅与 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]相关；更高的版本始终使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表进行排队。  
  
## <a name="options-for-merge-publications"></a>用于合并发布的选项  
  
### <a name="conflict-reporting"></a>冲突报告  
 **集中报告冲突**  
 决定是仅报告发布服务器上发生冲突的数据更改，还是同时报告发布服务器和订阅服务器上发生冲突的数据更改。 此选项是只读选项；对于用新建发布向导创建的发布，该选项默认设置为 **True** ，并且不能在创建发布后更改。 如果值为 **True** ，则表示只在发布服务器上报告冲突。 只能在报告冲突的位置查看冲突。 有关详细信息，请参阅 [Advanced Merge Replication Conflict Detection and Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)的“查看冲突”部分。  
  
### <a name="filtering"></a>Filtering  
 **允许参数化筛选器**  
 根据发布是否使用参数化筛选器设置此选项。 此选项始终是只读的。 有关详细信息，请参阅 [参数化行筛选器](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)。  
  
 **验证订阅服务器**  
 确定在验证订阅服务器是否具有正确的数据分区时，将使用哪些函数。 用逗号分隔多个值。 有关详细信息，请参阅[验证合并订阅服务器的分区信息](../../relational-databases/replication/validate-partition-information-for-a-merge-subscriber.md)。  
  
 **预计算分区**  
 仅限[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本。 确定是否通过预先计算哪些数据行属于哪个分区来优化同步。 如果发布满足预计算分区的条件，则此设置在默认情况下为 **True** 。 有关详细信息，请参阅[使用预计算分区优化参数化筛选器性能](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)。  
  
 **优化同步**  
 确定是否通过在每个订阅服务器上存储其他元数据来优化合并处理。 此优化功能已被预计算分区取代；只有在 **“预计算分区”** 设置为 **False** 时，才需要使用 **“优化同步”** 选项。 有关详细信息，请参阅 [参数化行筛选器](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)。  
  
### <a name="merge-processes"></a>合并进程  
 **限制并发进程**  
 决定是否限制可以同时运行的合并代理的数量。 如果发布具有大量可能同时同步的推送订阅，则通常使用此选项。  
  
 **最大并发进程数**  
 可以同时运行的合并代理的最大数量（需要 **“限制并发进程”**）。 如果正在同步的代理数量超过最大数量，则会将代理放入队列中，直到该数量低于最大数量。  
  
## <a name="see-also"></a>另请参阅  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [查看和修改发布属性](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
