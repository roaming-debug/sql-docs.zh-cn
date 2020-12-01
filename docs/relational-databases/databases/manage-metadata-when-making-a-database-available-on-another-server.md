---
description: 使数据库在其他服务器上可用时管理元数据
title: 使数据库在其他服务器上可用时管理元数据
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: 5d98cf2a-9fc2-4610-be72-b422b8682681
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
helpviewer_keywords:
- cross-database queries [SQL Server]
- logins [SQL Server], recreating on another server instance
- triggers [SQL Server], DLL
- user-defined error messages [SQL Server]
- permissions [SQL Server], metadata access
- Full-Text Engine [SQL Server]
- metadata [SQL Server], databases available to other instances
- jobs [SQL Server Agent], recreating on another server instance
- failover [SQL Server], managing metadata
- event notifications [SQL Server], metadata
- database mirroring [SQL Server], metadata
- startup options [SQL Server]
- restoring [SQL Server], onto another server instance
- linked servers [SQL Server], metadata
- WMI Provider for Server Events, metadata
- attaching databases [SQL Server]
- log shipping [SQL Server], metadata
- encryption [SQL Server], metadata
- server configuration [SQL Server]
- distributed queries [SQL Server], metadata
- extended stored procedures [SQL Server], metadata
- credentials [SQL Server], metadata
- copying databases
ms.openlocfilehash: 3dc93671874de47f45bd26ae12fa9ded44c9a4fd
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88412843"
---
# <a name="manage-metadata-when-making-a-database-available-on-another-server"></a>使数据库在其他服务器上可用时管理元数据
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本文与下列情况有关：  
  
-   配置 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 可用性组的可用性副本。  
  
-   设置数据库镜像。  
  
-   准备在日志传送配置中交换主服务器和辅助服务器的角色。  
  
-   将数据库还原到其他服务器实例。  
  
-   在其他服务器实例上附加数据库副本。  
  
 某些应用程序依赖于单个用户数据库范围之外的信息、实体和/或对象。 通常，应用程序具有对 **master** 和 **msdb** 数据库的依赖关系，并且还具有对用户数据库的依赖关系。 用户数据库正确运行所需的存储在该数据库外部的任何内容必须在目标服务器实例上可用。 例如，应用程序的登录名作为元数据存储在 **master** 数据库中，必须在目标服务器上重新创建这些登录名。 如果应用程序或数据库维护计划依赖于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业（其元数据存储在 **msdb** 数据库中），则必须在目标服务器实例上重新创建这些作业。 同样，服务器级触发器的元数据存储在 **master** 中。  
  
 将应用程序的数据库移动到其他服务器实例时，必须在目标服务器实例的 master 和 msdb 中重新创建依赖实体和依赖对象的所有元数据 。 例如，如果数据库应用程序使用服务器级触发器，则仅在新系统上附加或还原数据库是不够的。 如果不手动在 **master** 数据库中重新创建这些触发器的元数据，则数据库不能按预期方式工作。  
  
##  <a name="information-entities-and-objects-that-are-stored-outside-of-user-databases"></a><a name="information_entities_and_objects"></a> 存储在用户数据库外部的信息、实体和对象  
 本文的其余部分概要说明了可能影响在其他服务器实例上可用的数据库的潜在问题。 最好重新创建以下列表中列出的一种或多种信息、实体或对象。 若要查看概要内容，请单击该项的链接。  
  
