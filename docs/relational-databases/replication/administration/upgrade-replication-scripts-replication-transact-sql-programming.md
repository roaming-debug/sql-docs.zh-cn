---
title: 升级复制脚本（复制 SP）
description: 了解如何使用复制存储过程来升级脚本，这些脚本可用于以编程方式配置复制拓扑。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- scripts [SQL Server replication], upgrading
- upgrading SQL Server, replicated databases
- upgrading replication applications
- replication [SQL Server], scripting
- replication [SQL Server], upgrading
- upgrading replicated databases
ms.assetid: 0b8720bd-f339-4842-bc8f-b35a46f6d3ee
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 31850fe7f9ecf78af666faced53f552646de672a
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364681"
---
# <a name="upgrade-replication-scripts-replication-transact-sql-programming"></a>升级复制脚本（复制 Transact-SQL 编程）
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本文件可用于以编程方式配置复制拓扑。 有关详细信息，请参阅[复制系统存储过程概念](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)。  
  
> [!IMPORTANT]  
>  虽然不需要升级由 **sysadmin** 角色的成员执行的脚本，我们仍建议您按照本主题中的说明修改现有脚本。 按照 [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md)主题的“代理所需权限”部分的说明为每个复制代理指定一个具有最低权限的帐户。  
  
 这些安全改进允许您显式指定用于执行复制代理作业的 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 帐户，从而可对权限进行更多控制，这些安全改进会影响现有脚本中的以下存储过程：  
  
-   **sp_addpublication_snapshot** ：  
  
     现在，执行 [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md) 来创建用于在分发服务器上运行快照代理的作业时，应提供 Windows 凭据作为 `@job_login` 和 `@job_password`。  
  
-   **sp_addpushsubscription_agent** ：  
  
     现在应执行 [sp_addpushsubscription_agent #40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md) 来显式添加作业，并提供用于在分发服务器上运行分发代理作业的 Windows 凭据（`@job_login` 和 `@job_password`）。 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之前的 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]版本中，当创建推送订阅时会自动执行此操作。  
  
-   **sp_addmergepushsubscription_agent** ：  
  
     现在应执行 [sp_addmergepushsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md) 来显式添加作业，并提供用于在分发服务器上运行合并代理作业的 Windows 凭据（`@job_login` 和 `@job_password`）。 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之前的 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]版本中，当创建推送订阅时会自动执行此操作。  
  
