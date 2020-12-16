---
title: 创建发布 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio、Transact-SQL 或复制管理对象在 SQL Server 中创建发布。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- publications [SQL Server replication], creating
- articles [SQL Server replication], defining
- adding articles
- articles [SQL Server replication], adding
ms.assetid: 52ee6de9-1d58-4cb9-8711-372bddbe7154
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: eb8096edc6b4b567272bf3808e5ecaad3eca403c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464708"
---
# <a name="create-a-publication"></a>Create a Publication
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中创建发布。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **创建发布和定义项目，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [复制管理对象 (RMO)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   发布和项目名称不能包括下列任何字符：%、\*、[ , ]、|、:、"、? , ' , \ , / , < , >. 如果数据库中的对象包括上述任意字符，并且你希望复制它们，那么必须在“项目属性 - \<Article>”对话框（可从向导中的“项目”页面获得）中指定一个与相应对象名称不同的项目名称 。  
  
###  <a name="security"></a><a name="Security"></a> Security  
 如果可能，请在运行时提示用户输入安全凭据。 如果必须存储凭据，请使用 [Windows .NET Framework 提供的](/previous-versions/aa719848(v=vs.71)) Cryptographic Services [!INCLUDE[msCoName](../../../includes/msconame-md.md)] （加密服务）。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可以使用新建发布向导创建发布和定义项目。 创建发布之后，可在“发布属性 - \<Publication>”对话框中查看和修改发布属性。 有关从 Oracle 数据库创建发布的信息，请参阅[从 Oracle 数据库创建发布](../../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md)。  
  
#### <a name="to-create-a-publication-and-define-articles"></a>创建发布和定义项目  
  
1.  在 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 中连接到发布服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再右键单击 **“本地发布”** 文件夹。  
  
3.  单击 **“新建发布”** 。  
  
4.  按照新建发布向导中的页完成以下任务：  
  
    -   如果尚未在服务器上配置分发，请指定分发服务器。 有关如何配置分发的详细信息，请参阅[配置发布和分发](../../../relational-databases/replication/configure-publishing-and-distribution.md)。  
  
         如果在 **“分发服务器”** 页上指定将发布服务器用作其自己的分发服务器（本地分发服务器），而未将服务器配置为分发服务器，则新建发布向导将配置该服务器。 在 **“快照文件夹”** 页中指定分发服务器的快照文件夹。 快照文件夹只是指定共享的目录。向此文件夹中执行读写操作的代理必须对其具有足够的访问权限。 有关正确保护文件夹的详细信息，请参阅[保护快照文件夹](../../../relational-databases/replication/security/secure-the-snapshot-folder.md)。  
  
         如果指定另一台服务器作为分发服务器，则必须在 **“管理密码”** 页上输入密码来连接发布服务器和分发服务器。 此密码必须与在远程分发服务器上启用发布服务器时所指定的密码相匹配。  
  
         有关详细信息，请参阅 [Configure Distribution](../../../relational-databases/replication/configure-distribution.md)。  
  
    -   选择发布数据库。  
  
    -   选择发布类型。 有关详细信息，请参阅[复制类型](../../../relational-databases/replication/types-of-replication.md)。  
  
    -   指定要发布的数据和数据库对象；（可选）筛选来自表项目的列，并设置项目属性。  
  
    -   可选择筛选来自表项目的行。 有关详细信息，请参阅[筛选已发布数据](../../../relational-databases/replication/publish/filter-published-data.md)。  
  
    -   设置快照代理调度。  
  
    -   指定运行下列复制代理和进行连接的凭证：  
  
         \- 用于所有发布的快照代理。  
  
         \- 用于所有事务发布的日志读取器代理。  
  
         \- 用于允许更新订阅的事务发布的队列读取器代理。  
  
         有关详细信息，请参阅 [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md) 和 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)。  
  
    -   （可选）编写发布脚本。 有关详细信息，请参阅 [Scripting Replication](../../../relational-databases/replication/scripting-replication.md)。  
  
    -   指定发布的名称。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 可以使用复制存储过程以编程方式创建发布。 使用的存储过程将取决于要创建的发布的类型。  
  
