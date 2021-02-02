---
title: SQL Server 审核（数据库引擎）| Microsoft Docs
description: 了解 SQL Server 数据库引擎或单个数据库的服务器审核。 服务器审核包含服务器和数据库审核规范。
ms.custom: ''
ms.date: 01/01/2020
ms.prod: sql
ms.prod_service: security
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- audit
helpviewer_keywords:
- SQL Server Audit
- audits [SQL Server], SQL Server Audit
ms.assetid: 0c1fca2e-f22b-4fe8-806f-c87806664f00
author: davidtrigano
ms.author: datrigan
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 7c45a7bfb3f7dace1fb3d8b56d22977ec7768769
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076585"
---
# <a name="sql-server-audit-database-engine"></a>SQL Server 审核（数据库引擎）
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  “审核”[!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)] 实例或单独的数据库涉及到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 中发生的跟踪和记录事件。 通过[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核，您可以创建服务器审核，其中可以包含针对服务器级别事件的服务器审核规范和针对数据库级别事件的数据库审核规范。 可将审核的事件写入事件日志或审核文件。  
  
[!INCLUDE[ssMIlimitation](../../../includes/sql-db-mi-limitation.md)]
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的审核级别有若干种，具体取决于您的安装的政府要求或标准要求。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核提供若干必需的工具和进程，用于启用、存储和查看对各个服务器和数据库对象的审核。  
  
 您可以记录每个实例的服务器审核操作组，或记录每个数据库的数据库审核操作组或数据库审核操作。 在每次遇到可审核操作时，都将发生审核事件。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的所有版本均支持服务器级审核。 从 [!INCLUDE[ssSQL15_md](../../../includes/sssql16-md.md)] SP1 开始，所有版本都支持数据库级审核。 在此之前，数据库级审核限制为 Enterprise、Developer 和 Evaluation 版本。 有关详细信息，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
> [!NOTE]  
>  本     主题适用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。  有关 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]的信息，请参阅 [Get started with SQL database auditing（SQL 数据库审核入门）](/azure/azure-sql/database/auditing-overview)。  
  
## <a name="sql-server-audit-components"></a>SQL Server 审核组件  
  “审核”是将若干元素组合到一个包中，用于执行一组特定服务器操作或数据库操作。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核的组件组合生成的输出就称为审核，就如同报表定义与图形和数据元素组合生成报表一样。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核使用“扩展事件”  以帮助创建审核。 有关扩展事件的详细信息，请参阅 [扩展事件](../../../relational-databases/extended-events/extended-events.md)。  
  
### <a name="sql-server-audit"></a>SQL Server 审核  
 “SQL Server 审核”  对象收集单个服务器实例或数据库级操作和操作组以进行监视。 这种审核处于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例级别。 每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例可以具有多个审核。  
  
 定义审核时，将指定结果的输出位置。 这是审核的目标位置。 审核是在 *禁用* 状态下创建的，因此不会自动审核任何操作。 启用审核后，审核目标将从审核接收数据。  
  
### <a name="server-audit-specification"></a>服务器审核规范  
 “服务器审核规范”  对象属于审核。 您可以为每个审核创建一个服务器审核规范，因为它们都是在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例范围内创建的。  
  
 服务器审核规范可收集许多由扩展事件功能引发的服务器级操作组。 您可以在服务器审核规范中包括“审核操作组”  。 审核操作组是预定义的操作组，它们是 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]中发生的原子事件。 这些操作将发送到审核，审核将它们记录到目标中。  
  
 [SQL Server 审核操作组和操作](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md)主题介绍了服务器级别审核操作组。  
  
