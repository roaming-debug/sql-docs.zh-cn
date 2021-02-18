---
description: 管理作业步骤
title: 管理作业步骤
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server replication]
- job steps [SQL Server Agent]
- jobs [SQL Server Agent], Integration Services package step
- executable programs as job steps
- operating systems [SQL Server], job steps
- Transact-SQL job step
- job steps [Transact-SQL]
- Integration Services packages, job steps
- replication job steps [SQL Server]
- logs [SQL Server], jobs
- SQL Server Agent jobs, job steps
- ActiveX scripting jobs [SQL Server]
- job steps [Analysis Services]
ms.assetid: 51352afc-a0a4-428b-8985-f9e58bb57c31
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 8892ff46f9c6c9fd382b39284cbc4bc81449a5d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355182"
---
# <a name="manage-job-steps"></a>管理作业步骤
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

作业步骤是作业对数据库或服务器执行的操作。 每个作业必须至少有一个作业步骤。 作业步骤可以为：  
  
-   可执行程序和操作系统命令。  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，包括存储过程和扩展存储过程。  
  
-   PowerShell 脚本。  
  
-   [!INCLUDE[msCoName](../../includes/msconame_md.md)] ActiveX 脚本。  
  
-   复制任务。  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] 任务。  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
每个作业步骤都在特定的安全上下文中运行。 如果作业步骤指定一个代理，该作业步骤将在该代理凭据的安全上下文中运行。 如果作业步骤没有指定代理，该作业步骤将在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户的上下文中运行。 只有 sysadmin 固定服务器角色成员可以创建没有显式指定代理的作业。  
  
由于作业步骤在特定 [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows 用户的上下文中运行，所以该用户必须具有执行作业步骤所需的权限和配置。 例如，如果您创建一个需要驱动器号或通用命名约定 (UNC) 路径的作业，则在测试任务时，可使用您的 Windows 用户帐户来运行作业步骤。 但是，运行作业步骤的 Windows 用户还必须具有所需的权限、驱动器号配置权限或对所需驱动器的访问权限。 否则，作业步骤会失败。 为了防止出现这种问题，请确保每个作业步骤的代理都具有该作业步骤所执行任务的必要权限。 有关详细信息，请参阅 [安全性和保护（数据库引擎）](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)。  
  
## <a name="job-step-logs"></a>作业步骤日志  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理可以将某些作业步骤的输出写入操作系统文件，也可以将其写入 msdb 数据库中的 sysjobstepslogs 表。 下列类型的作业步骤的输出可以写入以上两个目标位置：  
  
-   可执行程序和操作系统命令。  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] 任务。  
  
只有作为 sysadmin 固定服务器角色成员的用户执行的作业步骤的输出可以写入操作系统文件。 如果作业步骤由作为 msdb 数据库中 SQLAgentUserRole、SQLAgentReaderRole 或 SQLAgentOperatorRole 等固定数据库角色成员的用户执行，则只能将这些作业步骤的输出写入 sysjobstepslogs 表。  
  
删除作业或作业步骤时，会自动删除作业步骤日志。  
  
> [!NOTE]  
> 复制任务和 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包作业步骤日志记录由它们各自的子系统处理。 无法使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理来为这些类型的作业步骤配置作业步骤日志记录。  
  
## <a name="executable-programs-and-operating-system-commands-as-job-steps"></a>作为作业步骤的可执行程序和操作系统命令  
可以将可执行程序和操作系统命令作为作业步骤。 这些文件的扩展名可以是 .bat、.cmd、.com 或 .exe。  
  
使用可执行程序或操作系统命令作为作业步骤时，必须指定以下内容：  
  
-   命令成功完成时返回的进程退出代码。  
  
-   要执行的命令。 若要执行操作系统命令，只需指定该命令本身。 对于外部程序，这就是该程序的名称和用于该程序的参数，例如： **C:\Program Files\Microsoft SQL Server\100\Tools\Binn\sqlcmd.exe -e -q "sp_who"**  
  
    > [!NOTE]  
    > 如果系统路径或执行作业步骤的用户的路径指定的目录中不包含此可执行程序，则必须提供可执行程序的完整路径。  
  
