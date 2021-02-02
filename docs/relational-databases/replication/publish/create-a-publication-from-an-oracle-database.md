---
description: 从 Oracle 数据库创建发布
title: 从 Oracle 数据库创建发布 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- publications [SQL Server replication], Oracle databases
- Oracle publishing [SQL Server replication], configuring
ms.assetid: b3812746-14b0-4b22-809e-b4a95e1c8083
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: cc7660d1990f556ae3ccfb8d34d66519dd2ecd76
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076775"
---
# <a name="create-a-publication-from-an-oracle-database"></a>从 Oracle 数据库创建发布
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中从 Oracle 数据库创建发布。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [先决条件](#Prerequisites)  
  
-   **从 Oracle 数据库创建发布，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   在创建发布之前，必须在 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 分发服务器上安装 Oracle 软件，并配置 Oracle 数据库。 有关详细信息，请参阅[配置 Oracle 发布服务器](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可以使用新建发布向导，从 Oracle 数据库创建快照发布或事务发布。  
  
 首次从 Oracle 数据库创建发布时，必须在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 分发服务器上标识 Oracle 发布服务器（对于来自同一数据库的后续发布，不需要执行此操作）。 标识 Oracle 发布服务器的操作可以从新建发布向导或“分发服务器属性 - \<Distributor>”对话框中完成；本主题介绍了“分发服务器属性 - \<Distributor>”对话框。   
  
#### <a name="to-identify-the-oracle-publisher-at-the-sql-server-distributor"></a>在 SQL Server 分发服务器上标识 Oracle 发布服务器  
  
1.  在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]中，连接到要将 Oracle 发布服务器用作分发服务器的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，然后展开服务器节点。  
  
2.  右键单击 **“复制”** 文件夹，然后单击 **“分发服务器属性”** 。  
  
3.  在“分发服务器属性 - \<Distributor>”对话框的“发布服务器”页上，依次单击“添加”和“添加 Oracle 发布服务器”。     
  
4.  在 **“连接到服务器”** 对话框中，单击 **“选项”** 按钮。  
  
5.  在 **“登录”** 选项卡上：  
  
    1.  输入 Oracle 数据库实例名称，或者选择 **“服务器实例”** 组合框中的 **“浏览更多”** 。  
  
    2.  选择 **“Oracle 标准身份验证”** （建议）或 **“Windows 身份验证”** 。  
  
         如果选择了 **“Windows 身份验证”** ，则必须将 Oracle 服务器配置为允许使用 Windows 凭据进行连接（有关详细信息，请参阅 Oracle 文档），并且当前必须使用为复制管理用户架构指定的同一 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 帐户进行登录。  
  
    3.  如果选择 **“Oracle 标准身份验证”** ，则在配置过程中，请输入在 Oracle 发布服务器上创建的复制管理用户架构的登录名和密码。  
  
6.  在 **“连接属性”** 选项卡上，选择 **“网关”** 或 **“完整”** 发布服务器类型。  
  
     **“完整”** 选项用于为快照和事务发布提供所支持的完整的 Oracle 发布功能集。 **“网关”** 选项提供特定的设计优化，以提高复制作为系统间的网关时的性能。 如果计划在多个事务发布中发布同一个表，则无法使用 **“网关”** 选项。 如果选择 **“网关”** ，则一个表可以最多出现在一个事务发布中或出现在任意数量的快照发布中。  
  
7.  单击 **“连接”** ，创建到 Oracle 发布服务器的连接，并配置该连接以进行复制。 “连接至服务器”对话框将关闭，你将返回到“分发服务器属性 - \<Distributor>”对话框。   
  
    > [!NOTE]  
    >  如果网络配置出现问题，则在此将收到一条错误。 如果连接 Oracle 数据库时遇到问题，请参阅 [Troubleshooting Oracle Publishers](../../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md)中的“SQL Server 分发服务器无法连接到 Oracle 数据库实例”部分。  
  
8.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

#### <a name="to-create-a-publication-from-an-oracle-database"></a>从 Oracle 数据库创建发布  
  
1.  连接到要将 Oracle 发布服务器用作分发服务器的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹。  
  
3.  右键单击 **“本地发布”** 文件夹，然后单击 **“新建 Oracle 发布”** 。  
  
4.  在新建发布向导的 **“Oracle 发布服务器”** 页上，选择 Oracle 发布服务器。 如果未显示 Oracle 发布服务器，请单击 **“添加 Oracle 发布服务器”** ，逐步执行上一过程中的步骤。  
  
5.  在 **“发布类型”** 页上，选择 **“快照发布”** 或 **“事务发布”** 。  
  
6.  在 **“项目”** 页上，选择要发布的数据库对象。  
  
     也可以通过展开表并清除一个或多个列的复选框，来筛选掉表列。 单击 **“项目属性”** 可以查看和修改项目属性，还可以根据需要指定备用数据类型映射。 有关数据类型映射的详细信息，请参阅[为 Oracle 发布服务器指定数据类型映射](../../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)。  
  
7.  也可以在 **“筛选表行”** 页上，应用筛选器发布一个或多个表的数据子集。  
  
8.  仅在创建所有对象并将所有所需数据添加到订阅数据库后，才能清除 **“快照代理”** 页上的 **“立即创建快照”** 。  
  
9. 在 **“代理安全性”** 页上，指定快照代理（适用于所有发布）和日志读取器代理（适用于事务发布）的凭据。 代理将使用指定的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Windows 帐户上下文运行并连接到 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 分发服务器。 代理使用指定为复制管理用户架构的帐户上下文建立到 Oracle 数据库的连接。 有关详细信息，请参阅[配置 Oracle 发布服务器](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)。  
  
     有关每个代理所需权限的详细信息，请参阅 [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md) 和 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)。  
  
