---
title: 安全的扩展性体系结构
description: 本文介绍 SQL Server 机器学习服务中扩展性框架的安全体系结构。 内容包括登录名和用户帐户、SQL Server Launchpad 服务、工作人员帐户、运行多个脚本和文件权限的安全性。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/14/2020
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.custom: contperfq1, seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: eb5ab3d1f6408bb63d194b964626bf303ba9e249
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94869993"
---
# <a name="security-architecture-for-the-extensibility-framework-in-sql-server-machine-learning-services"></a>SQL Server 机器学习服务中扩展性框架的安全体系结构

[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文介绍用于将 SQL Server 数据库引擎和相关组件与 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中的扩展性框架集成的安全体系结构。 它检查安全对象、服务、进程标识和权限。 本文所述的要点包括 Launchpad、SQLRUserGroup 和工作人员帐户的用途、外部脚本的进程隔离，以及如何将用户标识映射到工作人员帐户。

有关 SQL Server 中扩展性的主要概念和组件的详细信息，请参阅 [SQL Server 机器学习服务中的扩展性体系结构](extensibility-framework.md)。

## <a name="securables-for-external-script"></a>外部脚本的安全对象

外部脚本作为输入参数提交给为此目的而创建的[系统存储过程](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)，或者将其包装在自行定义的存储过程中。 可以用 R、Python 或外部语言（例如 Java 或 .NET）编写此脚本。 或者，你可以预先定型模型，并将其以二进制格式存储在数据库表中，并可在 T-SQL [PREDICT](../../t-sql/queries/predict-transact-sql.md) 函数中调用。

由于脚本是通过现有的数据库架构对象、存储过程和表提供的，因此没有适用于 SQL Server 机器学习服务的新[安全对象](../../relational-databases/security/securables.md)。

无论脚本的使用方式或包含的内容如何，都将创建并可能保存数据库对象，但不会引入新的对象类型来存储脚本。 因此，能否使用、创建和保存数据库对象很大程度上取决于为用户定义的数据库权限。

<a name="permissions"></a>

## <a name="permissions"></a>权限

SQL Server 的数据库登录名和角色的数据安全模型扩展到外部脚本。 需要 SQL Server 登录名或 Windows 用户帐户才能运行使用 SQL Server 数据的外部脚本，或以 SQL Server 作为计算上下文运行的外部脚本。 具有执行查询权限的数据库用户可以从外部脚本访问相同的数据。

登录名或用户帐户标识安全主体，该主体可能需要多个级别的访问权限，具体取决于外部脚本需求  ：

+ 访问启用了外部脚本的数据库的权限。
+ 从安全对象（如表）读取数据的权限。
+ 能够将新数据写入表（如模型或评分结果）。
+ 能够创建新对象，如表、使用外部脚本的存储过程或使用外部脚步作业的自定义函数。
+ 在 SQL Server 计算机上安装新包的权限，或使用提供给一组用户的包的权限。

使用 SQL Server 作为执行上下文来运行外部脚本的每个用户都必须映射到数据库中的用户。 你可以创建角色来管理权限集（而不是单独设置数据库用户权限），并向这些角色分配用户（而不是单独设置用户权限）。

有关详细信息，请参阅[向用户授予 SQL Server 机器学习服务的权限](../../machine-learning/security/user-permission.md)。

## <a name="permissions-when-using-an-external-client-tool"></a>使用外部客户端工具时所需的权限

如果在外部客户端工具中使用脚本的用户需要在数据库中运行外部脚本或访问数据库对象和数据，则必须将其登录名或帐户映射到数据库中的用户。 无论是从远程数据科学客户端发送外部脚本，还是使用 T-SQL 存储过程运行外部脚本，都需要相同的权限。

例如，假设创建了一个在本地计算机上运行的外部脚本，并且想要在 SQL Server 上运行该脚本。 必须确保符合以下条件：

+ 数据库允许远程连接。
+ 用于数据库访问的 SQL 登录名或 Windows 帐户已添加到实例级别的 SQL Server 中。
+ SQL 登录名或 Windows 用户必须有权执行外部脚本。 通常，此权限只能由数据库管理员添加。
+ 在外部脚本执行以下任一操作的每个数据库中，必须将 SQL 登录名或 Windows 用户添加为拥有相应权限的用户：
  + 检索数据。
  + 写入或更新数据。
  + 创建新对象，例如表或存储过程。

预配登录名或 Windows 用户帐户并为其提供必要的权限后，可以通过使用以 R 语言编写的数据源对象或以 Python 语言编写的 revoscalepy 库，或调用包含外部脚本的存储过程在 SQL Server 上运行外部脚本  。

每当从 SQL Server 启动外部脚本时，数据库引擎安全就会获取启动作业的用户的安全性上下文，并管理该用户或登录名到安全对象的映射。

因此，从远程客户端启动的所有外部脚本必须在连接字符串中指定登录名或用户信息。

<a name="launchpad"></a>

::: moniker range="=sql-server-2016||=sql-server-2017||=sqlallproducts-allversions"

## <a name="services-used-in-external-processing-launchpad"></a>用于外部处理的服务 (Launchpad)

扩展性框架向 SQL Server 安装中的[服务列表](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md#Service_Details)添加一个新的 NT 服务：[SQL Server Launchpad (MSSSQLSERVER)](extensibility-framework.md#launchpad)  。

数据库引擎使用 SQL Server Launchpad 服务将外部脚本会话实例化为单独的进程  。 
该进程在低权限帐户下运行； 与 SQL Server、Launchpad 本身以及执行存储过程或主机查询的用户标识不同。 在单独的进程中以低权限帐户运行脚本是 SQL Server 中外部脚本的安全和隔离模型的基础。

SQL Server 还将调用用户的标识映射到用于启动附属进程的低权限工作人员帐户。 在某些情况下，脚本或代码回调到 SQL Server 以获取数据和操作时，SQL Server 能够无缝地管理标识传输。 如果发出调用的用户具有足够的权限，则包含 SELECT 语句或调用函数以及其他编程对象的脚本通常都会成功。

> [!NOTE]
> 默认情况下，[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] 配置为在 NT Service\MSSQLLaunchpad 下运行，其中预配有运行外部脚本所需的所有必要权限  。 有关可配置选项的详细信息，请参阅 [SQL Server Launchpad 服务配置](../security/sql-server-launchpad-service-account.md)。

::: moniker-end

::: moniker range=">=sql-server-ver15||=sqlallproducts-allversions"

## <a name="services-used-in-external-processing-launchpad"></a>用于外部处理的服务 (Launchpad)

扩展性框架向 SQL Server 安装中的 [服务列表](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md#Service_Details)添加一个新的 NT 服务：[**SQL Server Launchpad (MSSSQLSERVER)**](extensibility-framework.md#launchpad)。

数据库引擎使用 SQL Server Launchpad 服务将外部脚本会话实例化为单独的进程  。 
此进程在 Launchpad 用户标识下运行，但在 AppContainer 中包含附加限制。 在 AppContainer 下，在单独的进程中运行脚本是 SQL Server 中外部脚本的安全和隔离模型的基础。

SQL Server 还将调用用户的标识映射到用于启动附属进程的低权限工作人员帐户。 在某些情况下，脚本或代码回调到 SQL Server 以获取数据和操作时，SQL Server 能够无缝地管理标识传输。 如果发出调用的用户具有足够的权限，则包含 SELECT 语句或调用函数以及其他编程对象的脚本通常都会成功。

> [!NOTE]
> 默认情况下，[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] 配置为在 NT Service\MSSQLLaunchpad 下运行，其中预配有运行外部脚本所需的所有必要权限  。 有关可配置选项的详细信息，请参阅 [SQL Server Launchpad 服务配置](../security/sql-server-launchpad-service-account.md)。

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=sqlallproducts-allversions"

## <a name="services-used-in-external-processing"></a>用于外部处理的服务

扩展性框架在 SQL Server 安装中添加了一个新的守护程序：[mssql-launchpadd](extensibility-framework.md#launchpad)。 mssql-launchpadd 在安装 mssql-server-extensibility 包时创建的低特权帐户 mssql_launchpadd 下运行。

仅支持一个数据库引擎实例，并且该实例绑定一个 Launchpad 服务。 执行脚本后，Launchpad 服务使用低特权用户帐户 mssql_satellite 在其自己的新 PID、IPC、装载和网络命名空间中启动单独的 Launchpad 进程。 每个附属进程都会继承 Launchpad 的 mssql_satellite 用户帐户，并在脚本执行期间使用该帐户。

有关详细信息，请参阅 [SQL Server 机器学习服务中的扩展性体系结构](extensibility-framework.md)。

::: moniker-end

<a name="sqlrusergroup"></a>

::: moniker range="=sql-server-2016||=sql-server-2017||=sqlallproducts-allversions"

## <a name="identities-used-in-processing-sqlrusergroup"></a>用于处理的标识 (SQLRUserGroup)

SQLRUserGroup（SQL 受限用户组）由 SQL Server 安装程序创建，并且包含低权限本地 Windows 用户帐户的池  。 需要外部进程时，Launchpad 使用可用的工作人员帐户来运行进程。 更具体地说，Launchpad 会激活可用的工作人员帐户，将其映射到调用用户的标识，并在工作人员帐户下运行脚本。

+ SQLRUserGroup 链接到特定的实例  。 已启用机器学习的每个实例都需要一个单独的工作人员帐户池。 帐户不能在各个实例之间共享。

+ 用户帐户池的大小是静态的，默认值为 20，即支持 20 个并发会话。 可同时启动的外部运行时会话数量受此用户帐户池大小的限制。 

+ 池中的工作人员帐户名称的格式为 SQLInstanceNamenn  。 例如，在默认实例上，SQLRUserGroup 包含名为 MSSQLSERVER01、MSSQLSERVER02…直至 MSSQLSERVER20 的帐户。

并行化任务不会使用其他帐户。 例如，如果用户运行使用并行处理的评分任务，则会针对所有线程重复使用相同的工作人员帐户。 如果要大量使用机器学习，可以增加用于运行外部脚本的帐户数量。 有关详细信息，请参阅[在 SQL Server 机器学习服务中扩展外部脚本的并发执行](../../machine-learning/administration/scale-concurrent-execution-external-scripts.md)。

### <a name="permissions-granted-to-sqlrusergroup"></a>向 SQLRUserGroup 授予的权限

默认情况下，SQLRUserGroup 的成员对 SQL Server Binn、R_SERVICES 和 PYTHON_SERVICES 目录中的文件具有读取和执行权限   。 这包括对使用 SQL Server 安装的 R 和 Python 发行版中的可执行文件、库和内置数据集的访问权限。 

若要保护 SQL Server 上的敏感资源，可以选择定义拒绝访问 SQLRUserGroup 的访问控制列表 (ACL)  。 相反，除了 SQL Server 本身之外，还可以授予访问主计算机上本地数据资源的权限。 

根据设计，SQLRUserGroup 不具有数据库登录名或任何数据权限  。 在某些情况下，你可能想要创建允许环回连接的登录名（特别是受信任的 Windows 标识是调用的用户时）。 此功能称为[默示身份验证](#implied-authentication)  。 有关详细信息，请参阅[将 SQLRUserGroup 添加为数据库用户](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)。

## <a name="identity-mapping"></a>标识映射

会话启动时，Launchpad 将调用用户的标识映射到工作人员帐户。 这种从外部 Windows 用户或有效 SQL 登录名到工作人员帐户的映射仅在执行外部脚本的 SQL 存储过程的生存期内有效。 来自同一登录名的并行查询将映射到同一用户辅助角色帐户。

在执行期间，Launchpad 创建了用于存储会话数据的临时文件夹，并在会话结束时将其删除。 目录访问受限。 对于 R，RLauncher 会执行此任务。 对于 Python，PythonLauncher 会执行此任务。 每个工作人员帐户只能访问自己的文件夹，不能访问高于其级别的文件夹中的文件。 不过，工作人员帐户可以读取、写入或删除已创建的会话工作文件夹下的子文件夹。 如果你是计算机的管理员，可以查看针对每个进程创建的目录。 每个目录按其会话 GUID 标识。

::: moniker-end

::: moniker range=">=sql-server-ver15||=sqlallproducts-allversions"

## <a name="appcontainer-isolation"></a>AppContainer 隔离

通过 [AppContainer](/windows/desktop/secauthz/appcontainer-isolation) 实现隔离。 在运行时，如果在存储过程或查询中检测到外部脚本，SQL Server 会调用 Launchpad，并请求提供特定于扩展的启动程序。 Launchpad 在进程中以标识调用相应的运行时环境，并实例化 AppContainer 以将其包含在内。 此更改十分有用，因为不再需要本地帐户和密码管理。 此外，在禁止本地用户帐户的安装上，不再依赖本地用户帐户意味着现在就可使用此功能。

因为由 SQL Server 实现，所以 AppContainer 是一种内部机制。 尽管你不会在进程监视器中看到 AppContainer 的物理证据，但是可以在出站防火墙规则中找到它们，这些规则是由安装程序创建的，用于防止进程进行网络调用。 有关详细信息，请参阅 [SQL Server 机器学习服务的防火墙配置](../../machine-learning/security/firewall-configuration.md)。

## <a name="identity-mapping"></a>标识映射

会话启动时，Launchpad 将调用用户的标识映射到 AppContainer  。

> [!Note]
> 在 SQL Server 2019 及更高版本中，SQLRUserGroup 现在只有一个成员，即一个 SQL Server Launchpad 服务帐户，而不是多个工作人员帐户  。

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=sqlallproducts-allversions"

## <a name="identity-mapping"></a>标识映射

Launchpadd（两个“D”- [mssql-Launchpadd](extensibility-framework.md#launchpad)）守护程序将调用用户的标识映射到带有“Launchpad GUID”文件夹和附属证书的单独的 Launchpad（一个“D”）进程   。 这些 Launchpad GUID 文件夹在 `/var/opt/mssql-extensibility/data/` 下创建。 Launchpad 进程使用此证书对 SQL 进行身份验证，然后在 Launchpad GUID 文件夹下为每个会话 GUID 创建临时文件夹。 附属（R、Python 或 ExtHost）进程可以访问 Launchpad GUID 文件夹，此文件夹下的证书及其会话 GUID 文件夹。

下面的 SQL 脚本输出 Launchpad 文件夹的内容。

```sql
EXECUTE sp_execute_external_script @language = N'R'
    ,@script = N'
print("Contents of /var/opt/mssql-extensibility/data :");
print(system("ls -al /var/opt/mssql-extensibility/data"));
print("Contents of Launchpad GUID folder:");
print(system("ls -al /var/opt/mssql-extensibility/data/*"));
print(system("ls -al /var/opt/mssql-extensibility/data/*/*"))
'
    ,@input_data_1 = N'SELECT 1 AS hello'
```

::: moniker-end

<a name="implied-authentication"></a>

::: moniker range="=sql-server-2016||=sql-server-2017||=sqlallproducts-allversions"

## <a name="implied-authentication-loopback-requests"></a>默示身份验证（环回请求）

默示身份验证介绍了连接请求行为，在该行为下，环回请求数据或操作时，以低权限工作人员帐户运行的外部进程会作为受信任的用户标识呈现给 SQL Server  。 默示身份验证概念是 Windows 身份验证特有的，在指定信任连接的 SQL Server 连接字符串中，用于处理来自外部进程（如 R 或 Python 脚本）的请求。 有时，它也称为环回  。

信任连接可从外部脚本使用，但仅适用于其他配置。 在扩展性体系结构中，外部进程在工作人员帐户下运行，从父级 SQLRUserGroup 继承权限  。 连接字符串指定 `Trusted_Connection=True` 时，连接请求中会显示工作人员帐户的标识，此请求对 SQL Server 是默认未知的。

若要成功进行信任连接，必须为 SQLRUserGroup 创建数据库登录名  。 完成此操作后，任何来自 SQLRUserGroup 的成员的信任连接都有权登录 SQL Server  。 有关分步说明，请参阅[将 SQLRUserGroup 添加到数据库登录名](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)。

信任连接不是最广泛使用的连接请求形式。 外部脚本指定连接时，通常使用 SQL 登录名或完全指定的用户名和密码（如果连接到 ODBC 数据源）。

### <a name="how-implied-authentication-works-for-external-script-sessions"></a>默示身份验证如何处理外部脚本会话

下图显示了 SQL Server 组件与语言运行时的交互，以及如何在 Windows 中进行默示身份验证。

![Windows 中的默示身份验证](../security/media/implied-auth-windows-2017.png)

::: moniker-end

::: moniker range=">=sql-server-ver15||=sqlallproducts-allversions"

## <a name="implied-authentication-loopback-requests"></a>默示身份验证（环回请求）

默示身份验证介绍了连接请求行为，在该行为下，环回请求数据或操作时，在 AppContainers 下运行的的外部进程会作为受信任的用户标识呈现给 SQL Server  。 默示身份验证概念不再是 Windows 身份验证独有的，在指定信任连接的 SQL Server 连接字符串中，用于处理来自外部进程（如 R 或 Python 脚本）的请求。 有时，它也称为环回  。

通过管理标识和凭据，AppContainer 可防止使用用户凭据来获取对资源的访问权限或登录到其他环境。 AppContainer 环境创建使用用户和应用程序的组合标识的标识符，因此凭据对每个用户/应用程序配对都是唯一的，并且应用程序无法模拟用户。 有关详细信息，请参阅 [AppContainer 隔离](/windows/win32/secauthz/appcontainer-isolation)。

有关环回连接的更多详细信息，请参阅[从 Python 或 R 脚本到 SQL Server 的环回连接](../connect/loopback-connection.md)。

### <a name="how-implied-authentication-works-for-external-script-sessions"></a>默示身份验证如何处理外部脚本会话

下图显示了 SQL Server 组件与语言运行时的交互，以及如何在 Windows 中进行默示身份验证。

![Windows 中的默示身份验证](../security/media/implied-auth-windows.png)

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=sqlallproducts-allversions"

## <a name="implied-authentication-loopback-requests"></a>默示身份验证（环回请求）

默示身份验证介绍了连接请求行为，在该行为下，环回请求数据或操作时，以自己的命名空间中的低特权 mssql_satellite 用户身份运行的外部进程会作为受信任的用户标识呈现给 SQL Server  。 有时，它也称为环回。

环回连接通过以下方式实现：使用 Launchpad GUID 文件夹中的附属证书，通过附属进程对 SQL Server 进行身份验证。 调用用户的标识映射到此证书，因此可以将使用证书连接回 SQL Server 的附属进程映射回调用用户。

有关详细信息，请参阅[从 Python 或 R 脚本到 SQL Server 的环回连接](../connect/loopback-connection.md)。

### <a name="how-implied-authentication-works-for-external-script-sessions"></a>默示身份验证如何处理外部脚本会话

下图显示了 SQL Server 组件与语言运行时的交互，以及如何在 Linux 中进行默示身份验证。

![Linux 中的默示身份验证](../security/media/implied-auth-linux.png)

::: moniker-end

## <a name="no-support-for-transparent-data-encryption-at-rest"></a>不支持静态透明数据加密

发送到或接收自外部脚本运行时的数据不支持[透明数据加密 (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)。 因为外部进程在 SQL Server 进程外部运行。 因此，外部运行时使用的数据不受数据库引擎的加密功能保护。 此行为与 SQL Server 计算机（从数据库读取数据并复制）上运行的任何其他客户端相同。

因此，TDE 将不会应用于外部脚本中使用的任何数据、也不会应用于保存到磁盘中的任何数据或任何持久化中间结果  。 但是，其他类型的加密（如在文件级别或文件夹级别应用的 Windows BitLocker 加密或第三方加密）仍适用。

对于 [Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)，外部运行时无法访问加密密钥。 因此，无法将数据发送到脚本。

## <a name="next-steps"></a>后续步骤

本文介绍了构建在[扩展性框架](../../machine-learning/concepts/extensibility-framework.md)中的安全体系结构的组件和交互模型。 本文所述的要点包括 Launchpad、SQLRUserGroup 和工作人员帐户的用途、外部脚本的进程隔离，以及如何将用户标识映射到工作人员帐户。 

下一步，请查看[授予权限](../../machine-learning/security/user-permission.md)的说明。 对于使用 Windows 身份验证的服务器，还应查看[将 SQLRUserGroup 添加到数据库登录名](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)，了解何时需要其他配置。