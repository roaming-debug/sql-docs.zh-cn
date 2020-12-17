---
description: 作业步骤属性 - 新建作业步骤（“常规”页）
title: 新建作业步骤属性（“常规”页）
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.job.stepgeneral.f1
ms.assetid: 8d1885ba-4386-4528-8f2b-68c16852720c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: e13935b59d22a6184de14a29910117b9611ad4cd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466468"
---
# <a name="job-step-properties---new-job-step-general-page"></a>作业步骤属性 - 新建作业步骤（“常规”页）

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

使用此页可以查看和更改 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业步骤的属性或定义新的作业步骤。  
  
若要导航到此页，请在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 对象资源管理器中展开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理，右键单击“作业”，单击“新建作业”，选择“步骤”页，再单击“新建”。 也可以通过以下方法导航到此页：在对象资源管理器中右键单击某项作业，单击“属性”，选择“步骤”页，再依次单击“新建”、“插入”或“编辑”。  
  
## <a name="options"></a>选项  
**步骤名称**  
设置作业步骤的名称。  
  
**类型**  
设置作业步骤使用的子系统。 显示的用于定义作业步骤的选项会根据所选子系统的不同而变化。  
  
**运行身份**  
为作业步骤设置代理帐户。 sysadmin 固定服务器角色的成员还可以指定“SQL 代理服务帐户”。  
  
**Database**  
设置在其中运行作业步骤的数据库。 此选项并不适用于所有作业步骤类型。  
  
**命令**  
设置作业步骤运行的命令。  
  
## <a name="options-for-transact-sql-job-steps"></a>Transact-SQL 作业步骤的选项  
**打开**  
从文件加载命令。  
  
**全选**  
选择命令文本。  
  
**复制**  
将所选文本复制到剪贴板。  
  
**粘贴**  
粘贴剪贴板的内容。  
  
**Parse**  
检查命令的语法。  
  
## <a name="options-for-activex-script-job-steps"></a>ActiveX 脚本作业步骤的选项  
  
> [!IMPORTANT]
> 在未来版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，将从 [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]代理中删除 ActiveX 脚本编写子系统。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。  
  
**VBScript**  
将 [!INCLUDE[msCoName](../../includes/msconame_md.md)] Visual Basic 脚本版本指定为作业步骤的语言。  
  
**JScript**  
将 JScript 指定为作业步骤的语言。  
  
**其他**  
键入用其他脚本语言编写的作业步骤的语言名称。  
  
**打开**  
从文件加载命令。  
  
**全选**  
选择命令文本。  
  
**复制**  
复制选定的文本。  
  
**粘贴**  
粘贴剪贴板的内容。  
  
## <a name="options-for-operating-system-cmdexec-job-steps"></a>操作系统 (CmdExec) 作业步骤的选项  
**成功命令的进程退出代码**  
键入命令返回的用来指示进程成功的退出代码。  
  
**打开**  
从文件加载命令。  
  
**全选**  
选择命令文本。  
  
**复制**  
复制选定的文本。  
  
**粘贴**  
粘贴剪贴板的内容。  
  
## <a name="options-for-powershell-job-steps"></a>PowerShell 作业步骤的选项  
**打开**  
从文件加载脚本。  
  
**全选**  
选择脚本文本。  
  
**复制**  
复制选定的文本。  
  
**粘贴**  
粘贴剪贴板的内容。  
  
## <a name="options-for-replication-distributor-job-steps"></a>复制分发服务器作业步骤的选项  
**全选**  
选择命令文本。  
  
**复制**  
复制选定的文本。  
  
**粘贴**  
粘贴剪贴板的内容。  
  
## <a name="options-for-replication-merge-job-steps"></a>复制合并作业步骤的选项  
**全选**  
选择命令文本。  
  
**复制**  
复制选定的文本。  
  
**粘贴**  
粘贴剪贴板的内容。  
  
## <a name="options-for-replication-queue-reader-job-steps"></a>复制队列读取器作业步骤的选项  
**Database**  
用于作业步骤的数据库。  
  
**全选**  
选择命令文本。  
  
**复制**  
复制选定的文本。  
  
**粘贴**  
粘贴剪贴板的内容。  
  
## <a name="options-for-replication-snapshot-job-steps"></a>复制快照作业步骤的选项  
**全选**  
选择命令文本。  
  
**复制**  
复制选定的文本。  
  
**粘贴**  
粘贴剪贴板的内容。  
  
## <a name="options-for-replication-transaction-log-reader-job-steps"></a>复制事务-日志读取器作业步骤的选项  
**全选**  
选择命令文本。  
  
**复制**  
复制选定的文本。  
  
**粘贴**  
粘贴剪贴板的内容。  
  
## <a name="options-for-sql-server-analysis-services-command-job-steps"></a>SQL Server Analysis Services 命令作业步骤的选项  
**Server**  
选择运行作业步骤的服务器。  
  
**打开**  
从文件加载命令。  
  
**全选**  
选择命令文本。  
  
**复制**  
复制选定的文本。  
  
**粘贴**  
粘贴剪贴板的内容。  
  
## <a name="options-for-sql-server-analysis-services-query-job-steps"></a>SQL Server Analysis Services 查询作业步骤的选项  
**Server**  
选择运行作业步骤的服务器。  
  
**Database**  
用于作业步骤的数据库。  
  
**打开**  
从文件加载命令。  
  
**全选**  
选择命令文本。  
  
**复制**  
复制选定的文本。  
  
**粘贴**  
粘贴剪贴板的内容。  
  
## <a name="options-for-integration-services-package-execution-job-steps"></a>Integration Services 包执行作业步骤的选项  
  
