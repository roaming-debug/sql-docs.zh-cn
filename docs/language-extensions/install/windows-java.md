---
title: 在 Windows 上安装 Java 语言扩展
titleSuffix: SQL Server Language Extensions
description: 了解如何在 Windows 上安装 SQL Server Java 语言扩展功能。
author: dphansen
ms.author: davidph
ms.date: 11/11/2020
ms.topic: how-to
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 94b54ce983679f790a560dc1971a36f9fb3c8551
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471788"
---
# <a name="install-sql-server-java-language-extension-on-windows"></a>在 Windows 上安装 SQL Server Java 语言扩展

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

了解如何为 Windows 上的 SQL Server 安装 [Java 语言扩展](../java-overview.md)组件。 Java 语言扩展是 [SQL Server 语言扩展](../language-extensions-overview.md)的一部分。

> [!NOTE]
> 本文适用于在 Windows 上安装 SQL Server Java 语言扩展。 对于 Linux，请参阅[在 Linux 上安装 SQL Server Java 语言扩展](../../linux/sql-server-linux-setup-language-extensions-java.md)。

<a name="prerequisites"></a>

## <a name="pre-install-checklist"></a>安装前清单

+ 如果要安装针对 Java 语言扩展的支持，则需要 SQL Server 2019 安装程序。

+ 需要数据库引擎实例。 无法只安装 Java 语言扩展功能，但可以将其以增量方式添加到现有实例。

