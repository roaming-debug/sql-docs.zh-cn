---
title: 数据库镜像和复制 (SQL Server) | Microsoft Docs
description: 了解如何将数据库镜像与复制一起使用以改进 SQL Server 中发布数据库的可用性。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- replication [SQL Server], database mirroring and
ms.assetid: 82796217-02e2-4bc5-9ab5-218bae11a2d6
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 16746834acfe8a53511166a35667f136b561c28a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353179"
---
# <a name="database-mirroring-and-replication-sql-server"></a>数据库镜像和复制 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  数据库镜像可以与复制一起使用以改进发布数据库的可用性。 数据库镜像涉及一个数据库的两个副本，这两个副本通常驻留在不同的计算机上。 在任何给定时间都只有一个数据库副本可供客户端使用。 该副本称为主体数据库。 客户端对主体数据库所做的更新应用到数据库的另一副本（称为镜像数据库）。 镜像涉及将在主体数据库上执行的每个插入、更新或删除操作的事务日志应用到镜像数据库上。  
  
 对发布数据库完全支持将复制故障转移到镜像数据库的功能；而对订阅数据库则提供有限支持。 对于分发数据库则不支持数据库镜像。 有关无需重新配置复制就可以恢复分发数据库或订阅数据库的信息，请参阅 [备份和还原复制数据库](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)。   
  
> [!NOTE]  
>  故障转移后，镜像数据库变为主体数据库。 在本主题中，“主体”和“镜像”始终是指原始主体和镜像。  
  
## <a name="requirements-and-considerations-for-using-replication-with-database-mirroring"></a>将复制与数据库镜像一起使用的要求和注意事项  
 将复制与数据库镜像一起使用时，注意以下要求和注意事项：  
  
-   主体数据库和镜像数据库必须共享分发服务器。 建议此处使用远程分发服务器，如果发布服务器有意外故障转移，则远程分发服务器可以提供较大的容错能力。  
  
-   对于合并复制，以及对于使用只读订阅服务器或排队更新订阅服务器的事务复制，复制支持对发布数据库进行镜像。 不支持即时更新对等拓扑中的订阅服务器、Oracle 发布服务器、发布服务器并重新发布。  
  
-   存在于数据库外部的元数据和对象不复制到镜像数据库，包括登录名、作业、链接服务器等等。 如果要求镜像数据库中有元数据和对象，则必须手动复制它们。 有关详细信息，请参阅[角色切换后登录名和作业的管理 (SQL Server)](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)。  
  
## <a name="configuring-replication-with-database-mirroring"></a>配置复制以及数据库镜像  
 配置复制和数据库镜像包括五个步骤。 在下面的部分中将详细说明每个步骤。  
  
1.  配置发布服务器。  
  
2.  配置数据库镜像。  
  
3.  配置镜像数据库，使其使用与主体数据库相同的分发服务器。  
  
4.  配置用于故障转移的复制代理。  
  
5.  向复制监视器添加主体数据库和镜像数据库。  
  
 也可以相反的顺序执行步骤 1 和步骤 2。  
  
#### <a name="to-configure-database-mirroring-for-a-publication-database"></a>配置发布数据库的数据库镜像  
  
1.  配置发布服务器：  
  
    1.  建议使用远程分发服务器。 有关如何配置分发的详细信息，请参阅 [配置分发](../../relational-databases/replication/configure-distribution.md)。  
  
    2.  可以为快照发布及事务发布和/或合并发布启用数据库。 对于将包含多种发布类型的镜像数据库，必须使用 [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)为同一节点上的两种类型都启用数据库。 例如，可以在主体数据库上执行下面的存储过程调用：  
  
        ```  
        exec sp_replicationdboption @dbname='<PublicationDatabase>', @optname='publish', @value=true;  
        exec sp_replicationdboption @dbname='<PublicationDatabase>', @optname='mergepublish', @value=true;  
        ```  
  
         有关创建发布的详细信息，请参阅 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)。  
  
