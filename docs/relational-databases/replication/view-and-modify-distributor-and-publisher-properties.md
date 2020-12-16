---
title: 查看和修改分发服务器和发布服务器属性
description: 了解如何使用 SQL Server Management Studio （SSMS）、Transact-SQL (T-SQL) 或复制管理对象 (RMO) 修改分发服务器和发布服务器的属性。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing replication properties
- Distributors [SQL Server replication], modifying
- modifying replication properties, Distributors
- Distributors [SQL Server replication], properties
ms.assetid: 5dae1d59-c377-4c6e-adc9-b68c5b328f79
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 7cd44046d1c21fc365adc0608592b5139270f97b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479528"
---
# <a name="view-and-modify-distributor-and-publisher-properties"></a>查看和修改分发服务器和发布服务器属性
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中查看和修改分发服务器和发布服务器属性。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [建议](#Recommendations)  
  
     [安全性](#Security)  
  
-   **查看和修改分发服务器和发布服务器属性，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [复制管理对象 (RMO)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   对于运行低于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 版本的发布服务器，sysadmin 固定服务器角色中的用户可以在“订阅服务器”页上注册订阅服务器。 从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]开始，不再需要为复制显式注册订阅服务器。  
  
###  <a name="security"></a><a name="Security"></a> Security  
 如果可能，请在运行时提示用户输入安全凭据。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-view-and-modify-distributor-properties"></a>查看和修改分发服务器属性  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中连接到分发服务器，然后展开服务器节点。  
  
2.  右键单击 **“复制”** 文件夹，然后单击 **“分发服务器属性”** 。  
  
3.  在“分发服务器属性 - \<Distributor>”对话框中查看和修改属性。  
  
    -   若要查看和修改分发数据库的属性，请在该对话框的“常规”页上单击该数据库的属性按钮 ( **...** )。  
  
    -   若要查看和修改与分发服务器关联的发布服务器属性，请在该对话框的 **“发布服务器”** 页面上单击发布服务器的属性按钮 ( **...** )。  
  
    -   若要访问复制代理的配置文件，请单击此对话框的 **“常规”** 页上的 **“默认配置文件”** 按钮。 有关详细信息，请参阅 [Replication Agent Profiles](../../relational-databases/replication/agents/replication-agent-profiles.md)。  
  
    -   若要更改管理存储过程在发布服务器上执行以及在分发服务器上更新信息时所用帐户的密码，请在该对话框的 **“发布服务器”** 页面上的 **“密码”** 和 **“确认密码”** 框中输入新密码。 有关详细信息，请参阅[保护分发服务器的安全](../../relational-databases/replication/security/secure-the-distributor.md)。  
  
4.  根据需要修改属性，然后单击 **“确定”** 。  
  
#### <a name="to-view-and-modify-publisher-properties"></a>查看和修改发布服务器属性  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中连接到发布服务器，然后展开服务器节点。  
  
2.  右键单击 **“复制”** 文件夹，然后单击 **“发布服务器属性”** 。  
  
3.  在“发布服务器属性 - <发布服务器>”对话框中查看和修改属性。  
  
    -   **sysadmin** 固定服务器角色中的用户可以在 **“发布数据库”** 页上为复制启用数据库。 启用数据库并不会发布该数据库，而是允许该数据库的 **db_owner** 固定数据库角色中的任何用户在该数据库中创建一个或多个发布。  
  
4.  根据需要修改属性，然后单击 **“确定”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 可以使用复制存储过程以编程方式查看发布服务器和分发服务器属性。  
  
#### <a name="to-view-distributor-and-distribution-database-properties"></a>查看分发服务器和分发数据库属性  
  
1.  执行 [sp_helpdistributor](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md) 可返回有关分发服务器、分发数据库和工作目录的信息。  
  
2.  执行 [sp_helpdistributiondb](../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md) 可返回指定的分发数据库的属性。  
  
#### <a name="to-change-distributor-and-distribution-database-properties"></a>更改分发服务器和分发数据库属性  
  
1.  在分发服务器上，执行 [sp_changedistributor_property](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md) 可修改分发服务器属性。  
  
2.  在分发服务器上，执行 [sp_changedistributiondb](../../relational-databases/system-stored-procedures/sp-changedistributiondb-transact-sql.md) 可修改分发数据库属性。  
  
3.  在分发服务器上，执行 [sp_changedistributor_password](../../relational-databases/system-stored-procedures/sp-changedistributor-password-transact-sql.md) 可更改分发服务器密码。  
  
    > [!IMPORTANT]  
    >  如果可能，请在运行时提示用户输入安全凭据。 如果必须将凭据存储在脚本文件中，请确保该文件的安全以防受到未经授权的访问。  
  
4.  在分发服务器上，执行 [sp_changedistpublisher](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md) ，以便使用分发服务器更改发布服务器的属性。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 下面的示例 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本返回有关分发服务器和分发数据库的信息。  
  
 [!code-sql[HowTo#sp_helpdistributor](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_1.sql)]  
  
 [!code-sql[HowTo#sp_helpdistributiondb](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_2.sql)]  
  
 该示例更改分发服务器的保持期、连接到分发服务器时使用的密码以及分发服务器检查各种复制代理的状态时采用的时间间隔（也称为检测信号时间间隔）。  
  
> [!IMPORTANT]  
>  如果可能，请在运行时提示用户输入安全凭据。 如果必须将凭据存储在脚本文件中，请确保该文件的安全以防受到未经授权的访问。  
  
 [!code-sql[HowTo#sp_changedistributor_property](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_3.sql)]  
  
 [!code-sql[HowTo#sp_changedistributiondb](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_4.sql)]  
  
 [!code-sql[HowTo#sp_changedistributor_password](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_5.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
  
#### <a name="to-view-and-modify-distributor-properties"></a>查看和修改分发服务器属性  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与分发服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.ReplicationServer> 类的实例。 传递步骤 1 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象。  
  
3.  （可选）检查 <xref:Microsoft.SqlServer.Replication.ReplicationServer.IsDistributor%2A> 属性以验证当前连接到的服务器是否为分发服务器。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.Load%2A> 方法获取该服务器的属性。  
  
5.  （可选）若要更改属性，请为一个或多个可在 <xref:Microsoft.SqlServer.Replication.ReplicationServer> 对象上设置的分发服务器属性设置新值。  
  
6.  （可选）如果将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> 对象的 <xref:Microsoft.SqlServer.Replication.ReplicationServer> 属性设置为 **true**，则调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> 方法来提交对服务器的更改。  
  
#### <a name="to-view-and-modify-distribution-database-properties"></a>查看和修改分发数据库属性  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与分发服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.DistributionDatabase> 类的实例。 指定名称属性并传递步骤 1 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象。  
  
3.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该服务器的属性。 如果此方法返回 **false**，则该服务器上不存在指定名称的数据库。  
  
4.  （可选）若要更改属性，请为可以设置的 <xref:Microsoft.SqlServer.Replication.DistributionDatabase> 属性中的一个设置新值。  
  
5.  （可选）如果将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> 对象的 <xref:Microsoft.SqlServer.Replication.DistributionDatabase> 属性设置为 **true**，则调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> 方法来提交对服务器的更改。  
  
#### <a name="to-view-and-modify-publisher-properties"></a>查看和修改发布服务器属性  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.DistributionPublisher> 类的实例。 指定 <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Name%2A> 属性并传递步骤 1 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象。  
  
3.  （可选）若要更改属性，请为可以设置的 <xref:Microsoft.SqlServer.Replication.DistributionPublisher> 属性中的一个设置新值。  
  
4.  （可选）如果将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> 对象的 <xref:Microsoft.SqlServer.Replication.DistributionPublisher> 属性设置为 **true**，则调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> 方法来提交对服务器的更改。  
  
#### <a name="to-change-the-password-for-the-administrative-connection-from-the-publisher-to-the-distributor"></a>更改从发布服务器到分发服务器的管理连接的密码  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与分发服务器的连接。  
  
2.  创建的 <xref:Microsoft.SqlServer.Replication.ReplicationServer> 类的实例。  
  
3.  将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中创建的连接。  
  
4.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.Load%2A> 方法获取该对象的属性。  
  
5.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationServer.ChangeDistributorPassword%2A> 方法。 为 *password* 参数传递新的密码值。  
  
    > [!IMPORTANT]  
    >  如果可能，请在运行时提示用户输入安全凭据。 如果必须存储凭据，请使用 [Windows .NET Framework 提供的](/previous-versions/aa719848(v=vs.71)) Cryptographic Services [!INCLUDE[msCoName](../../includes/msconame-md.md)] （加密服务）。  
  
6.  （可选）执行下列步骤以更改每个使用该分发服务器的远程发布服务器上的密码：  
  
    1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
    2.  创建的 <xref:Microsoft.SqlServer.Replication.ReplicationServer> 类的实例。  
  
    3.  将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 6a 中创建的连接。  
  
    4.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.Load%2A> 方法获取该对象的属性。  
  
    5.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationServer.ChangeDistributorPassword%2A> 方法。 为 *password* 参数传递步骤 5 中的新密码值。  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 此示例显示如何更改分发和分发数据库属性。  
  
> [!IMPORTANT]  
>  若要避免将凭据存储在代码中，应当在运行时提供新分发服务器密码。  
  
 [!code-cs[HowTo#rmo_ChangeDistPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changedistpub)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeDistPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changedistpub)]  
  
## <a name="see-also"></a>另请参阅  
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [禁用发布和分发](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [“配置分发”](../../relational-databases/replication/configure-distribution.md)   
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [分发服务器和发布服务器信息脚本](../../relational-databases/replication/administration/distributor-and-publisher-information-script.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [查看发布服务器的信息和执行其任务（复制监视器）](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)  
  