-   **sp_addpullsubscription_agent** ：  
  
     现在，执行 [sp_addpullsubscription_agent #40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md) 来创建用于在订阅服务器上运行分发代理的作业时，应提供 Windows 凭据作为 `@job_login` 和 `@job_password`。  
  
-   **sp_addmergepullsubscription_agent** ：  
  
     现在，执行 [sp_addmergepullsubscription_agent #40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) 来创建用于在订阅服务器上运行合并代理的作业时，应提供 Windows 凭据作为 `@job_login` 和 `@job_password`。  
  
-   **sp_addlogreader_agent** ：  
  
     现在应执行 [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md) 来手动添加作业，并提供用于在分发服务器上运行日志读取器代理的 Windows 凭据。 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之前的 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]版本中，当创建事务发布时会自动执行此操作。  
  
-   **sp_addqreader_agent** ：  
  
     现在应执行 [sp_addqreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md) 来手动添加作业，并提供用于在分发服务器上运行队列读取器代理的 Windows 凭据。 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之前的 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]版本中，当创建支持排队更新的事务发布时会自动执行此操作。  
  
 在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 引入的安全模式中，复制代理始终使用 `@job_name` 和 `@job_password` 中提供的凭据通过 Windows 身份验证连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的本地实例。 有关运行复制代理作业时所使用的 Windows 帐户的要求，请参阅 [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
> [!IMPORTANT]  
>  如果可能，请在运行时提示用户输入安全凭据。 如果将凭据保存在脚本文件中，请确保该文件本身受到安全保护。  
  
### <a name="to-upgrade-scripts-that-configure-a-snapshot-or-transactional-publication"></a>升级用于配置快照发布或事务发布的脚本  
  
1.  在现有脚本中的 [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) 前面，针对发布服务器上的发布数据库执行 [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)。 指定用于运行日志读取器代理的 Windows 凭据作为 `@job_name` 和 `@job_password`。 如果代理在连接到发布服务器时将使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证，则还必须将 `@publisher_security_mode` 的值指定为 0，并为 `@publisher_login` 和 `@publisher_password` 指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录信息。 这样，便为发布数据库创建了一个日志读取器代理作业。  
  
    > [!NOTE]  
    >  此步骤仅针对事务发布，无需对快照发布执行该步骤。  
  
2.  （可选）在 [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) 前面，针对分发服务器上的分发数据库执行 [sp_addqreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md)。 指定用于运行队列读取器代理的 Windows 凭据作为 `@job_name` 和 `@job_password` 运行。 这样，便为分发服务器创建了一个队列读取器代理作业。  
  
    > [!NOTE]  
    >  仅需要对支持排队更新订阅服务器的事务发布执行此步骤。  
  
3.  （可选）更新 [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) 的执行，以便对用于实现新复制功能的参数设置任何非默认的值。  
  
4.  在 [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) 后面，针对发布服务器上的发布数据库执行 [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)。 指定 `@publication` 和快照代理用其作为 `@job_name` 和 `@job_password`.运行的 Windows 凭据。 如果代理在连接到发布服务器时将使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证，则还必须将 `@publisher_security_mode` 的值指定为 0，并为 `@publisher_login` 和 `@publisher_password` 指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录信息。 此操作将为发布创建一个快照代理作业。  
  
5.  （可选）更新 [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) 的执行，以便对用于实现新复制功能的参数设置任何非默认的值。  
  
### <a name="to-upgrade-scripts-that-add-subscriptions-to-a-snapshot-or-transactional-publication"></a>升级向快照发布或事务发布添加订阅的脚本  
  
1.  在执行用于创建订阅的存储过程之后，请确保执行用于创建分发代理作业的存储过程以同步订阅。 所用的存储过程将取决于订阅类型。  
  
    -   对于请求订阅，请更新 [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md) 的执行，为 `@job_name` 和 `@job_password` 提供用于在订阅服务器上运行分发代理的 Windows 凭据。 此操作将在执行 [sp_addpullsubscription](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)后完成。 有关详细信息，请参阅 [创建请求订阅](../../../relational-databases/replication/create-a-pull-subscription.md)。  
  
    -   对于推送订阅，请在发布服务器上执行 [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)。 指定 `@subscriber`、`@subscriber_db`、`@publication`，并为 `@job_name` 和 `@job_password` 指定在分发服务器上运行分发代理所使用的 Windows 凭据，同时指定此代理作业的计划。 有关详细信息，请参阅 [Specify Synchronization Schedules](../../../relational-databases/replication/specify-synchronization-schedules.md)。 此操作将在执行 [sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)后完成。 有关详细信息，请参阅 [创建推送订阅](../../../relational-databases/replication/create-a-push-subscription.md)。  
  
### <a name="to-upgrade-scripts-that-configure-a-merge-publication"></a>升级用于配置合并发布的脚本  
  
1.  （可选）在现有脚本中，更新 [sp_addmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) 的执行，以便对用于实现新复制功能的参数设置任何非默认的值。  
  
2.  在 [sp_addmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) 后面，针对发布服务器上的发布数据库执行 [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)。 指定 `@publication` 和快照代理用其作为 `@job_name` 和 `@job_password`.运行的 Windows 凭据。 如果代理在连接到发布服务器时将使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证，则还必须将 `@publisher_security_mode` 的值指定为 0，为 `@publisher_login` 和 `@publisher_password` 指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录信息。 此操作将为发布创建一个快照代理作业。  
  
3.  （可选）更新 [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 的执行，以便对用于实现新复制功能的参数设置任何非默认的值。  
  
### <a name="to-upgrade-scripts-that-add-subscriptions-to-a-merge-publication"></a>升级向合并发布添加订阅的脚本  
  
1.  执行完用于创建订阅的存储过程后，请确保执行用于创建合并代理作业的存储过程以同步订阅。 所用的存储过程将取决于订阅类型。  
  
    -   对于请求订阅，请更新 [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) 的执行，为 `@job_name` 和 `@job_password` 提供用于在订阅服务器上运行合并代理的 Windows 凭据。 此操作将在执行 [sp_addmergepullsubscription](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)后完成。 有关详细信息，请参阅 [创建请求订阅](../../../relational-databases/replication/create-a-pull-subscription.md)。  
  
    -   对于推送订阅，请在发布服务器上执行 [sp_addmergepushsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md)。 指定 `@subscriber`、`@subscriber_db`、`@publication`，并为 `@job_name` 和 `@job_password` 指定在分发服务器上运行合并代理所使用的 Windows 凭据，同时指定此代理作业的计划。 有关详细信息，请参阅 [Specify Synchronization Schedules](../../../relational-databases/replication/specify-synchronization-schedules.md)。 此操作将在执行 [sp_addmergesubscription](../../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)后完成。 有关详细信息，请参阅 [创建推送订阅](../../../relational-databases/replication/create-a-push-subscription.md)。  
  
## <a name="examples"></a>示例  

### <a name="a-sql-server-2000-script-to-create-a-transactional-publication"></a>A. 用于创建事务发布的 SQL Server 2000 脚本

 下面是一个用于创建 Product 表的事务发布的 [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] 脚本示例。 此发布支持以排队更新作为故障转移的立即更新。 为了方便阅读，删除了默认参数。  
  
 [!code-sql[HowTo#sp_createtranpub_NWpreupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_1.sql)]  
  
### <a name="b-sql-server-2005-and-later-script-to-create-a-transactional-publication"></a>B. 用于创建事务发布的 SQL Server 2005 及更高版本的脚本
 下面的示例将对上一个用于创建事务发布的脚本进行更新，从而可以在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 和更高版本中成功运行。 此发布支持以排队更新作为故障转移的立即更新。 在此示例中显式声明了新参数的默认值。  
  
> [!NOTE]  
>  Windows 凭据是在运行时使用 **sqlcmd** 脚本变量来提供的。  
  
 [!code-sql[HowTo#sp_createtranpub_NWpostupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_2.sql)]  
  
### <a name="c-sql-server-2000-script-to-create-a-merge-publication"></a>C. 用于创建合并发布的 SQL Server 2000 脚本
 下面是一个可创建 Customers 表的合并发布的 [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] 脚本示例。 为了方便阅读，删除了默认参数。  
  
 [!code-sql[HowTo#sp_createmergepub_NWpreupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_3.sql)]  
  
### <a name="d-sql-server-2005-and-later-script-to-create-a-merge-publication"></a>D. 用于创建合并发布的 SQL Server 2005 及更高版本的脚本
 下面的示例将对上一个用于创建合并发布的脚本进行升级，从而可以在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 和更高版本中成功运行。 在此示例中显式声明了新参数的默认值。  
  
> [!NOTE]  
>  Windows 凭据是在运行时使用 **sqlcmd** 脚本变量来提供的。  
  
 [!code-sql[HowTo#sp_createmergepub_NWpostupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_4.sql)]  
  
### <a name="e-sql-server-2000-script-to-create-a-push-subscription-to-a-transactional-publication"></a>E. 用于创建对事务发布的推送订阅的 SQL Server 2000 脚本
 下面是一个可创建对事务发布的推送订阅的 [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] 脚本示例。 为了方便阅读，删除了默认参数。  
  
 [!code-sql[HowTo#sp_createtranpushsub_NWpreupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_5.sql)]  
  
### <a name="f-sql-server-2005-and-later-script-to-create-a-push-subscription-to-a-transactional-publication"></a>F. 用于创建对事务发布的推送订阅的 SQL Server 2005 及更高版本的脚本
 下面的示例将对上一个用于创建对事务发布的推送订阅的脚本进行升级，从而可在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 和更高版本中成功运行。 在此示例中显式声明了新参数的默认值。  
  
> [!NOTE]  
>  Windows 凭据是在运行时使用 **sqlcmd** 脚本变量来提供的。  
  
 [!code-sql[HowTo#sp_createtranpushsub_NWpostupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_6.sql)]  
  
### <a name="g-sql-server-2000-script-to-create-a-push-subscription-to-a-merge-publication"></a>G. 用于创建对合并发布的推送订阅的 SQL Server 2000 脚本
 下面是一个可创建对合并发布的推送订阅的 [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] 脚本示例。 为了方便阅读，删除了默认参数。  
  
 [!code-sql[HowTo#sp_createmergepushsub_NWpreupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_7.sql)]  
  
### <a name="h-sql-server-2005-and-later-script-to-create-a-push-subscription-to-a-merge-publication"></a>H. 用于创建对合并发布的推送订阅的 SQL Server 2005 及更高版本的脚本
 下面的示例将对上一个用于创建对合并发布的推送订阅的脚本进行升级，从而可在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 和更高版本中成功运行。 在此示例中显式声明了新参数的默认值。  
  
> [!NOTE]  
>  Windows 凭据是在运行时使用 **sqlcmd** 脚本变量来提供的。  
  
 [!code-sql[HowTo#sp_createmergepushsub_NWpostupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_8.sql)]  
  
### <a name="i-sql-server-2000-script-to-create-a-pull-subscription-to-a-transactional-publication"></a>I. 用于创建对事务发布的拉取订阅的 SQL Server 2000 脚本
 下面是一个可创建事务发布的请求订阅的 [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] 脚本示例。 为了方便阅读，删除了默认参数。  
  
 [!code-sql[HowTo#sp_createmergepushsub_NWpreupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_7.sql)]  
  
### <a name="j-sql-server-2005-and-later-script-to-create-a-pull-subscription-to-a-transactional-publication"></a>J. 用于创建对事务发布的拉取订阅的 SQL Server 2005 及更高版本的脚本
 下面的示例将对上一个用于创建对事务发布的请求订阅的脚本进行升级，从而可在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 和更高版本中成功运行。 在此示例中显式声明了新参数的默认值。  
  
> [!NOTE]  
>  Windows 凭据是在运行时使用 **sqlcmd** 脚本变量来提供的。  
  
 [!code-sql[HowTo#sp_createtranpullsub_NWpostupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_9.sql)]  
  
### <a name="k-sql-server-2000-script-to-create-a-pull-subscription-to-a-merge-publication"></a>K. 用于创建对合并发布的拉取订阅的 SQL Server 2000 脚本
 下面是一个可创建对合并发布的请求订阅的 [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] 脚本示例。 为了方便阅读，删除了默认参数。  
  
 [!code-sql[HowTo#sp_createmergepullsub_NWpreupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_10.sql)]  
  
### <a name="l-sql-server-2005-and-later-script-to-create-a-pull-subscription-to-a-merge-publication"></a>L. 用于创建对合并发布的拉取订阅的 SQL Server 2005 及更高版本的脚本
 下面的示例将对上一个用于创建对合并发布的请求订阅的脚本进行升级，从而可在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 和更高版本中成功运行。 在此示例中显式声明了新参数的默认值。  
  
> [!NOTE]  
>  Windows 凭据是在运行时使用 **sqlcmd** 脚本变量来提供的。  
  
 [!code-sql[HowTo#sp_createmergepullsub_NWpostupgrade](../../../relational-databases/replication/codesnippet/tsql/upgrade-replication-scri_11.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)   
 [ssSDSFull](../../../relational-databases/replication/create-a-push-subscription.md)   
 [Create a Pull Subscription](../../../relational-databases/replication/create-a-pull-subscription.md)   
 [查看和修改复制安全设置](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [MSSQL_ENG021797](../../../relational-databases/replication/mssql-eng021797.md)   
 [MSSQL_ENG021798](../../../relational-databases/replication/mssql-eng021798.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [升级复制的数据库](../../../database-engine/install-windows/upgrade-replicated-databases.md)  
  
  
