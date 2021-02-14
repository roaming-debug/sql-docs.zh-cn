---
title: Windows 的隔离更改
description: 本文介绍 Windows 上 SQL Server 2019 中机器学习服务的隔离机制的更改。 这些更改会影响 SQLRUserGroup、防火墙规则、文件权限和默示身份验证。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/05/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: ace298ff0e69ef3e3ee964c15d894280d6ef1b99
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082361"
---
# <a name="sql-server-2019-on-windows-isolation-changes-for-machine-learning-services"></a>Windows 上的 SQL Server 2019：机器学习服务的隔离更改
[!INCLUDE [SQL Server 2019 - Windows only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

本文介绍 Windows 上 SQL Server 2019 中机器学习服务的隔离机制的更改。 这些更改会影响 SQLRUserGroup、防火墙规则、文件权限和默示身份验证  。

有关详细信息，请参阅如何[在 Linux 上安装 SQL Server 机器学习服务](sql-machine-learning-services-windows-install.md)。

## <a name="changes-to-isolation-mechanism"></a>隔离机制的更改

在 Windows 上，SQL Server 2019 安装程序更改了外部进程的隔离机制。 此更改将本地工作线程帐户替换为 [AppContainer](/windows/desktop/secauthz/appcontainer-isolation)，这是用于 Windows 上运行的客户端应用程序的隔离技术。 

修改后，管理员没有特定的操作项。 在新的或已升级的服务器上，从 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 执行的所有外部脚本和代码都会自动遵循新的隔离模型。 

概括而言，此版本的主要区别是：

+ 不再创建或使用 SQL 受限用户组 (SQLRUserGroup) 下的本地用户帐户来运行外部进程  。 AppContainer 将替换它们。
+ SQLRUserGroup 成员身份已更改  。 成员身份仅由 SQL Server Launchpad 服务帐户组成，而非多个本地用户帐户。 R 和 Python 进程现在在 Launchpad 服务标识下执行，并通过 AppContainer 隔离。

虽然隔离模型发生了更改，但安装向导和命令行参数在 SQL Server 2019 中保持不变。 有关安装帮助，请参阅[安装 SQL Server 机器学习服务](sql-machine-learning-services-windows-install.md)。

## <a name="about-appcontainer-isolation"></a>关于 AppContainer 隔离

在以前的版本中，SQLRUserGroup 包含一个用于隔离和运行外部进程的本地 Windows 用户帐户池 (MSSQLSERVER00-MSSQLSERVER20)  。 需要运行外部进程时，SQL Server Launchpad 服务将获取一个可用帐户，并使用该帐户运行进程。 

在 SQL Server 2019 中，安装程序不再创建本地工作线程帐户。 而是通过 [AppContainer](/windows/desktop/secauthz/appcontainer-isolation) 来实现隔离。 在运行时，如果在存储过程或查询中检测到嵌入式脚本或代码，SQL Server 会调用 Launchpad，并请求提供特定于扩展的启动程序。 Launchpad 在进程中以标识调用相应的运行时环境，并实例化 AppContainer 以将其包含在内。 此更改十分有用，因为不再需要本地帐户和密码管理。 此外，在禁止本地用户帐户的安装上，不再依赖本地用户帐户意味着现在就可使用此功能。

因为由 SQL Server 实现，所以 AppContainer 是一种内部机制。 尽管你不会在进程监视器中看到 AppContainer 的物理证据，但是可以在出站防火墙规则中找到它们，这些规则是由安装程序创建的，用于防止进程进行网络调用。

## <a name="firewall-rules-created-by-setup"></a>由安装程序创建的防火墙规则

默认情况下，SQL Server 通过创建防火墙规则来禁用出站连接。 在过去，这些规则基于本地用户帐户，其中安装程序为 SQLRUserGroup 创建了一个出站规则，该规则拒绝对其成员进行网络访问（每个工作线程帐户均作为受该规则约束的本地原则列出）  。 

作为迁移到 AppContainer 的一部分，有基于 AppContainer SID 的新防火墙规则：每个规则分别针对由 SQL Server 安装程序创建的 20 个 AppContainer。 防火墙规则名称的命名约定是阻止 SQL Server 实例 MSSQLSERVER 中的 AppContainer-00 的网络访问，其中 00 是 AppContainer 的编号（默认情况下为 00-20），MSSQLSERVER 是 SQL Server 实例的名称  。 

> [!Note]
> 如果需要网络调用，可以禁用 Windows 防火墙中的出站规则。

<a name="file-permissions"></a>

## <a name="file-permissions"></a>文件权限

默认情况下，外部 Python 和 R 脚本只对其工作目录具有读取访问权限。 

如果你的 Python 或 R 脚本需要访问任何其他目录，则需要向“NT Service\MSSQLLaunchpad”服务用户帐户和此目录上的“ALL APPLICATION PACKAGES”授予“读取和执行”和/或“写入”权限     。

请按照以下步骤授予访问权限。

1. 在“文件资源管理器”中，右键单击要用作工作目录的文件夹，然后选择“属性”  。
1. 选择“安全性”并单击“编辑...”以更改权限   。
1. 单击“添加...” 
1. 请确保“从此位置”为本地计算机名称  。
1. 在“输入要选择的对象名称”中输入“ALL APPLICATION PACKAGES”，单后单击“检查名称”    。 单击“确定”。 
1. 在“允许”列下选择“读取和执行”   。
1. 如果要授予写入权限，请在“允许”列下选择“写入”   。
1. 单击两次“确定”。  

### <a name="program-file-permissions"></a>程序文件权限

与以前的版本一样，SQLRUserGroup 继续提供 SQL Server Binn、R_SERVICES 和 PYTHON_SERVICES 目录中的可执行文件的读取和执行权限     。 在此版本中，SQLRUserGroup 的唯一成员是 SQL Server Launchpad 服务帐户  。  当 Launchpad 服务启动 R 或 Python 执行环境时，该进程作为 Launchpad 服务运行。

## <a name="implied-authentication"></a>隐式身份验证

如前所述，如果脚本或代码必须使用受信任的身份验证连接回 SQL Server 以检索数据或资源，则默示身份验证仍需要其他配置  。 其他配置包括为 SQLRUserGroup 创建数据库登录名，其唯一成员现在是单个 SQL Server Launchpad 服务帐户，而不是多个工作线程帐户  。 有关此任务的详细信息，请参阅[将 SQLRUserGroup 添加为数据库用户](../security/create-a-login-for-sqlrusergroup.md)。


## <a name="symbolic-link-created-by-setup"></a>由安装程序创建的符号链接

将创建到当前默认 R_SERVICES 和 PYTHON_SERVICES 的符号链接作为 SQL Server 安装程序的一部分   。 如果不想创建此链接，另一种方法是向指向文件夹的层次结构授予“所有应用程序包”的读取权限。


## <a name="see-also"></a>另请参阅

+ [在 Windows 上安装 SQL Server 机器学习服务](sql-machine-learning-services-windows-install.md)
+ [在 Linux 上安装 SQL Server 机器学习服务](../../linux/sql-server-linux-setup-machine-learning.md)