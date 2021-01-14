---
title: 安装 SQL Server 2016 R Services
titleSuffix: ''
description: 了解如何在 Windows 上安装 SQL Server 2016 R Services。 可以使用 R Services 在数据库中执行 R 脚本。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: contperf-fy20q4
monikerRange: =sql-server-2016
ms.openlocfilehash: 78b208be4f893f7029ae09c7c0b624b3642b8f35
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100184"
---
# <a name="install-sql-server-2016-r-services"></a>安装 SQL Server 2016 R Services

[!INCLUDE[SQL Server 2016 only](../../includes/applies-to-version/sqlserver2016-only.md)]

了解如何在 Windows 上安装 SQL Server 2016 R Services。 可以使用 R Services 在数据库中执行 R 脚本。

> [!NOTE]
> 在 SQL Server 2017 及更高版本中，R 与 Python 一起随附在[机器学习服务](../sql-server-machine-learning-services.md)中。 如果你想获取 R 并具有 SQL Server 2017 或更高版本，请参阅[安装 SQL Server 机器学习服务](sql-machine-learning-services-windows-install.md)以添加该功能。

<a name="bkmk_prereqs"></a>

## <a name="pre-install-checklist"></a>安装前清单

+ 需要数据库引擎实例。 不能只安装 R，但可将其以增量方式添加到现有实例。

