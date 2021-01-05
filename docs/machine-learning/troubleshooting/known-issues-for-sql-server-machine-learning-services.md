---
title: Python 和 R 的已知问题
description: 本文描述了 SQL Server 机器学习服务和 SQL Server 2016 R Services 中所提供的 Python 和 R 组件的已知问题或限制。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/13/2020
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: contperf-fy20q4
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 4c4b293b13ec59421ea5827572203ba13e526212
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878768"
---
# <a name="known-issues-in-sql-server-machine-learning-services"></a>SQL Server 机器学习服务中的已知问题
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文描述了 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)和 [SQL Server 2016 R Services](../r/sql-server-r-services.md) 中所提供的 Python 和 R 组件的已知问题或限制。

## <a name="setup-and-configuration-issues"></a>安装和配置问题

若要查看初始安装和配置的相关过程的说明，请参阅[安装 SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md)。 其中包含有关升级、并行安装和安装新 R 或新 Python 组件的信息。

### <a name="1-inconsistent-results-in-mkl-computations-due-to-missing-environment-variable"></a>1.MKL 计算中结果不一致的原因在于缺少环境变量

**适用于：** R_SERVER 二进制文件 9.0、9.1、9.2 或 9.3。

R_SERVER 使用 Intel 数学内核库 (MKL)。 对于涉及 MKL 的计算，如果系统缺少环境变量，则结果可能会不一致。 

设置环境变量 `'MKL_CBWR'=AUTO` 以确保 R_SERVER 中的条件数值可再现性。 有关详细信息，请参阅[条件数值可再现性 (CNR) 简介](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr)。

**解决方法**

1. 在“控制面板”中，单击“系统和安全” > “系统” > “高级系统设置” > “环境变量”     。

2. 创建新的用户或系统变量。 

   + 将变量名称设置为“MKL_CBWR”。
   + 将“变量值”设置为“AUTO”。

3. 重启 R_SERVER。 在 SQL Server 上，可以重启 SQL Server Launchpad 服务。

> [!NOTE]
> 如果要在 Linux 上运行 SQL Server 2019，请在用户主目录中编辑或创建 *.bash_profile*，添加 `export MKL_CBWR="AUTO"` 行。 通过在 bash 命令提示符处键入 `source .bash_profile` 来执行此文件。 通过在 R 命令提示符处键入 `Sys.getenv()` 来重启 R_SERVER。

### <a name="2-r-script-runtime-error-sql-server-2017-cu5-cu7-regression"></a>2.R 脚本运行时错误（SQL Server 2017 CU5-CU7 回归）

对于 SQL Server 2017，在累积更新 5 到 7 中，**rlauncher.config** 文件（其中临时目录文件路径包含空格）中存在回归。 此回归在 CU8 中已更正。

运行 R 脚本时显示的错误包括以下消息：

> 无法与“R”脚本的运行时通信。  请检查“R”运行时的要求。
>
> 来自外部脚本的 STDERR 消息： 
>
> 错误：无法创建“R_TempDir” 

**解决方法**

如果可以使用 CU8，则应用 CU8。 或者，通过在提升的命令提示符上使用卸载/安装来运行 **registerrext**，可以重新创建 **rlauncher.config**。 

```cmd
<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe /uninstall /sqlbinnpath:<SQLInstanceBinnPath> /userpoolsize:0 /instance:<SQLInstanceName>

<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe /install /sqlbinnpath:<SQLInstanceBinnPath> /userpoolsize:0 /instance:<SQLInstanceName>
```

以下示例显示了安装到“C:\Program Files\Microsoft SQL Server\"的默认实例“MSSQL14.MSSQLSERVER”的命令：

```cmd
"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRext.exe" /uninstall /sqlbinnpath:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Binn" /userpoolsize:0 /instance:MSSQLSERVER

"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRext.exe" /install /sqlbinnpath:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Binn" /userpoolsize:0 /instance:MSSQLSERVER
```

### <a name="3-unable-to-install-sql-server-machine-learning-features-on-a-domain-controller"></a>3.无法在域控制器上安装 SQL Server 机器学习功能

如果尝试在域控制器上安装 SQL Server 2016 R Services 或 SQL Server 机器学习服务，则安装会失败，并出现以下错误：

> 在此功能的安装过程中出错 
>
> 找不到具有标识的组 
>
> 组件错误代码：  0x80131509