-   [服务器配置设置](#server_configuration_settings)  
  
-   [凭据](#credentials)  
  
-   [跨数据库查询](#cross_database_queries)  
  
-   [数据库所有权](#database_ownership)  
  
-   [分布式查询/链接服务器](#distributed_queries_and_linked_servers)  
  
-   [加密数据](#encrypted_data)  
  
-   [用户定义的错误消息](#user_defined_error_messages)  
  
-   [事件通知和 Windows Management Instrumentation (WMI) 事件（服务器级）](#event_notif_and_wmi_events)  
  
-   [扩展的存储过程](#extended_stored_procedures)  
  
-   [SQL Server 属性的全文引擎](#ifts_service_properties)  
  
-   [作业](#jobs)  
  
-   [登录名](#logins)  
  
-   [权限](#permissions)  
  
-   [复制设置](#replication_settings)  
  
-   [Service Broker 应用程序](#sb_applications)  
  
-   [启动过程](#startup_procedures)  
  
-   [触发器（服务器级）](#triggers)  
  
##  <a name="server-configuration-settings"></a><a name="server_configuration_settings"></a> Server Configuration Settings  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本会选择性地安装和启动密钥服务和功能。 这有助于减少系统可遭受攻击的外围应用。 在新安装的默认配置中，许多功能并未启用。 如果数据库依赖于默认处于禁用状态的服务或功能，则必须在目标服务器实例上启用此服务或功能。  
  
 有关这些设置以及启用或禁用它们的详细信息，请参阅[服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)。  
  
  
##  <a name="credentials"></a><a name="credentials"></a> 凭据  
 凭据是包含连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]以外的资源时所需的身份验证信息的记录。 大多数凭据包含一个 Windows 登录名和密码。  
  
 有关此功能的详细信息，请参阅 [凭据（数据库引擎）](../../relational-databases/security/authentication-access/credentials-database-engine.md)。  
  
> **注意：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的代理帐户使用凭据。 若要了解代理帐户的凭据 ID，请使用 [sysproxies](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md) 系统表。  
  
  
##  <a name="cross-database-queries"></a><a name="cross_database_queries"></a> Cross-Database Queries  
 DB_CHAINING 和 TRUSTWORTHY 数据库选项默认设置为 OFF。 如果针对原始数据库将这两个选项之一设置为 ON，则可能必须对目标服务器实例上的数据库启用这两个选项。 有关详细信息，请参阅 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)。  
  
 附加和分离操作都会禁用数据库的跨数据库所有权链接。 有关如何启用链接的详细信息，请参阅 [cross db ownership chaining 服务器配置选项](../../database-engine/configure-windows/cross-db-ownership-chaining-server-configuration-option.md)。  
  
 有关详细信息，另请参阅[设置镜像数据库以使用可信属性 (Transact-SQL)](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md)  
  
  
##  <a name="database-ownership"></a><a name="database_ownership"></a> Database Ownership  
 在其他计算机上还原数据库时，启动还原操作的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录用户或 Windows 用户将自动成为新数据库的所有者。 还原数据库时，系统管理员或新数据库所有者可以更改数据库所有权。  
  
##  <a name="distributed-queries-and-linked-servers"></a><a name="distributed_queries_and_linked_servers"></a> 分布式查询和链接服务器  
 OLE DB 应用程序支持分布式查询和链接服务器。 分布式查询访问相同或不同计算机上多个异类数据源中的数据。 链接服务器配置使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以对远程服务器上的 OLE DB 数据源执行命令。 有关这些功能的详细信息，请参阅[链接服务器（数据库引擎）](../../relational-databases/linked-servers/linked-servers-database-engine.md)。  
  
  
##  <a name="encrypted-data"></a><a name="encrypted_data"></a> Encrypted Data  
 如果在其他服务器实例上可用的数据库包含加密数据，并且数据库主密钥由原始服务器上的服务主密钥保护，则最好重新进行服务主密钥加密。 “数据库主密钥  ”是一种对称密钥，用于在加密数据库中保护证书的私钥和非对称密钥的私钥。 当创建数据库主密钥时，会使用 Triple DES 算法以及用户提供的密码对其进行加密。  
  
 若要对服务器实例上的数据库主密钥启用自动解密，请使用服务主密钥对此密钥的副本进行加密。 此加密副本存储在此数据库以及 **master** 中。 通常，每当主密钥更改时，便会在不进行提示的情况下更新存储在 **master** 中的副本。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 最初尝试使用实例的服务主密钥解密数据库主密钥。 如果解密失败，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在凭据存储区中搜索与需要其主密钥的数据库具有相同系列 GUID 的主密钥凭据。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 尝试使用每个匹配的凭据对数据库主密钥进行解密，直到成功解密或者没有更多的凭据为止。 必须使用 OPEN MASTER KEY 语句和密码打开未使用服务主密钥进行加密的主密钥。  
  
 对加密数据库执行复制、还原或附加到新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例等操作时，由服务主密钥加密的数据库主密钥的副本不存储在目标服务器实例上的 **master** 中。 在目标服务器实例上，必须打开数据库的主密钥。 若要打开主密钥，请执行以下语句：OPEN MASTER KEY DECRYPTION BY PASSWORD **='**_password_**'**。 建议您通过执行下面的语句对数据库主密钥启用自动解密：ALTER MASTER KEY ADD ENCRYPTION BY SERVICE MASTER KEY。 此 ALTER MASTER KEY 语句使用数据库主密钥（使用服务主密钥加密）的副本来设置服务器实例。 有关详细信息，请参阅 [OPEN MASTER KEY (Transact-SQL)](../../t-sql/statements/open-master-key-transact-sql.md) 和 [ALTER MASTER KEY (Transact-SQL)](../../t-sql/statements/alter-master-key-transact-sql.md)。  
  
 有关如何启用镜像数据库主秘钥自动加密的详细信息，请参阅 [设置加密的镜像数据库](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)。  
  
 有关详细信息，请参阅：  
  
-   [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
-   [设置加密的镜像数据库](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
-   [在两个服务器上创建相同的对称密钥](../../relational-databases/security/encryption/create-identical-symmetric-keys-on-two-servers.md)  
  
  
##  <a name="user-defined-error-messages"></a><a name="user_defined_error_messages"></a> User-defined Error Messages  
 用户定义的错误消息位于 [sys.messages](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md) 目录视图中。 此目录视图存储在 **master** 中。 如果数据库应用程序依赖于用户定义的错误消息并且此数据库在其他服务器实例上可用，则请使用 [sp_addmessage](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md) 在目标服务器实例上添加这些用户定义的消息。  

  
##  <a name="event-notifications-and-windows-management-instrumentation-wmi-events-at-server-level"></a><a name="event_notif_and_wmi_events"></a> 事件通知和 Windows Management Instrumentation (WMI) 事件 （在服务器级别）  
  
### <a name="server-level-event-notifications"></a>服务器级事件通知  
 服务器级事件通知存储在 **msdb** 中。 因此，如果数据库应用程序依赖于服务器级事件通知，则必须在目标服务器实例上重新创建该事件通知。 若要查看服务器实例上的事件通知，请使用 [sys.server_event_notifications](../../relational-databases/system-catalog-views/sys-server-event-notifications-transact-sql.md) 目录视图。 有关详细信息，请参阅 [Event Notifications](../../relational-databases/service-broker/event-notifications.md)。  
  
 此外，使用 [!INCLUDE[ssSB](../../includes/sssb-md.md)]传递事件通知。 传入消息的路由不包括在包含服务的数据库中。 相反，显式路由存储在 **msdb** 中。 如果服务使用 **msdb** 数据库中的显式路由将传入的消息路由到该服务，则在将数据库附加到其他实例时，必须重新创建此路由。  
  
### <a name="windows-management-instrumentation-wmi-events"></a>Windows Management Instrumentation (WMI) 事件  
 使用服务器事件的 WMI 提供程序，可以使用 Windows Management Instrumentation (WMI) 监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的事件。 必须在目标服务器实例所在的计算机上定义任何依赖于服务器级事件（此事件通过数据库所依赖的 WMI 提供程序显示）的应用程序。 WMI 事件提供程序使用在 **msdb** 中定义的目标服务创建事件通知。  
  
> **注意：** 有关详细信息，请参阅 [WMI Provider for Server Events 的概念](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md)。  
  
 **使用 SQL Server Management Studio 创建 WMI 警报**  
  
-   [创建 WMI 事件警报](../../ssms/agent/create-a-wmi-event-alert.md)  
  
### <a name="how-event-notifications-work-for-a-mirrored-database"></a>镜像数据库事件通知工作原理  
 因为镜像数据库可以进行故障转移，所以涉及镜像数据库的事件通知的跨数据库传递是按照定义以远程方式进行的。 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 以 *镜像路由* 的形式为镜像数据库提供特殊支持。 镜像路由有两个地址：一个针对主体服务器实例，另一个针对镜像服务器实例。  
  
 通过设置镜像路由，您可以使 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 路由支持数据库镜像。 使用镜像路由， [!INCLUDE[ssSB](../../includes/sssb-md.md)] 能够透明地将会话重定向到当前的主体服务器实例。 例如，有一项由镜像数据库 Database_A 承载的服务 Service_A。 假定您需要由 Database_B 承载的另一项服务 Service_B 与 Service_A 进行对话。 为了实现此对话，Database_B 必须包含 Service_A 的镜像路由。 此外，Database_A 必须包含 Service_B 的非镜像 TCP 传输路由，与本地路由不同的是，该路由在故障转移后保持有效。 这些路由使 ACK 能够在故障转移后恢复。 由于发送方的服务始终以相同方式命名，因此路由必须指定 Broker 实例。  
  
 不管镜像数据库中的服务是发起方服务还是目标服务，下列情况均要求使用镜像路由：  
  
-   如果目标服务位于镜像数据库中，则发起方服务必须具有返回目标的镜像路由。 但是，目标可以具有返回发起方的常规路由。  
  
-   如果发起方服务位于镜像数据库中，则目标服务必须具有返回发起方的镜像路由，以传递确认和应答。 但是，发起方可能拥有指向目标的常规路由。  
  
  
##  <a name="extended-stored-procedures"></a><a name="extended_stored_procedures"></a> Extended Stored Procedures  
  
> **重要说明！** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [CLR 集成](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md) 。  
  
 扩展存储过程使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 扩展存储过程 API 进行编程。 **sysadmin** 固定服务器角色的成员可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例注册扩展存储过程，并授予用户执行此过程的权限。 扩展存储过程只能添加到 **master** 数据库。  
  
 扩展存储过程直接在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的地址空间中运行，它们可能会引起内存泄漏或其他问题，从而降低服务器的性能和可靠性。 您应考虑将扩展存储过程存储在独立于包含被引用数据的实例的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中。 还应考虑使用分布式查询访问数据库。  
  
  > [!IMPORTANT]
  > 将扩展存储过程添加到服务器并向其他用户授予 EXECUTE 权限之前，系统管理员应全面查看每个扩展存储过程，以确保它不包含有害代码或恶意代码。  
  
 有关更多详细信息，请参阅 [GRANT 对象权限 (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)、[DENY 对象权限 (Transact-SQL)](../../t-sql/statements/deny-object-permissions-transact-sql.md) 和 [REVOKE 对象权限 (Transact-SQL)](../../t-sql/statements/revoke-object-permissions-transact-sql.md)。  
  
  
##  <a name="full-text-engine-for-sql-server-properties"></a><a name="ifts_service_properties"></a> Full-Text Engine for SQL Server Properties  
 全文引擎的属性是通过 [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)设置的。 请确保目标服务器实例具有这些属性的必需设置。 有关这些属性的详细信息，请参阅 [FULLTEXTSERVICEPROPERTY (Transact SQL)](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)。  
  
 此外，如果原始服务器实例和目标服务器示例具有不同版本的 [断字符和词干分析器](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md) 组件或 [全文搜索筛选器](../../relational-databases/search/configure-and-manage-filters-for-search.md) 组件，则全文索引和查询的行为可能有所不同。 此外， [同义词库](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md) 存储在特定于实例的文件中。 您必须将这些文件的副本传输到目标服务器实例上的相同位置，或者在新的实例上重新创建这些文件。  
  
> **注意：** 当将包含全文目录文件的 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 数据库附加到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 服务器实例上时，会将目录文件从其以前的位置与其他数据库文件一起附加，这与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]中的情况相同。 有关详细信息，请参阅 [全文搜索升级](../../relational-databases/search/upgrade-full-text-search.md)。  
  
 有关详细信息，请参阅：  
  
-   [备份和还原全文目录和索引](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)  
  
-   [数据库镜像和全文目录 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md)  

  
##  <a name="jobs"></a><a name="jobs"></a> 作业  
 如果数据库依赖于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业，则必须在目标服务器实例上重新创建这些作业。 作业取决于其环境。 如果计划在目标服务器实例上重新创建现有作业，则可能必须修改目标服务器实例，以便与原始服务器实例上此作业的环境相匹配。 下面是重要的环境因素：  
  
-   作业使用的登录名  
  
     若要创建或执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业，首先必须将作业所需的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名添加到目标服务器实例。 有关详细信息，请参阅 [配置帐户以创建和管理 SQL Server 代理作业](../../ssms/agent/configure-a-user-to-create-and-manage-sql-server-agent-jobs.md)。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务启动帐户  
  
     服务启动帐户可以定义运行 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 代理的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Windows 帐户及其网络权限。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理在指定的用户帐户下运行。 代理服务的上下文会影响作业及其运行环境的设置。 帐户必须有权访问作业所需的资源（如网络共享）。 有关如何选择和修改服务启动帐户的信息，请参阅 [选择 SQL Server 代理服务帐户](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md)。  
  
     为了正常操作，必须对服务启动帐户进行配置，使其具有正确的域、文件系统和注册表权限。 此外，作业可能还需要必须针对服务帐户配置的共享网络资源。 有关详细信息，请参阅 [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务与特定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例关联，具有自己的注册表配置单元，并且其作业通常与此注册表配置单元中的一个或多个设置具有依赖关系。 若要按预期方式运行，作业需要这些注册表设置。 如果使用脚本在其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务中重新创建一个作业，则此服务的注册表中可能没有用于该作业的正确设置。 为使重新创建的作业在目标服务器实例上正常运行，原始和目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务应具有相同的注册表设置。  
  
    > [!CAUTION]  
    >  如果其他作业需要当前设置，则通过更改目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务上的注册表设置来处理重新创建的作业可能会出现问题。 此外，错误编辑注册表可能会严重损坏您的系统。 更改注册表项之前，建议您备份计算机中的所有重要数据。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的代理帐户  
  
     [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的代理帐户定义指定作业步骤的安全上下文。 对于要在目标服务器实例上运行的作业，必须在此实例上手动重新创建此作业所需的所有代理。 有关详细信息，请参阅 [创建 SQL Server 代理程序代理](../../ssms/agent/create-a-sql-server-agent-proxy.md) 和 [对使用代理的多服务器作业进行故障排除](../../ssms/agent/troubleshoot-multiserver-jobs-that-use-proxies.md)。  
  
 有关详细信息，请参阅：  
  
-   [执行作业](../../ssms/agent/implement-jobs.md)  
  
-   [角色切换后登录名和作业的管理 (SQL Server)](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)（为镜像数据库）  
  
-   [配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md) （安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例时）  
  
-   [配置 SQL Server 代理](../../ssms/agent/configure-sql-server-agent.md) （安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例时）  
  
-   [实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)  
  
 **查看现有作业及其属性**  
  
-   [监视作业活动](../../ssms/agent/monitor-job-activity.md)  
  
-   [sp_help_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)  
  
-   [View Job Step Information](../../ssms/agent/view-job-step-information.md)  
  
-   [dbo.sysjobs (Transact-SQL)](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md)  
  
 **创建作业**  
  
-   [创建作业](../../ssms/agent/create-a-job.md)  
  
#### <a name="best-practices-for-using-a-script-to-re-create-a-job"></a>使用脚本重新创建作业的最佳实践  
 建议您首先编写简单作业的脚本，接下来在其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务上重新创建此作业，然后运行此作业以查看它是否按预期方式工作。 这样便可确定不兼容性并尝试进行解决。 如果已编写脚本的作业在新环境中未按预期方式工作，则建议您创建可在此环境中正常工作的等价作业。  
  

##  <a name="logins"></a><a name="logins"></a> 登录名  
 登录到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例需要有效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 在身份验证过程中会使用此登录名，以验证主体是否可以连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。 在服务器实例上未定义或错误定义了其相应 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的数据库用户无法登录到实例。 这样的用户被称为此服务器实例上的数据库的“孤立用户”  。 当数据库还原、附加或复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的其他实例之后，数据库用户便可变为孤立用户。  
  
 若要为数据库原始副本中的部分或全部对象生成脚本，可以使用生成脚本向导，并在 **“选择脚本选项”** 对话框中将 **“编写登录脚本”** 选项设置为 **True**。  
  
> **注意：** 有关如何为镜像数据库设置登录名的信息，请参阅 [数据库镜像或 AlwaysOn 可用性组设置登录帐户 (SQL Server)](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md) 和 [角色切换后登录名和作业的管理 (SQL Server)](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md)。  
  
  
##  <a name="permissions"></a><a name="permissions"></a> 权限  
 当数据库在其他服务器实例上可用时，下列类型的权限可能受到影响。  
  
-   对系统对象的 GRANT、REVOKE 或 DENY 权限  
  
-   对服务器实例的 GRANT、REVOKE 或 DENY 权限（*服务器级权限*）  
  
### <a name="grant-revoke-and-deny-permissions-on-system-objects"></a>对系统对象的 GRANT、REVOKE 和 DENY 权限  
 对系统对象（例如存储过程、扩展存储过程、函数和视图）的权限存储在 **master** 数据库中，并且必须在目标服务器实例上进行配置。  
  
 若要为数据库原始副本中的部分或全部对象生成脚本，可以使用生成脚本向导，并在“选择脚本选项”对话框中将“编写对象级权限脚本”选项设置为 **True**。  
  
   > [!IMPORTANT]
   > 如果编写登录脚本，则不编写密码的脚本。 如果登录名使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，则必须在目标上修改脚本。  
  
 在 [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md) 目录视图中可以查看系统对象。 在 [master](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md) 数据库中的 **sys.database_permissions** 目录视图中可以查看对系统对象的权限。 有关查询这些目录视图并授予系统对象权限的信息，请参阅 [GRANT 系统对象权限 (Transact-SQL)](../../t-sql/statements/grant-system-object-permissions-transact-sql.md)。 有关更多详细信息，请参阅 [REVOKE 系统对象权限 (Transact-SQL)](../../t-sql/statements/revoke-system-object-permissions-transact-sql.md) 和 [DENY 系统权限 (Transact-SQL)](../../t-sql/statements/deny-system-object-permissions-transact-sql.md)。  
  
### <a name="grant-revoke-and-deny-permissions-on-a-server-instance"></a>对服务器实例的 GRANT、REVOKE 和 DENY 权限  
 服务器范围的权限存储在 **master** 数据库中，并且必须在目标服务器实例上进行配置。 有关服务器实例的服务器权限的信息，请查询 [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) 目录视图；有关服务器主体的信息，请查询 [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)目录视图；有关服务器角色成员身份的信息，请查询 [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md) 目录视图。  
  
 有关更多详细信息，请参阅 [GRANT 服务器权限 (Transact-SQL)](../../t-sql/statements/grant-server-permissions-transact-sql.md)、[REVOKE 服务器权限 (Transact-SQL)](../../t-sql/statements/revoke-server-permissions-transact-sql.md) 和 [DENY 服务器权限 (Transact-SQL)](../../t-sql/statements/deny-server-permissions-transact-sql.md)。  
  
#### <a name="server-level-permissions-for-a-certificate-or-asymmetric-key"></a>证书或非对称密钥的服务器级权限  
 不能向证书或非对称密钥直接授予服务器级权限。 相反，可以向专门针对特定证书或非对称密钥创建的映射登录名授予服务器级权限。 因此，每个需要服务器级权限的证书或非对称密钥都需要自己的“证书映射登录名  ”或“非对称密钥映射登录名 ”。 若要为证书或非对称密钥授予服务器级权限，请向其映射登录名授予相应权限。  
  
> **注意：** 映射登录名仅用于对使用相应证书或非对称密钥签名的代码进行授权。 映射登录名不能用于身份验证。  
  
 映射登录名及其权限都位于 **master** 中。 如果证书或非对称密钥位于 **master** 之外的数据库中，则必须在 **master** 中重新创建证书或非对称密钥并将其映射到登录名。 如果将数据库移动、复制或还原到其他服务器实例，则必须在目标服务器实例的 **master** 数据库中重新创建其证书或非对称密钥，将证书或非对称密钥映射到登录名，并向此登录名授予必需的服务器级权限。  
  
 **创建证书或非对称密钥**  
  
-   [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
 **将证书或非对称密钥映射到登录名**  
  
-   [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)  
  
 **为映射登录名分配权限**  
  
-   [GRANT 服务器权限 (Transact-SQL)](../../t-sql/statements/grant-server-permissions-transact-sql.md)  
  
 有关证书和非对称密钥的详细信息，请参阅 [Encryption Hierarchy](../../relational-databases/security/encryption/encryption-hierarchy.md)。  
  
## <a name="trustworthy-property"></a>Trustworthy 属性
TRUSTWORTHY 数据库属性用于指明此 SQL Server 实例是否信任该数据库以及其中的内容。 为安全起见，默认情况下，附加数据库后此选项需设置为 OFF，即使源服务器上此选项被设置为 ON。 有关此属性的详细信息，请参阅 [TRUSTWORTHY 数据库属性](../security/trustworthy-database-property.md)，有关如何启用此选项的信息，请参阅 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)。  


##  <a name="replication-settings"></a><a name="replication_settings"></a> Replication Settings  
 如果将复制数据库的备份还原到其他服务器或数据库，则无法保留复制设置。 在这种情况下，您必须在还原备份后重新创建所有发布和订阅。 为使此过程更加简单，请创建用于当前复制设置以及启用和禁用复制的脚本。 为了帮助重新创建复制设置，请复制这些脚本，并更改服务器名称引用以用于目标服务器实例。  
  
 有关详细信息，请参阅[备份和还原复制的数据库](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)、[数据库镜像和复制 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md) 以及[日志传送和复制 (SQL Server)](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md)。  
  
  
##  <a name="service-broker-applications"></a><a name="sb_applications"></a> Service Broker Applications  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 应用程序的许多相关内容都将随数据库一起移动。 但是，应用程序的某些相关内容必须在新位置重新创建或重新配置。  为安全起见，默认情况下，从其他服务器附加数据库后，is_broker_enabled 和 is_honoor_broker_priority_on 的选项设置为 OFF。 有关如何将这些选项设置为 ON 的详细信息，请参阅 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)。  
  
  
##  <a name="startup-procedures"></a><a name="startup_procedures"></a> Startup Procedures  
 启动过程是指标记为自动执行并在每次启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时执行的存储过程。 如果数据库依赖于启动过程，则必须在目标服务器实例上定义这些启动过程并将其配置为启动时自动执行。  

  
##  <a name="triggers-at-server-level"></a><a name="triggers"></a> Triggers (at Server Level)  
 DDL 触发器激发存储过程以响应各种数据定义语言 (DDL) 事件。 这些事件主要与以关键字 CREATE、ALTER 和 DROP 开头的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句对应。 执行 DDL 式操作的系统存储过程也可以激发 DDL 触发器。  
  
 有关此功能的详细信息，请参阅 [DDL Triggers](../../relational-databases/triggers/ddl-triggers.md)。  
  
  
## <a name="see-also"></a>另请参阅  
 [包含的数据库](../../relational-databases/databases/contained-databases.md)   
 [将数据库复制到其他服务器](../../relational-databases/databases/copy-databases-to-other-servers.md)   
 [数据库分离和附加 (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [故障转移到日志传送辅助服务器 (SQL Server)](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)   
 [数据库镜像会话期间的角色切换 (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [设置加密的镜像数据库](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)   
 [SQL Server 配置管理器](../../relational-databases/sql-server-configuration-manager.md)   
 [孤立用户故障排除 (SQL Server)](../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md)  
  
  