+ 为实现业务连续性，R Services 支持 [Always On 可用性组](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。 必须在每个节点上安装 R Services 并配置包。

+ 请勿在 SQL Server Always On 故障转移群集 (FCI) 上安装 R Services。 用于隔离 R 进程的安全机制与 SQL Server Always On 故障转移群集 (FCI) 环境不兼容。

+ 请勿在域控制器上安装 R Services。 安装程序的 R Services 部分将失败。

+ 请勿在运行数据库内实例的同一台计算机上安装“共享功能” > “R Server (独立版)” 。

+ 支持并行安装其他版本的 R，但不建议这样做。 之所以支持，是因为 SQL Server 实例使用自己的开源 R 分发副本。 然而，在 SQL Server 外部的 SQL Server 计算机上运行使用 R 的代码可能会导致各种问题：

  + 你将使用不同于在 SQL Server 中运行时使用的库和可执行文件，因此会获得不同的结果。
  + SQL Server 无法管理在外部库中运行的 R 脚本，从而导致资源争用。
  
> [!IMPORTANT]
> 安装完成后，请务必完成本文中所述的其他配置后步骤。 这些步骤包括启用 SQL Server 以使用外部脚本，以及以你的名义添加 SQL Server 运行 R 作业所需的帐户。 配置更改通常需要重启实例或重启 Launchpad 服务。

## <a name="get-the-installation-media"></a>获取安装介质

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

<a name="bkmk_ga_instalpatch"></a>

### <a name="install-patch-requirement"></a>安装修补程序要求

Microsoft 已发现特定版本的 Microsoft VC++ 2013 运行时二进制文件存在问题，这些二进制文件是作为 SQL Server 系统必备进行安装的。 如果未安装适用于该 VC 运行时二进制文件的更新，则在某些情况下 SQL Server 可能会遇到稳定性问题。 在安装 SQL Server 之前，请按照 [SQL Server 发行说明](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch)中的说明来确定你的电脑是否需要 VC 运行时二进制文件的修补程序。  

<a name="bkmk2016top"></a>

## <a name="run-setup"></a>运行安装程序

对于本地安装，必须以管理员身份运行安装程序。 如果从远程共享安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则必须使用对远程共享具有读取和执行权限的域帐户。

1. 启动 SQL Server 2016 的安装向导。

1. 在“安装”选项卡上，选择“全新 SQL Server 独立安装或向现有安装添加功能”   。

    ![安装 R Services（数据库内）](media/2016-setup-installation-rsvcs.png "使用 R Services 开始安装数据库引擎")

1. 在“功能选择”页上，选择以下选项  ：

    + 选择“数据库引擎服务”  。 使用机器学习的每个实例都需要数据库引擎。
    + 选择“R Services (数据库内)”  。 安装对在数据库中使用 R 的支持。

    ![R Services 功能选择](media/2016setup-rsvcs-features.png "为数据库中的 R Services 选择这些功能")

    > [!IMPORTANT]
    > 请勿同时安装 R Server 和 R Services。 

1. 在“同意安装 Microsoft R Open”  页上，单击“接受”  。
  
    需要此许可协议方可下载 Microsoft R Open，其中包括分发开放源代码 R 基础包和工具，以及 Microsoft R 开发团队提供的增强型 R 包和连接提供程序。
  
1. 接受许可协议后，在安装程序准备期间将出现短暂暂停。 当按钮可用时，单击“下一步”  。

1. 在“准备安装”页上，验证是否包括以下项，然后选择“安装”   。

    + 数据库引擎服务
    + R Services（数据库内）

1. 安装完成后，如果收到重启计算机的指示，请立即重启。 安装完成后，请务必阅读来自安装向导的消息。 有关详细信息，请参阅 [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。

## <a name="set-environment-variables"></a>设置环境变量

（仅适用于 R 功能集成）应设置“MKL_CBWR”环境变量，以确保从 Intel 数学核心函数库 (MKL) 计算得到[一致的输出结果](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr)。

1. 在“控制面板”中，单击“系统和安全” > “系统” > “高级系统设置” > “环境变量”   。

1. 创建新的用户或系统变量。

    + 将变量名称设置为 `MKL_CBWR`
    + 将变量值设置为 `AUTO`

此步骤需要重启服务器。 可以在完成所有配置工作之前暂停重启。

<a name="bkmk_enableFeature"></a>

##  <a name="enable-script-execution"></a>启用脚本执行

1. 打开 [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) 或 [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md)。

1. 连接到安装了 R Services 的实例，单击“新建查询”打开查询窗口，然后运行以下命令：

   ```sql
   sp_configure
   ```

    属性 `external scripts enabled` 的值目前应为 **0**。 这是因为默认情况下该功能处于关闭状态。 必须先由管理员显式启用此功能，然后才能运行 R 脚本。

1. 若要启用外部脚本编写功能，请运行以下语句：
  
    ```sql
    EXEC sp_configure  'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```
  
## <a name="restart-the-service"></a>重新启动服务。

安装完成后，请先重启数据库引擎，然后再继续执行下一步，以启用脚本执行。

重启服务也会自动重启相关的 [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] 服务。

可以使用右键单击 SSMS 中实例的“重启”命令，或者使用 [SQL Server 配置管理器](../../relational-databases/sql-server-configuration-manager.md)来重启服务。

## <a name="verify-installation"></a>验证安装

使用以下步骤验证用于启动外部脚本的所有组件是否都在运行。

1. 在 SQL Server Management 中打开新的“查询”窗口，然后运行以下命令：

    ```sql
    EXEC sp_configure 'external scripts enabled'
    ```

    **run_value** 现在应已设置为 1。

1. 打开 SQL Server 配置管理器，并验证“SQL Server Launchpad 服务”是否正在运行。 应该为每个安装了 R 的数据库引擎实例提供一项服务。 有关该服务的详细信息，请参阅[扩展性框架](../concepts/extensibility-framework.md)。

1. 如果 Launchpad 正在运行，则应该能够运行简单的 R，以验证外部脚本运行时是否可以与 SQL Server 通信。

    在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 Azure Data Studio 中打开新的“查询”窗口，然后运行如下脚本：

    ```sql
    EXEC sp_execute_external_script  @language =N'R',
    @script=N'
    OutputDataSet <- InputDataSet;
    ',
    @input_data_1 =N'SELECT 1 AS hello'
    WITH RESULT SETS (([hello] int not null));
    GO
    ```

    如果是首次加载外部脚本运行时，则该脚本可能需要一些时间才能运行。 结果应如下所示：

    | Hello |
    |----|
    | 1|

<a name="apply-cu"></a>

## <a name="apply-updates"></a>应用更新

建议将最新的服务包和累积更新应用于数据库引擎和机器学习组件。

在连接 Internet 的设备上，累积更新通常通过 Windows 更新进行应用，但也可以使用以下步骤进行可控更新。 为数据库引擎应用更新时，安装程序将为你在同一实例上安装的 R 库拉取累积更新。 

在断开连接的服务器上，需要执行额外的步骤。 有关详细信息，请参阅 [在没有 Internet 连接的计算机上安装 > 应用累积更新](sql-ml-component-install-without-internet-access.md#apply-cu)。

1. 开始使用已安装的基线实例：SQL Server 2016 初始版本、SQL Server 2016 SP 1 或 SQL Server 2016 SP 2。

1. 请转到累积更新列表：[Microsoft SQL Server 的最新更新](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

1. 选择最新服务包（尚未作为基线实例安装）和累积更新。 自动下载并提取可执行文件。

1. 运行安装程序。 接受许可条款，然后在“功能选择”页上，查看应用了累积更新的功能。 应看到为当前实例安装的每个功能，包括 R Services。 安装程序会下载更新所有功能所需的 CAB 文件。

1. 继续执行向导，接受 R 分发的许可条款。

> [!NOTE]
> SQL Server 2016 SP2 的累积更新 (CU) 14 和更高版本包含 R 运行时的更新版本。 有关详细信息，请参阅[更改默认语言运行时版本](change-default-language-runtime-version.md)。

<a name="bkmk_FollowUp"></a>

## <a name="additional-configuration"></a>其他配置

如果外部脚本验证步骤成功，则可以从 SQL Server Management Studio、Azure Data Studio 或者能够向服务器发送 T-SQL 语句的其他任何客户端运行 R 命令。

如果在运行命令时遇到错误，请查看本部分中的其他配置步骤。 服务或数据库可能需要进行其他适当的配置。

在实例级别，其他配置可能包括：

* [SQL Server 机器学习服务的防火墙配置](../../machine-learning/security/firewall-configuration.md)。
* [启用其他网络协议](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)。
* [启用远程连接](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)。
* [管理磁盘配额](/windows/desktop/fileio/managing-disk-quotas)以避免外部脚本运行耗尽磁盘空间的任务。

<a name="bkmk_configureAccounts"></a>
<a name="bkmk_AllowLogon"></a>

数据库上可能需要以下配置更新：

* [向用户授予 SQL Server 机器学习服务的权限](../../machine-learning/security/user-permission.md)
* [将 SQLRUserGroup 添加为数据库用户](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)

> [!NOTE]
> 并非所有列出的更改都是必需的，也可能不需要任何更改。 要求取决于安全架构、SQL Server 的安装位置，以及期望用户以何种方式连接到数据库和运行外部脚本。 可在此处找到其他安装指南：[安装 SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md)

## <a name="suggested-optimizations"></a>建议的优化

可能还需要优化服务器以支持通过 R 进行机器学习，或安装预先训练的模型。

### <a name="add-more-worker-accounts"></a>添加更多辅助角色帐户

如果可能经常使用 R，或预期会有很多用户同时运行脚本，则可以增加分配给 Launchpad 服务的辅助角色帐户数目。 有关详细信息，请参阅[在 SQL Server 机器学习服务中扩展外部脚本的并发执行](../administration/scale-concurrent-execution-external-scripts.md)。

<a name="bkmk_optimize"></a>

### <a name="optimize-the-server-for-external-script-execution"></a>优化服务器以执行外部脚本

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的默认设置旨在为数据库引擎支持的各种服务（可能包括提取、转换和加载 (ETL) 进程、报告、审核和使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据的应用程序）优化服务器平衡。 因此，在默认设置下，可能会发现机器学习（尤其是占用大量内存的操作）的资源有时被限制或被阻止。

若要确保机器学习作业的优先级并为其分配适当资源，建议使用 SQL Server Resource Governor 配置外部资源池。 可能还要更改分配到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库引擎的内存量，或增加 [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] 服务下运行的帐户数。

- 要配置用于管理外部资源的资源池，请参阅[创建外部资源池](../../t-sql/statements/create-external-resource-pool-transact-sql.md)。
  
- 若要更改为数据库保留的内存量，请参阅[服务器内存配置选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md)。
  
- 若要更改可以通过 [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] 启动的 R 帐户数，请参阅[在 SQL Server 机器学习服务中扩展外部脚本的并发执行](../administration/scale-concurrent-execution-external-scripts.md)。

如果使用的是 Standard Edition，且没有安装 Resource Governor，则可以使用动态管理视图 (DMV) 和扩展事件以及 Windows 事件监视来帮助管理 R 使用的服务器资源。

### <a name="install-additional-r-packages"></a>安装其他 R 包

为 SQL Server 创建的 R 解决方案可以调用基本 R 函数、随 SQL Server 一起安装的专有包中的函数以及与 SQL Server 安装的开放源代码 R 版本兼容的第三方 R 包。

要通过 SQL Server 使用的包必须安装在实例使用的默认库中。 如果在计算机上单独安装了 R，或者将包安装到了用户库，则无法从 T-SQL 使用这些包。

安装和管理 R 包的过程在 SQL Server 2016 和 SQL Server 2017 中有所不同。 在 SQL Server 2016 中，数据库管理员必须安装用户所需的 R 包。 在 SQL Server 2017 中，可通过设置用户组共享每个数据库级别的包，或者通过配置数据库角色使用户能够安装其自己的包。 有关详细信息，请参阅[使用 R 工具安装包](../package-management/install-r-packages-standard-tools.md)。

## <a name="next-steps"></a>后续步骤

R 开发人员可以开始使用一些简单的示例，并了解 R 如何与 SQL Server 协同工作的基础知识。 有关下一步，请参阅以下链接：

+ [快速入门：在 T-SQL 中运行 R](../tutorials/quickstart-r-create-script.md)
+ [适用于 SQL 机器学习的 R 教程](../tutorials/r-tutorials.md)
+ [SQL 机器学习文档](../index.yml)