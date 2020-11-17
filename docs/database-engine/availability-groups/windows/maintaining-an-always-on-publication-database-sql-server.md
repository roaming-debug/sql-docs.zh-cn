---
title: 将复制的发布服务器数据库作为可用性组的一部分管理
description: 有关如何管理和维护在 SQL 复制中充当发布服务器并且还加入 Always On 可用性组的数据库的说明。
ms.custom: seodec18
ms.date: 05/18/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 55b345fe-2eb9-4b04-a900-63d858eec360
author: cawrites
ms.author: chadam
ms.openlocfilehash: 864fca7c1d2983bec6296f1e82304cff31f658cb
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584196"
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
  
  
