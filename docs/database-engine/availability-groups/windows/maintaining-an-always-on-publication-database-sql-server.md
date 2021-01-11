---
title: 将复制的发布服务器数据库作为可用性组的一部分管理
description: 有关如何管理和维护在 SQL 复制中充当发布服务器并且还加入 Always On 可用性组的数据库的说明。
ms.custom: seodec18
ms.date: 05/18/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 55b345fe-2eb9-4b04-a900-63d858eec360
author: cawrites
ms.author: chadam
ms.openlocfilehash: f07f8eaa1dc5657c2dfdb296bc9efffec9b308b2
ms.sourcegitcommit: e5664d20ed507a6f1b5e8ae7429a172a427b066c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2020
ms.locfileid: "97697134"
---
# <a name="manage-a-replicated-publisher-database-as-part-of-an-always-on-availability-group"></a>将复制的发布服务器数据库作为 Always On 可用性组的一部分管理
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  本主题将探讨使用 AlwaysOn 可用性组时维护发布数据库的特殊注意事项。  
  
##  <a name="maintaining-a-published-database-in-an-availability-group"></a><a name="MaintainPublDb"></a> 在可用性组中维护已发布的数据库  
 维护 AlwaysOn 发布数据库与维护标准的发布数据库基本相同，需要注意以下事项：  
  
-   必须在主副本主机上进行管理。 在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]中，发布显示在主副本主机以及可读辅助副本的 **“本地发布”** 文件夹下。 在故障转移后，如果无法读取已提升为主副本的辅助副本，则您可能必须手动刷新 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] 以反映更改。  
  
-   复制监视器将始终在原始发布服务器下显示发布信息。 但是，通过将原始发布服务器添加为服务器，可以使用复制监视器从任意副本查看此信息。  
  
-   当使用存储过程或复制管理对象 (RMO) 在当前主副本上管理复制时，对于需要指定发布服务器名称的情况，您必须指定已经启用了数据库复制的实例（原始发布服务器）的名称。 若要确定相应的名称，请使用 **PUBLISHINGSERVERNAME** 函数。 当发布数据库联接一个可用性组时，存储在辅助数据库副本中的复制元数据将与主副本上的相同。 因此，对于主副本上已启用复制的发布数据库而言，辅助副本上的系统表中存储的发布服务器实例名称将是主副本的名称，而不是辅助副本的名称。 如果将发布数据库故障转移到辅助副本，则这种情况会影响复制的配置和维护。 例如，如果你在发生故障转移后在次要副本上使用存储过程配置复制，而且你希望对在不同副本上启用的发布数据库使用请求订阅，则必须将原始发布服务器的名称（而非当前发布服务器的名称）指定为 sp_addpullsubscription 或 sp_addmergepulllsubscription 的 \@publisher 参数    。 但是，如果您在故障转移之后启用一个发布数据库，则存储在系统表中的发布服务器实例名称将为当前主副本主机的名称。 在此情况下，你应对 \@publisher 参数使用当前主要副本的主机名  。  
  
    > [!NOTE]  
    >  对于一些过程（如 sp_addpublication），只支持对不属于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例的发布服务器使用 \@publisher 参数；在此情况下，该参数与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] AlwaysOn 无关   。  
  
-   若要在故障转移后在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] 中同步订阅，应将来自订阅服务器的请求订阅与来自活动发布服务器的推送订阅进行同步。  
  
##  <a name="removing-a-published-database-from-an-availability-group"></a><a name="RemovePublDb"></a> 从可用性组中删除已发布的数据库  
 如果从可用性组中删除了已发布的数据库，或者删除了包含已发布的成员数据库的可用性组，则应考虑以下问题。  
  
-   如果从可用性组主要副本中删除原始发布服务器上的发布数据库，则必须运行 sp_redirect_publisher，但请勿指定 \@redirected_publisher 参数的值，这样便可以删除发布服务器/数据库对的重定向   。  
  
    ```  
    EXEC sys.sp_redirect_publisher   
        @original_publisher = 'MyPublisher',  
        @published_database = 'MyPublishedDB';  
    ```  
  
     该数据库将在主副本上处于“正在恢复”状态，且必须还原。 在执行还原操作之后，针对原始发布服务器的复制工作应保持不变。  
  
-   如果发布数据库从原始发布服务器故障转移到某个副本，并且从可用性组主要副本中删除了该数据库，则使用存储过程 **sp_redirect_publisher** 将原始发布服务器显式重定向到新的发布服务器。 该数据库将处于“正在恢复”状态，且必须还原。 在执行还原操作之后，复制应会继续按照其在可用性组下的方式工作。  
  
    ```  
    EXEC sys.sp_redirect_publisher   
        @original_publisher = 'MyPublisher',  
        @published_database = 'MyPublishedDB',  
        @redirected_publisher = 'MyNewPublisher';  
    ```  
  
     不要从分发服务器中删除原始发布服务器的远程服务器，即使无法再访问该服务器也是如此。 在分发服务器上需要原始发布服务器的服务器元数据来满足发布元数据查询。  
  