## <a name="transact-sql-job-steps"></a>Transact-SQL 作业步骤  
创建 [!INCLUDE[tsql](../../includes/tsql-md.md)] 作业步骤时，必须执行下列操作：  
  
-   确定要在其中运行作业的数据库。  
  
-   键入要执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 该语句可以调用存储过程，也可以调用扩展存储过程。  
  
还可以选择将现有的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 文件作为作业步骤的命令打开。  
  
[!INCLUDE[tsql](../../includes/tsql-md.md)] 作业步骤不使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的代理帐户。 而是由作业步骤的所有者或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户（如果作业步骤的所有者是 sysadmin 固定服务器角色的成员）运行作业步骤。 Sysadmin 固定服务器角色的成员还可以使用 sp_add_jobstep 存储过程的 [!INCLUDE[tsql](../../includes/tsql-md.md)] database_user_name *参数来指定* 作业步骤在其他用户的上下文中运行。 有关详细信息，请参阅 [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)。  
  
> [!NOTE]  
> 一个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 作业步骤可以包含多个批处理。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 作业步骤可以包含嵌入的 GO 命令。  
  
## <a name="powershell-scripting-job-steps"></a>PowerShell 脚本作业步骤  
当创建 PowerShell 脚本作业步骤时，必须指定以下两项之一作为步骤的命令：  
  
-   PowerShell 脚本的文本。  
  
-   要打开的现有 PowerShell 脚本文件。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理 PowerShell 子系统打开一个 PowerShell 会话，并加载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell 管理单元。用作作业步骤命令的 PowerShell 脚本可以引用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell 提供程序和 cmdlet。 有关使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell 管理单元编写 PowerShell 脚本的详细信息，请参阅 [SQL Server PowerShell](../../powershell/sql-server-powershell.md)。  
  
## <a name="activex-scripting-job-steps"></a>ActiveX 脚本作业步骤  
  
> [!IMPORTANT]
> 在未来版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，ActiveX 脚本作业步骤将从 [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]代理中删除。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。  
  
创建 ActiveX 脚本作业步骤时，必须执行下列操作：  
  
-   确定编写作业步骤所使用的脚本语言。  
  
-   编写 ActiveX 脚本。  
  
还可以将现有的 ActiveX 脚本文件作为作业步骤的命令打开。 另外，可以在外部编译 ActiveX 脚本命令（例如，使用 Microsoft Visual Basic），然后作为可执行程序运行。  
  
当作业步骤命令是 ActiveX 脚本时，可以使用 SQLActiveScriptHost 对象将结果输出到作业步骤历史记录日志或创建 COM 对象。 SQLActiveScriptHost 是一个全局对象，由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理宿主系统引入脚本命名空间。 该对象具有两种方法（Print 和 CreateObject）。 下面的示例说明 ActiveX 脚本在 Visual Basic Scripting Edition (VBScript) 中是如何实现的。  
  
```  
' VBScript example for ActiveX Scripting job step  
' Create a Dmo.Server object. The object connects to the  
' server on which the script is running.  
  
Set oServer = CreateObject("SQLDmo.SqlServer")  
oServer.LoginSecure = True  
oServer.Connect "(local)"  
'Disconnect and destroy the server object  
oServer.DisConnect  
Set oServer = nothing  
```  
  
## <a name="replication-job-steps"></a>复制作业步骤  
使用复制创建发布和订阅时，默认情况下会创建复制作业。 创建的作业类型由复制的类型（快照、事务或合并）和所用选项确定。  
  
复制作业步骤将激活下列复制代理之一：  
  
-   快照代理（快照作业）  
  
-   日志读取器代理（LogReader 作业）  
  
-   分发代理（分发作业）  
  
-   合并代理（合并作业）  
  
-   队列读取器代理（QueueReader 作业）  
  
设置复制后，您可以指定以下列三种方式之一运行复制代理：在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理启动后连续运行、按需运行或按计划运行。 有关复制代理的详细信息，请参阅 [复制代理概述](../../relational-databases/replication/agents/replication-agents-overview.md)。  
  
## <a name="analysis-services-job-steps"></a>Analysis Services 作业步骤  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理支持两种不同类型的 Analysis Services 作业步骤：命令作业步骤和查询作业步骤。  
  