10. 在 **“向导操作”** 页上，根据需要，可以选择为发布编写脚本。 有关详细信息，请参阅 [Scripting Replication](../../../relational-databases/replication/scripting-replication.md)。  
  
11. 在 **“完成该向导”** 页上，指定发布的名称。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 在将 Oracle 数据库配置为发布服务器后，可以使用系统存储过程创建事务发布或快照发布，方法与从 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 发布服务器创建发布相同。  
  
#### <a name="to-create-an-oracle-publication"></a>创建 Oracle 发布  
  
1.  将 Oracle 数据库配置为发布服务器。 有关详细信息，请参阅[配置 Oracle 发布服务器](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)。  
  
2.  如果不存在远程分发服务器，请配置远程分发服务器。 有关详细信息，请参阅 [Configure Publishing and Distribution](../../../relational-databases/replication/configure-publishing-and-distribution.md)。  
  
3.  在 Oracle 发布服务器将使用的远程分发服务器上，执行 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)。 为 \@publisher 指定 Oracle 数据库实例的透明网络底层 (TNS) 名称，为 \@publisher_type 指定值 ORACLE 或 ORACLE GATEWAY   。 `Specify` 从 Oracle 发布服务器连接到远程 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 分发服务器时使用的以下安全模式之一：  
  
    -   若要使用 Oracle 标准身份验证（默认值），请将 \@security_mode 的值指定为 0，并将 \@login 和 \@password 分别指定为 Oracle 发布服务器上创建的复制管理用户架构在配置过程中所设定的登录名和密码   。  
  
        > [!IMPORTANT]  
        >  如果可能，请在运行时提示用户输入安全凭据。 如果将凭据存储在脚本文件中，则必须确保文件的安全以防受到未经授权的访问。  
  
    -   若要使用 Windows 身份验证，请将 \@security_mode 的值指定为 1 。  
  
        > [!NOTE]  
        >  若要使用 Windows 身份验证，Oracle 服务器必须配置为允许使用 Windows 凭据的连接（有关更多信息，请参见 Oracle 文档）；而且，您当前必须使用为复制管理用户架构指定的同一 Microsoft Windows 帐户登录到计算机。  
  
4.  为发布数据库创建日志读取器代理作业。  
  
    -   如果不确定是否存在针对某个已发布数据库的日志读取器代理作业，请在该 Oracle 发布服务器使用的分发服务器的分发数据库中执行 [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md)。 为 \@publisher 指定 Oracle 发布服务器的名称。 如果结果集为空，则必须创建一个日志读取器代理作业。  
  
    -   如果已经存在针对该发布数据库的日志读取器代理作业，请继续执行步骤 5。  
  
    -   在该 Oracle 发布服务器使用的分发服务器的分发数据库中，执行 [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)。 为 \@job_login 和 \@job_password 指定运行该代理所用的 Windows 凭据 。  
  
        > [!NOTE]  
        >  \@job_login 参数必须与步骤 3 中提供的登录名匹配。 不要提供发布服务器安全信息。 日志读取器代理使用步骤 3 中提供的安全信息连接到发布服务器。  
  
5.  在分发服务器上的分发数据库中，执行 [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) 以创建发布。 有关详细信息，请参阅 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)。  
  
6.  在分发服务器上的分发数据库中，执行 [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)。 为 \@publication 指定在步骤 4 中使用的发布名称，并为 \@job_name 和 \@password 指定运行快照代理所用的 Windows 凭据  。 若要在连接到发布服务器时使用 Oracle 标准身份验证，还必须为 \@publisher_security_mode 指定 0 值，并为 \@publisher_login 和 \@publisher_password 指定 Oracle 登录信息   。 此操作将为发布创建一个快照代理作业。  
  
## <a name="see-also"></a>另请参阅  
 [配置 Oracle 发布服务器](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)   
 [发布数据和数据库对象](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [为 Oracle 发布服务器配置事务集作业（复制 Transact-SQL 编程）](../../../relational-databases/replication/administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)   
 [Oracle 发布概述](../../../relational-databases/replication/non-sql/oracle-publishing-overview.md)   
 [授予 Oracle 权限的脚本](../../../relational-databases/replication/non-sql/script-to-grant-oracle-permissions.md)  
  
  
