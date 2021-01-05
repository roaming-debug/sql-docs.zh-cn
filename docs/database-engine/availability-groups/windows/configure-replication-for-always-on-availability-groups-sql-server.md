---
title: 使用可用性组配置复制
description: 了解使用 Always On 可用性组配置 SQL Server 复制所需的详细过程。
ms.custom: seodec18
ms.date: 01/25/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 4e001426-5ae0-4876-85ef-088d6e3fb61c
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7a8fc6787491c50b490ca0b9c25ab0d45acd52f2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97639973"
---
# <a name="configure-replication-with-always-on-availability-groups"></a>使用 AlwaysOn 可用性组配置复制

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  配置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制和 AlwaysOn 可用性组涉及七个步骤。 在下面的各节中将详细说明每个步骤。  
  
##  <a name="1-configure-the-database-publications-and-subscriptions"></a><a name="step1"></a> 1.配置数据库发布和订阅  
 **配置分发服务器**  
  
 无法使用 SQL Server 2012 和 SQL Server 2014 将分发数据库置于可用性组中。 SQL 2016 及更高版本支持将分发数据库放置在可用性组中，但用于合并、双向或对等复制拓扑的分发数据库除外。 有关详细信息，请参阅[配置可用性组中的分发数据库](../../../relational-databases/replication/configure-distribution-availability-group.md)。
  
1.  在分发服务器上配置分发。 如果要使用存储过程来进行配置，则运行 **sp_adddistributor**。 使用 \@password 参数来标识在远程发布服务器连接到分发服务器时将使用的密码。 在设置远程分发服务器时，每台远程发布服务器上也将需要密码。  
  
    ```  
    USE master;  
    GO  
    EXEC sys.sp_adddistributor  
        @distributor = 'MyDistributor',  
        @password = '**Strong password for distributor**';  
    ```  
  
2.  在分发服务器上创建分发数据库。 如果要使用存储过程来进行配置，则运行 **sp_adddistributiondb**。  
  
    ```  
    USE master;  
    GO  
    EXEC sys.sp_adddistributiondb  
        @database = 'distribution',  
        @security_mode = 1;  
    ```  
  