### <a name="database-audit-specification"></a>数据库审核规范  
 “数据库审核规范”  对象也属于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核。 针对每个审核，您可以为每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库创建一个数据库审核规范。  
  
 数据库审核规范可收集由扩展事件功能引发的数据库级审核操作。 你可以向数据库审核规范添加审核操作组或审核事件。 *审核事件* 是可以由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 引擎审核的原子操作。  “审核操作组”是预定义的操作组。 它们都位于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库作用域。 这些操作将发送到审核，审核将它们记录到目标中。 在用户数据库审核规范中不要包括服务器范围的对象，例如系统视图。  
  
 [SQL Server 审核操作组和操作](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md)主题介绍了数据库级别的审核操作组和审核操作。  
  
### <a name="target"></a>目标  
 审核结果将发送到目标，目标可以是文件、Windows 安全事件日志或 Windows 应用程序事件日志。 必须定期查看和归档这些日志，以确保目标具有足够的空间来写入更多记录。  
  
> [!IMPORTANT]  
>  任何经过身份验证的用户可以读取和写入到 Windows 应用程序事件日志。 应用程序事件日志要求的权限比 Windows 安全事件日志低，安全性低于 Windows 安全事件日志。  
  
 必须将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户应添加到 **生成安全审核** 策略中才能写入 Windows 安全日志。 默认情况下，本地系统、本地服务和网络服务都是此策略的一部分。 此设置可通过使用安全策略管理单元 (secpol.msc) 配置。 此外，对于“成功”  和“失败”  均必须启用“审核对象访问” 安全策略。 此设置可通过使用安全策略管理单元 (secpol.msc) 配置。 在 [!INCLUDE[wiprlhext](../../../includes/wiprlhext-md.md)] 或 Windows Server 2008 中，可通过使用审核策略程序 ( **AuditPol.exe)** 从命令行设置更详细的 **应用程序生成** 策略。 有关启用 Windows 安全日志写入的步骤的详细信息，请参阅 [将 SQL Server 审核事件写入安全日志](../../../relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log.md)。 有关 Auditpol.exe 程序的详细信息，请参阅知识库文章 921469 [如何使用组策略配置详细的安全审核设置](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/921469)。 Windows 事件日志对于 Windows 操作系统具有全局性。 有关 Windows 事件日志的详细信息，请参阅 [事件查看器概述](/previous-versions/windows/it-pro/windows-server-2003/cc737015(v=ws.10))。 如果需要关于审核的更精准权限，请使用二进制文件目标。  
  
 在您将审核信息保存到某一文件时，为了帮助避免被篡改，可以通过以下方式限制对文件位置的访问：  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户必须同时具有读取和写入权限。  
  
-   审核管理员通常需要读取和写入权限。 这就假设审核管理员是 Windows 帐户，可以管理审核文件（例如，将审核文件复制到其他共享、备份这些文件等等）。  
  
-   获得授权可读取审核文件的审核读取者必须具有读取权限。  
  
 甚至当 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 正在写入某个文件时，其他 Windows 用户如果具有权限，也可以读取该审核文件。 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 并未采用排他锁来防止读取操作。  
  
 因为 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 可以访问文件，所以，具有 CONTROL SERVER 权限的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名可以使用 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 来访问审核文件。 若要记录任何正在读取审核文件的用户，请在 master.sys.fn_get_audit_file 中定义审核。 这将记录具有 CONTROL SERVER 权限且已通过 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]访问审核文件的登录名。  
  
 如果审核管理员将文件复制到其他位置（用于存档等），新位置的 ACL 应降至以下权限：  
  
-   审核管理员 - 读/写  
  
-   审核读取者 - 读  
  
 建议您从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的单独实例（例如， [!INCLUDE[ssExpress](../../../includes/ssexpress-md.md)]的单独实例）生成审核报告（如果只有审核管理员或审核读取者可以访问此实例）。 通过使用 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 的单独实例进行报告，可以帮助防止未获授权的用户访问审核记录。  
  
 可以通过使用 Windows BitLocker 数据加密或 Windows 加密文件系统对存储审核文件的文件夹进行加密，从而提供附加保护机制来防止未授权访问。  
  
 有关写入目标的审核记录的详细信息，请参阅 [SQL Server Audit Records](../../../relational-databases/security/auditing/sql-server-audit-records.md)。  
  
