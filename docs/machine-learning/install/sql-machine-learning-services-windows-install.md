---
title: 在 Windows 上安装
description: 了解如何在 Windows 上安装 SQL Server 机器学习服务。 可使用机器学习服务在数据库中执行 Python 和 R 脚本。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/29/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017'
ms.openlocfilehash: 592a365024edbbe4ee2743a156ee480b76849ba9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096836"
---
# <a name="install-sql-server-machine-learning-services-python-and-r-on-windows"></a>在 Windows 上安装 SQL Server 机器学习服务（Python 和 R）

[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

了解如何在 Windows 上安装 SQL Server 机器学习服务。 可使用机器学习服务在数据库中执行 Python 和 R 脚本。

## <a name="pre-install-checklist"></a><a name="bkmk_prereqs"></a> 安装前清单

+ 需要数据库引擎实例。 不能只安装 Python 或 R 功能，但可以将它们逐渐添加到现有实例。

+ 为了实现业务连续性，机器学习服务支持 [Always On 可用性组](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。 在每个节点上安装机器学习服务并配置包。

+ SQL Server 2017 中的 [Always On 故障转移群集实例 (FCI)](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md) 不支持安装机器学习服务。 SQL Server 2019 及更高版本支持上述服务。
 
+ 不要在域控制器上安装机器学习服务。 安装程序的机器学习服务部分将失败。

+ 不要在运行数据库实例的同一台计算机上安装“共享功能” > “Machine Learning Server（独立）” 。 一个独立服务器将争夺相同的资源，从而降低这两个安装的性能。

+ 支持并行安装 Python 和 R 的其他版本，但不建议这样做。 之所以支持，是因为 SQL Server 实例使用自己的开源 R 和 Anaconda 分发副本。 不建议这样做，因为在 SQL Server 外部的 SQL Server 计算机上运行使用 Python 和 R 的代码可能会导致各种问题：
    
  + 使用不同的库和可执行文件将创建不一致的结果，而不是 SQL Server 中运行的结果。
  + SQL Server 无法管理在外部库中运行的 R 和 Python 脚本，从而导致资源争用。

::: moniker range=">=sql-server-ver15"
> [!NOTE]
> 默认情况下，机器学习服务安装在 SQL Server 大数据群集上。 如果使用大数据群集，则无需按照本文中的步骤进行操作。 有关详细信息，请参阅[在大数据群集上使用机器学习服务（Python 和 R）](../../big-data-cluster/machine-learning-services.md)。
::: moniker-end

> [!IMPORTANT]
> 安装完成后，请务必完成本文中所述的配置后步骤。 这些步骤包括启用 SQL Server 以使用外部脚本，以及以你的名义添加 SQL Server 运行 R 和 Python 作业所需的帐户。 配置更改通常需要重启实例或重启 Launchpad 服务。

## <a name="get-the-installation-media"></a>获取安装介质

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

::: moniker range="=sql-server-2017"
有关哪些 SQL Server 版本支持将 Python 和 R 与机器学习服务集成的详细信息，请参阅 [SQL Server 2017 的版本和受支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)。
::: moniker-end

::: moniker range="=sql-server-ver15"
有关哪些 SQL Server 版本支持将 Python 和 R 与机器学习服务集成的详细信息，请参阅 [SQL Server 2019 (15.x) 的版本和受支持的功能](../../sql-server/editions-and-components-of-sql-server-version-15.md)。
::: moniker-end

## <a name="run-setup"></a>运行安装程序

对于本地安装，必须以管理员身份运行安装程序。 如果从远程共享安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则必须使用对远程共享具有读取和执行权限的域帐户。

1. 启动 SQL Server 的安装向导。
  
1. 在“安装”选项卡上，选择“全新 SQL Server 独立安装或向现有安装添加功能” 。

   ::: moniker range="=sql-server-2017"
   ![全新 SQL Server 独立安装](media/2017setup-installation-page-mlsvcs.png)
   ::: moniker-end

   ::: moniker range="=sql-server-ver15"
   ![全新 SQL Server 独立安装](media/2019setup-installation-page-mlsvcs.png)
   ::: moniker-end

1. 在“功能选择”  页上，选择以下选项：

   ::: moniker range="=sql-server-2017"

   - **数据库引擎服务**
     
     若要将 R 和 Python 与 SQL Server 结合使用，则必须安装数据库引擎实例。 可使用默认实例或命名实例。

   - **机器学习服务（数据库内）**
     
     此选项安装支持 R 和 Python 脚本执行的数据库服务。

   ::: moniker-end

   ::: moniker range="=sql-server-ver15"

   - **数据库引擎服务**
     
     若要将 R 或 Python 与 SQL Server 结合使用，则必须安装数据库引擎实例。 可使用默认实例或命名实例。

   - **机器学习服务（数据库内）**
     
     此选项安装支持 R 和 Python 脚本执行的数据库服务。

   ::: moniker-end

   - **R**
     
     选中此选项可添加 Microsoft R 包、解释器和开源 R。 
     
   - **Python**
     
     选中此选项可添加 Microsoft Python 包、Python 3.5 可执行文件以及从 Anaconda 分发中选择库。
     
   ::: moniker range="=sql-server-ver15"
   有关安装和使用 Java 的信息，请参阅[在 Windows 上安装 SQL Server 语言扩展](../../language-extensions/install/windows-java.md)。
   ::: moniker-end
   
   ::: moniker range="=sql-server-2017"
   ![R 和 Python 的功能选项](media/2017setup-features-page-mls-rpy.PNG "R 和 Python 的安装选项")
   ::: moniker-end
   
   ::: moniker range="=sql-server-ver15"
   ![R 和 Python 的功能选项](media/2019setup-features-page-mls-rpy.png "R 和 Python 的安装选项")
   ::: moniker-end
   
   > [!NOTE]
   > 
   > 不要选择 Machine Learning Server（独立）对应的选项。 在“共享功能”下安装 Machine Learning Server 的选项适用于在单独的计算机上使用。

::: moniker range="=sql-server-2017"

4. 在“同意安装 Microsoft R Open”页上，选择“接受”，然后选择“下一步”  。 

许可协议涵盖以下内容：
+ Microsoft R Open
+ 开放源代码 R 基本包和工具
+ Microsoft 开发团队提供的增强 R 包和连接提供程序。

1. 在“同意安装 Python”页上，选择“接受”，然后选择“下一步”  。 Python 开源许可协议还涵盖了 Anaconda 和相关工具，以及 Microsoft 开发团队提供的一些新 Python 库。

   > [!NOTE]
   >  如果正在使用的计算机无法访问 Internet，则可以在此时暂停安装以单独下载安装程序。 有关详细信息，请参阅[在没有 Internet 连接的情况下安装机器学习组件](../install/sql-ml-component-install-without-internet-access.md)。

1. 在“准备安装”页面上，验证是否已包括这些选择，然后选择“安装” 。
  
   + 数据库引擎服务
   + 机器学习服务（数据库内）
   + R 和/或 Python

   请注意配置文件存储路径 `..\Setup Bootstrap\Log` 下的文件夹位置。 安装完成后，可以在“摘要”文件中查看已安装的组件。

1. 安装完成后，如果收到重启计算机的指示，请立即重启。 安装完成后，请务必阅读来自安装向导的消息。 有关详细信息，请参阅 [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。

::: moniker-end

::: moniker range="=sql-server-ver15"

1. 在“同意安装 Microsoft R Open”页上，选择“接受”，然后选择“下一步”  。 该许可协议涵盖 Microsoft R Open，其中包括分发开源 R 基础包和工具，以及 Microsoft 开发团队提供的增强型 R 包和连接提供程序。

2. 在“同意安装 Python”页上，选择“接受”，然后选择“下一步”  。 Python 开源许可协议还涵盖了 Anaconda 和相关工具，以及 Microsoft 开发团队提供的一些新 Python 库。

3. 在“准备安装”页面上，验证是否已包括这些选择，然后选择“安装” 。
  
   + 数据库引擎服务
   + 机器学习服务（数据库内）
   + R 和/或 Python

   请注意配置文件存储路径 `..\Setup Bootstrap\Log` 下的文件夹位置。 安装完成后，可以在“摘要”文件中查看已安装的组件。

4. 安装完成后，如果收到重启计算机的指示，请立即重启。 安装完成后，请务必阅读来自安装向导的消息。 有关详细信息，请参阅 [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。

::: moniker-end

## <a name="set-environment-variables"></a>设置环境变量

（仅适用于 R 功能集成）应设置“MKL_CBWR”环境变量，以确保从 Intel 数学核心函数库 (MKL) 计算得到[一致的输出结果](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr)。

1. 在“控制面板”中，单击“系统和安全” > “系统” > “高级系统设置” > “环境变量”   。

2. 创建新的用户或系统变量。 

   + 将变量名称设置为 `MKL_CBWR`
   + 将变量值设置为 `AUTO`

此步骤需要重启服务器。 如果要启用脚本执行，则可以在完成所有配置工作之前暂停重启。

<a name="bkmk_enableFeature"></a>

## <a name="enable-script-execution"></a>启用脚本执行

1. 打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。 

    > [!TIP]
    > 可以从此页下载并安装相应的版本：[下载 SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)。
    > 
    > 还可以使用 [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md)，它支持管理任务和针对 SQL Server 的查询。
  
2. 连接到安装了机器学习服务的实例，单击“新建查询”打开查询窗口，然后运行以下命令：

    ```sql
    sp_configure
    ```

    属性 `external scripts enabled` 的值目前应为 **0**。 默认情况下，此功能处于关闭状态。 必须先由管理员显式启用此功能，然后才能运行 R 或 Python 脚本。
    
3.  若要启用外部脚本编写功能，请运行以下语句：
    
    ```sql
    EXEC sp_configure  'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```
    
    如果已为 R 语言启用此功能，请不要对 Python 再次运行 reconfigure 命令。 基础扩展性平台支持这两种语言。

## <a name="restart-the-service"></a>重新启动服务。

安装完成后，重启数据库引擎。

重启服务也会自动重启相关的 [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] 服务。

可以使用右键单击 SSMS 中实例的“重启”命令、使用“控制面板”中的“服务”面板，或者使用 [SQL Server 配置管理器](../../relational-databases/sql-server-configuration-manager.md)来重启服务 。

## <a name="verify-installation"></a>验证安装

使用以下步骤验证用于启动外部脚本的所有组件是否都在运行。

1. 在 SQL Server Management 中打开新的“查询”窗口，然后运行以下命令：
    
   ```sql
   EXECUTE sp_configure  'external scripts enabled'
   ```

   run_value 设置为 1。
    
2. 打开“服务”面板或 SQL Server 配置管理器，并验证“SQL Server Launchpad”是否正在运行 。 应该为每个安装了 R 或 Python 的数据库引擎实例提供一项服务。 有关该服务的详细信息，请参阅[扩展性框架](../concepts/extensibility-framework.md)。 
   
3. 如果 Launchpad 正在运行，则可以运行简单的 Python 和 R 脚本，以验证外部脚本运行时是否可以与 SQL Server 通信。

   在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中打开新的“查询”窗口，然后运行如下脚本：
   + 对于 R
   
     ```sql
     EXEC sp_execute_external_script  @language =N'R',
     @script=N'
     OutputDataSet <- InputDataSet;
     ',
     @input_data_1 =N'SELECT 1 AS hello'
     WITH RESULT SETS (([hello] int not null));
     GO
     ```
     
   + 对于 Python
     
     ```sql
     EXEC sp_execute_external_script  @language =N'Python',
     @script=N'
     OutputDataSet = InputDataSet;
     ',
     @input_data_1 =N'SELECT 1 AS hello'
     WITH RESULT SETS (([hello] int not null));
     GO
     ```
   
   **结果**

   如果是首次加载外部脚本运行时，则该脚本可能需要一些时间才能运行。 结果应如下所示：

   | Hello |
   |----|
   | 1|

> [!NOTE]
> 不会自动返回 Python 脚本中使用的列或标题。 若要在输出结果中添加列名称，则必须为返回数据集指定架构。 为此，请使用存储过程的 WITH RESULTS 参数，命名列并指定 SQL 数据类型。
>
> 例如，可以添加以下行来生成任意列名：`WITH RESULT SETS ((Col1 AS int))`

::: moniker range="=sql-server-2017"
<!-- There are no updates yet available for 2019, and there's no 2019 update list site. When updates become available, add 2019 information to this section. -->

<a name="apply-cu"></a>

## <a name="apply-updates"></a>应用更新

建议将最新的累积更新应用于数据库引擎和机器学习组件。

在连接 Internet 的设备上，累积更新通常通过 Windows 更新进行应用，但也可以使用以下步骤进行可控更新。 为数据库引擎应用更新时，安装程序将为你在同一实例上安装的任何 Python 或 R 功能拉取累积更新。 

断开连接的服务器需要执行额外的步骤。 有关详细信息，请参阅 [在没有 Internet 连接的计算机上安装 > 应用累积更新](sql-ml-component-install-without-internet-access.md#apply-cu)。

1. 开始使用已安装的基线实例：SQL Server 2017 初始版本

2. 请转到累积更新列表：[Microsoft SQL Server 的最新更新](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

3. 选择最新的累积更新。 自动下载并提取可执行文件。

4. 运行安装程序。 接受许可条款，然后在“功能选择”页上，查看应用了累积更新的功能。 应看到为当前实例安装的每个功能，包括机器学习功能。 安装程序会下载更新所有功能所需的 CAB 文件。

   ![已安装功能的摘要](media/cumulative-update-feature-selection.png)

5. 继续执行向导，接受 R 和 Python 分发的许可条款。 

::: moniker-end

## <a name="additional-configuration"></a>其他配置

如果外部脚本验证步骤成功，则可以从 SQL Server Management Studio、Visual Studio Code 或者能够向服务器发送 T-SQL 语句的其他任何客户端运行 R 或 Python 命令。

如果在运行命令时遇到错误，请查看本部分中的其他配置步骤。 服务或数据库可能需要进行其他适当的配置。

在实例级别，其他配置可能包括：

* [SQL Server 机器学习服务的防火墙配置](../../machine-learning/security/firewall-configuration.md)
* [启用其他网络协议](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
* [启用远程连接](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)
* [为 SQLRUserGroup 创建登录名](../../machine-learning/security/create-a-login-for-sqlrusergroup.md)
* [管理磁盘配额](/windows/desktop/fileio/managing-disk-quotas)以避免外部脚本运行耗尽磁盘空间的任务

::: moniker range=">=sql-server-ver15"
在 Windows 上的 SQL Server 2019 中，隔离机制已发生更改。 此机制会影响 SQLRUserGroup、防火墙规则、文件权限和默示身份验证。 有关详细信息，请参阅 [机器学习服务的隔离更改](sql-server-machine-learning-services-2019.md)。
::: moniker-end

<a name="bkmk_configureAccounts"></a> 
<a name="permissions-external-script"></a> 

数据库上可能需要以下配置更新：

* [向用户授予 SQL Server 机器学习服务的权限](../../machine-learning/security/user-permission.md)

> [!NOTE]
> 是否需要其他配置取决于安全架构、SQL Server 的安装位置，以及期望用户以何方式连接到数据库和运行外部脚本。

## <a name="suggested-optimizations"></a>建议的优化

现在所有工作已顺利进行，可能还需要优化服务器以支持机器学习，或安装预先训练的机器学习模型。

::: moniker range="=sql-server-2017"
### <a name="add-more-worker-accounts"></a>添加更多辅助角色帐户

如果预期会有很多用户同时运行脚本，则可以增加分配给 Launchpad 服务的辅助角色帐户数目。 有关详细信息，请参阅[在 SQL Server 机器学习服务中扩展外部脚本的并发执行](../administration/scale-concurrent-execution-external-scripts.md)。
::: moniker-end

### <a name="optimize-the-server-for-script-execution"></a>优化服务器执行脚本

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的默认设置旨在为数据库引擎支持的各种服务（可能包括提取、转换和加载 (ETL) 进程、报告、审核和使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据的应用程序）优化服务器平衡。 在默认设置下，机器学习（尤其是占用大量内存的操作）的资源有时会被限制或被阻止。

若要确保机器学习作业的优先级并为其分配适当资源，建议使用 SQL Server Resource Governor 配置外部资源池。 可能还要更改分配到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库引擎的内存量，或增加 [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] 服务下运行的帐户数。

- 要配置用于管理外部资源的资源池，请参阅[创建外部资源池](../../t-sql/statements/create-external-resource-pool-transact-sql.md)。
  
- 若要更改为数据库保留的内存量，请参阅[服务器内存配置选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md)。
  
- 若要更改可以通过 [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] 启动的 R 帐户数，请参阅[在 SQL Server 机器学习服务中扩展外部脚本的并发执行](../administration/scale-concurrent-execution-external-scripts.md)。

如果使用的是 Standard Edition 且没有 Resource Governor，则可以使用动态管理视图 (DMV) 和扩展事件以及 Windows 事件监视来帮助管理服务器资源。

### <a name="install-additional-python-and-r-packages"></a>安装其他 Python 和 R 包

为 SQL Server 创建的 Python 和 R 解决方案可以调用基本功能、随 SQL Server 一起安装的专有包中的功能以及与 SQL Server 安装的开源 Python 和 R 版本兼容的第三方包。

要通过 SQL Server 使用的包必须安装在实例使用的默认库中。 如果在计算机上单独安装了 Python 或 R，或者将包安装到了用户库，则无法从 T-SQL 使用这些包。

若要安装和管理其他包，可以将用户组设置为共享每个数据库级别的包，或者通过配置数据库角色使用户能够安装自己的包。 有关详细信息，请参阅[安装 Python 包](../package-management/install-additional-python-packages-on-sql-server.md)和[安装新的 R 包](../package-management/install-additional-r-packages-on-sql-server.md)。

## <a name="next-steps"></a>后续步骤

Python 开发人员可以通过以下教程了解如何将 Python 与 SQL Server 一起使用：

+ [Python 教程：在 SQL Server 机器学习服务中使用线性回归来预测雪橇租赁](../tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Python 教程：配合使用 K-Means 群集和 SQL Server 机器学习服务对客户进行分类](../tutorials/python-clustering-model.md)

R 开发人员可以开始使用一些简单的示例，并了解 R 如何与 SQL Server 协同工作的基础知识。 有关下一步，请参阅以下链接：

+ [快速入门：在 T-SQL 中运行 R](../tutorials/quickstart-r-create-script.md)
+ [教程：适用于 R 开发人员的数据库内分析](../tutorials/r-taxi-classification-introduction.md)