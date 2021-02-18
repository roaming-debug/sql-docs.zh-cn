---
description: SQL Server 代理固定数据库角色
title: SQL Server 代理固定数据库角色
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- roles [SQL Server], SQL Server Agent
- SQL Server Agent, roles
- SQLAgentUserRole database role
- SQLAgentReaderRole database role
- multiple roles
- security [SQL Server Agent], fixed database roles
- fixed database roles [SQL Server]
- SQLAgentOperatorRole database role
ms.assetid: 719ce56b-d6b2-414a-88a8-f43b725ebc79
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 57de6b3b9d4282155af520ba53d8ec7aff96f194
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346115"
---
# <a name="sql-server-agent-fixed-database-roles"></a>SQL Server 代理固定数据库角色
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 具有下列 msdb 数据库固定数据库角色，使管理员可以更好地控制对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的访问。 下面按从低到高的访问权限列出了角色：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
如果用户不是其中某个角色的成员，连接到 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，“对象资源管理器”中的“SQL Server 代理”将不可见。 用户必须是这些固定数据库角色之一的成员，或者是 **sysadmin** 固定服务器角色的成员才能使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理。  
  
## <a name="permissions-of-sql-server-agent-fixed-database-roles"></a>SQL Server 代理固定数据库角色的权限  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理数据库角色权限相互之间存在包含关系，也就是说，较高特权的角色继承较低特权的角色对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理对象（包括警报、运算符、作业、计划和代理）的权限。 例如，如果已向最低特权的 **SQLAgentUserRole** 成员授予访问 proxy_A 的权限，那么即使没有为 **SQLAgentReaderRole** 和 **SQLAgentOperatorRole** 的成员显式授予访问 proxy_A 的权限，它们也将自动拥有访问此代理的权限。 这可能会产生安全隐患，将在下面有关每个角色的部分中对此进行详细的讨论。  
  
### <a name="sqlagentuserrole-permissions"></a>SQLAgentUserRole 权限  
**SQLAgentUserRole** 是具有最低特权的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理固定数据库角色。 它只对运算符、本地作业和作业计划拥有权限。 **SQLAgentUserRole** 的成员只对它们所拥有的本地作业和作业计划拥有权限。 它们不能使用多服务器作业（主服务器作业和目标服务器作业），也不能通过更改作业所有权来获得对它们还没有拥有的作业的访问权限。 **SQLAgentUserRole** 的成员只能在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 的“作业步骤属性”对话框中查看可用的代理列表。 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 对象资源管理器中，**SQLAgentUserRole** 的成员只能看到“作业”节点。  
  
> [!IMPORTANT]  
> 在向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理数据库角色成员授予代理访问权限之前，请考虑此设置的安全含义 。 **SQLAgentReaderRole** 和 **SQLAgentOperatorRole** 自动成为 **SQLAgentUserRole** 的成员。 这意味着 **SQLAgentReaderRole** 和 **SQLAgentOperatorRole** 的成员可以访问已被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQLAgentUserRole **的所有** 代理，并且可以使用这些代理。  
  
下表汇总了 **SQLAgentUserRole** 对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理对象的权限。  
  
|操作|运算符|本地作业<br /><br />（仅限于所拥有的作业）|作业计划<br /><br />（仅限于所拥有的计划）|代理|  
|----------|-------------|-----------------------------------|-------------------------------------------|-----------|  
|创建/修改/删除|否|是<br /><br />无法更改作业所有权。|是|否|  
|视图列表（枚举）|是<br /><br />可以获得可在 **sp_notify_operator** 和 Management Studio 的“作业属性”对话框中使用的可用运算符列表。|是|是|是<br /><br />只能在 Management Studio 的“作业步骤属性”对话框中使用代理列表。|  
|启用/禁用|否|是|是|不适用|  
|查看属性|否|是|是|否|  
|执行/停止/开始|不适用|是|不适用|不适用|  
|查看作业历史记录|不适用|是|不适用|不适用|  
|删除作业历史记录|不适用|否<br /><br />必须为 **SQLAgentUserRole** 的成员显式授予对 **sp_purge_jobhistory** 的 EXECUTE 权限才能删除它们所拥有的作业的作业历史记录。 这些成员不能删除任何其他作业的历史记录。|不适用|不适用|  
|附加/分离|不适用|不适用|是|不适用|  
  
### <a name="sqlagentreaderrole-permissions"></a>SQLAgentReaderRole 权限  
**SQLAgentReaderRole** 包括所有的 **SQLAgentUserRole** 权限，以及查看可用的多服务器作业及其属性和历史记录的列表的权限。 此角色的成员还可以查看所有可用作业和作业计划以及它们的属性的列表，而不只是它们所拥有的那些作业和作业计划。 **SQLAgentReaderRole** 成员不能通过更改作业所有权来获得对它们还没有拥有的作业的访问权限。 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 对象资源管理器中，**SQLAgentReaderRole** 的成员只能看到“作业”节点。  
  