3.  配置远程发布服务器。 如果要使用存储过程来配置分发服务器，则运行 **sp_adddistpublisher**。 \@security_mode 参数可用于确定如何将从复制代理运行的发布服务器验证存储过程连接到当前主要副本。 如果设置为 1，则使用 Windows 身份验证来连接到当前主副本。 如果设置为 0，则将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证与指定的 \@login 和 \@password 值一起使用 。 指定的登录名和密码必须在每个辅助副本上均有效才能让验证存储过程成功地连接到相应的副本。  
  
    > [!NOTE]  
    >  如果任何已修改的复制代理在分发服务器之外的计算机上运行，则使用 Windows 身份验证连接到主副本的方法将要求为副本主机之间的通信配置 Kerberos 身份验证。 使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名连接到当前主副本的方法无需 Kerberos 身份验证。  
  
    ```  
    USE master;  
    GO  
    EXEC sys.sp_adddistpublisher  
        @publisher = 'AGPrimaryReplicaHost',  
        @distribution_db = 'distribution',  
        @working_directory = '\\MyReplShare\WorkingDir',  
        @login = 'MyPubLogin',  
        @password = '**Strong password for publisher**';  
    ```  
  
 有关详细信息，请参阅 [sp_adddistpublisher (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)。  
  
 **在原始发布服务器上配置发布服务器**  
  
1.  配置远程分发。 如果要使用存储过程来配置发布服务器，则运行 **sp_adddistributor**。 为 \@password 指定与在分发服务器上运行 sp_adddistrbutor 时使用的相同的值来设置分发。  
  
    ```  
    exec sys.sp_adddistributor  
        @distributor = 'MyDistributor',  
        @password = 'MyDistPass'  
    ```  
  
2.  启用数据库复制。 如果要使用存储过程来配置发布服务器，则运行 **sp_replicationdboption**。 如果要为数据库同时配置事务复制和合并复制，则必须分别启用它们。  
  
    ```  
    USE master;  
    GO  
    EXEC sys.sp_replicationdboption  
        @dbname = 'MyDBName',  
        @optname = 'publish',  
        @value = 'true';  
  
    EXEC sys.sp_replicationdboption  
        @dbname = 'MyDBName',  
        @optname = 'merge publish',  
        @value = 'true';  
    ```  
  
3.  创建复制发布、文章和订阅。 有关如何配置复制的详细信息，请参阅“发布数据和数据库对象”。  
  
##  <a name="2-configure-the-always-on-availability-group"></a><a name="step2"></a> 2.配置 AlwaysOn 可用性组  
 在目标主副本上，创建包含已发布的（或即将要发布的）数据库作为成员数据库的可用性组。 如果使用可用性组向导，则您可允许该向导最初同步辅助副本数据库，或者您可以使用备份和还原手动执行初始化。  
  
 为可用性组创建一个 DNS 侦听器，复制代理将使用它连接到当前主副本。 指定的侦听器名称将用作原始发布服务器/已发布数据库对的重定向的目标。 例如，如果您使用 DDL 来配置可用性组，则可使用以下代码示例为名为 `MyAG` 的现有可用性组指定可用性组侦听器：  
  
```  
ALTER AVAILABILITY GROUP 'MyAG'   
    ADD LISTENER 'MyAGListenerName' (WITH IP (('10.120.19.155', '255.255.254.0')));  
```  
  
 有关详细信息，请参阅[创建和配置可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)。  

  
##  <a name="3-ensure-that-all-of-the-secondary-replica-hosts-are-configured-for-replication"></a><a name="step3"></a> 3.确保对所有次要副本主机配置复制  
 在每个辅助副本主机上，确保已将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置为支持复制。 可在每个辅助副本主机上运行以下查询来确定是否安装了复制功能：  
  
```  
USE master;  
GO  
DECLARE @installed int;  
EXEC @installed = sys.sp_MS_replication_installed;  
SELECT @installed;  
```  
  
 如果 \@installed 设置为 0，则必须将复制功能添加到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装中。  
  
##  <a name="4-configure-the-secondary-replica-hosts-as-replication-publishers"></a><a name="step4"></a> 4.将辅助副本主机配置为复制发布服务器  
 辅助副本不能充当复制发布服务器或重新发布服务器，但必须配置复制以便在故障转移之后辅助副本可以接管。 在分发服务器上，为每个辅助副本主机配置分发。 指定在向分发服务器添加原始发布服务器时所指定的相同的分发数据库和工作目录。 如果使用存储过程来配置分发，请使用 **sp_adddistpublisher** 将远程发布服务器与分发服务器关联起来。 如果对原始发布服务器使用了 \@login 和 \@password，则在你添加辅助副本主机作为发布服务器时为每个辅助副本主机指定相同的值 。  
  
```  
EXEC sys.sp_adddistpublisher  
    @publisher = 'AGSecondaryReplicaHost',  
    @distribution_db = 'distribution',  
    @working_directory = '\\MyReplShare\WorkingDir',  
    @login = 'MyPubLogin',  
    @password = '**Strong password for publisher**';  
```  
  
 在每个辅助副本主机上配置分发。 将原始发布服务器的分发服务器标识为远程分发服务器。 使用最初在分发服务器上运行 **sp_adddistributor** 时所使用的密码。 如果要使用存储过程来配置分发，请使用 sp_adddistributor 的 \@password 参数来指定密码。  
  
```  
EXEC sp_adddistributor   
    @distributor = 'MyDistributor',  
    @password = '**Strong password for distributor**';  
```  
  
 在每个辅助副本主机上，确保数据库发布的推送订阅服务器显示为链接服务器。 如果要使用存储过程来配置远程发布服务器，请使用 **sp_addlinkedserver** 将订阅服务器（如果尚未存在）作为链接服务器添加到发布服务器。  
  
```  
EXEC sys.sp_addlinkedserver   
    @server = 'MySubscriber';  
```  
  
##  <a name="5-redirect-the-original-publisher-to-the-ag-listener-name"></a><a name="step5"></a> 5.将原始发布服务器重定向到 AG 侦听器名称  
 在分发数据库上的分发服务器中，运行存储过程 **sp_redirect_publisher** 以将原始发布服务器和已发布数据库与可用性组的可用性组侦听程序名称关联起来。  
  
```  
USE distribution;  
GO  
EXEC sys.sp_redirect_publisher   
@original_publisher = 'MyPublisher',  
    @publisher_db = 'MyPublishedDB',  
    @redirected_publisher = 'MyAGListenerName';  
```  
  
##  <a name="6-run-the-replication-validation-stored-procedure-to-verify-the-configuration"></a><a name="step6"></a> 6.运行复制验证存储过程以验证配置  
 在分发数据库上的分发服务器中，运行存储过程 **sp_validate_replica_hosts_as_publishers** 以确认现已将所有副本主机配置为充当已发布数据库的发布服务器。  
  
```  
USE distribution;  
GO  
DECLARE @redirected_publisher sysname;  
EXEC sys.sp_validate_replica_hosts_as_publishers  
    @original_publisher = 'MyPublisher',  
    @publisher_db = 'MyPublishedDB',  
    @redirected_publisher = @redirected_publisher output;  
```  
  
 应在每个可用性组副本主机上使用具有足够授权的登录名来运行存储过程 **sp_validate_replica_hosts_as_publishers** ，以查询有关可用性组的信息。 与 **sp_validate_redirected_publisher** 不同，它将使用调用方的凭据，且不会使用保留在 msdb.dbo.MSdistpublishers 中的登录名来连接可用性组副本。  
  
> [!NOTE]  
>  在验证不允许读取访问或要求指定读取意图的次要副本主机时，**sp_validate_replica_hosts_as_publishers** 将失败，并显示以下错误。  
>   
>  消息 21899，级别 11，状态 1，过程 **sp_hadr_verify_subscribers_at_publisher**，第 109 行  
>   
>  重定向的发布服务器“MyReplicaHostName”处的查询失败，该查询用于确定是否有原始发布服务器“MyOriginalPublisher”的订阅服务器的 sysserver 条目，出现错误“976”，错误消息“错误 976，级别 14，状态 1，消息：目标数据库‘MyPublishedDB’正参与某个可用性组，查询当前无法访问该数据库。 数据移动被挂起，或者未启用可用性副本以便用于读访问。 若要允许对该可用性组中的这一数据库和其他数据库进行只读访问，请对组中一个或多个辅助可用性副本启用只读访问权限。  有关详细信息，请参阅 **联机丛书中的** ALTER AVAILABILITY GROUP [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 语句。  
>   
>  副本主机“MyReplicaHostName”遇到了一个或多个发布服务器验证错误。  
  
 这是预期的行为。 必须通过在主机上直接查询 sysserver 条目来验证这些次要副本主机上是否存在订阅服务器条目。  
  
##  <a name="7-add-the-original-publisher-to-replication-monitor"></a><a name="step7"></a> 7.向复制监视器添加原始发布服务器  
 在每个可用性组副本上，向复制监视器添加原始发布服务器。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **复制**  
  
-   [维护 AlwaysOn 发布数据库 (SQL Server)](../../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)  
  
-   [复制、更改跟踪、更改数据捕获和 AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)  
  
-   [复制管理常见问题解答](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
 **创建和配置可用性组**  
  
-   [使用可用性组向导 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [使用“新建可用性组”对话框 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [创建可用性组 (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [创建可用性组 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
-   [在添加或修改可用性副本时指定终结点 URL (SQL Server)](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [为 AlwaysOn 可用性组创建数据库镜像终结点 (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [将辅助副本联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [为可用性组手动准备辅助数据库 (SQL Server)](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [将辅助数据库联接到可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
- [针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
- [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) 
- [AlwaysOn 可用性组：互操作性 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
- [SQL Server 复制](../../../relational-databases/replication/sql-server-replication.md)  
- 如果使用非默认端口，请参阅[在 Always On 可用性组中演练发布服务器、分发服务器、订阅服务器](https://repltalk.com/2019/03/09/walkthrough-publisher-distributor-subscriber-in-alwayson-availability-groups)。
