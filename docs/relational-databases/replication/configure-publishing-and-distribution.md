---
title: 配置发布和分发 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio、Transact-SQL 或复制管理对象在 SQL Server 中配置发布和分发。
ms.custom: ''
ms.date: 09/23/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], distribution
- distribution configuration [SQL Server replication]
- publishing [SQL Server replication], configuring
ms.assetid: 3cfc8966-833e-42fa-80cb-09175d1feed7
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 72e6a2ed252e895f1e55a7263c089e8e618ec115
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484909"
---
# <a name="configure-publishing-and-distribution"></a>配置发布和分发
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
 本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中配置发布和分发。

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前 

###  <a name="security"></a><a name="Security"></a> Security 
有关详细信息，请参阅[查看和修改复制安全设置](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)。

##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio 
可以使用新建发布向导或配置分发向导配置分发。 配置分发服务器后，查看并修改“分发服务器属性 - \<Distributor>”对话框中的属性。 若要配置分发服务器以便 `db_owner` 固定数据库角色的成员可以创建发布，或者要配置不是发布服务器的远程分发服务器，请使用“配置分发向导”。

#### <a name="to-configure-distribution"></a>配置分发 

1. 在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中，连接到将要作为分发服务器的服务器（许多情况下，发布服务器和分发服务器是同一服务器），然后展开该服务器节点。

2. 右键单击 **“复制”** 文件夹，然后单击 **“配置分发”** 。

3. 随着配置分发向导执行下列操作： 

  - 选择分发服务器。 若要使用本地分发服务器，请选择“ServerName 将充当自己的分发服务器; SQL Server 将创建分发数据库和日志”。 若要使用远程分发服务器，请选择 **“使用以下服务器作为分发服务器”** ，再选择一个服务器。 该服务器必须已配置为分发服务器，并且启用发布服务器使用此分发服务器。 有关详细信息，请参阅[在分发服务器上启用远程发布服务器 (SQL Server Management Studio)](../../relational-databases/replication/enable-a-remote-publisher-at-a-distributor-sql-server-management-studio.md)。

     如果选择远程分发服务器，则必须在 **“管理密码”** 页上输入从发布服务器连接到分发服务器的密码。 此密码必须与在远程分发服务器上启用发布服务器时所指定的密码相匹配。

  - 指定根快照文件夹（适用于本地分发服务器）。 快照文件夹只是指定共享的目录。向此文件夹中执行读写操作的代理必须对其具有足够的访问权限。 每个使用此分发服务器的发布服务器都在根文件夹下创建一个文件夹，而每个发布则在发布服务器文件夹下创建用于存储快照文件的文件夹。 有关正确保护文件夹的详细信息，请参阅[保护快照文件夹的安全](../../relational-databases/replication/security/secure-the-snapshot-folder.md)。

  - 指定分发数据库（适用于本地分发服务器）。 分发数据库存储事务性复制的所有复制和事务类型的元数据和历史记录数据。

  - 还可以让其他发布服务器使用该分发服务器（可选）。 如果其他发布服务器能够使用分发服务器，则必须在 **“分发服务器密码”** 页上输入从这些发布服务器连接到分发服务器的密码。

  - 还可以为配置设置编写脚本（可选）。 有关详细信息，请参阅 [Scripting Replication](../../relational-databases/replication/scripting-replication.md)。

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL 
可以使用复制存储过程以编程方式配置复制发布和分发。
### <a name="to-configure-publishing-using-a-local-distributor"></a>使用本地分发服务器配置发布
1. 执行 [sp_get_distributor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-get-distributor-transact-sql.md) 以确定服务器是否已配置为分发服务器。

  - 如果结果集中 `installed` 的值为 `0`，请在分发服务器上对 master 数据库执行 [sp_adddistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md)。

  - 如果结果集中 `distribution db installed` 的值为 `0`，请在分发服务器上对 master 数据库执行 [sp_adddistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md)。 对 `@database` 指定分发数据库名称。 也可以对 `@max_distretention` 指定最长事务保持期，并对 `@history_retention` 指定历史记录保持期。 如果要创建一个新的数据库，请指定所需的数据库属性参数。

2. 在分发服务器（也是发布服务器）上，执行 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)，同时对 `@working_directory` 指定用作默认快照文件夹的 UNC 共享。

   对于 SQL 托管实例上的分发服务器，将 Azure 存储帐户用于 `@working_directory`，将存储访问密钥用于 `@storage_connection_string`。 

3. 在发布服务器上执行 [sp_replicationdboption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)。 对 `@dbname` 指定要发布的数据库，对 `@optname` 指定复制类型，并对 `@value` 指定值 `true`。

#### <a name="to-configure-publishing-using-a-remote-distributor"></a>使用远程分发服务器配置发布 

1. 执行 [sp_get_distributor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-get-distributor-transact-sql.md) 以确定服务器是否已配置为分发服务器。

   - 如果结果集中 `installed` 的值为 `0`，请在分发服务器上对 master 数据库执行 [sp_adddistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md)。 对 `@password` 指定强密码。 发布服务器在连接到分发服务器时使用 `distributor_admin` 帐户的此密码。

   - 如果结果集中 `distribution db installed` 的值为 `0`，请在分发服务器上对 master 数据库执行 [sp_adddistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md)。 对 `@database` 指定分发数据库名称。 也可以对 `@max_distretention` 指定最长事务保持期，并对 `@history_retention` 指定历史记录保持期。 如果要创建一个新的数据库，请指定所需的数据库属性参数。