-   如果删除了整个可用性组，则有关成员复制数据库的行为与从可用性组中删除某个已发布的数据库时的行为相同。 在已经还原数据库并且已经修改重定向之后，即可从最后的主副本中恢复复制。 如果在数据库的原始发布服务器上还原数据库，则应删除重定向。 如果在其他主机上还原数据库，则应将重定向显式定向到新的主机。  
  
    > [!NOTE]  
    >  在删除包含已发布的成员数据库的可用性组时，或在删除可用性组中的已发布的数据库时，已发布的数据库的所有副本都将处于“正在恢复”状态。 这些副本在还原之后都将显示为已发布的数据库。 只应与发布元数据一起保留一份副本。 若要对已发布的数据库副本禁用复制，首先应删除该数据库中的所有订阅和发布。  
  
     运行 **sp_dropsubscription** 以删除发布订阅。 确保将参数 \@ignore_distributor 设置为 1，以便为分发服务器上活动的发布数据库保留元数据  。  
  
    ```  
    USE MyDBName;  
    GO  
  
    EXEC sys.sp_dropsubscription   
        @subscriber = 'MySubscriber',  
        @publication = 'MyPublication',  
        @article = 'all',  
        @ignore_distributor = 1;  
    ```  
  
     运行 **sp_droppublication** 以删除所有发布。 同样，确保将参数 \@ignore_distributor 设置为 1，以便为分发服务器上活动的发布数据库保留元数据  。  
  
    ```  
    EXEC sys.sp_droppublication   
        @publication = 'MyPublication',  
        @ignore_distributor = 1;  
    ```  
  
     运行 **sp_replicationdboption** 以禁用对该数据库的复制。  
  
    ```  
    EXEC sys.sp_replicationdboption  
        @dbname = 'MyDBName',  
        @optname = 'publish',  
        @value = 'false';  
    ```  
  
     此时，可保留或删除已发布的数据库的副本。  

## <a name="remove-original-publisher"></a>删除原始发布服务器

可能存在要从 Always On 可用性组中删除原始发布服务器的实例（替换旧服务器、OS 升级等）。 按照此部分中的步骤从可用性组中删除发布服务器。 

假设你有服务器 N1、N2 和 D1，其中 N1 和 N2 是可用性组 AG1 的主副本和辅助副本，N1 是事务发布的原始发布服务器，而 D1 是分发服务器。 你希望将原始发布服务器 N1 替换为新发布服务器 N3。 

若要删除发布服务器，请按照以下步骤操作： 

1. 在节点 N3 中安装和配置 SQL Server。 SQL Server 版本必须与原始发布服务器相同。 
1. 在分发服务器 D1 上，使用 [sp_adddistpublisher](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) 将 N3 添加为发布服务器。 
1. 将 N3 配置为将 D1 作为其分发服务器的发布服务器。 
1. 将 N3 作为副本添加到可用性组 AG1。 
1. 在 N3 副本上，验证发布的推送订阅服务器是否显示为链接服务器。 使用 [sp_addlinkedserver](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) 或 SQL Server Management Studio。 
1. 同步 N3 后，将可用性组作为主副本故障转移到 N3。 
1. 从可用性组 AG1 中删除 N1。 

请考虑以下要求：
- 不要从分发服务器中删除原始发布服务器的远程服务器（本例为 N1）或与之关联的任何元数据，即使无法再访问该服务器也是如此。 在分发服务器上需要原始发布服务器的服务器元数据来满足发布元数据查询，如果没有，复制将失败。 
- 对于 SQL Server 2014，在删除原始发布服务器后，你将不能使用原始发布服务器名称在复制监视器中管理复制。 如果尝试在复制监视器中将新副本注册为发布服务器，则不会显示信息，因为没有与之关联的元数据。 若要在此方案中管理复制，必须右键单击 SQL Server Management Studio (SSMS) 中的单个发布和订阅。
- 对于 SQL Server 2016 SP2-CU3、SQL Server 2017 CU6 及更高版本，请在复制监视器中注册可用性组发布服务器的侦听器，以使用 SQL Server Management Studio 17.7 和更高版本管理复制。 
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [为 AlwaysOn 可用性组配置复制 (SQL Server)](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)  
  
-   [复制、更改跟踪、更改数据捕获和 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)  
  
-   [复制管理常见问题解答](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
-   [复制订阅服务器和 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [AlwaysOn 可用性组：互操作性 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [SQL Server 复制](../../../relational-databases/replication/sql-server-replication.md)  
  
  