### <a name="analysis-services-command-job-steps"></a>Analysis Services 命令作业步骤  
创建 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] 命令作业步骤时，必须：  
  
-   标识要运行作业步骤的数据库 OLAP 服务器。  
  
-   键入要执行的语句。 对于  Execute[!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)]  方法，此语句必须为 XML。 对于  Discover[!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)]  方法，此语句可能不包含完整的 SOAP 信封或 XML。 注意：虽然 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 支持完整的 SOAP 信封和 **Discover** 方法，但是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业步骤却不支持。  
  
### <a name="analysis-services-query-job-steps"></a>Analysis Services 查询作业步骤  
创建 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] 查询作业步骤时，必须：  
  
-   标识要运行作业步骤的数据库 OLAP 服务器。  
  
-   键入要执行的语句。 该语句必须是一个多维表达式 (MDX) 查询。  
  
有关 MDX 的详细信息，请参阅 [MDX 语句基础知识 (MDX)](/analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services?viewFallbackFrom=sql-server-ver15)。  
  
## <a name="integration-services-packages"></a>Integration Services 包  
当创建 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包作业步骤时，必须执行下列操作：  
  
-   确定包的源。  
  
-   确定包的位置。  
  
-   如果包需要配置文件，则确定配置文件。  
  
-   如果包需要命令文件，则确定命令文件。  
  
-   确定用于包的验证。 例如，可以指定包必须签名，也可以指定包必须具有特定的包 ID。  
  
-   确定包的数据源。  
  
-   确定包的日志提供程序。  
  
-   指定运行包之前要设置的变量和值。  
  
-   确定执行选项。  
  
-   添加或修改命令行选项。  
  
请注意，如果将包部署到 SSIS 目录并且指定 **SSIS 目录** 作为包的来源，则会自动获取包中的大多数此类配置信息。 在“配置”选项卡下，可以指定环境、参数值、连接管理器值、属性重写以及包是否在 32 位运行时环境下运行。  
  
有关创建运行 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的作业步骤的详细信息，请参阅 [包的 SQL Server 代理作业](../../integration-services/packages/sql-server-agent-jobs-for-packages.md)。  
  
## <a name="related-tasks"></a>Related Tasks  
  
|说明|主题|  
|-|-|  
|描述如何创建带有可执行程序的作业步骤。|[创建 CmdExec 作业步骤](../../ssms/agent/create-a-cmdexec-job-step.md)|  
|介绍如何重置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理权限。|[配置帐户以创建和管理 SQL Server 代理作业](../../ssms/agent/configure-a-user-to-create-and-manage-sql-server-agent-jobs.md)|  
|介绍如何创建 [!INCLUDE[tsql](../../includes/tsql-md.md)] 作业步骤。|[Create a Transact-SQL Job Step](../../ssms/agent/create-a-transact-sql-job-step.md)|  
|说明如何定义 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理 Transact-SQL 作业步骤的选项。|[Define Transact-SQL Job Step Options](../../ssms/agent/define-transact-sql-job-step-options.md)|  
|介绍如何创建 ActiveX 脚本作业步骤。|[Create an ActiveX Script Job Step](../../ssms/agent/create-an-activex-script-job-step.md)|  
|介绍如何创建和定义用于执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Analysis Services 命令和查询的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业步骤。|[Create an Analysis Services Job Step](../../ssms/agent/create-an-analysis-services-job-step.md)|  
|介绍在作业执行期间失败时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应执行什么操作。|[Set Job Step Success or Failure Flow](../../ssms/agent/set-job-step-success-or-failure-flow.md)|  
|说明如何在“作业步骤属性”对话框中查看作业步骤的详细信息。|[View Job Step Information](../../ssms/agent/view-job-step-information.md)|  
|说明如何删除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业步骤日志。|[Delete a Job Step Log](../../ssms/agent/delete-a-job-step-log.md)|  
  
## <a name="see-also"></a>另请参阅  
[sysjobstepslogs (Transact-SQL)](../../relational-databases/system-tables/dbo-sysjobstepslogs-transact-sql.md)  
[创建作业](../../ssms/agent/create-jobs.md)  
[sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
