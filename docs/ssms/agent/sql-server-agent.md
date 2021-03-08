---
description: SQL Server 代理
title: SQL Server 代理
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, about SQL Server Agent
- automatic administration steps
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/03/2021
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 751a627530b7d1017bfd8f0f89aa626ea84bc35c
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186531"
---
# <a name="sql-server-agent"></a>SQL Server 代理

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

SQL Server 代理是一项 Microsoft Windows 服务，它在执行计划的管理任务，这些任务 SQL Server 中被称为“作业”。

## <a name="benefits-of-sql-server-agent"></a><a name="Benefits"></a>SQL Server 代理的好处

SQL Server 代理使用 SQL Server 存储作业信息。 作业包含一个或多个作业步骤。 每个步骤都有自己的任务。例如，备份数据库。

SQL Server 代理可以按照计划运行作业，也可以在响应特定事件时运行作业，还可以根据需要运行作业。 例如，如果希望在每个工作日下班后备份公司的所有服务器，就可以使该任务自动执行。 安排备份在星期一到星期五 22:00 之后运行。 如果备份遇到问题，SQL Server 代理可记录该事件并通知你。

> [!NOTE]
> 默认情况下，SQL Server 安装后 SQL Server 代理服务处于禁用状态，除非用户明确选择自动启动该服务。

## <a name="sql-server-agent-components"></a><a name="Components"></a>SQL Server Agent Components

SQL Server 代理使用下列组件来定义要执行的任务、执行任务的时间以及报告任务成功或失败的方式。

### <a name="jobs"></a>作业

作业是 SQL Server 代理执行的一系列指定操作。 使用作业能够定义可一次或多次运行的，并且可以监视其成功或失败状态的管理任务。 一个作业可在一台本地服务器或者多台远程服务器上运行。

> [!IMPORTANT]
> SQL Server 故障转移群集实例上发生故障转移事件时正在运行的 SQL Server 代理作业在故障转移到其他故障转移群集节点后不恢复。 SQL Server 在 Hyper-V 节点暂停时，如果该暂停导致故障转移到其他节点，则正在运行的代理作业不恢复。 已开始但由于故障转移事件而未能完成的作业在日志中记录为已开始，但不提供附加的条目来指明完成或失败。 SQL Server 代理作业好像永远不会结束。

可以通过下列几种方式来运行作业：

- 根据一个或多个计划。

- 响应一个或多个警报。

- 通过执行 sp_start_job 存储过程。

作业中的每个操作都是一个“作业步骤” 。 例如，作业步骤可以运行 Transact\-SQL 语句、执行 SSIS 包或向 Analysis Services 服务器发出命令。 作业步骤作为作业的一部分进行管理。

所有作业步骤均在特定的安全上下文中运行。 对于使用 Transact\-SQL 的作业步骤，请使用 EXECUTE AS 语句设置作业步骤的安全上下文。 对于其他类型的作业步骤，请使用代理帐户来设置作业步骤的安全上下文。

### <a name="schedules"></a>计划

“计划”  指定了作业运行的时间。 多个作业可按同一计划运行，可将多个计划应用到同一作业。 计划可为作业运行时间定义以下条件：

- 每当 SQL Server 代理启动时。

- 每当计算机的 CPU 使用率处于定义的空闲状态水平时。

- 在特定日期和时间运行一次。

- 按重复的计划运行。

有关详细信息，请参阅 [创建计划并将计划附加到作业](../../ssms/agent/create-and-attach-schedules-to-jobs.md)。

### <a name="alerts"></a>警报

“警报”  是对特定事件的自动响应。 例如，事件可以是启动的作业，也可以是达到特定阈值的系统资源。 可以定义警报产生的条件。

警报可以响应下列任一条件：

- SQL Server 事件

- SQL Server 性能条件

- 运行 SQL Server 代理的计算机上的 Microsoft Windows Management Instrumentation (WMI) 事件

警报可以执行下列操作：

- 通知一个或多个操作员

- 运行作业