> [!IMPORTANT]  
> 在向  Agentdatabaserole [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  的成员授予代理访问权限之前，请考虑此操作是否安全。 **SQLAgentReaderRole** 的成员将自动成为 **SQLAgentUserRole** 的成员。 这意味着 **SQLAgentReaderRole** 成员可以访问已被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQLAgentUserRole **的所有** 代理，并且可以使用这些代理。  
  
下表汇总了 **SQLAgentReaderRole** 对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理对象的权限。  
  
|操作|运算符|本地作业|多服务器作业|作业计划|代理|  
|----------|-------------|--------------|--------------------|-----------------|-----------|  
|创建/修改/删除|否|是（仅拥有的作业）<br /><br />无法更改作业所有权。|否|是（仅拥有的计划）|否|  
|视图列表（枚举）|是<br /><br />可以获得可在 **sp_notify_operator** 和 Management Studio 的“作业属性”对话框中使用的可用运算符列表。|是|是|是|是<br /><br />只能在 Management Studio 的“作业步骤属性”对话框中使用代理列表。|  
|启用/禁用|否|是（仅拥有的作业）|否|是（仅拥有的计划）|不适用|  
|查看属性|否|是|是|是|否|  
|编辑属性|否|是（仅拥有的作业）|否|是（仅拥有的计划）|否|  
|执行/停止/开始|不适用|是（仅拥有的作业）|否|不适用|不适用|  
|查看作业历史记录|不适用|是|是|不适用|不适用|  
|删除作业历史记录|不适用|否<br /><br />必须为 **SQLAgentReaderRole** 的成员显式授予对 **sp_purge_jobhistory** 的 EXECUTE 权限才能删除它们所拥有的作业的作业历史记录。 这些成员不能删除任何其他作业的历史记录。|否|不适用|不适用|  
|附加/分离|不适用|不适用|不适用|是（仅拥有的计划）|不适用|  
  
### <a name="sqlagentoperatorrole-permissions"></a>SQLAgentOperatorRole 权限  
**SQLAgentOperatorRole** 是具有最高特权的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理固定数据库角色。 它的权限包括 **SQLAgentUserRole** 和 **SQLAgentReaderRole** 的所有权限。 此角色的成员还可以查看运算符和代理的属性，并且可以枚举服务器上的可用代理和警报。  
  
**SQLAgentOperatorRole** 的成员还拥有对本地作业和计划的其他权限。 它们可以执行、停止或启动所有本地作业，还可以删除服务器上的任何本地作业的作业历史记录。 它们还可以启用或禁用服务器上的所有本地作业和计划。 若要启用或禁用本地作业或计划，此角色的成员必须使用存储过程 **sp_update_job** 和 **sp_update_schedule**。 SQLAgentOperatorRole 的成员只能指定那些指定了作业名称、计划名称或标识符的参数和 \@enabled 参数 。 如果它们指定了任何其他参数，则执行这些存储过程将失败。 **SQLAgentOperatorRole** 的成员不能通过更改作业所有权来获得对它们还没有拥有的作业的访问权限。  
  
在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 对象资源管理器中，**SQLAgentOperatorRole** 的成员可以看到“作业”、“警报”、“操作员”和“代理”节点。 但此角色的成员看不到“错误日志”节点。  
  
> [!IMPORTANT]  
> 在向  Agentdatabaserole [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  的成员授予代理访问权限之前，请考虑此操作是否安全。 **SQLAgentOperatorRole** 的成员将自动成为 **SQLAgentUserRole** 和 **SQLAgentReaderRole** 的成员。 这意味着 **SQLAgentOperatorRole** 的成员可以访问已被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQLAgentUserRole **或** SQLAgentReaderRole **的所有** 代理，并且可以使用这些代理。  
  
下表汇总了 **SQLAgentOperatorRole** 对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理对象的权限。  
  
|操作|警报|运算符|本地作业|多服务器作业|作业计划|代理|  
|----------|----------|-------------|--------------|--------------------|-----------------|-----------|  
|创建/修改/删除|否|否|是（仅拥有的作业）<br /><br />无法更改作业所有权。|否|是（仅拥有的计划）|否|  
|视图列表（枚举）|是|是<br /><br />可以获得可在 **sp_notify_operator** 和 Management Studio 的“作业属性”对话框中使用的可用运算符列表。|是|是|是|是|  
|启用/禁用|否|否|是<br /><br />SQLAgentOperatorRole 的成员可以通过使用存储过程 sp_update_job 并指定 \@enabled 和 \@job_id （或 \@job_name）参数的值来启用或禁用它们尚未拥有的本地作业。 如果此角色的成员为此存储过程指定任何其他参数，则执行此过程将会失败。|否|是<br /><br />SQLAgentOperatorRole 的成员可以通过使用存储过程 sp_update_schedule 并指定 \@enabled 和 \@schedule_id （或 \@name）参数的值来启用或禁用它们尚未拥有的计划。 如果此角色的成员为此存储过程指定任何其他参数，则执行此过程将会失败。|不适用|  
|查看属性|是|是|是|是|是|是|  
|编辑属性|否|否|是（仅拥有的作业）|否|是（仅拥有的计划）|否|  
|执行/停止/开始|不适用|不适用|是|否|不适用|不适用|  
|查看作业历史记录|不适用|不适用|是|是|不适用|不适用|  
|删除作业历史记录|不适用|不适用|是|否|不适用|不适用|  
|附加/分离|不适用|不适用|不适用|不适用|是（仅拥有的计划）|不适用|  
  
## <a name="assigning-users-multiple-roles"></a>为用户分配多个角色  
**sysadmin** 固定服务器角色的成员可以访问所有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理功能。 如果用户不是 **sysadmin** 角色的成员，但却是多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理固定数据库角色的成员，那么记住这些角色具有相互包含的权限模型很重要。 因为较高特权的角色总是包含较低特权角色的所有权限，所以作为多个角色的成员的用户将自动将权限与其所属的最高特权的角色相关联。  
  
## <a name="see-also"></a>另请参阅  
[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)  
[sp_update_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)  
[sp_update_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md)  
[sp_notify_operator (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-notify-operator-transact-sql.md)  
[sp_purge_jobhistory (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-purge-jobhistory-transact-sql.md)  