失败的原因是，在域控制器上，该服务无法创建运行机器学习所需的 20 个本地帐户。 通常，不建议在域控制器上安装 SQL Server。 有关详细信息，请参阅[支持公告 2032911](https://support.microsoft.com/help/2032911/you-may-encounter-problems-when-installing-sql-server-on-a-domain-cont)。

### <a name="4-install-the-latest-service-release-to-ensure-compatibility-with-microsoft-r-client"></a>4.安装最新的 Service Release，确保与 Microsoft R Client 兼容

如果安装最新版本的 Microsoft R Client，并使用它在远程计算上下文中的 SQL Server 上运行 R，则可能收到类似以下的错误：

> 计算机上运行的 Microsoft R Client 版本为 9.x.x，与 8.x.x 版的 Microsoft R Server 不兼容。  请下载并安装兼容版本。

SQL Server 2016 要求客户端上的 R 库与服务器上的 R 库完全匹配。 已删除 R Server 9.0.1 之后版本的限制。 但是，如果遇到此错误，请验证客户端和服务器使用的 R 库的版本，并根据需要更新客户端以匹配服务器版本。

每当安装 SQL Server Service Release 时，随 SQL Server R Services 一起安装的 R 版本就会更新。 若要确保始终拥有最新版本的 R 组件，请务必安装所有服务包。

若要确保与 Microsoft R Client 9.0.0 兼容，请安装本[支持文章](https://support.microsoft.com/kb/3210262)中介绍的更新。

若要避免 R 包出现问题，还可以升级服务器上安装的 R 库版本，方法是将服务协议改为使用现代生命周期支持策略，如[下一节](#bkmk_sqlbindr)中所述。 执行此操作时，随 SQL Server 一起安装的 R 版本将按照用于更新机器学习服务器（以前称为 Microsoft R Server）的同一计划进行更新。

**适用于：** R Server 9.0.0 版本或更早版本的 SQL Server 2016 R Services

### <a name="5-r-components-missing-from-cu3-setup"></a>5.CU3 安装中缺少的 R 组件

预配有限数量的 Azure 虚拟机，而不使用应包含在 SQL Server 中的 R 安装文件。 此问题适用于从 2018-01-05 到 2018-01-23 期间预配的虚拟机。 如果在 2018-01-05 到 2018-01-23 期间对 SQL Server 2017 应用了 CU3 更新，此问题可能也会影响本地安装。

已提供一个 Service Release，其中包含 R 安装文件的正确版本。

+ [SQL Server 2017 KB4052987 的累积更新包 3](https://www.microsoft.com/download/details.aspx?id=56128)。

若要安装组件并修复 SQL Server 2017 CU3，则必须卸载 CU3，重新安装更新后的版本：

1. 下载更新后的 CU3 安装文件，其中包含 R 安装程序。
2. 卸载 CU3。 在“控制面板”中，搜索“卸载更新”，然后选择“SQL Server 2017 (KB4052987)（64 位）的修补程序 3015”  。 继续执行卸载步骤。
3. 双击刚才下载的 KB4052987 更新，重新安装 CU3 更新：`SQLServer2017-KB4052987-x64.exe`。 按照安装说明进行操作。

### <a name="6-unable-to-install-python-components-in-offline-installations-of-sql-server-2017-ctp-20-or-later"></a>6.无法在 SQL Server 2017 CTP 2.0 或更高版本的脱机安装中安装 Python 组件

如果在没有 Internet 访问权限的计算机上安装 SQL Server 2017 的预发行版本，则安装程序可能无法显示提示下载的 Python 组件位置的页面。 在这种情况下，可以安装机器学习服务功能，但不能安装 Python 组件。

此问题在发布版本中得以修复。 此外，此限制不适用于 R 组件。

**适用于：** 具有 Python 的 SQL Server 2017

### <a name="warning-of-incompatible-version-when-you-connect-to-an-older-version-of-sql-server-r-services-from-a-client-by-using-sssqlv14_md"></a><a name="bkmk_sqlbindr"></a> 使用 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 从客户端连接到旧版 SQL Server R Services 时，出现版本不兼容的警告

在 SQL Server 2016 计算上下文中运行 R 代码时，可能会看到以下错误：

> 计算机上运行的 Microsoft R Client 版本为 9.0.0，与 8.0.3 版的 Microsoft R Server 不兼容。  请下载并安装兼容版本。

如果以下两个语句之一为 true，则会显示此消息，

+ 使用 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 的安装向导在客户端计算机上安装 R Server（独立版）。
+ 使用[单独的 Windows 安装程序](/machine-learning-server/install/r-server-install-windows)安装了 Microsoft R Server。

若要确保服务器和客户端使用的版本相同，你可能需要使用 Microsoft R Server 9.0 和更高版本支持的绑定来升级 SQL Server 2016 实例中的 R 组件  。 若要确定 R Services 版本是否支持升级，请参阅[升级使用 SqlBindR.exe 的 R Services 实例](../install/upgrade-r-and-python.md)。

**适用于：** R Server 9.0.0 版本或更早版本的 SQL Server 2016 R Services

### <a name="7-setup-for-sql-server-2016-service-releases-might-fail-to-install-newer-versions-of-r-components"></a>7.安装了 SQL Server 2016 Service Release 可能无法安装较新版本的 R 组件

在未连接到 Internet 的计算机上安装累积更新或安装 SQL Server 2016 的服务包时，安装向导可能无法显示使用下载的 CAB 文件更新 R 组件的提示。 同时安装多个组件和数据库引擎时，通常会发生此故障。

解决方法之一是使用命令行并指定 `MRCACHEDIRECTORY` 参数（如本示例所示）来安装 Service Release，本示例安装 CU1 更新：

`C:\<path to installation media>\SQLServer2016-KB3164674-x64.exe /Action=Patch /IACCEPTROPENLICENSETERMS /MRCACHEDIRECTORY=<path to CU1 CAB files>`

若要获取最新的安装程序，请参阅[在没有 Internet 连接的情况下安装机器学习组件](../install/sql-ml-component-install-without-internet-access.md)。

**适用于：** R Server 9.0.0 版本或更早版本的 SQL Server 2016 R Services

### <a name="8-launchpad-services-fails-to-start-if-the-version-is-different-from-the-r-version"></a>8.当版本与 R 版本不同时，无法启动 Launchpad 服务

如果从数据库引擎单独安装 SQL Server R Services，并且生成版本不同，则可能会在系统事件日志中看到以下错误：

> SQL Server Launchpad 服务启动失败是由于以下错误：  该服务未及时响应启动请求或控制请求。

例如，如果使用发行版本安装数据库引擎，应用修补程序升级数据库引擎，然后使用发行版本添加 R Services 功能，则可能出现此错误。

若要避免此问题，请使用实用工具（如文件管理器）比较 Launchpad.exe 版本与 SQL 二进制文件版本（如 sqldk.dll）。 所有组件都应该有相同的版本号。 如果升级一个组件，请务必向其他所有已安装的组件应用相同的升级。

在实例的 `Binn` 文件夹中查找 Launchpad。 例如，在默认安装的 SQL Server 2016 中路径可能是 `C:\Program Files\Microsoft SQL Server\MSSQL.13.InstanceNameMSSQL\Binn`。 

### <a name="9-remote-compute-contexts-are-blocked-by-a-firewall-in-sql-server-instances-that-are-running-on-azure-virtual-machines"></a>9.远程计算上下文遭到在 Azure 虚拟机上运行的 SQL Server 实例中防火墙的阻止

如果已经在 Azure 虚拟机上安装 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]，你可能无法使用需使用虚拟机工作区的计算上下文。 原因是在默认情况下，Azure 虚拟机上的防火墙包含一项规则，该规则阻止本地 R 用户帐户的网络访问。

解决方法之一是，在 Azure 虚拟机上打开“高级安全 Windows 防火墙”，选择“出站规则”，并禁用以下规则   ：阻止 SQL Server 实例 MSSQLSERVER 中 R 本地用户帐户进行网络访问  。 你还可以使规则保持启用状态，但将安全属性更改为“如果安全则允许”  。

### <a name="10-implied-authentication-in-sqlexpress"></a>10.SQLEXPRESS 中的隐式身份验证

使用集成 Windows 身份验证从远程数据科学工作站运行 R 作业时，SQL Server 会使用隐式身份验证生成脚本可能需要的任何本地 ODBC 调用  。 但是，此功能在 RTM 版 SQL Server Express Edition 中不起作用。

若要解决此问题，建议升级到更高的 Service Release。

如果升级不可行，解决方法之一是使用 SQL 登录名运行可能需要嵌入式 ODBC 调用的远程 R 作业。

**适用于：** SQL Server 2016 R Services Express Edition

### <a name="11-performance-limits-when-libraries-used-by-sql-server-are-called-from-other-tools"></a>11.从其他工具调用 SQL Server 使用的库时的性能限制

可能从外部应用程序（如 RGui）调用针对 SQL Server 安装的机器学习库。 这样做可能是完成某些任务（如安装新包，或在非常简短的代码示例上运行即席测试）的最简便方法。 但是，在 SQL Server 之外，性能可能会受到限制。

例如，即使使用 Enterprise Edition 版 SQL Server，但如果使用外部工具运行 R 代码，则 R 将以单线程模式运行。 若要在 SQL Server 中获得性能优势，请启动 SQL Server 连接，并使用 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 调用外部脚本运行时。

一般情况下，避免从外部工具调用 SQL Server 使用的机器学习库。 如果需要调试 R 或 Python 代码，通常在 SQL Server 外更容易进行调试。 若要获取 SQL Server 中的相同库，可以安装 Microsoft R Client 或 [SQL Server 2017 Machine Learning Server (Standalone)](../install/sql-machine-learning-standalone-windows-install.md)。

### <a name="12-sql-server-data-tools-does-not-support-permissions-required-by-external-scripts"></a>12.SQL Server Data Tools 不支持外部脚本所需的权限

使用 Visual Studio 或 SQL Server Data Tools 发布数据库项目时，如果任何主体具有特定于外部脚本执行的权限，则可能会出现类似以下错误：

> *TSQL 模型：对数据库实施反向工程时检测到错误。未识别且未导入权限。*

当前 DACPAC 模型不支持 R Services 或机器学习服务使用的权限，例如 GRANT ANY EXTERNAL SCRIPT 或 EXECUTE ANY EXTERNAL SCRIPT。 更高版本将会解决此问题。

解决方法之一是在后期部署脚本中运行其他 GRANT 语句。

### <a name="13-external-script-execution-is-throttled-due-to-resource-governance-default-values"></a>13.由于资源调控默认值中止外部脚本执行

在 Enterprise Edition 中，可以使用资源池管理外部脚本进程。 在某些早期发布版本中，可以分配给 R 进程的最大内存为 20%。 因此，如果服务器有 32 GB 的 RAM，则 R 可执行文件（RTerm.exe 和 BxlServer.exe）在单个请求中最多可使用 6.4 GB。

如果遇到资源限制，请检查当前的默认值。 如果 20% 不够，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的文档，了解如何更改此值。

**适用于：** SQL Server 2016 R Services Enterprise Edition

### <a name="14-error-when-using-sp_execute_external_script-without-libcso-on-linux"></a>14.在 Linux 上使用不具有 `libc++.so` 的 `sp_execute_external_script` 时出现的错误

在未安装 `libc++.so` 的全新 Linux 计算机上，使用 Java 或外部语言运行 `sp_execute_external_script` (SPEES) 查询会失败，因为 `commonlauncher.so` 未能加载 `libc++.so`。

例如：

```sql
EXECUTE sp_execute_external_script @language = N'Java'
    , @script = N'JavaTestPackage.PassThrough'
    , @parallel = 0
    , @input_data_1 = N'select 1'
WITH RESULT SETS((col1 INT NOT NULL))
GO
```

此操作失败，并出现与以下内容类似的消息：

```text
Msg 39012, Level 16, State 14, Line 0

Unable to communicate with the runtime for 'Java' script for request id: 94257840-1704-45E8-83D2-2F74AEB46CF7. Please check the requirements of 'Java' runtime.
```

`mssql-launchpadd` 日志将显示与以下内容类似的错误消息：

```text
Oct 18 14:03:21 sqlextmls launchpadd[57471]: [launchpad] 2019/10/18 14:03:21 WARNING: PopulateLauncher failed: Library /opt/mssql-extensibility/lib/commonlauncher.so not loaded. Error: libc++.so.1: cannot open shared object file: No such file or directory
```

**解决方法**

可以使用以下解决方法之一：

1. 将 `libc++*` 从 `/opt/mssql/lib` 复制到默认系统路径 `/lib64`

1. 将以下条目添加到 `/var/opt/mssql/mssql.conf` 以公开路径：

   ```text
   [extensibility]
   readabledirectories = /opt/mssql
   ```

**适用于：** Linux 上的 SQL Server 2019

### <a name="15-installation-or-upgrade-error-on-fips-enabled-servers"></a>15.启用 FIPS 的服务器上发生的安装或升级错误

如果使用“机器学习服务和语言扩展”功能安装 SQL Server 2019，或在启用了[美国联邦信息处理标准 (FIPS)](/windows/security/threat-protection/security-policy-settings/system-cryptography-use-fips-compliant-algorithms-for-encryption-hashing-and-signing) 的服务器上升级 SQL Server 实例，将收到以下错误：

> *安装扩展性功能时出错，收到错误消息：AppContainer 创建失败，收到错误消息 NONE，声明此实现不是经过 Windows 平台 FIPS 验证的加密算法的一部分。*

**解决方法**

在安装带有机器学习服务和语言扩展功能的 SQL Server 2019 或升级 SQL Server 实例之前禁用 FIPS。 在安装或升级完成后，可以重新启用 FIPS。

**适用于：** SQL Server 2019

## <a name="r-script-execution-issues"></a>R 脚本执行问题

本部分包含特定于在 SQL Server 上运行 R 的已知问题，以及由 Microsoft 发布的 R 库和工具（包括 RevoScaleR）相关的一些问题。

有关可能会影响 R 解决方案的其他已知问题，请参阅[机器学习服务器](/machine-learning-server/resources-known-issues)站点。

### <a name="1-access-denied-warning-when-executing-r-scripts-on-sql-server-in-a-non-default-location"></a>1.在非默认位置的 SQL Server 上执行 R 脚本时出现的拒绝访问警告

如果 SQL Server 的实例已安装到非默认位置（如 `Program Files` 文件夹外部），则尝试运行用安装包的脚本时，将引发警告 ACCESS_DENIED。 例如：

> 在 `normalizePath(path.expand(path), winslash, mustWork)` 中：path[2]="~ExternalLibraries/R/8/1"：  拒绝访问

原因是 R 函数尝试读取路径，如果内置用户组 SQLRUserGroup 没有读取访问权限，则读取失败  。 引发的警告不会阻止执行当前的 R 脚本，但每当用户运行任何其他 R 脚本时，该警告都可能重复出现。

如果已将 SQL Server 安装到默认位置，则不会发生此错误，因为所有 Windows 用户都有对 `Program Files` 文件夹的读取权限。

在即将发布的 Service Release 中解决了这一问题。 解决方法之一是向 SQLRUserGroup 组提供对 `ExternalLibraries` 的所有父文件夹的读取访问权限  。

### <a name="2-serialization-error-between-old-and-new-versions-of-revoscaler"></a>2.旧版和新版 RevoScaleR 之间的序列化错误

将使用序列化格式的模型传递到远程 SQL Server 实例时，可能会出现以下错误： 

> memDecompress(data, type = decompress) 出现错误，memDecompress(2) 出现内部错误 -3。 

如果使用最新版序列化函数 [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel) 保存了模型，但反序列化模型的 SQL Server 实例具有旧版 RevoScaleR API（来自 SQL Server 2017 CU2 或更早版本），则会引发此错误。

解决方法之一是将 SQL Server 2017 实例升级到 CU3 或更高版本。

如果 API 版本相同，或者要将使用较旧序列化函数保存的模型移到使用较新版序列化 API 的服务器，则不会出现此错误。

换言之，将相同版本的 RevoScaleR 同时用于序列化和反序列化操作。

### <a name="3-real-time-scoring-does-not-correctly-handle-the-_learningrate_-parameter-in-tree-and-forest-models"></a>3.实时评分无法正确处理树和林模型中的 learningRate 参数 

如果使用决策树方法或决策林方法创建模型，并指定学习速率，则与使用 `rxPredict` 相比，使用 `sp_rxpredict` 或 SQL `PREDICT` 函数时可能会出现不一致的结果。

原因是 API 中处理序列化模型的错误，并且仅限于 `learningRate` 参数：例如，在 [rxBTrees](/machine-learning-server/r-reference/revoscaler/rxbtrees) 中，或

在即将发布的 Service Release 中解决了这一问题。

### <a name="4-limitations-on-processor-affinity-for-r-jobs"></a>4.R 作业的处理器关联限制

在初始发布版 SQL Server 2016 中，可以仅对第一个 k 组中的 CPU 设置处理器关联。 例如，如果服务器是包含两个 k 组的双套接字计算机，则仅将第一个 k 组中的处理器用于 R 进程。 配置 R 脚本作业的资源调控时，应用相同的限制。

此问题已在 SQL Server 2016 Service Pack 1 中解决。 我们建议升级到最高的 Service Release。

**适用于：** SQL Server 2016 R Services RTM 版本

### <a name="5-changes-to-column-types-cannot-be-performed-when-reading-data-in-a-sql-server-compute-context"></a>5.在 SQL Server 计算环境中读取数据时，无法更改列类型

如果计算环境设置为 SQL Server 实例，则无法在 R 代码中使用 _colClasses_ 参数（或其他类似参数）来更改列的数据类型。

例如，如果列 CRSDepTimeStr 尚不是整数，则以下语句将导致错误：

```R
data <- RxSqlServerData(
  sqlQuery = "SELECT CRSDepTimeStr, ArrDelay  FROM AirlineDemoSmall", 
  connectionString = connectionString, 
  colClasses = c(CRSDepTimeStr = "integer"))
```

解决方法之一是将 SQL 查询重新编写为使用 CAST 或 CONVERT，并通过使用正确的数据类型将数据呈现给 R。 一般情况下，与在 R 代码中更改数据相比，使用 SQL 处理数据可以获得更好的性能。

**适用于：** SQL Server 2016 R Services

### <a name="6-limits-on-size-of-serialized-models"></a>6.序列化模型大小限制

若要将模型保存到 SQL Server 表，则必须序列化模型，并将其保存为二进制格式。 理论上，可使用此方法来存储的模型的最大大小为 2 GB，这是 SQL Server 中 varbinary 列的最大大小。

如果需要使用更大的模型，可使用以下解决方法：

+ 采取措施减小模型大小。 一些开放源代码 R 包在模型对象中包含大量信息，可删除其中的大部分信息以进行部署。 
+ 使用功能选择删除不必要的列。
+ 如果使用开放源代码算法，请考虑使用 MicrosoftML 或 RevoScaleR 中相应算法的类似实现。 这些包已针对部署方案进行了优化。
+ 模型经过合理化并且使用前面的步骤减小大小后，请查看在将模型传递到 SQL Server 之前，是否可以使用基础 R 中的 [memCompress](https://www.rdocumentation.org/packages/base/versions/3.4.1/topics/memCompress) 函数来减小模型大小。 当模型接近 2 GB 限制时，这样操作是最佳选择。
+ 对于更大的模型，可以使用 SQL Server [FileTable](../../relational-databases/blob/filetables-sql-server.md) 功能来存储模型，而不使用 varbinary 列。

    若要使用 FileTable，则必须添加防火墙例外，因为存储在 FileTable 中的数据是由 SQL Server 中的文件流文件系统驱动程序管理的，默认防火墙规则阻止网络文件访问。 有关详细信息，请参阅[启用 FileTable 的先决条件](../../relational-databases/blob/enable-the-prerequisites-for-filetable.md)。

    启用 FileTable 后，若要编写模型，可以使用 FileTable API 从 SQL 获取路径，然后从代码中将模型写入该位置。 如果需要读取模型，可以从 SQL 获取路径，然后使用脚本中的路径调用模型。 有关详细信息，请参阅[使用文件输入输出 API 访问 FileTable](../../relational-databases/blob/access-filetables-with-file-input-output-apis.md)。

### <a name="7-avoid-clearing-workspaces-when-you-execute-r-code-in-a-ssnoversion-compute-context"></a>7.在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算上下文中执行 R 代码时，请避免清除工作区

如果在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算上下文中运行 R 代码时使用 R 命令清除对象的工作区，或者如果在使用 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 调用 R 脚本的过程中清除工作区，则可能收到如下错误：找不到工作区对象 revoScriptConnection 

`revoScriptConnection` 是 R 工作区中的对象，它包含有关从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]调用的 R 会话的信息。 但是，如果 R 代码包含清除工作区的命令（例如 `rm(list=ls()))`，则将同时清除有关会话以及 R 工作区中其他对象的所有信息。

解决方法之一是在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中运行 R 时，避免随意清除变量和其他对象。 尽管在 R 控制台中工作时经常清除工作区，但却可能因此造成意外后果。

+ 若要删除特定的变量，请使用 R `remove` 函数：例如 `remove('name1', 'name2', ...)`
+ 若要删除多个变量，请将临时变量的名称保存到列表中，然后执行定期的垃圾回收。

### <a name="8-restrictions-on-data-that-can-be-provided-as-input-to-an-r-script"></a>8.可作为输入提供给 R 脚本的数据的限制

你无法在 R 脚本中使用以下类型的查询结果：

- 引用 AlwaysEncrypted 列的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询中的数据。
  
- 引用屏蔽列的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询中的数据。
  
     如果你需要在 R 脚本中使用屏蔽数据，一种可能的解决方法是在临时表中创建该数据的副本，然后改为使用该数据。

### <a name="9-use-of-strings-as-factors-can-lead-to-performance-degradation"></a>9.将字符串用作系数，可能会导致性能降低

使用字符串类型变量作为系数，可以很大程度上增加用于 R 操作的内存量。 这是在使用 R 时的一个普遍已知问题，有很多该主题的相关文章。 例如，请参阅 [Factors are not first-class citizens in R, by John Mount, in R-bloggers)](https://www.r-bloggers.com/factors-are-not-first-class-citizens-in-r/)（系数不是 R 中的一等公民，作者：John Mount，R-bloggers）或 [stringsAsFactors:An unauthorized biography, by Roger Peng](https://simplystatistics.org/2015/07/24/stringsasfactors-an-unauthorized-biography/)（stringsAsFactors：未经授权的传记，作者：Roger Peng）。 

尽管此问题并不特定于 SQL Server，但它可能会显著影响 SQl Server 中运行的 R 代码的性能。 字符串通常存储为 varchar 或 nvarchar，并且如果字符串数据的列有多个唯一值，则通过 R 将这些值内部转换为整数并返回到字符串的过程甚至可能导致内存分配错误。

如果不是绝对需要用于其他操作的字符串数据类型，则从性能和规模的角度来看，将字符串值映射到数字（整数）数据类型作为数据准备的一部分是有帮助的。

有关此问题的讨论以及其他提示，请参阅 [R Services 性能 - 数据优化](../r/r-and-data-optimization-r-services.md)。

### <a name="10-arguments-varstokeep-and-varstodrop-are-not-supported-for-sql-server-data-sources"></a>10.SQL Server 数据源不支持 varsToKeep 和 varsToDrop 参数  

使用 rxDataStep 函数将结果写入到表时，使用 varsToKeep 和 varsToDrop 是将要包含或排除的列指定为操作的一部分的简便方法   。 但是，SQL Server 数据源不支持这些参数。

### <a name="11-limited-support-for-sql-data-types-in-sp_execute_external_script"></a>11.sp\_execute\_external\_script 中 SQL 数据类型的有限支持

SQL 中支持的数据类型并非全都可在 R 中使用。解决方法之一是考虑在将数据传递给 sp\_execute\_external\_script 之前，将不受支持的数据类型强制转换为受支持的数据类型。

有关详细信息，请参阅 [R 库和数据类型](../r/r-libraries-and-data-types.md)。

### <a name="12-possible-string-corruption-using-unicode-strings-in-varchar-columns"></a>12.使用 varchar 列中的 Unicode 字符串可能导致的字符串损坏

将 varchar 列中的 Unicode 数据从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 传递到 R/Python 可能会导致字符串损坏。 这是因为在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则中这些 Unicode 字符串的编码可能与 R/Python 中使用的默认 UTF-8 编码不匹配。 

若要将任何非 ASCII 字符串数据从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发送到 R/Python，请使用 UTF-8 编码（在 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 中可用）或同样使用 nvarchar 类型。

### <a name="13-only-one-value-of-type-raw-can-be-returned-from-sp_execute_external_script"></a>13.只能从 `sp_execute_external_script` 中返回一个 `raw` 类型的值

从 R 返回二进制数据类型（R raw 数据类型）时，必须在输出数据框架中发送该值  。

对于 raw 以外的数据类型，可以通过添加 OUTPUT 关键字来返回参数值和存储过程的结果  。 有关详细信息，请参阅[参数](../../relational-databases/stored-procedures/parameters.md)。

如果要使用包含 raw 类型值的多个输出集，一种可能的解决方法是多次调用存储过程，或者使用 ODBC 将结果集发送回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  。

### <a name="14-loss-of-precision"></a>14.精度损失

因为 [!INCLUDE[tsql](../../includes/tsql-md.md)] 和 R 支持不同的数据类型，因此，在转换期间，数字数据类型的精度可能会降低。

有关隐式数据类型转换的详细信息，请参阅 [R 库和数据类型](../r/r-libraries-and-data-types.md)。

### <a name="15-variable-scoping-error-when-you-use-the-transformfunc-parameter"></a>15.使用 transformFunc 参数时出现的变量范围错误

若要在建模时转换数据，可以在函数（如 `rxLinmod` 或 `rxLogit`）中传递 transformFunc 参数  。 但是，嵌套的函数调用可能会导致 SQL Server 计算上下文中出现范围错误，即使调用能够在本地计算上下文中正常工作。

> 用于分析的示例数据集没有变量 

例如，假设你在本地全局环境中定义了 `f` 和 `g` 这两个函数，而且 `g` 调用 `f`。 在包含 `g` 的分布式或远程调用中，对 `g` 的调用可能会失败，并出现此错误，因为找不到 `f`，即使你已同时将 `f` 和 `g` 传递给远程调用。

如果遇到此问题，你可以通过在 `f` 的定义中（ `g`可正常调用 `g` 的任何位置）嵌入 `f`的定义来解决该问题。

例如：

```R
f <- function(x) { 2*x * 3 }
g <- function(y) {
              a <- 10 * y
               f(a)
}
```

若要避免此错误，请重写定义，如下所示：

```R
g <- function(y){
              f <- function(x) { 2*x +3}
              a <- 10 * y
              f(a)
}
```

### <a name="16-data-import-and-manipulation-using-revoscaler"></a>16.使用 RevoScaleR 导入和操作数据

从数据库读取 varchar 列时，会截掉空格  。 为了避免这种情况，请将字符串包含在非空格字符中。

使用函数（如 `rxDataStep`）创建包含 varchar 列的数据库表时，将会根据数据样本估算列宽  。 如果宽度可能会变化，则可能需要将所有字符串填补到公共长度。

使用 `rxImport` 或 `rxTextToXdf` 的重复调用来导入和追加行，并将多个输入文件合并为单个.xdf 文件时，不支持使用转换来更改变量的数据类型。

### <a name="17-limited-support-for-rxexec"></a>17.对 rxExec 的有限支持

在 SQL Server 2016 中，RevoScaleR 包提供的 `rxExec` 函数只能在单线程模式下使用。

### <a name="18-increase-the-maximum-parameter-size-to-support-rxgetvarinfo"></a>18.增加最大参数大小以支持 rxGetVarInfo

如果你使用变量数极多（例如，超过 40,000 个）的数据集，在启动 R 时设置 `max-ppsize` 标志才能使用 `rxGetVarInfo` 等函数。 `max-ppsize` 标志指定指针保护堆栈的最大大小。

如果你使用 R 控制台（例如，RGui.exe 或 RTerm.exe），可以通过键入以下命令，将 max-ppsize 的值设置为 500000  ：

```R
R --max-ppsize=500000
```

### <a name="19-issues-with-the-rxdtree-function"></a>19.rxDTree 函数的问题

`rxDTree` 函数目前不支持公式内转换。 具体而言，不支持使用 `F()` 语法即时创建系数。 但是，数字数据会自动装箱。

有序系数被视为与所有 RevoScaleR 分析函数（ `rxDTree`除外）中的系数相同。

### <a name="20-datatable-as-an-outputdataset-in-r"></a>20.在 R 中作为 OutputDataSet 的 data.table

SQL Server 2017 累积更新 13 (CU13) 和更早版本中不支持使用 `data.table` 作为 R 中的 `OutputDataSet`。 可能会出现以下消息：

``` text
Msg 39004, Level 16, State 20, Line 2
A 'R' script error occurred during execution of 
'sp_execute_external_script' with HRESULT 0x80004004.
Msg 39019, Level 16, State 2, Line 2
An external script error occurred: 
Error in alloc.col(newx) : 
  Internal error: length of names (0) is not length of dt (11)
Calls: data.frame ... as.data.frame -> as.data.frame.data.table -> copy -> alloc.col

Error in execution.  Check the output for more information.
Error in eval(expr, envir, enclos) : 
  Error in execution.  Check the output for more information.
Calls: source -> withVisible -> eval -> eval -> .Call
Execution halted
```

SQL Server 2017 累积更新 14 (CU14) 和更高版本中不支持 `data.table` 作为 R 中的 `OutputDataSet`。

### <a name="21-running-a-long-script-fails-while-installing-a-library"></a>21.安装库时运行长脚本失败

运行长期外部脚本会话，并让 dbo 并行尝试在不同的数据库上安装库可以终止脚本。

例如，对母版运行此外部脚本：

```sql
USE MASTER
DECLARE @language nvarchar(1) = N'R'
DECLARE @script nvarchar(max) = N'Sys.sleep(100)'
DECLARE @input_data_1 nvarchar(max) = N'select 1'
EXEC sp_execute_external_script @language = @language, @script = @script, @input_data_1 = @input_data_1 with result sets none
go
```

当 dbo 在 LibraryManagementFunctional 中并行安装库时：

```sql
USE [LibraryManagementFunctional]
go

CREATE EXTERNAL LIBRARY [RODBC] FROM (CONTENT = N'/home/ani/var/opt/mssql/data/RODBC_1.3-16.tar.gz') WITH (LANGUAGE = 'R')
go

DECLARE @language nvarchar(1) = N'R'
DECLARE @script nvarchar(14) = N'library(RODBC)'
DECLARE @input_data_1 nvarchar(8) = N'select 1'
EXEC sp_execute_external_script @language = @language, @script = @script, @input_data_1 = @input_data_1
go
```

之前针对母版的长期外部脚本将终止，并显示以下错误消息：

> 执行“sp_execute_external_script”时发生“R”脚本错误(HRESULT 0x800704d4)。 

**解决方法**

请勿与长期查询并行运行库安装。 或在完成安装后重新运行长期查询。

**适用于：** 仅限 Linux 和大数据群集上的 SQL Server 2019。

### <a name="22-sql-server-stops-responding-when-executing-r-scripts-containing-parallel-execution"></a>22.在执行包含并行执行的 R 脚本时，SQL Server 停止响应

SQL Server 2019 包含影响使用并行执行的 R 脚本的回归。 例如，将 `rxExec` 与 `RxLocalPar` 计算上下文以及使用并行包的脚本结合使用。 此问题是由并行包在 SQL Server 中执行，同时又在写入到空设备时遇到的错误引起的。

**适用于：** SQL Server 2019。

### <a name="23-precision-loss-for-moneynumericdecimalbigint-data-types"></a>23.money/numeric/decimal/bigint 数据类型的精度损失

使用 `sp_execute_external_script` 执行 R 脚本时输入的数据可为 money、numeric、decimal 和 bigint 数据类型。 但是，因为它们会转换为 R 的数值类型，所以对于非常高或具有小数点的值，会发生精度损失。

+ **Money**：有时，美分值会不精确，将发出警告：“警告：无法精确表示的美分值”。  
+ **numeric/decimal**：带有 R 脚本的 `sp_execute_external_script` 不提供对这些数据类型的完整值的支持，会更改十进制数的最后几位数，尤其是带有小数的数字。
+ **bigint**：R 最多支持 53 位整数，然后将开始发生精度损失。

## <a name="python-script-execution-issues"></a>Python 脚本执行问题

本部分包含特定于在 SQL Server 上运行 Python 的已知问题，以及由 Microsoft 发布的 Python 包相关的一些问题，包括 [revoscalepy](/r-server/python-reference/revoscalepy/revoscalepy-package) 和 [microsoftml](/r-server/python-reference/microsoftml/microsoftml-package)。

### <a name="1-call-to-pretrained-model-fails-if-path-to-model-is-too-long"></a>1.如果模型的路径太长，则调用预先训练的模型会失败

如果在 SQL Server 2017 的早期版本中安装了预先训练的模型，则训练的模型文件的完整路径可能会太长，以致 Python 无法读取。 此限制在更高版本的 Service Release 中得以修复。

有几种可能的解决方法：

+ 安装预先训练的模型时，请选择自定义位置。
+ 如果可能，请在具有较短路径（如 C:\SQL\MSSQL14.MSSQLSERVER）的自定义安装路径下安装 SQL Server 实例。
+ 使用 Windows 实用工具 [Fsutil](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc788097(v=ws.11)) 创建将模型文件映射到较短路径的硬链接。
+ 更新到最新版 Service Release。

### <a name="2-error-when-saving-serialized-model-to-sql-server"></a>2.将序列化模型保存到 SQL Server 时出现的错误

如果将模型传递到远程 SQL Server 实例，并尝试使用 [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) 中的 `rx_unserialize` 函数读取二进制模型，则可能会显示以下错误： 

> 名称错误：未定义名称“rx_unserialize_model” 

如果使用最新版序列化函数保存了模型，但反序列化模型的 SQL Server 实例无法识别序列化 API，则会引发此错误。

若要解决此问题，将 SQL Server 2017 实例升级到 CU3 或更高版本。

### <a name="3-failure-to-initialize-a-varbinary-variable-causes-an-error-in-bxlserver"></a>3.初始化 varbinary 变量失败会导致 BxlServer 中出现错误

如果你使用 `sp_execute_external_script` 在 SQL Server 中运行 Python 代码，并且代码具有类型为 varbinary(max)、varchar(max) 或类似类型的输出变量，则必须将该变量初始化或设置为脚本的一部分。 否则，数据交换组件 BxlServer 会引发错误并停止工作。

此限制将在即将发布的 Service Release 中得以解决。 解决方法之一是确保在 Python 脚本中初始化变量。 可以使用任何有效值，如以下示例中所示：

```sql
declare @b varbinary(max);
exec sp_execute_external_script
  @language = N'Python'
  , @script = N'b = 0x0'
  , @params = N'@b varbinary(max) OUTPUT'
  , @b = @b OUTPUT;
go
```

```sql
declare @b varchar(30);
exec sp_execute_external_script
  @language = N'Python'
  , @script = N' b = ""  '
  , @params = N'@b varchar(30) OUTPUT'
  , @b = @b OUTPUT;
go
```

### <a name="4-telemetry-warning-on-successful-execution-of-python-code"></a>4.在成功执行 Python 代码时出现的遥测警告

从 SQL Server 2017 CU2 开始，即使 Python 代码成功运行，也可能出现以下消息：

> 来自外部脚本的 STDERR 消息：
> ~PYTHON_SERVICES\lib\site-packages\revoscalepy\utils\RxTelemetryLogger
> SyntaxWarning：在全局声明之前使用 telemetry_state   

此问题已在 SQL Server 2017 累积更新 3 (CU3) 中得以解决。 

### <a name="5-numeric-decimal-and-money-data-types-not-supported"></a>5.不支持 numeric、decimal 和 money 数据类型

从 SQL Server 2017 累积更新 12 (CU12) 开始，将 Python 与 `sp_execute_external_script`结合使用时，不支持 WITH RESULT SETS 中的 numeric、decimal 和 money 数据类型。 可能会出现以下消息：

> *[代码：39004，SQL 状态：S1000]  执行“sp_execute_external_script”时发生“Python”脚本错误(HRESULT 0x80004004)。*
>
> *[代码：39019，SQL 状态：S1000]  发生外部脚本错误：*
>
> *SqlSatelliteCall 错误：输出架构中不支持的类型。支持的类型：bit、smallint、int、datetime、smallmoney、real 和 float。char 和 varchar 部分受支持。*

此问题已在 SQL Server 2017 累积更新 14 (CU14) 中得以解决。

### <a name="6-bad-interpreter-error-when-installing-python-packages-with-pip-on-linux"></a>6.在 Linux 上使用 pip 安装 Python 包时出现的解释器错误 

在 SQL Server 2019 上，如果尝试使用 pip  。 例如：

```bash
/opt/mssql/mlservices/runtime/python/bin/pip -h
```

然后，会显示此错误：

> bash: /opt/mssql/mlservices/runtime/python/bin/pip: /opt/microsoft/mlserver/9.4.7/bin/python/python: 错误解释器：  无此文件或目录

**解决方法**

从 [Python 包机构 (PyPA)](https://www.pypa.io) 安装 pip  ：

```bash
wget 'https://bootstrap.pypa.io/get-pip.py' 
/opt/mssql/mlservices/bin/python/python ./get-pip.py 
```

**建议**

请参阅[使用 sqlmlutils 安装 Python 包](../package-management/install-additional-python-packages-on-sql-server.md)。

**适用于：** Linux 上的 SQL Server 2019

### <a name="7-unable-to-install-python-packages-using-pip-after-installing-sql-server-2019-on-windows"></a>7.在 Windows 上安装 SQL Server 2019 后，无法使用 pip 安装 Python 包

在 Windows 上安装 SQL Server 2019 后，尝试从 DOS 命令行通过 pip 安装 Python 包将失败  。 例如：

```bash
pip install quantfolio
```

将返回以下错误：

> 虽然 pip 配置了需要 TLS/SSL 的位置，但 Python 中的 SSL 模块不可用。 

这是 Anaconda 包特有的问题。 此问题将在即将发布的 Service Release 中得以解决。

**解决方法**

复制以下文件：

+ `libssl-1_1-x64.dll`
+ `libcrypto-1_1-x64.dll`

从文件夹 <br>
`C:\Program Files\Microsoft SQL Server\MSSSQL15.MSSQLSERVER\PYTHON_SERVICES\Library\bin`

复制到文件夹 <br>
`C:\Program Files\Microsoft SQL Server\MSSSQL15.MSSQLSERVER\PYTHON_SERVICES\DLLs`

然后打开新的 DOS 命令 shell 提示符。

**适用于：** Windows 上的 SQL Server 2019

### <a name="8-error-when-using-sp_execute_external_script-without-libcaboso-on-linux"></a>8.在 Linux 上使用不具有 `libc++abo.so` 的 `sp_execute_external_script` 时出现的错误

在未安装 `libc++abi.so` 的全新 Linux 计算机上，运行 `sp_execute_external_script` (SPEES) 查询会失败，并出现“无此文件或目录”的错误。

例如：

```text
EXEC sp_execute_external_script
    @language = N'Python'
    , @script = N'
OutputDataSet = InputDataSet'
    , @input_data_1 = N'select 1'
    , @input_data_1_name = N'InputDataSet'
    , @output_data_1_name = N'OutputDataSet'
    WITH RESULT SETS (([output] int not null));
Msg 39012, Level 16, State 14, Line 0
Unable to communicate with the runtime for 'Python' script for request id: 94257840-1704-45E8-83D2-2F74AEB46CF7. Please check the requirements of 'Python' runtime.
STDERR message(s) from external script:

Failed to load library /opt/mssql-extensibility/lib/sqlsatellite.so with error libc++abi.so.1: cannot open shared object file: No such file or directory.

SqlSatelliteCall error: Failed to load library /opt/mssql-extensibility/lib/sqlsatellite.so with error libc++abi.so.1: cannot open shared object file: No such file or directory.
STDOUT message(s) from external script:
SqlSatelliteCall function failed. Please see the console output for more information.
Traceback (most recent call last):
  File "/opt/mssql/mlservices/libraries/PythonServer/revoscalepy/computecontext/RxInSqlServer.py", line 605, in rx_sql_satellite_call
    rx_native_call("SqlSatelliteCall", params)
  File "/opt/mssql/mlservices/libraries/PythonServer/revoscalepy/RxSerializable.py", line 375, in rx_native_call
    ret = px_call(functionname, params)
RuntimeError: revoscalepy function failed.
Total execution time: 00:01:00.387
```

**解决方法**

运行以下命令：

```bash
sudo cp /opt/mssql/lib/libc++abi.so.1 /opt/mssql-extensibility/lib/
```

**适用于：** Linux 上的 SQL Server 2019

### <a name="9-cannot-install-tensorflow-package-using-sqlmlutils"></a>9.无法使用 sqlmlutils 安装 tensorflow 包 

[sqlmlutils 包](../package-management/install-additional-python-packages-on-sql-server.md)用于在 SQL Server 2019 中安装 Python 包。 你需要下载、安装和更新 [Microsoft Visual C++ 2015-2019 Redistributable (x64)](https://visualstudio.microsoft.com/downloads/)。 但是，无法使用 sqlmlutils 安装 tensorflow 包。 Tensorflow 包依赖于 numpy 的新版本，而不是 SQL Server 中安装的版本。 但是，numpy 是预安装的系统包，在尝试安装 tensorflow 时 sqlmlutils 无法更新该包。

**解决方法**

在管理员模式下使用命令提示符运行以下命令，并将“MSSQLSERVER”替换为你的 SQL 实例的名称：

   ```cmd
   "C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\PYTHON_SERVICES\python.exe" -m pip install --upgrade tensorflow
   ```

   如果收到“TLS/SSL”错误，请参阅本文前面的 [7.无法使用 pip 安装 Python 包](#7-unable-to-install-python-packages-using-pip-after-installing-sql-server-2019-on-windows)。

**适用于：** Windows 上的 SQL Server 2019

## <a name="revolution-r-enterprise-and-microsoft-r-open"></a>Revolution R Enterprise 和 Microsoft R Open

本部分列出 Revolution Analytics 提供的 R 连接、开发和性能工具特定的问题。 在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 的早期预发布版本中提供了这些工具。

一般情况下，我们建议卸载这些早期版本并安装最新版本的 SQL Server 或 Microsoft R Server。

### <a name="1-revolution-r-enterprise-is-not-supported"></a>1.不支持 Revolution R Enterprise

不支持并行安装 Revolution R Enterprise 与任何版本的 [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)]。

如果你有 Revolution R Enterprise 的现有许可证，你不能将该版本安装在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例所在的同一台计算机上，也不能安装在用于连接 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的任何工作站上。

[!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] 的一些预发行版本包含 Revolution Analytics 创建的适用于 Windows 的 R 开发环境。 不再提供且不支持此工具。

为了与 [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] 兼容，我们建议改为安装 Microsoft R Client。 [针对 Visual Studio 的 R 工具](https://marketplace.visualstudio.com/items?itemName=MikhailArkhipov007.RTVS2019)和 [Visual Studio Code](https://code.visualstudio.com/) 还支持 Microsoft R 解决方案。

### <a name="2-compatibility-issues-with-sqlite-odbc-driver-and-revoscaler"></a>2.SQLite ODBC 驱动程序与 RevoScaleR 的兼容性问题

SQLite ODBC 驱动程序的修订版 0.92 与 RevoScaleR 不兼容。 已知 0.88-0.91 和 0.93 修订版及更高版本是兼容的。

## <a name="next-steps"></a>后续步骤

[SQL Server 中的机器学习疑难解答](machine-learning-troubleshooting-overview.md)