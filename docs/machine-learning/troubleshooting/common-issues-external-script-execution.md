---
title: 解决启动板服务的问题
description: 本文提供了一些关于阻止 SQL Server 受信任的启动板服务启动的疑难解答指南，包括配置问题或更改，或缺少网络协议。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/30/2019
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: b57fdb3abd3482d6a395e1e6690f2e628a2a3e9e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470678"
---
# <a name="troubleshoot-issues-with-launchpad-service-and-external-script-execution-in-sql-server"></a>解决启动板服务和 SQL Server 中外部脚本执行的问题
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文提供 SQL Server 受信任的启动板服务相关问题的疑难解答指南。 启动板服务支持 R 和 Python 的外部脚本执行。 多种问题可能会阻止启动板启动，包括配置问题或更改，或缺少网络协议。  

## <a name="determine-whether-launchpad-is-running"></a>确定启动板是否正在运行

1. 打开“服务”面板 (Services.msc)  。 或者，从命令行中键入 SQLServerManager13.msc 或 SQLServerManager14.msc，打开 [SQL Server 配置管理器](../../relational-databases/sql-server-configuration-manager.md)   。

2. 记下运行启动板的服务帐户。 启用 R 或 Python 的每个实例都应具有其自己的启动板服务实例。 例如，命名实例的服务可能类似于 MSSQLLaunchpad $ InstanceName  。

3. 如果服务停止，则重启该服务。 重启时，如果出现任何配置问题，则会在系统事件日志中发布一条消息，并再次停止该服务。 查看系统事件日志以获取有关服务停止原因的详细信息。

4. 查看 RSetup.log 的内容，并确保安装中没有错误。 例如，消息“以代码 0 退出”表示服务启动失败  。

5. 若要查找其他错误，请查看 rlauncher.log 的内容。

## <a name="check-the-launchpad-service-account"></a>查看启动板服务帐户

默认服务帐户可能是“NT Service $ SQL2016”或“NT Service $ SQL2017”\$\$。 最后一部分可能会有所不同，具体取决于 SQL 实例名称。

启动板服务 (Launchpad.exe) 通过使用低特权服务帐户运行。 但是，若要启动 R 和 Python 并与数据库实例进行通信，启动板服务帐户需要以下用户权限：

- 以服务身份登录 (SeServiceLogonRight)
- 替换进程级别标记 (SeAssignPrimaryTokenPrivilege)
- 跳过遍历检查 (SeChangeNotifyPrivilege)
- 调整进程的内存配额 (SeIncreaseQuotaSizePrivilege)

有关这些用户权限的信息，请参阅[配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)中的“Windows 特权和权限”部分。

> [!TIP]
> 如果你熟悉如何使用支持诊断平台 (SDP) 工具进行 SQL Server 诊断，则可以使用 SDP 查看名为 MachineName_UserRights.txt 的输出文件。

## <a name="user-group-for-launchpad-cannot-log-on-locally"></a>启动板的用户组无法在本地登录

在安装机器学习服务的过程中，SQL Server 将创建 Windows 用户组 SQLRUserGroup，然后为该组预配全部所需的权限，使启动板能够连接到 SQL Server 并运行外部脚本作业  。 如果启用了该用户组，它也将用于执行 Python 脚本。

但是，在实施了限制性更强的安全策略的组织中，该组所需的权限可能已被手动删除，或者可能被策略自动吊销。 如果已删除权限，则启动板将无法再连接到 SQL Server，并且 SQL Server 无法调用外部运行时。

若要解决此问题，请确保组 **SQLRUserGroup** 拥有系统权限“允许本地登录”。 

有关详细信息，请参阅[配置 Windows 服务帐户和权限](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)。

## <a name="permissions-to-run-external-scripts"></a>运行外部脚本的权限

即使 Launchpad 配置正确，如果用户没有运行 R 或 Python 脚本的权限，它也会返回错误。

如果以数据库管理员的身份安装 SQL Server 或你是数据库所有者，则会自动被授予此权限。 但是，其他用户具有的权限通常更加有限。 如果他们尝试运行 R 脚本，则会收到 Launchpad 错误。

若要解决此问题，在 SQL Server Management Studio 中，安全管理员可以通过运行以下脚本来修改 SQL 登录名或 Windows 用户帐户：

```sql
GRANT EXECUTE ANY EXTERNAL SCRIPT TO <username>
```