有关详细信息，请参阅 [“警报”](../../ssms/agent/alerts.md)。  

### <a name="operators"></a>运算符

“操作员”  定义的是负责维护一个或多个 SQL Server 实例的个人的联系信息。 在有些企业中，操作员职责被分配给一个人。 在拥有多个服务器的企业中，操作员职责可以由多人分担。 操作员不涉及安全信息，因此不会定义安全主体。  

SQL Server 可以向操作员发送警报通知，方法是通过...

- 电子邮件

- 寻呼程序（通过电子邮件）

- **net send**

> [!NOTE]
> SQL Server 代理所在的计算机必须启动 Windows Messenger 服务，才能使用net send 发送通知。

> [!IMPORTANT]
> 在 SQL Server 的未来版本中，将从 SQL Server 代理中删除寻呼程序和 net send 选项。 请避免在新的开发工作中使用这些功能，并考虑修改当前使用这些功能的应用程序。  

若要使用电子邮件或寻呼程序向操作员发送通知，必须将 SQL Server 代理配置为使用数据库邮件。 有关详细信息，请参阅[数据库邮件](../../relational-databases/database-mail/database-mail.md)。

可以将操作员定义为一组个人的别名。 这样，该别名的所有成员就可以同时进行验证。 有关详细信息，请参阅[运算符](../../ssms/agent/operators.md)。

## <a name="security-for-sql-server-agent-administration"></a><a name="Security"></a>SQL Server 代理管理的安全性

SQL Server 代理在 msdb 数据库中使用了 SQLAgentUserRole、 SQLAgentReaderRole 和 SQLAgentOperatorRole 固定数据库角色，用于控制非 SQL Server sysadmin 固定服务器角色成员的用户对代理的访问    。 除了这些固定数据库角色之外，子系统和代理还有助于数据库管理员确保每个作业步骤运行时都具有执行其任务所需的最低权限。  

### <a name="roles"></a>角色

msdb 数据库中的 SQLAgentUserRole、 SQLAgentReaderRole 和 SQLAgentOperatorRole 固定数据库角色的成员，以及 sysadmin 固定服务器角色的成员具有访问 SQL Server 代理的权限    。 不属于这些角色的用户不能使用 SQL Server 代理。 有关用于 SQL Server 代理的角色的详细信息，请参阅[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)。

### <a name="subsystems"></a>子系统

子系统是预定义的对象，表示作业步骤可用的功能。 每个代理都可以访问一个或多个子系统。 子系统可以提供安全性，因为它们分隔了对可用于代理的功能的访问。 除了 Transact\-SQL 作业步骤，每个作业步骤都在代理的上下文中运行。 Transact\-SQL 作业步骤使用 EXECUTE AS 命令将安全上下文设置为作业的所有者。  

SQL Server 定义了下表中列出的子系统：  

|子系统名称|说明|
|--------------|-----------|
|Microsoft ActiveX 脚本|运行 ActiveX 脚本作业步骤。<br /><br />**警告** 在未来版本的 Microsoft SQL Server 中，将从 SQL Server 代理中删除 ActiveX 脚本编写子系统。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。|  
|操作系统 (**CmdExec**)|运行可执行程序。|
|PowerShell|运行 PowerShell 脚本作业步骤。|
|复制分发服务器|运行激活复制分发服务器代理的作业步骤。|
|复制合并|运行激活复制合并代理的作业步骤。|
|复制队列读取器|运行激活复制队列读取器代理的作业步骤。|
|复制快照|运行激活复制快照代理的作业步骤。|
|复制事务日志读取器|运行激活复制日志读取器代理的作业步骤。|
|Analysis Services 命令|运行 Analysis Services 命令。|
|Analysis Services 查询|运行 Analysis Services 查询。|
|SSIS 包执行|运行 SSIS 包。|

> [!NOTE]
> 由于 Transact\-SQL 作业步骤中不使用代理，因此没有用于 Transact\-SQL 作业步骤的 SQL Server 代理子系统。  

