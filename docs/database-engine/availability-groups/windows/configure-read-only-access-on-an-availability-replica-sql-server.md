---
title: 配置对可用性组次要副本的只读访问
description: '将次要副本配置为只可对 Always On 可用性组进行读访问。 '
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- connection access to availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 22387419-22c4-43fa-851c-5fecec4b049b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1604aa22c53a24fa565061325b3c68a1640c490c
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584437"
---
# <a name="configure-read-only-access-to-a-secondary-replica-of-an-always-on-availability-group"></a>配置对 Always On 可用性组的次要副本的只读访问
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  默认情况下，允许对主要副本进行读写和读意向访问，不允许连接到 AlwaysOn 可用性组的次要副本。 本主题说明了如何通过使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]或 PowerShell 来配置 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中 AlwaysOn 可用性组的可用性副本的连接访问。  
  
 有关对次要副本允许只读访问的含义的信息以及有关对连接访问的介绍，请参阅[关于对可用性副本的客户端连接访问 (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md) 和[活动次要副本：可读次要副本（Always On 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)。  
  
 
##  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> 先决条件和限制  
  
-   若要配置不同的连接访问，您必须连接到承载主副本的服务器实例。  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
  
|任务|权限|  
|----------|-----------------|  
|在创建可用性组时配置副本|需要 **sysadmin** 固定服务器角色的成员资格，以及 CREATE AVAILABILITY GROUP 服务器权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。|  
|修改可用性副本|对可用性组要求 ALTER AVAILABILITY GROUP 权限、CONTROL AVAILABILITY GROUP 权限、ALTER ANY AVAILABILITY GROUP 权限或 CONTROL SERVER 权限。|  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **配置对可用性副本的访问**  
  
1.  在对象资源管理器中，连接到承载主副本的服务器实例，然后展开服务器树。  
  
2.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
3.  单击要更改其副本的可用性组。  
  
4.  右键单击该可用性副本，然后单击“属性”  。  
  
5.  在 **“可用性副本属性”** 对话框中，可以更改主角色和辅助角色的连接访问设置，如下所示：  
  
    -   对于辅助角色，从 **“可读取辅助角色”** 下拉列表中选择一个新值，如下所示：  
  
         **是**  
         不允许与此副本的辅助数据库的用户连接。 它们不可用于读访问。 这是默认设置。  
  
         **仅限读意向**  
         仅允许与此副本的辅助数据库的只读连接。 辅助数据库全都可用于读访问。  
  
         **是**  
         允许与此副本的辅助数据库的所有连接，但仅限读访问。 辅助数据库全都可用于读访问。  
  
    -   对于主角色，从 **“主角色中的连接”** 下拉列表中选择一个新值，如下所示：  
  
         **允许所有连接**  
         主副本中的数据库允许所有连接。 这是默认设置。  
  
         **允许读/写连接**  
         在 Application Intent 属性设置为 **ReadWrite** 或者未设置 Application Intent 连接属性时，将允许连接。 不允许 Application Intent 连接属性设置为 **ReadOnly** 的连接。 这可帮助阻止客户错误地将读意向工作负荷连接到主副本。 有关 Application Intent 连接属性的详细信息，请参阅 [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **配置对可用性副本的访问**  
  
> [!NOTE]  
>  有关此过程的示例，请参阅本节后面的 [示例 (Transact-SQL)](#TsqlExample)。  
  
1.  连接到承载主副本的服务器实例。  
  
2.  若要指定的是新可用性组的副本，请使用 [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句。 如果正在添加或修改现有可用性组的副本，请使用 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句。  
  
    -   若要配置辅助角色的连接访问，请在 ADD REPLICA 或 MODIFY REPLICA WITH 子句中指定 SECONDARY_ROLE 选项，如下所示：  
  
         SECONDARY_ROLE **(** ALLOW_CONNECTIONS **=** { NO | READ_ONLY | ALL } **)**  
  
         其中：  
  
         是  
         不允许与此副本的辅助数据库的直接连接。 它们不可用于读访问。 这是默认设置。  
  
         READ_ONLY  
         仅允许与此副本的辅助数据库的只读连接。 辅助数据库全都可用于读访问。  
  
         ALL  
         允许与此副本的辅助数据库的所有连接，但仅限读访问。 辅助数据库全都可用于读访问。  
  
3.  若要配置主角色的连接访问，请在 ADD REPLICA 或 MODIFY REPLICA WITH 子句中指定 PRIMARY_ROLE 选项，如下所示：  
  
     PRIMARY_ROLE **(** ALLOW_CONNECTIONS **=** { READ_WRITE | ALL } **)**  
  
     其中：  
  
     READ_WRITE  
     不允许 Application Intent 连接属性设置为 **ReadOnly** 的连接。  在 Application Intent 属性设置为 **ReadWrite** 或者未设置 Application Intent 连接属性时，将允许连接。 有关 Application Intent 连接属性的详细信息，请参阅 [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
     ALL  
     主副本中的数据库允许所有连接。 这是默认设置。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 下面的示例将辅助副本添加到名为 *AG2* 的可用性组。 一个独立服务器实例 *COMPUTER03\HADR_INSTANCE* 被指定为承载新的可用性副本。 将此副本配置为对主角色允许读写连接，对辅助角色仅允许读意向连接。  
  
```  
ALTER AVAILABILITY GROUP AG2   
   ADD REPLICA ON   
      'COMPUTER03\HADR_INSTANCE' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER03:7022',  
         PRIMARY_ROLE ( ALLOW_CONNECTIONS = READ_WRITE ),  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY )  
         );   
GO  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> 使用 PowerShell  
 **配置对可用性副本的访问**  
  
> [!NOTE]  
>  有关代码示例，请参阅本节后面的 [示例 (PowerShell)](#PSExample)。  
  
1.  将目录 (**cd**) 更改为托管主副本的服务器实例。  
  
2.  在将可用性副本添加到可用性组中时，使用 **New-SqlAvailabilityReplica** cmdlet。 在修改现有可用性副本时，使用 **Set-SqlAvailabilityReplica** cmdlet。 相关参数如下：  
  
    -   若要配置辅助角色的连接访问，请指定 **ConnectionModeInSecondaryRole**_secondary_role_keyword_ 参数，其中 *secondary_role_keyword* 等于以下值之一：  
  
         **AllowNoConnections**  
         不允许直接连接到辅助副本中的数据库，且不支持读取这些数据库。 这是默认设置。  
  
         **AllowReadIntentConnectionsOnly**  
         只允许连接应用程序意向属性设置为 **ReadOnly** 的辅助副本中的数据库。 有关此属性的详细信息，请参阅 [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
         **AllowAllConnections**  
         允许针对辅助副本中的数据库的所有连接进行只读访问。  
  
    -   若要配置主要角色的连接访问，请指定 **ConnectionModeInPrimaryRole**_primary_role_keyword_ 参数，其中 *primary_role_keyword* 等于以下值之一：  
  
         **AllowReadWriteConnections**  
         不允许 Application Intent 连接属性设置为 ReadOnly 的连接。 在 Application Intent 属性设置为 ReadWrite 或者未设置 Application Intent 连接属性时，将允许连接。 有关 Application Intent 连接属性的详细信息，请参阅 [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
         **AllowAllConnections**  
         主副本中的数据库允许所有连接。 这是默认设置。  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 **PowerShell 环境中使用** Get-Help [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
###  <a name="example-powershell"></a><a name="PSExample"></a> 示例 (PowerShell)  
 下面的示例将 **ConnectionModeInSecondaryRole** 和 **ConnectionModeInPrimaryRole** 参数均设置为 **AllowAllConnections**。  
  
```  
Set-Location SQLSERVER:\SQL\PrimaryServer\default\AvailabilityGroups\MyAg  
$primaryReplica = Get-Item "AvailabilityReplicas\PrimaryServer"  
Set-SqlAvailabilityReplica -ConnectionModeInSecondaryRole "AllowAllConnections" `   
-InputObject $primaryReplica  
Set-SqlAvailabilityReplica -ConnectionModeInPrimaryRole "AllowAllConnections" `   
-InputObject $primaryReplica  
  
```  
  
##  <a name="follow-up-after-configuring-read-only-access-for-an-availability-replica"></a><a name="FollowUp"></a> 跟进：为可用性副本配置只读访问后  
 **对可读取辅助副本的只读访问**  
  
-   使用 [bcp 实用工具](../../../tools/bcp-utility.md) 或 [sqlcmd 实用工具](../../../tools/sqlcmd-utility.md)时，你可以通过指定 **-K ReadOnly** 开关来对允许只读访问的任意次要副本指定只读访问。  
  
-   使客户端应用程序能够连接到可读取辅助副本：  
  
|先决条件|链接|  
|------------------|----------|  
|确保可用性组具有侦听器。|[创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)|  
|为可用性组配置只读路由。|[为可用性组配置只读路由 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)|  
  
 **在故障转移后可能会影响触发器和作业的因素**  
  
 如果您在非可读取辅助数据库或可读取辅助数据库上正运行时具有将失败的触发器和作业，则需要编写针对这些触发器和作业的脚本，以便对给定副本进行检查以确定该数据库是主数据库还是可读取辅助数据库。 若要获取该信息，请使用 [DATABASEPROPERTYEX](../../../t-sql/functions/databasepropertyex-transact-sql.md) 函数以返回数据库的 Updateability 属性。 若要标识只读数据库，请按如下所示将 READ_ONLY 指定为值：  
  
```  
DATABASEPROPERTYEX([db name],'UpdateAbility') = N'READ_ONLY'  
```  
  
 若要标识读写数据库，请将 READ_WRITE 指定为值：  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [为可用性组配置只读路由 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
  
-   [Always On：可读次要副本的价值主张](/archive/blogs/sqlserverstorageengine/alwayson-value-proposition-of-readable-secondary)  
  
-   [Always On：为什么存在两个选项用于为读取工作负荷启用辅助副本？](/archive/blogs/sqlserverstorageengine/alwayson-why-there-are-two-options-to-enable-a-secondary-replica-for-read-workload)  
  
-   [Always On：设置可读次要副本](/archive/blogs/sqlserverstorageengine/alwayson-setting-up-readable-seconary-replica)  
  
-   [Always On：我启用了可读次要副本，但我的查询却受阻？](/archive/blogs/sqlserverstorageengine/alwayson-i-just-enabled-readable-secondary-but-my-query-is-blocked)  
  
-   [Always On：在可读次要副本、只读数据库和数据库快照上提供最新统计信息](/archive/blogs/sqlserverstorageengine/alwayson-making-latest-statistics-available-on-readable-secondary-read-only-database-and-database-snapshot)  
  
-   [Always On：使用只读数据库、数据库快照和次要副本上的统计信息的挑战](/archive/blogs/sqlserverstorageengine/alwayson-challenges-with-statistics-on-readonly-database-database-snapshot-and-secondary-replica)  
  
-   [Always On：在次要副本上运行报表工作负荷时对主工作负荷的影响](/archive/blogs/sqlserverstorageengine/alwayson-impact-on-the-primary-workload-when-you-run-reporting-workload-on-the-secondary-replica)  
  
-   [Always On：将可读次要副本上的报表工作负荷映射到快照隔离的影响](/archive/blogs/sqlserverstorageengine/alwayson-impact-of-mapping-reporting-workload-on-readable-secondary-to-snapshot-isolation)  
  
-   [Always On：最大程度减少在次要副本上运行工作负荷时对 REDO 线程的阻止](/archive/blogs/sqlserverstorageengine/alwayson-minimizing-blocking-of-redo-thread-when-running-reporting-workload-on-secondary-replica)  
  
-   [Always On：可读次要副本和数据延迟](/archive/blogs/sqlserverstorageengine/alwayson-readable-secondary-and-data-latency)  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [活动次要副本：可读次要副本（Always On 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [关于对可用性副本的客户端连接访问 (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)  
  