2. 在分发服务器上，执行 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)，同时对 `@working_directory` 指定用作默认快照文件夹的 UNC 共享。 如果分发服务器在连接到发布服务器时使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，还必须对 `@security_mode` 指定值 `0`，并对 `@login` 和 `@password` 指定 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录信息。

   对于 SQL 托管实例上的分发服务器，将 Azure 存储帐户用于 `@working_directory`，将存储访问密钥用于 `@storage_connection_string`。 

3. 在发布服务器上，对 master 数据库执行 [sp_adddistributor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md)。 对 `@password` 指定第 1 步中使用的强密码。 发布服务器在连接到分发服务器时将使用此密码。

4. 在发布服务器上执行 [sp_replicationdboption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)。 对 `@dbname` 指定要发布的数据库，对 `@optname` 指定复制类型，并对 `@value` 指定值 true。

###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL) 
下面的示例说明了如何以编程方式配置发布和分发。 在本示例中，使用脚本变量提供要配置为发布服务器和本地分发服务器的服务器的名称。 可以使用复制存储过程以编程方式配置复制发布和分发。

[!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/configure-publishing-and_1.sql)] 

##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO) 

#### <a name="to-configure-publishing-and-distribution-on-a-single-server"></a>在单个服务器上配置发布和分发 

1. 使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与服务器的连接。

2. 创建的 <xref:Microsoft.SqlServer.Replication.ReplicationServer> 类的实例。 传递步骤 1 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 。

3. 创建的 <xref:Microsoft.SqlServer.Replication.DistributionDatabase> 类的实例。

4. 将 <xref:Microsoft.SqlServer.Replication.DistributionDatabase.Name%2A> 属性设置为数据库名称，并将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 。

5. 通过调用 <xref:Microsoft.SqlServer.Replication.ReplicationServer.InstallDistributor%2A> 方法安装分布服务器。 传递步骤 3 中的 <xref:Microsoft.SqlServer.Replication.DistributionDatabase> 对象。

6. 创建的 <xref:Microsoft.SqlServer.Replication.DistributionPublisher> 类的实例。

7. 设置 <xref:Microsoft.SqlServer.Replication.DistributionPublisher>的以下属性： 

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Name%2A> - 发布服务器的名称。

  - <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> - 步骤 1 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 。

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.DistributionDatabase%2A> - 在步骤 5 中创建的数据库的名称。

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.WorkingDirectory%2A> - 用于访问快照文件的共享目录。

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.PublisherSecurity%2A> - 连接到发布服务器时使用的安全模式。 建议使用<xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> 。

8. 调用 <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Create%2A> 方法。

#### <a name="to-configure-publishing-and-distribution-using-a-remote-distributor"></a>使用远程分发服务器配置发布和分发 

1. 使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与远程分发服务器的连接。

2. 创建的 <xref:Microsoft.SqlServer.Replication.ReplicationServer> 类的实例。 传递步骤 1 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 。

3. 创建的 <xref:Microsoft.SqlServer.Replication.DistributionDatabase> 类的实例。

4. 将 <xref:Microsoft.SqlServer.Replication.DistributionDatabase.Name%2A> 属性设置为数据库名称，并将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 。

5. 通过调用 <xref:Microsoft.SqlServer.Replication.ReplicationServer.InstallDistributor%2A> 方法安装分布服务器。 指定安全密码（在连接到远程分发服务器时由发布服务器使用）和步骤 3 中的 <xref:Microsoft.SqlServer.Replication.DistributionDatabase> 对象。 有关详细信息，请参阅[保护分发服务器的安全](../../relational-databases/replication/security/secure-the-distributor.md)。

   > `IMPORTANT!!` 如果可能，请在运行时提示用户输入安全凭据。 如果必须存储凭据，请使用 [Windows .NET Framework 提供的](/previous-versions/aa719848(v=vs.71)) Cryptographic Services [!INCLUDE[msCoName](../../includes/msconame-md.md)] （加密服务）。

6. 创建的 <xref:Microsoft.SqlServer.Replication.DistributionPublisher> 类的实例。

7. 设置 <xref:Microsoft.SqlServer.Replication.DistributionPublisher>的以下属性： 

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Name%2A> - 本地发布服务器的名称。

  - <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> - 步骤 1 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 。

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.DistributionDatabase%2A> - 在步骤 5 中创建的数据库的名称。

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.WorkingDirectory%2A> - 用于访问快照文件的共享目录。

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.PublisherSecurity%2A> - 连接到发布服务器时使用的安全模式。 建议使用<xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> 。

8. 调用 <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Create%2A> 方法。

9. 使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与本地发布服务器的连接。

10. 创建的 <xref:Microsoft.SqlServer.Replication.ReplicationServer> 类的实例。 传递步骤 9 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 。

11. 调用 <xref:Microsoft.SqlServer.Replication.ReplicationServer.InstallDistributor%2A> 方法。 传递远程分发服务器的名称和在步骤 5 中指定的远程分发服务器的密码。

> [!IMPORTANT]
> 如果可能，请在运行时提示用户输入安全凭据。 如果必须存储凭据，请使用 Windows .NET Framework 提供的 [Cryptographic Services](/previous-versions/aa719848(v=vs.71)) （加密服务）。

###  <a name="example-rmo"></a><a name="PShellExample"></a> 示例 (RMO) 
可以通过使用复制管理对象 (RMO) 以编程方式配置复制发布和分发。

[!code-cs[HowTo#rmo_AddDistPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_adddistpub)] 

[!code-vb[HowTo#rmo_vb_AddDistPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_adddistpub)] 

## <a name="see-also"></a>另请参阅 
[查看和修改分发服务器和发布服务器属性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)  
[Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
[配置分发](../../relational-databases/replication/configure-distribution.md)  
[复制管理对象概念](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)  
[为 AlwaysOn 可用性组配置复制 (SQL Server)](../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)