SQL Server 代理会强制实施子系统限制，即使代理的安全主体正常拥有在作业步骤中运行任务的权限也是如此。 例如，如果一个用户是 sysadmin 固定服务器角色的成员，则即使该用户可以运行 SSIS 包，其代理仍将无法运行 SSIS 作业步骤，除非该代理可以访问 SSIS 子系统。  

### <a name="proxies"></a>代理

SQL Server 代理使用代理管理安全上下文。 一个代理可用于多个作业步骤。 **sysadmin** 固定服务器角色的成员可创建代理。  

每个代理对应于一个安全凭据。 每个代理都可与一组子系统和一组登录名相关联。 代理仅可用于使用与该代理相关联的子系统的作业步骤。 若要创建使用特定代理的作业步骤，作业所有者必须使用与此代理相关联的登录名，或者是对代理具有无限访问权限的角色成员。 **sysadmin** 固定服务器角色的成员可以无限制地访问代理。 **SQLAgentUserRole**、 **SQLAgentReaderRole** 或 **SQLAgentOperatorRole** 的成员仅能使用授予他们特定访问权限的代理。 这些 SQL Server 代理固定数据库角色的每个成员用户必须获得访问特定代理的权限，才能创建使用那些代理的作业步骤。  

## <a name="related-tasks"></a>Related Tasks

使用以下步骤来配置 SQL Server 代理以自动管理 SQL Server：

1. 确定哪些管理任务或服务器事件定期执行以及这些任务或事件是否可以通过编程方式进行管理。 如果任务涉及一系列可预见的步骤并且在特定时间或响应特定事件时执行，则该任务非常适合自动化。

2. 使用 SQL Server Management Studio、Transact\-SQL 脚本或 SQL Server 管理对象 (SMO) 定义一组作业、计划、警报和操作员。 有关详细信息，请参阅 [创建作业](../../ssms/agent/create-jobs.md)。  

3. 运行你定义的 SQL Server 代理作业。

> [!NOTE]
> 对于默认的 SQL Server 实例，SQL Server 服务命名为 SQLSERVERAGENT。 对于命名实例，SQL Server 代理服务将被命名为 SQLAgent$instancename。

如果您正在运行 SQL Server 的多个实例，则可以使用多服务器管理来自动管理所有实例的公共任务。 有关详细信息，请参阅 [企业范围的自动化管理](../../ssms/agent/automated-administration-across-an-enterprise.md)。

通过以下任务以开始使用 SQL Server 代理：

|说明|主题|
|-----------|-----|
|介绍如何配置 SQL Server 代理。|[配置 SQL Server 代理](../../ssms/agent/configure-sql-server-agent.md)|  
|介绍如何启动、停止和暂停 SQL Server 代理服务。|[启动、停止或暂停 SQL Server 代理服务](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)|
|介绍为 SQL Server 代理服务指定帐户时的注意事项。|[为 SQL Server 代理服务选择帐户](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md)|
|介绍如何使用 SQL Server 代理错误日志。|[SQL Server 代理错误日志](../../ssms/agent/sql-server-agent-error-log.md)|  
|介绍如何使用性能对象。|[使用性能对象](../../ssms/agent/use-performance-objects.md)|
|介绍维护计划向导，它是一个实用工具，可用来创建作业、警报和运算符来对 SQL Server 实例进行自动管理。|[使用维护计划向导](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)|
|介绍如何使用 SQL Server 代理将管理任务自动化。|[自动执行管理任务（SQL Server 代理）](../../ssms/agent/automated-administration-tasks-sql-server-agent.md)|

## <a name="nosqlps"></a>NOSQLPS

[!INCLUDE [sql-server-powershell-no-sqlps](../../includes/sql-server-powershell-no-sqlps.md)]

## <a name="see-also"></a>另请参阅

- [外围应用配置器](../../relational-databases/security/surface-area-configuration.md)
- [SQL Server 代理与 PowerShell](../../powershell/sql-server-powershell.md#sql-server-agent)