## <a name="overview-of-using-sql-server-audit"></a>使用 SQL Server 审核概述  
 可以使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 定义审核。 在创建并启用审核后，目标将接收各项。  
  
 您可以使用 Windows 中的 **“事件查看器”** 实用工具来读取 Windows 事件。 对于文件目标，你可以使用 **中的“日志文件查看器”** [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 或 [fn_get_audit_file](../../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md) 函数来读取目标文件。  
  
 以下是创建和使用审核的一般过程。  
  
1.  创建审核并定义目标。  
  
2.  创建映射到审核的服务器审核规范或数据库审核规范。 启用审核规范。  
  
3.  启用审核。  
  
4.  通过使用 Windows“事件查看器” 、“日志文件查看器” 或 fn_get_audit_file 函数来读取审核事件。  

 有关详细信息，请参阅 [创建服务器审核和服务器审核规范](../../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md) 和 [创建服务器审核和数据库审核规范](../../../relational-databases/security/auditing/create-a-server-audit-and-database-audit-specification.md)。  
  
## <a name="considerations"></a>注意事项  
 如果在启动审核期间出现问题，则服务器将不会启动。 在这种情况下，可以在命令行中使用 -f 选项来启动服务器。  
  
 如果由于为审核指定了 ON_FAILURE=SHUTDOWN 而使得审核失败导致服务器关闭或不启动，则 MSG_AUDIT_FORCED_SHUTDOWN 事件将写入日志。 由于在第一次遇到此设置时将出现关机，此事件将写入一次。 在出现有关审核导致关闭的失败消息后，将写入此事件。 管理员可以使用 -m 标志以单用户模式启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，从而绕过审核引起的关闭。 如果在单用户模式下启动，则会将指定了 ON_FAILURE=SHUTDOWN 的任何审核降级为在相应会话中以 ON_FAILURE=CONTINUE 运行。 使用 -m 标志启动 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 时，MSG_AUDIT_SHUTDOWN_BYPASSED 消息将写入错误日志。  
  
 有关服务启动选项的详细信息，请参阅 [数据库引擎服务启动选项](../../../database-engine/configure-windows/database-engine-service-startup-options.md)。  
  
### <a name="attaching-a-database-with-an-audit-defined"></a>将数据库附加到已定义的审核  
 如果附加的数据库具有审核规范并且指定的 GUID 在服务器上不存在，则将导致“  孤立”审核规范。 因为服务器实例上不存在具有匹配 GUID 的审核，所以将不记录审核事件。 若要更正此情况，请使用 ALTER DATABASE AUDIT SPECIFICATION 命令将孤立审核规范连接到现有服务器审核。 或者，使用 CREATE SERVER AUDIT 命令创建一个具有指定 GUID 的新服务器审核。  
  
 您可以将定义了审核规范的数据库连接到不支持 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核的另一 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本，如 [!INCLUDE[ssExpress](../../../includes/ssexpress-md.md)] ，但它不会记录审核事件。  
  
### <a name="database-mirroring-and-sql-server-audit"></a>数据库镜像和 SQL Server 审核  
 已定义了数据库审核规范并使用数据库镜像的数据库将包括此数据库审核规范。 若要对已镜像的 SQL 实例进行正确的处理，必须配置下列项：  
  
-   镜像服务器必须拥有具有相同 GUID 的审核才能使数据库审核规范能够写入审核记录。 这可以通过使用命令 CREATE AUDIT WITH GUID **=** _\<GUID from source Server Audit_> 进行配置。  
  
-   对于二进制文件目标，镜像服务器服务帐户对要写入审核记录的位置必须具有相应的权限。  
  
-   对于 Windows 事件日志目标，镜像服务器所在计算机上的安全策略必须允许服务帐户访问安全事件日志或应用程序事件日志。  
  
### <a name="auditing-administrators"></a>审核管理员  
 **sysadmin** 固定服务器角色的成员在每个数据库中均标识为 **dbo** 用户。 若要审核管理员的操作，请审核 **dbo** 用户的操作。  
  
## <a name="creating-and-managing-audits-with-transact-sql"></a>使用 Transact-SQL 创建和管理审核  
 可以使用 DDL 语句、动态管理视图和函数以及目录视图来实现 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核的所有方面。  
  
### <a name="data-definition-language-statements"></a>数据定义语言语句  
 可以使用下列 DDL 语句创建、更改和删除审核规范：  
  
|DDL 语句|说明| 
|-|-|  
|[ALTER AUTHORIZATION](../../../t-sql/statements/alter-authorization-transact-sql.md)|更改安全对象的所有权。|  
|[ALTER DATABASE AUDIT SPECIFICATION](../../../t-sql/statements/alter-database-audit-specification-transact-sql.md)|使用 SQL Server 审核功能更改数据库审核规范对象。|  
|[ALTER SERVER AUDIT](../../../t-sql/statements/alter-server-audit-transact-sql.md)|使用 SQL Server 审核功能更改服务器审核对象。|  
|[ALTER SERVER AUDIT SPECIFICATION](../../../t-sql/statements/alter-server-audit-specification-transact-sql.md)|使用 SQL Server 审核功能更改服务器审核规范对象。|  
|[CREATE DATABASE AUDIT SPECIFICATION](../../../t-sql/statements/create-database-audit-specification-transact-sql.md)|使用 SQL Server 审核功能创建数据库审核规范对象。|  
|[CREATE SERVER AUDIT](../../../t-sql/statements/create-server-audit-transact-sql.md)|使用 SQL Server 审核创建服务器审核对象。|  
|[CREATE SERVER AUDIT SPECIFICATION](../../../t-sql/statements/create-server-audit-specification-transact-sql.md)|使用 SQL Server 审核功能创建服务器审核规范对象。|  
|[DROP DATABASE AUDIT SPECIFICATION](../../../t-sql/statements/drop-database-audit-specification-transact-sql.md)|使用 SQL Server 审核功能删除数据库审核规范对象。|  
|[DROP SERVER AUDIT](../../../t-sql/statements/drop-server-audit-transact-sql.md)|使用 SQL Server Audit 功能删除服务器审核对象。|  
|[DROP SERVER AUDIT SPECIFICATION](../../../t-sql/statements/drop-server-audit-specification-transact-sql.md)|使用 SQL Server 审核功能删除服务器审核规范对象。|  
  
### <a name="dynamic-views-and-functions"></a>动态视图和函数  
 下表列出了可用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核的动态视图和函数。  
  
|动态视图和函数|说明|  
|---------------------------------|-----------------|  
|[sys.dm_audit_actions](../../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)|为可在审核日志中报告的每项审核操作以及可配置为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Audit 一部分的每个审核操作组返回一行。|  
|[sys.dm_server_audit_status](../../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)|提供有关当前审核状态的信息。|  
|[sys.dm_audit_class_type_map](../../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)|返回一个表，将审核日志中的 class_type 字段映射到 sys.dm_audit_actions 中的 class_desc 字段。|  
|[fn_get_audit_file](../../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)|从由服务器审核创建的审核文件返回信息。|  
  
### <a name="catalog-views"></a>目录视图  
 下表列出了可用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核的目录视图。  
  
|目录视图|说明|  
|-------------------|-----------------|  
|[sys.database_audit_specifications](../../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)|包含服务器实例上的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核中的数据库审核规范的相关信息。|  
|[sys.database_audit_specification_details](../../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)|包含所有数据库的服务器实例上 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核中的数据库审核规范的相关信息。|  
|[sys.server_audits](../../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)|服务器实例中每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核都各占一行。|  
|[sys.server_audit_specifications](../../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)|包含有关服务器实例上 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核中的服务器审核规范的信息。|  
|[sys.server_audit_specifications_details](../../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)|包含服务器实例上的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核中的服务器审核规范详细信息（操作）的相关信息。|  
|[sys.server_file_audits](../../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)|包含有关服务器实例上 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 审核中的文件审核类型的存储扩展信息。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Audit 的每一个功能和命令都有其独特的权限需求。  
  
 若要创建、更改或删除服务器审核或服务器审核规范，服务器主体要求具有 ALTER ANY SERVER AUDIT 或 CONTROL SERVER 权限。 若要创建、更改或删除数据库审核规范，数据库主体必须具有 ALTER ANY DATABASE AUDIT 权限或针对该数据库的 ALTER 或 CONTROL 权限。 此外，主题还必须具有连接到数据库的权限或者具有 ALTER ANY SERVER AUDIT 或 CONTROL SERVER 权限。  
  
 拥有 VIEW ANY DEFINITION 权限，可有权查看服务器级别审核视图；拥有 VIEW DEFINITION 权限，可有权查看数据库级别审核视图。 拒绝这些权限，则无法查看目录视图，即使主体拥有 ALTER ANY SERVER AUDIT 或 ALTER ANY DATABASE AUDIT 权限，也是如此。  
  
 有关如何授予权限的详细信息，请参阅[GRANT(Transact-SQL)](../../../t-sql/statements/grant-transact-sql.md)。  
  
> [!CAUTION]  
>  具有 sysadmin 角色的主体可以篡改任意审核组件；具有 db_owner 角色的主体可以篡改数据库中的审核规范。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Audit 将验证将创建或更改审核规范的登录帐户是否至少具有 ALTER ANY DATABASE AUDIT 权限。 但是，它不会在您附加数据库时进行验证。 您应假定所有的数据库审核规范的可信度只是相当于具有 sysadmin 或 db_owner 角色的主体。  
  
## <a name="related-tasks"></a>Related Tasks  
 [创建服务器审核和服务器审核规范](../../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
 [创建服务器审核和数据库审核规范](../../../relational-databases/security/auditing/create-a-server-audit-and-database-audit-specification.md)  
  
 [查看 SQL Server 审核日志](../../../relational-databases/security/auditing/view-a-sql-server-audit-log.md)  
  
 [将 SQL Server 审核事件写入安全日志](../../../relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log.md)  
  
## <a name="topics-closely-related-to-auditing"></a>与审核密切相关的主题  
 [服务器属性（“安全性”页）](../../../database-engine/configure-windows/server-properties-security-page.md)  
 介绍如何为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]启用登录审核。 审核记录存储在 Windows 应用程序日志中。  
  
 [c2 审核模式服务器配置选项](../../../database-engine/configure-windows/c2-audit-mode-server-configuration-option.md)  
 介绍 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中的 C2 符合安全标准审核模式。  
  
 [安全审核事件类别 (SQL Server Profiler)](../../../relational-databases/event-classes/security-audit-event-category-sql-server-profiler.md)  
 介绍您可以在 [!INCLUDE[ssSqlProfiler](../../../includes/sssqlprofiler-md.md)]中使用的审核事件。 有关详细信息，请参阅 [SQL Server Profiler](../../../tools/sql-server-profiler/sql-server-profiler.md)。  
  
 [SQL 跟踪](../../../relational-databases/sql-trace/sql-trace.md)  
 介绍如何使用 SQL 跟踪（而不使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 事件探查器）从你自己的应用程序中手动创建跟踪。  
  
 [DDL 触发器](../../../relational-databases/triggers/ddl-triggers.md)  
 介绍如何使用数据定义语言 (DDL) 触发器来跟踪对数据库的更改。  
  
 [Microsoft TechNet：SQL Server 技术中心：SQL Server 2005 安全性和保护](../../../sql-server/index.yml)  
 提供有关 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安全性的最新信息。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 审核操作组和操作](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md)   
 [SQL Server 审核记录](../../../relational-databases/security/auditing/sql-server-audit-records.md)  
  