有关详细信息，请参阅 [GRANT (Transact-SQL](../../t-sql/statements/grant-transact-sql.md)。

## <a name="common-launchpad-errors"></a>常见 Launchpad 错误

本节列出了 Launchpad 返回的最常见的错误消息。

::: moniker range=">=sql-server-2016"
## <a name="unable-to-launch-runtime-for-r-script"></a>“无法启动 R 脚本的运行时”

如果 R 用户的 Windows 组（也用于 Python）无法登录到运行 R Services 的实例，则可能会看到以下错误：

- 尝试运行 R 脚本时生成的错误：

    * 无法启动“R”脚本的运行时。  请检查“R”运行时的配置。

    * 发生外部脚本错误。  无法启动运行时。

- [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] 服务生成的错误：

    * 无法初始化启动器 RLauncher.dll 

    * 未注册任何启动器 dll！ 

    * 安全日志指示帐户 NT SERVICE 无法登录 

若要了解如何为此用户组提供必要的权限，请参阅 [安装 SQL Server R Services](../install/sql-r-services-windows-install.md)。

> [!NOTE]
> 如果使用 SQL 登录名从远程工作站运行 R 脚本，则没有此限制。
::: moniker-end

## <a name="logon-failure-the-user-has-not-been-granted-the-requested-logon-type"></a>“登录失败: 尚未授予用户请求的登录类型”

默认情况下，[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] 使用以下帐户启动：`NT Service\MSSQLLaunchpad`。 帐户通过 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 安装程序配置为具有所有必需的权限。

如果将不同的帐户分配到 Launchpad，或者权限被 SQL Server 计算机上的某个策略删除，该帐户可能不会获得所需的权限，同时会出现以下错误：

>*ERROR_LOGON_TYPE_NOT_GRANTED 1385 (0x569) 登录失败: 未授予用户在此计算机上的请求登录类型。*

若要为新服务帐户授予必要的权限，请使用本地安全策略应用程序并更新帐户的权限，以便包括以下权限：

+ 调整进程的内存配额 (SeIncreaseQuotaPrivilege)
+ 跳过遍历检查 (SeChangeNotifyPrivilege)
+ 以服务身份登录 (SeServiceLogonRight)
+ 替换进程级别标记 (SeAssignPrimaryTokenPrivilege)

## <a name="unable-to-communicate-with-the-launchpad-service"></a>“无法与 Launchpad 服务通信”

如果已经安装并启用了机器学习功能，但是在尝试运行 R 或 Python 脚本时遇到此错误，则实例的 Launchpad 服务可能已停止运行。

1. 通过 Windows 命令提示符打开 SQL Server 配置管理器。 有关详细信息，请参阅 [SQL Server Configuration Manager](../../relational-databases/sql-server-configuration-manager.md)。

2. 右键单击“SQL Server Launchpad 实例”，然后选择“属性”  。

3. 选择“服务”选项卡，然后检查该服务是否正在运行  。 如果未运行，请将“启动模式”更改为“自动”，然后选择“应用”    。

4. 重启服务通常可以解决问题，从而可以运行机器学习脚本。 如果重启不能解决问题，请记下“二进制文件路径”属性中的路径和参数，然后执行以下操作  ：

    a. 检查启动器的 .config 文件，确保工作目录有效。

    b. 确保 Launchpad 使用的 Windows 组可以连接到 SQL Server 实例。

    c. 如果对服务属性做了任何更改，请重启 Launchpad 服务。

## <a name="fatal-error-creation-of-tmpfile-failed"></a>“严重错误: tmpFile 创建失败”

在这种情况下，你已经成功安装了机器学习功能，并且 Launchpad 正在运行。 尝试运行一些简单的 R 或 Python 代码，但是 Launchpad 失败，并显示如下错误： 

>无法与 R 脚本的运行时通信。  请检查 R 运行时的要求。

同时，外部脚本运行时将以下消息作为 STDERR 消息的一部分写入： 

>“严重错误: tmpFile 创建失败”  。

此错误表明 Launchpad 尝试使用的帐户没有登录数据库的权限。 实施严格的安全策略时可能会发生这种情况。 若要确定是否是这种情况，请查看 SQL Server 日志，并检查登录时是否拒绝了 MSSQLSERVER01 帐户。 R\_SERVICES 或 PYTHON\_SERVICES 特定的日志中提供了相同的信息。 查找 ExtLaunchError.log。

默认情况下，将设置 20 个帐户并将其与 Launchpad.exe 进程关联，名称为 MSSQLSERVER01 至 MSSQLSERVER20。 如果大量使用 R 或 Python，则可以增加帐户数量。

若要解决此问题，请确保该组对已安装并启用了机器学习功能的本地实例具有“允许本地登录”权限  。 在某些环境中，此权限级别可能需要网络管理员的 GPO 例外。

## <a name="not-enough-quota-to-process-this-command"></a>“配额不足，无法处理此命令”

此错误可能意味着以下情况之一：

- Launchpad 可能没有足够的外部用户来运行外部查询。 例如，如果同时运行 20 个以上的外部查询，并且只有 20 个默认用户，则一个或多个查询可能会失败。

- 没有足够的内存来处理 R 任务。 在默认环境中此错误最常发生，SQL Server 可能最多占用计算机资源的 70%。 有关如何通过 R 修改服务器配置以支持更多资源使用的信息，请参阅[操作 R 代码](../r/operationalizing-your-r-code.md)。

## <a name="cant-find-package"></a>“找不到包”

如果在 SQL Server 中运行 R 代码并收到此消息，但在 SQL Server 外部运行相同代码时未得到此消息，则表示该软件包未安装到 SQL Server 使用的默认库位置。

出现此错误的原因有多种：

- 在服务器上安装了新包，但访问被拒绝，因此 R 将软件包安装到了用户库。

- 安装了 R Services，然后安装了另一个 R 工具或一组库，包括 Microsoft R Server（独立版）、Microsoft R Client、RStudio 等。

若要确定实例使用的 R 包库的位置，请打开 SQL Server Management Studio（或任何其他数据库查询工具），连接到该实例，然后运行以下存储过程：

```sql
EXEC sp_execute_external_script @language = N'R',  
@script = N' print(normalizePath(R.home())); print(.libPaths());'; 
```

#### <a name="sample-results"></a>示例结果

*STDOUT message(s) from external script:*

[1] "C:\\Program Files\\Microsoft SQL Server\\MSSQL13.SQL2016\\R_SERVICES" 

[1] "C:/Program Files/Microsoft SQL Server/MSSQL13.SQL2016/R_SERVICES/library" 

若要解决此问题，必须将包重新安装到 SQL Server 实例库。

::: moniker range=">=sql-server-2016"
>[!NOTE]
>如果已升级 SQL Server 2016 实例以使用最新版本的 Microsoft R，则默认库位置会有所不同。 有关详细信息，请参阅 [使用 SqlBindR 升级 R Services 的实例](../install/upgrade-r-and-python.md)。
::: moniker-end

::: moniker range=">=sql-server-2016"
## <a name="launchpad-shuts-down-due-to-mismatched-dlls"></a>Launchpad 由于 DLL 不匹配而关闭

如果安装具有其他功能的数据库引擎，修补服务器，然后在以后使用原始媒体添加机器学习功能，则可能安装了错误版本的机器学习组件。 Launchpad 检测到版本不匹配时，它将关闭并创建转储文件。

若要避免此问题，请确保安装与服务器实例处于相同修补程序级别的任何新功能。

**错误的升级方法：**

1. 安装不带 R Services 的 SQL Server 2016。
2. 升级 SQL Server 2016 累积更新 2。
3. 使用 RTM 媒体安装 R Services（数据库内）。

**正确的升级方法：**

1. 安装不带 R Services 的 SQL Server 2016。
2. 将 SQL Server 2016 升级到所需的补丁程序级别。 例如，先安装 Service Pack 1，然后再安装累积更新 2。
3. 若要以正确的补丁程序级别添加功能，请再次运行 SP1 和 CU2 安装程序，然后选择“R Services (数据库内)”。 

## <a name="launchpad-fails-to-start-if-8dot3-notation-is-required"></a>如果需要 8dot3 表示法，Launchpad 无法启动

> [!NOTE] 
> 在较早的系统上，如果有 8dot3 表示法要求，Launchpad 可能无法启动。 以后版本将消除此要求。 SQL Server 2016 R Services 客户应安装以下之一：
> * SQL Server 2016 SP1 和 CU1：[SQL Server 累积更新 1](https://support.microsoft.com/help/3208177/cumulative-update-1-for-sql-server-2016-sp1)。
> * SQL Server 2016 RTM、累积更新 3 和可按需提供的[修补程序](https://support.microsoft.com/help/3210110/on-demand-hotfix-update-package-for-sql-server-2016-cu3)。

为了与 R 兼容，SQL Server 2016 R Services（数据库内）要求安装了该功能的驱动器支持使用“8dot3 表示法”创建短文件名  。 8\.3 文件名也称作“短文件名”，用于实现与之前的旧版 Microsoft Windows 或者作为一个长文件名的备用文件名  。

如果安装 R 的卷不支持短文件名，则从 SQL Server 启动 R 的进程可能找不到正确的可执行文件，且 Launchpad 将不会启动。

解决方法之一是应在安装了 SQL Server 和 R Services 的卷上启用 8dot3 表示法。 然后，必须提供 R Services 配置文件中工作目录的短名称。

1. 若要启用 8dot3 表示法，请运行包含 8dot3name 参数的 fsutil 实用程序，操作步骤如下所述：[fsutil 8dot3name](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff621566(v=ws.11))  。

2. 启用 8dot3 表示法后，打开 RLauncher.config 文件并记下属性 `WORKING_DIRECTORY`。 有关如何查找此文件的信息，请参阅[机器学习疑难解答的数据收集](data-collection-ml-troubleshooting-process.md)。

3. 使用包含 file 参数的 fsutil 实用程序，指定在 WORKING_DIRECTORY 中指定的文件夹的短文件路径  。

4. 编辑配置文件，指定在 WORKING_DIRECTORY 属性中输入的相同工作目录。 也可以指定一个不同的工作目录并选择一个已经与 8dot3 表示法兼容的现有路径。
::: moniker-end

## <a name="next-steps"></a>后续步骤

[机器学习服务疑难解答和已知问题](machine-learning-troubleshooting-overview.md)

[收集数据进行机器学习故障排除](data-collection-ml-troubleshooting-process.md)

[安装 SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md)

[数据库引擎连接疑难解答](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)