#### <a name="to-create-a-snapshot-or-transactional-publication"></a>创建快照发布或事务发布  
  
1.  在发布服务器上，对发布数据库执行 [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) 以允许使用快照复制或事务复制发布当前数据库。  
  
2.  对于事务发布，确定发布数据库中是否存在日志读取器代理作业。 （对于快照发布，不需要此步骤）。  
  
    -   如果发布数据库中存在日志读取器代理作业，则继续执行步骤 3。  
  
    -   如果无法确定已发布的数据库是否存在日志读取器代理作业，请在发布服务器上对发布数据库执行 [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md)。  
  
    -   如果结果集为空，则创建日志读取器代理作业。 在发布服务器上，执行[sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)。 为 \@job_name 和 \@password 指定运行该代理时所使用的 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 凭据 。 如果代理在连接到发布服务器时使用 SQL Server 身份验证，则还必须将 \@publisher_security_mode 的值指定为 0，并为 \@publisher_login 和 \@publisher_password 指定 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录信息   。 继续执行步骤 3。  
  
3.  在发布服务器上，执行[sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)。 为 \@publication指定发布名称，并且对于 \@repl_freq 参数，为快照发布指定 snapshot 值或为事务发布指定 continuous 值   。 指定任何其他发布选项。 这便定义了发布。  
  
    > [!NOTE]  
    >  发布名称不能包括下列字符：  
    >   
    >  % * [ ] | : " ? \ / < >  
  
4.  在发布服务器上，执行[sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)。 将 \@publication 指定为步骤 3 中使用的发布名称，并为 \@snapshot_job_name 和 \@password 指定运行该快照代理时所使用的 Windows 凭据  。 如果代理在连接到发布服务器时使用 SQL Server 身份验证，则还必须将 \@publisher_security_mode 的值指定为 0 并为 \@publisher_login 和 \@publisher_password 指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录信息   。 此操作将为发布创建一个快照代理作业。  
  
    > [!IMPORTANT]  
    >  使用远程分发服务器配置发布服务器时，为所有参数提供的值（包括 *job_login* 和 *job_password*）都会以纯文本方式发送到该分发服务器。 在执行此存储过程之前，应该对发布服务器及其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