### <a name="general-tab"></a>“常规”选项卡  
指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ([!INCLUDE[ssIS](../../includes/ssis_md.md)]) 包的位置以及使用的身份验证方法。 选择此选项卡后，可以使用以下选项。  
  
**包源**  
指定 [!INCLUDE[ssIS](../../includes/ssis_md.md)] 包的存储位置。 选择以下选项之一：  
  
-   **SQL Server**  
  
-   **文件系统**  
  
-   **SSIS 包存储区**  
  
**Server**  
键入存储 [!INCLUDE[ssIS](../../includes/ssis_md.md)] 包的服务器名。 仅当为“包源”指定了 **SQL Server** 或“SSIS 包存储区”时，此选项才可用。  
  
**Use Windows Authentication**  
使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Windows 身份验证登录到 [!INCLUDE[msCoName](../../includes/msconame_md.md)] 。  
  
**Use SQL Server Authentication**  
使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证登录 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果选择了此身份验证方法，请输入相应的“用户名”和“密码”。  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供身份验证是为了向后兼容。 为了增强安全性，请使用 Windows 身份验证（如果可能的话）。  
  
**包**  
键入包的位置。  
  
> [!IMPORTANT]  
> 对于受密码保护的 [!INCLUDE[ssIS](../../includes/ssis_md.md)] 包，请单击“配置”选项卡，在“包密码”对话框中输入密码。 否则，执行受密码保护包的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业将失败。  
  
### <a name="configurations-tab"></a>“配置”选项卡  
为 [!INCLUDE[ssIS](../../includes/ssis_md.md)] 包指定配置选项。 选择此选项卡后，以下选项可用：  
  
**配置文件**  
列出包的配置文件。  
  
**添加**  
添加包的配置文件。  
  
**删除**  
删除包的配置文件。  
  
**上移**  
将所选的配置文件上移。  
  
**“下移”**  
将所选的配置文件下移。  
  
### <a name="command-files-tab"></a>“命令文件”选项卡  
选择包的命令文件。 将按照命令文件在列表中的显示顺序对其进行处理。 选择此选项卡后，可以使用以下选项。  
  
**命令文件**  
列出包的命令文件。  
  
**添加**  
添加命令文件。  
  
**删除**  
删除所选的命令文件。  
  
**上移**  
将所选的命令文件上移。  
  
**“下移”**  
将所选的命令文件下移。  
  
### <a name="data-sources-tab"></a>“数据源”选项卡  
在此选项卡上查看包中指定的数据源。  
  
**连接管理器**  
查看数据源的名称。  
  
**说明**  
查看数据源的说明。  
  
**连接字符串**  
查看数据源的连接字符串。  
  
### <a name="execution-options-tab"></a>“执行选项”选项卡  
在此选项卡上查看或更改包的执行选项。  
  
**发生验证警告时包失败**  
如果希望在出现验证警告时包执行失败，请选择此选项。  
  
**验证但不执行包**  
如果希望作业步骤验证但不执行包，请选择此选项。  
  
**最大并发可执行文件数**  
可以同时运行的可执行文件的最大数量。  
  
**启用包检查点**  
如果希望作业步骤使用包检查点，请选择此选项。  
  
**检查点文件**  
键入包检查点文件的名称。  
  
**...**  
浏览以找到包检查点文件。  
  
**覆盖重新启动选项**  
如果希望为此作业步骤指定的重新启动选项与包中指定的重新启动选项不同，请选择此选项。  
  
**重新启动选项**  
选择包重新启动时要执行的操作。  
  
### <a name="logging-tab"></a>“日志记录”选项卡  
在此选项卡上查看或更改包的日志提供程序。  
  
**日志提供程序**  
选择日志提供程序的 ClassID。  
  
**配置字符串**  
键入日志提供程序的配置字符串。  
  
**删除**  
删除日志提供程序。  
  
### <a name="set-values-tab"></a>“设置值”选项卡  
在此选项卡上查看或更改包的属性值。  
  
**属性路径**  
查看或更改属性的路径。  
  
**值**  
查看或更改属性的值。  
  
**删除**  
删除属性。  
  
### <a name="verification-tab"></a>“验证”选项卡  
在此选项卡上为作业步骤选择验证选项。  
  
**仅执行已签名的包**  
仅运行已经签名的包。 选择此选项后，如果包没有签名，则作业步骤失败。  
  
**验证包内部版本**  
仅运行有特定内部版本号的包。 选择此选项后，如果包没有特定内部版本号，则作业步骤失败。  
  
**生成**  
键入包的内部版本号。  
  
**验证包 ID**  
仅运行有特定 ID 的包。 选择此选项后，如果包没有指定的 ID，则作业步骤将失败。  
  
**包 ID**  
键入包 ID。  
  
**验证版本 ID**  
仅运行有特定版本 ID 的包。 选择此选项后，如果包没有指定的版本 ID，则作业步骤将失败。  
  
**版本 ID**  
键入版本 ID。  
  
### <a name="command-line-tab"></a>“命令行”选项卡  
为包指定命令行选项。 选择此选项卡后，以下选项可用：  
  
**还原原始选项**  
使用在此对话框中设置的命令行选项。  
  
**手动编辑命令行**  
在命令行窗口中指定选项。  
  
**命令行**  
键入用于此包的命令行选项。  
  
## <a name="see-also"></a>另请参阅  
[管理作业步骤](../../ssms/agent/manage-job-steps.md)  
[包的 SQL Server 代理作业](../../integration-services/packages/sql-server-agent-jobs-for-packages.md)  
[管理复制代理](../../relational-databases/replication/agents/replication-agent-administration.md)  