2.  配置数据库镜像。 有关详细信息，请参阅[使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md) 和[设置数据库镜像 (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)。  
  
3.  配置镜像的分发。 将镜像名称指定为发布服务器，并指定主体数据库使用的同一分发服务器和快照文件夹。 例如，如果想使用存储过程配置复制，可以在分发服务器上执行 [sp_adddistpublisher](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) ，然后在镜像上执行 [sp_adddistributor](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) 。 对于 **sp_adddistpublisher**：  
  
    -   将 \@publisher 参数的值设置为镜像的网络名称。  
  
    -   将 \@working_directory 参数的值设置为主体数据库使用的快照文件夹。  
  
4.  为“-PublisherFailoverPartner”代理参数指定镜像名称。 下列代理在故障转移后需要使用此代理参数来标识镜像：  
  
    -   快照代理（对于所有发布）  
  
    -   日志读取器代理（对于所有事务发布）  
  
    -   队列读取器代理（对于支持排队更新订阅的事务发布）  
  
    -   合并代理（对于合并订阅）  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 复制侦听程序（replisapi.dll：用于使用 Web 同步进行同步的合并订阅）  
  
    -   SQL 合并 ActiveX 控件（对于与控件同步的合并订阅）  
  
     分发代理和分发 ActiveX 控件没有此参数，因为它们不连接到发布服务器。  
  
     对代理参数所做的更改在下次启动代理时生效。 如果代理连续运行，则必须停止该代理，然后重新启动。 可以在代理配置文件中和从命令提示符指定参数。 有关详细信息，请参阅：  
  
    -   [查看和修改复制代理命令提示符参数 (SQL Server Management Studio)](../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
    -   [Replication Agent Executables Concepts](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  
     建议将“-PublisherFailoverPartner”添加到代理配置文件，然后在配置文件中指定镜像名称。 例如，如果您通过存储过程配置复制，请执行以下操作：  
  
    ```  
    -- Execute sp_help_agent_profile in the context of the distribution database to get the list of profiles.  
    -- Select the profile id of the profile that needs to be updated from the result set.  
    -- In the agent_type column returned by sp_help_agent_profile:   
    -- 1 = Snapshot Agent; 2 = Log Reader Agent; 3 = Distribution Agent; 4 = Merge Agent; 9 = Queue Reader Agent.  
  
    exec sp_help_agent_profile;  
  
    -- Setting the -PublisherFailoverPartner parameter in the default Snapshot Agent profile (profile 1).  
    -- Execute sp_add_agent_parameter in the context of the distribution database.  
    exec sp_add_agent_parameter @profile_id = 1, @parameter_name = N'-PublisherFailoverPartner', @parameter_value = N'<Failover Partner Name>';  
  
    -- Setting the -PublisherFailoverPartner parameter in the default Merge Agent profile (profile 6).  
    -- Execute sp_add_agent_parameter in the context of the distribution database.  
    exec sp_add_agent_parameter @profile_id = 6, @parameter_name = N'-PublisherFailoverPartner', @parameter_value = N'<Failover Partner Name>';  
    ```  
  
5.  向复制监视器添加主体数据库和镜像数据库。 有关详细信息，请参阅 [从复制监视器中添加和删除发布服务器](../../relational-databases/replication/monitor/add-and-remove-publishers-from-replication-monitor.md)。  
  
## <a name="maintaining-a-mirrored-publication-database"></a>维护镜像发布数据库  
 维护镜像发布数据库与维护非镜像数据库基本相同，需要注意以下事项：  
  
-   管理和监视必须在活动服务器上进行。 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，发布仅出现在活动服务器的 **“本地发布”** 文件夹下方。 例如，如果故障转移到镜像数据库，则发布显示在镜像数据库中，而不再显示在主体数据库中。 如果数据库故障转移到镜像数据库，则可能需要手动刷新 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 和复制监视器才能反映更改。  
  
-   复制监视器会在对象树中同时显示主体数据库和镜像数据库的“发布服务器”节点。 如果主体数据库位于活动服务器，则仅在复制监视器的主体数据库节点下显示发布信息。  
  
     如果镜像数据库位于活动服务器：  
  
    -   代理出错时，只在主体数据库节点上指出错误，而不在镜像数据库节点上指出。  
  
    -   主体数据库不可用时，主体数据库节点和镜像数据库节点会显示相同的发布列表。 这时，应对镜像数据库节点下的发布执行监视。  
  
-   当使用存储过程或复制管理对象 (RMO) 在镜像数据库上管理复制时，对于需要指定发布服务器名称的情况，必须指定已经为复制启用了数据库的实例的名称。 若要确定相应的名称，请使用函数 [publishingservername](../../t-sql/functions/replication-functions-publishingservername.md)。  
  
     如果对发布数据库做了镜像，则镜像数据库中存储的复制元数据与主体数据库中存储的元数据相同。 因此，对于为主体数据库上的复制启用的发布数据库，在镜像数据库上的系统表中存储的发布服务器实例名称是主体数据库的名称，而不是镜像数据库的名称。 如果发布数据库故障转移到镜像数据库，则这种情况会影响复制的配置和维护。 例如，如果故障转移后使用镜像数据库上的存储过程配置复制，并且希望添加对主体数据库上启用的发布数据库的请求订阅，则必须为 sp_addpullsubscription 或 sp_addmergepullsubscription 的 \@publisher 参数指定主体数据库名称，而不是镜像数据库名称。  
  
     如果故障转移到镜像数据库后在镜像数据库上启用发布数据库，则存储在系统表中的发布服务器实例名称是镜像数据库的名称；在此情况下，应将镜像数据库的名称用于 \@publisher 参数。  
  
    > [!NOTE]  
    >  某些情况下，如 sp_addpublication，只有非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器支持 \@publisher 参数；在这些情况下，它与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库镜像无关。  
  
-   若要在故障转移后在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中同步订阅：请同步来自订阅服务器的请求订阅以及来自活动发布服务器的推送订阅。  
  
### <a name="replication-behavior-if-mirroring-is-removed"></a>删除镜像后的复制行为  
 如果从已发布数据库中删除了数据库镜像，请谨记以下情况：  
  
-   如果主体数据库上的发布数据库不再有镜像，则复制依照原主体数据库无改变地继续工作。  
  
-   如果发布数据库从主体数据库故障转移到镜像数据库，而且镜像关系随后被禁用或删除，则复制代理不再对镜像数据库起作用。 如果主体数据库永久丢失，请禁用复制，然后用指定为发布服务器的像镜重新配置复制。  
  
-   如果完全删除数据库镜像，镜像数据库将处于恢复状态，必须还原才能起作用。 就复制而言，已恢复数据库的行为取决于是否指定了 KEEP_REPLICATION 选项。 在将已发布数据库还原到创建备份的服务器以外的服务器上时，此选项强制还原操作保留复制设置。 仅当另一个发布数据库不可用时才使用 KEEP_REPLICATION 选项。 如果另一个发布数据库仍然完好且仍在复制，则不支持此选项。 有关 KEEP_REPLICATION 的详细信息，请参阅 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)。  
  
## <a name="log-reader-agent-behavior"></a>日志读取器代理的行为  
 下表说明了日志读取器代理对于数据库镜像各种运行模式的行为。  
  
|运行模式|镜像数据库不可用时日志读取器代理的行为|  
|--------------------|------------------------------------------------------------|  
|具有自动故障转移的高安全性模式|如果镜像数据库不可用，则日志读取器代理将命令传播到分发数据库。 直到镜像数据库回到联机状态并且具有来自主体数据库的所有事务，主体数据库才能故障转移到镜像数据库。|  
|高性能模式|如果镜像数据库不可用，则主体数据库公开（即无镜像）运行。 但是，日志读取器代理仅复制那些在镜像服务器上受保护的事务。 如果是强制服务，并且镜像服务器充当主体服务器的角色，则日志读取器代理将依照镜像服务器工作并开始拾取新事务。<br /><br /> 请注意，如果镜像服务器落后于主体服务器，就会加大复制滞后。|  
|不带自动故障转移的高安全性模式|保证所有已提交的事务均在镜像服务器的磁盘上受到保护。 日志读取器代理仅复制那些在镜像服务器上受保护的事务。 如果镜像服务器不可用，则主体服务器禁止数据库中的进一步活动；因此，日志读取器代理没有事务可以复制。|  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 复制](../../relational-databases/replication/sql-server-replication.md)   
 [日志传送和复制 (SQL Server)](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md)  
  
  