5.  向发布添加项目。 有关详细信息，请参阅 [定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
6.  启动快照代理作业以为此发布生成初始快照。 有关详细信息，请参阅 [创建并应用初始快照](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
#### <a name="to-create-a-merge-publication"></a>创建合并发布  
  
1.  在发布服务器上，执行 [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) 以允许使用合并复制发布当前数据库。  
  
2.  在发布服务器上，对发布数据库执行 [sp_addmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)。 为 \@publication 指定发布的名称，并指定任何其他发布选项。 这便定义了发布。  
  
    > [!NOTE]  
    >  发布名称不能包括下列字符：  
    >   
    >  % * [ ] | : " ? \ / < >  
  
3.  在发布服务器上，执行[sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)。 将 \@publication 指定为步骤 2 中使用的发布名称，并为 \@snapshot_job_name 和 \@password 指定运行该快照代理时所使用的 Windows 凭据  。 如果代理在连接到发布服务器时使用 SQL Server 身份验证，则还必须将 \@publisher_security_mode 的值指定为 0 并为 \@publisher_login 和 \@publisher_password 指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录信息   。 此操作将为发布创建一个快照代理作业。  
  
    > [!IMPORTANT]  
    >  使用远程分发服务器配置发布服务器时，为所有参数提供的值（包括 *job_login* 和 *job_password*）都会以纯文本方式发送到该分发服务器。 在执行此存储过程之前，应该对发布服务器及其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
4.  向发布添加项目。 有关详细信息，请参阅 [定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
5.  启动快照代理作业以为此发布生成初始快照。 有关详细信息，请参阅 [创建并应用初始快照](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 此示例创建事务发布。 脚本变量用于传递创建快照代理和日志读取器代理作业时所需的 Windows 凭据。  
  
 [!code-sql[HowTo#sp_AddTranPub](../../../relational-databases/replication/codesnippet/tsql/create-a-publication_1.sql)]  
  
 此示例创建合并发布。 脚本变量用于传递创建快照代理作业时所需的 Windows 凭据。  
  
 [!code-sql[HowTo#sp_AddMergePub](../../../relational-databases/replication/codesnippet/tsql/create-a-publication_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
 可以使用复制管理对象 (RMO) 以编程方式创建发布。 用于创建发布的 RMO 类取决于所创建发布的类型。  
  
#### <a name="to-create-a-snapshot-or-transactional-publication"></a>创建快照发布或事务发布  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  为发布数据库创建 <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> 类的实例，将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 的实例，然后调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法。 如果 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 返回 **false**，请验证该数据库是否存在。  
  
3.  如果 <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledTransPublishing%2A> 属性为 **false**，请将其设置为 **true**。  
  
4.  对于事务发布，请检查 <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentExists%2A> 属性的值。 如果该属性为 **true**，说明已经存在一个针对此数据库的日志读取器代理作业。 如果该属性为 **false**，请执行如下操作：  
  
    -   设置 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> 和 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> 或 <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentProcessSecurity%2A> 字段，为运行日志读取器代理所用的 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 帐户提供凭据。  
  
        > [!NOTE]  
        >  如果发布是由 <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentProcessSecurity%2A> 固定服务器角色的成员创建的，则不需要设置 **P:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentProcessSecurity** 。 在这种情况下，代理会模拟 SQL Server Agent 帐户。 有关详细信息，请参阅 [复制代理安全模式](../../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
    -   （可选）在使用 SQL Server 身份验证连接到发布服务器时设置 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> 的 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> 和 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> 或 <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentPublisherSecurity%2A> 字段。  
  
    -   调用 <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.CreateLogReaderAgent%2A> 方法，为该数据库创建日志读取器代理作业。  
  
5.  创建 <xref:Microsoft.SqlServer.Replication.TransPublication> 类的实例，并设置此对象的以下属性：  
  
    -   将 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 设置为步骤 1 中的 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>设置为已发布的数据库的名称。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>属性设置为发布的名称。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.PublicationType> 设置为 <xref:Microsoft.SqlServer.Replication.PublicationType.Transactional> 或 <xref:Microsoft.SqlServer.Replication.PublicationType.Snapshot>。  
  
    -   设置 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> 的 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> 和 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> 字段，为运行快照代理所用的 Windows 帐户提供凭据。 如果使用 Windows 身份验证，在快照代理连接到本地分发服务器或建立远程连接时，也会使用此帐户。  
  
        > [!NOTE]  
        >  如果发布是由 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> 固定服务器角色的成员创建的，则不需要设置 **P:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentProcessSecurity** 。 在这种情况下，代理会模拟 SQL Server Agent 帐户。 有关详细信息，请参阅 [复制代理安全模式](../../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
    -   （可选）如果使用 SQL Server 身份验证连接到发布服务器，设置 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> 的 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> 和 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> 或 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentPublisherSecurity%2A> 字段。  
  
    -   （可选）使用“或”逻辑 OR 运算符（在 Visual C# 中为 **|** ，在 Visual Basic 中为 **Or** ）和“异或”逻辑 OR 运算符（在 Visual C# 中为 **^** ，在 Visual Basic 中为 **Xor** ），将 <xref:Microsoft.SqlServer.Replication.PublicationAttributes> 属性的值设置为 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 属性的值。  
  
    -   （可选）将 <xref:Microsoft.SqlServer.Replication.TransPublication.PublisherName%2A> 设置为发布服务器的名称（如果发布服务器不是 SQL Server 发布服务器）。  
  
6.  调用 <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> 方法来创建发布。  
  
    > [!IMPORTANT]  
    >  在使用远程分发服务器配置发布服务器时，为所有属性提供的值（包括 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A>）都会以纯文本形式发送到该分发服务器。 调用 <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> 方法之前，应先对发布服务器与其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
7.  调用 <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> 方法，为发布创建快照代理作业。  
  
#### <a name="to-create-a-merge-publication"></a>创建合并发布  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  为发布数据库创建 <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> 类的实例，将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置为步骤 1 中的 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 的实例，然后调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法。 如果 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 返回 **false**，请验证该数据库是否存在。  
  
3.  如果 <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledMergePublishing%2A> Property is **false**，请将其设置为 **true**，然后调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A>。  
  
4.  创建 <xref:Microsoft.SqlServer.Replication.MergePublication> 类的实例，并设置此对象的以下属性：  
  
    -   将 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 设置为步骤 1 中的 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>设置为已发布的数据库的名称。  
  
    -   将 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>属性设置为发布的名称。  
  
    -   设置 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> 的 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> 和 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> 字段，为运行快照代理所用的 Windows 帐户提供凭据。 如果使用 Windows 身份验证，在快照代理连接到本地分发服务器或建立远程连接时，也会使用此帐户。  
  
        > [!NOTE]  
        >  如果发布是由 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> 固定服务器角色的成员创建的，则不需要设置 **P:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentProcessSecurity** 。 有关详细信息，请参阅 [复制代理安全模式](../../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
    -   （可选）使用“或”逻辑 OR 运算符（在 Visual C# 中为 **|** ，在 Visual Basic 中为 **Or** ）和“异或”逻辑 OR 运算符（在 Visual C# 中为 **^** ，在 Visual Basic 中为 **Xor** ），将 <xref:Microsoft.SqlServer.Replication.PublicationAttributes> 属性的值设置为 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 属性的值。  
  
5.  调用 <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> 方法来创建发布。  
  
    > [!IMPORTANT]  
    >  在使用远程分发服务器配置发布服务器时，为所有属性提供的值（包括 <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A>）都会以纯文本形式发送到该分发服务器。 调用 <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> 方法之前，应先对发布服务器与其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
6.  调用 <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> 方法，为发布创建快照代理作业。  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> 示例 (RMO)  
 本例将 AdventureWorks 数据库设置为支持事务发布，定义了一个日志读取器代理作业，并且创建了 AdvWorksProductTran 发布。 必须为此发布定义一个项目。 创建日志读取器代理作业和快照代理作业所需的 Windows 帐户凭据在运行时进行传递。 若要了解如何使用 RMO 定义快照项目和事务项目，请参阅 [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)。  
  
 [!code-cs[HowTo#rmo_CreateTranPub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createtranpub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateTranPub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createtranpub)]  
  
 本例将 AdventureWorks 数据库设置为支持合并发布，并且创建了 AdvWorksSalesOrdersMerge 发布。 仍然必须为此发布定义项目。 创建快照代理作业所需的 Windows 帐户凭据在运行时进行传递。 若要了解如何使用 RMO 定义合并项目，请参阅 [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)。  
  
 [!code-cs[HowTo#rmo_CreateMergePub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepub)]  
  
## <a name="see-also"></a>另请参阅  
 [将 sqlcmd 与脚本变量结合使用](../../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)   
 [发布数据和数据库对象](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Replication Management Objects Concepts](../../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)   
 [查看和修改发布属性](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [“配置分发”](../../../relational-databases/replication/configure-distribution.md)   
 [保护分发服务器](../../../relational-databases/replication/security/secure-the-distributor.md)   
 [保护发布服务器](../../../relational-databases/replication/security/secure-the-publisher.md)  
  