+ 为实现业务连续性，语言扩展支持 [Always On 可用性组](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。 必须在每个节点上安装语言扩展并配置包。

+ SQL Server 2019 中的故障转移群集上支持安装 Java 语言扩展。

+ 请勿在域控制器上安装 SQL Server 语言扩展或 Java 语言扩展。 安装程序的语言扩展部分将失败。

+ 默认情况下，语言扩展和[机器学习服务](../../machine-learning/index.yml)安装在 SQL Server 大数据群集上。 如果使用大数据群集，则无需按照本文中的步骤进行操作。 有关详细信息，请参阅[在大数据群集上使用机器学习服务（Python 和 R）](../../big-data-cluster/machine-learning-services.md)。

> [!IMPORTANT]
> 安装完成后，请务必完成本文中所述的配置后步骤。 这些步骤包括使 SQL Server 能够使用外部代码，以及添加 SQL Server 代表你运行 Java 代码所需的帐户。 配置更改通常需要重启实例或重启 Launchpad 服务。

<a name="java-jre-jdk"></a>

## <a name="java-jre-or-jdk"></a>Java JRE 或 JDK

可以通过两种方式使用 SQL Server 来安装和使用 Java：

1. 使用默认的 Java 运行时，Zulu Open JRE 版本 11.0.3。 SQL Server 安装支持并包含此运行时。

1. 使用首选 Java 分发，而不是默认的 Java 运行时。

    Java 11 目前是 Windows 上受支持的版本。 Java Runtime Environment (JRE) 是最低要求，但如果需要 Java 编译器和开发包，Java 开发工具包 (JDK) 也很有用。 因为 JDK 包含所有这些内容，所以如果安装 JDK，则不需要 JRE。 在 Windows 上，我们建议尽可能将 JDK 安装在默认 `/Program Files/` 文件夹下。 否则，需要其他配置才能向可执行文件授予权限。 有关详细信息，请参阅本文档的[授予权限 (Windows)](#perms-nonwindows) 部分。

    > [!NOTE]
    > 由于 Java 可向后兼容，早期版本可能可以正常运行，但受支持和测试的 SQL Server 2019 版本是 Java 11。

## <a name="get-the-installation-media"></a>获取安装介质

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

## <a name="run-setup"></a>运行安装程序

对于本地安装，必须以管理员身份运行安装程序。 如果从远程共享安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则必须使用对远程共享具有读取和执行权限的域帐户。

1. 启动 SQL Server 2019 的安装向导。
  
1. 在“安装”选项卡上，选择“全新 SQL Server 独立安装或向现有安装添加功能”   。

    ![SQL Server 2019 安装](../media/sql-install.png) 

1. 在“功能选择”  页上，选择以下选项：
  
    - **数据库引擎服务**
  
        要将语言扩展与 SQL Server 结合使用，必须安装数据库引擎的实例。 可使用默认实例或命名实例。
  
    - **机器学习服务和语言扩展**
  
        此选项将安装支持 Java 代码执行的语言扩展组件。

        - 如果要安装默认的 Java 运行时 Zulu Open JRE 11.0.3，请选择“机器学习服务和语言扩展”和“Java”   。

        - 如果要使用自己的 Java 运行时，请选择“机器学习服务和语言扩展”  。 请勿选择“Java”。

        如果要使用 R 和 Python，请参阅[在 Windows 上安装 SQL Server 机器学习服务](../../machine-learning/install/sql-machine-learning-services-windows-install.md)。

    ![语言扩展的功能选项](../media/sql-install-feature-selection.png)

1. 如果在上一步中选择“Java”来安装默认的 Java 运行时，则会出现“Java 安装位置”页面   。

    选择“安装此安装随附的 Open JRE 11.0.3”  。

    ![选择 Java 安装位置](../media/sql-install-openjdk.png)

    > [!NOTE]
    > “提供已在此计算机上安装的其他版本的位置”不适用于语言扩展  。

1. 在“准备安装”页面上，验证是否已包括这些选择，然后选择“安装”   。
  
    + 数据库引擎服务
    + 机器学习服务和语言扩展

    请注意配置文件存储路径 `..\Setup Bootstrap\Log` 下的文件夹位置。 安装完成后，可以在“摘要”文件中查看已安装的组件。

6. 安装完成后，如果收到重启计算机的指示，请立即重启。 安装完成后，请务必阅读来自安装向导的消息。 有关详细信息，请参阅 [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。

## <a name="add-the-jre_home-variable"></a>添加 JRE_HOME 变量

`JRE_HOME` 是指定 Java 解释器位置的系统环境变量。 在此步骤中，在 Windows 上为该变量创建一个系统环境变量。

1. 查找并复制 JRE 起始路径。

    例如，默认的 Java 运行时 Zulu JRE 11.0.3 的 JRE 起始路径为 `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Binn\AZUL-OpenJDK-JRE\`。

    根据 SQL Server 安装路径，或者如果选择其他 Java 运行时，JDK 或 JRE 的位置可能不同于上述示例路径。 即使安装了 JDK，通常还会在安装过程中获得 JRE 子文件夹，因此，请在此情况下指向 JRE 文件夹。 Java 扩展将尝试从路径 `%JRE_HOME%\bin\server` 加载 `jvm.dll`。

1. 在“控制面板”中，依次打开“系统和安全”、“系统”，然后选择“高级系统属性”。

1. 选择“环境变量”。

1. 使用 JDK/JRE 路径的值（见步骤 1）为 `JRE_HOME` 创建新的系统变量。

1. 重启 [Launchpad](../concepts/extensibility-framework.md#launchpad)。

    1. 打开[“SQL Server 配置管理器”](../../relational-databases/sql-server-configuration-manager.md)。

    1. 在“SQL Server 服务”下，右键单击“SQL Server Launchpad”，然后选择“重启”  。

<a name="perms-nonwindows"></a>

## <a name="grant-access-to-non-default-jre-folder"></a>授予非默认 JRE 文件夹的访问权限

如果未安装 SQL Server 附带的默认 Zulu Open JRE，并且未在程序文件下安装 JDK 或 JRE，则需要执行以下步骤。 从提升的行运行 icacls 命令，以向 SQLRUsergroup 和 SQL Server 服务帐户（位于 ALL_APPLICATION_PACKAGES）授予访问 JRE 的权限     。 这些命令将以递归方式授予给定目录路径下的所有文件和文件夹的访问权限。

1. 授予 SQLRUserGroup 权限

    对于命名实例，请将实例名称追加至 SQLRUsergroup（例如 `SQLRUsergroupINSTANCENAME`）。

    ```cmd
    icacls "<PATH to JRE>" /grant "SQLRUsergroup":(OI)(CI)RX /T
    ```
    
    如果已在 Windows 的程序文件下的默认文件夹中安装了 JDK/JRE，则可以跳过此步骤。

1. 授予 AppContainer 权限

    ```cmd
    icacls “<PATH to JRE>” /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    > [!NOTE]
    > 上述命令向计算机 SID S-1-15-2-1 授予权限，这等同于英语版本的 Windows 上的所有应用程序包。 此外，也可以在英语版本的 Windows 上使用 `icacls "<PATH to JRE>" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T`。

## <a name="restart-the-service"></a>重新启动服务。

安装完成后，请先重启数据库引擎，然后再继续执行下一步，以启用脚本执行。

重启 SQL Server 服务也会自动重启相关的 SQL Server Launchpad 服务。

可以使用右键单击 SSMS 中实例的“重启”命令、使用“控制面板”中的“服务”面板，或者使用 [SQL Server 配置管理器](../../relational-databases/sql-server-configuration-manager.md)来重启服务 。

## <a name="enable-script-execution"></a>启用脚本执行

1. 打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。 

1. 连接到已安装语言扩展的实例，单击“新建查询”以打开查询窗口，并运行以下命令：

    ```sql
    sp_configure
    ```

    属性 `external scripts enabled` 的值目前应为 **0**。 该功能默认处于关闭状态，必须在管理员显式启用后才能运行 Java 代码。

1. 若要启用外部脚本编写功能，请运行以下语句：

    ```sql
    EXEC sp_configure 'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```

    如果已为机器学习服务启用此功能，请勿再次为语言扩展重新配置。 基础扩展性平台支持这两种方法。

<a name="register_external_language"></a>

## <a name="register-external-language"></a>注册外部语言

对于想要在其中使用语言扩展的每个数据库，需要使用 [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) 注册外部语言。

以下示例将一种名为 Java 的外部语言添加到 Windows 上的 SQL Server 的数据库中。

```SQL
CREATE EXTERNAL LANGUAGE Java
FROM (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

有关详细信息，请参阅 [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md)。

## <a name="verify-installation"></a>验证安装

在安装程序日志中检查实例的安装状态。

使用以下步骤验证用于启动外部脚本的所有组件是否都在运行。

1. 在 SQL Server Management Studio 或 Azure Data Studio 中打开新的查询窗口，然后运行以下语句：

    ```sql
    EXEC sp_configure 'external scripts enabled'
    ```

    run_value 现已设置为 1。

1. 打开“服务”面板或 SQL Server 配置管理器，并验证“SQL Server Launchpad”是否正在运行 。 应有一项服务适用于已安装语言扩展的每个数据库引擎实例。 有关该服务的详细信息，请参阅[扩展性框架](../concepts/extensibility-framework.md)。

## <a name="additional-configuration"></a>其他配置

如果验证步骤成功，则可以从 SQL Server Management Studio、Azure Data Studio、Visual Studio Code 或者能够向服务器发送 T-SQL 语句的其他任何客户端运行 Java 命令。

如果在运行命令时遇到错误，请查看本部分中的其他配置步骤。 服务或数据库可能需要进行其他适当的配置。

在实例级别，其他配置可能包括：

+ [SQL Server 机器学习服务的防火墙配置](../../machine-learning/security/firewall-configuration.md)
+ [启用其他网络协议](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
+ [启用远程连接](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)
+ [为 SQLRUserGroup 创建登录名](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)

<a name="bkmk_configureAccounts"></a>
<a name="permissions-external-script"></a>

数据库上可能需要以下配置更新：

+ [向用户授予 SQL Server 机器学习服务的权限](../../machine-learning/security/user-permission.md)
+ [向用户授予执行特定语言的权限](../../t-sql/statements/create-external-language-transact-sql.md#permissions)

> [!NOTE]
> 是否需要其他配置取决于安全架构、SQL Server 的安装位置，以及期望用户以何方式连接到数据库和运行外部脚本。

## <a name="suggested-optimizations"></a>建议的优化

一切正常运行后，可能还需要优化服务器以支持 Java 语言扩展。

### <a name="optimize-the-server-for-java-language-extension"></a>针对 Java 语言扩展优化服务器

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的默认设置旨在为数据库引擎支持的各种服务（可能包括提取、转换和加载 (ETL) 进程、报告、审核和使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据的应用程序）优化服务器平衡。 因此，在默认设置下，可能会发现语言扩展（尤其是占用大量内存的操作）的资源有时被限制或被阻止。

若要确保语言扩展作业的优先级别并为其分配相应资源，建议使用 SQL Server Resource Governor 配置外部资源池。 可能还要更改分配到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库引擎的内存量，或增加 [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] 服务下运行的帐户数。

+ 要配置用于管理外部资源的资源池，请参阅[创建外部资源池](../../t-sql/statements/create-external-resource-pool-transact-sql.md)。
  
+ 若要更改为数据库保留的内存量，请参阅[服务器内存配置选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md)。
  
如果使用的是 Standard Edition 且没有 Resource Governor，则可以使用动态管理视图 (DMV) 和扩展事件以及 Windows 事件监视来帮助管理服务器资源。

## <a name="next-steps"></a>后续步骤

Java 开发人员可以开始体验一些简单的示例，并了解 Java 如何与 SQL Server 配合工作的基础知识。 有关下一步，请参阅以下链接：

+ [教程：正则表达式与 Java](../tutorials/search-for-string-using-regular-expressions-in-java.md)
