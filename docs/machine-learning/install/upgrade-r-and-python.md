---
title: 升级 Python 和 R 运行时（绑定）
description: 使用 sqlbindr.exe 绑定到 Machine Learning Server，以升级 SQL Server 机器学习服务或 SQL Server R Services 中的 Python 和 R 运行时。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 7cdf5b2d0c223130245604f5b27674a714ab4d8a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082348"
---
# <a name="upgrade-python-and-r-runtime-with-binding-in-sql-server-machine-learning-services"></a>使用 SQL Server 机器学习服务中的绑定升级 Python 和 R 运行时
[!INCLUDE [SQL Server 2016 and 2017](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

本文介绍如何使用称为“绑定”的 am 安装过程来升级 [SQL Server 2016 R Services](../r/sql-server-r-services.md) 或 [SQL Server 2017 机器学习服务](../sql-server-machine-learning-services.md)中的 R 或 Python 运行时。 可以通过绑定  到 Microsoft Machine Learning Server  来获取[更高版本的 Python 和 R](#version-map)。

> [!IMPORTANT]
> 本文介绍了一种用于升级 R 和 Python 运行时的旧方法，称为“绑定”。 如果已安装 **SQL Server 2016 Services Pack (SP) 2 的累积更新 (CU) 14 或更高版本** 或 **SQL Server 2017 的累积更新 (CU) 22 或更高版本**，请改为参阅如何 [将默认 R 或 Python 语言运行时更改为更高版本](change-default-language-runtime-version.md)。

## <a name="what-is-binding"></a>什么是绑定？

绑定是一个安装过程，它将 R_SERVICES 和 PYTHON_SERVICES 文件夹中的内容替换为 [Microsoft Machine Learning Server](/machine-learning-server) 中较新的可执行文件、库和工具。

服务模式随附的已上传组件已更改。 服务更新与[新式生命周期](https://support.microsoft.com/help/30881/modern-lifecycle-policy)中的 [Microsoft R Server 和 Machine Learning Server 的支持时间线](/machine-learning-server/resources-servicing-support)保持一致。

除了组件版本和服务更新之外，绑定不会改变安装的基础：

- Python 和 R 集成仍是数据库引擎实例的一部分。
- 授权不变（没有与绑定相关的额外费用）。
- SQL Server 支持策略仍适用于数据库引擎。

本文的其余部分将介绍绑定机制以及它如何用于 SQL Server 的每个版本。

> [!NOTE]
> 绑定只适用于绑定到 SQL Server 实例的数据库内实例。 在这种情况下，独立安装不一定需要绑定。

::: moniker range="=sql-server-2016"
**SQL Server 2016 绑定注意事项**

对于 SQL Server 2016 R Services 客户，绑定提供：

- 更新后的 R 包。
- 不属于原始安装的新包 ([MicrosoftML](/machine-learning-server/r-reference/microsoftml/microsoftml-package))
- 预先训练的机器学习[模型](/machine-learning-server/install/microsoftml-install-pretrained-models)，用于情绪分析和图像检测。

每次 [Microsoft Machine Learning Server](/machine-learning-server/index) 有新的主要和次要发布时，都会进一步刷新所有绑定。
::: moniker-end

## <a name="version-map"></a>版本映射

下表是版本映射。 每个映射都显示跨发布的包版本。 如果绑定到 Microsoft Machine Learning Server（在自 Machine Learning Server 9.2.1 起添加了 Python 支持之前的旧称为“R Server”），可以审阅升级路径。

绑定不保证 R 或 Anaconda 是最新版。 如果你绑定到 Microsoft Machine Learning Server，R 或 Python 版本通过安装程序进行安装，这可能不是 Web 上的最新版本。

::: moniker range="=sql-server-2016"
[**SQL Server 2016 R Services**](../install/sql-r-services-windows-install.md)

组件 |初始版本 | [R Server 9.0.1](/machine-learning-server/install/r-server-install-windows) | [R Server 9.1](/machine-learning-server/install/r-server-install-windows) | [Machine Learning Server 9.2.1](/machine-learning-server/install/machine-learning-server-windows-install) | [Machine Learning Server 9.3](/machine-learning-server/install/machine-learning-server-windows-install) |  [Machine Learning Server 9.4.7](/machine-learning-server/install/machine-learning-server-windows-install)
----------|----------------|----------------|--------------|---------|-------|-------|
R 上的 Microsoft R Open (MRO) | R 3.2.2     | R 3.3.2   |R 3.3.3   | R 3.4.1  | R 3.4.3 | R 3.5.2
[RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler) | 8.0.3  | 9.0.1 |  9.1 |  9.2.1 |  9.3 |  9.4.7 |
[MicrosoftML](/machine-learning-server/r-reference/microsoftml/microsoftml-package)| n.a. | 9.0.1 |  9.1 |  9.2.1 |  9.3 | 9.4.7 |
[预定型模型](/machine-learning-server/install/microsoftml-install-pretrained-models)| n.a. | 9.0.1 |  9.1 |  9.2.1 |  9.3 | 9.4.7 |
[sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils)| n.a. | 1.0 |  1.0 |  1.0 |  1.0 | 1.0 |
[olapR](/machine-learning-server/r-reference/olapr/olapr) | n.a. | 1.0 |  1.0 |  1.0 |  1.0 | 1.0 |
::: moniker-end

::: moniker range="=sql-server-2017"
[**SQL Server 2017 机器学习服务**](../install/sql-machine-learning-services-windows-install.md)

组件 |初始版本 | Machine Learning Server 9.3 | Machine Learning Server 9.4.7 |
----------|----------------|---------|---------|
R 上的 Microsoft R Open (MRO) | R 3.3.3 | R 3.4.3 | R 3.5.2 |
[RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler) |   9.2 |  9.3 | 9.4.7 |
[MicrosoftML](/machine-learning-server/r-reference/microsoftml/microsoftml-package) | 9.2  | 9.3| 9.4.7 |
[sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils)| 1.0 |  1.0 | 1.0 |
[olapR](/machine-learning-server/r-reference/olapr/olapr) | 1.0 |  1.0 | 1.0 |
Python 3.5 上的 Anaconda 4.2  | 4.2/3.5.2 | 4.2/3.5.2 |
[revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) | 9.2  | 9.3| 9.4.7 |
[microsoftml](/machine-learning-server/python-reference/microsoftml/microsoftml-package) | 9.2  | 9.3| 9.4.7 |
[预定型模型](/machine-learning-server/install/microsoftml-install-pretrained-models) | 9.2 | 9.3| 9.4.7 |
::: moniker-end

## <a name="how-component-upgrade-works"></a>组件升级的工作原理

如果你将现有 Python 和 R 安装绑定到 Machine Learning Server，可执行文件、Python 和 R 库会升级。

如果你在包含 Python 或 R 集成的现有 SQL Server 数据库引擎实例上运行安装程序，绑定由 [Microsoft Machine Learning Server 安装程序](/machine-learning-server/install/machine-learning-server-windows-install)执行。 

安装程序会检测现有功能并提示你重新绑定到 Machine Learning Server。

在绑定期间，`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES` 和 `\PYTHON_SERVICES` 的内容被 `C:\Program Files\Microsoft\ML Server\R_SERVER` 和 `\PYTHON_SERVER` 的较新可执行文件和库覆盖。

绑定只适用于 Python 和 R 功能。 用于 Python 和 R 的开放源代码包包括：

- Anaconda
- Microsoft R Open
- 专有包 RevoScaleR
- Revoscalepy

绑定不会更改数据库引擎实例的支持模型或 SQL Server 的版本。

绑定是可逆操作。 通过[取消绑定实例](#bkmk_Unbind)并修复 SQL Server 数据库引擎实例，可以还原为 SQL Server 服务。

<a name="bkmk_BindWizard"></a>

## <a name="bind-to-machine-learning-server-using-setup"></a>使用安装程序绑定到 Machine Learning Server

若要使用安装程序将 SQL Server 绑定到 Microsoft Machine Learning Server，请按照步骤操作。 

1. 在 SSMS 中，运行 `SELECT @@version` 以验证服务器是否满足最低版本要求。

   对于 SQL Server 2016 R Services，最低版本为 [Service Pack 1](https://www.microsoft.com/download/details.aspx?id=54276) 和 [CU3](https://support.microsoft.com/help/4019916/cumulative-update-3-for-sql-server-2016-sp1)。

1. 检查 R 基础版本和 ReVoSalcER 包版本，以确认现有版本低于计划替换的版本。 

    ```sql
    EXECUTE sp_execute_external_script
    @language=N'R'
    ,@script = N'str(OutputDataSet);
    packagematrix <- installed.packages();
    Name <- packagematrix[,1];
    Version <- packagematrix[,3];
    OutputDataSet <- data.frame(Name, Version);'
    , @input_data_1 = N''
    WITH RESULT SETS ((PackageName nvarchar(250), PackageVersion nvarchar(max) ))
    ```

1. 关闭 SSMS 和任何与 SQL Server 具有开放连接的其他工具。 绑定将覆盖程序文件。 如果 SQL Server 有打开的会话，则绑定将失败，显示绑定错误代码 6。

1. 将 Microsoft Machine Learning Server 下载到包含要升级的实例的计算机上。 我们推荐[最新版本](/machine-learning-server/install/machine-learning-server-windows-install#download-machine-learning-server-installer)。

1. 解压缩文件夹并启动位于 MLSWIN93 下的 ServerSetup.exe。

1. 在“配置安装”上，确认要升级的组件，并查看兼容实例的列表  。

1. 在“许可协议”页面上，选择“我接受这些条款”以接受 Machine Learning Server 的许可条款   。 

1. 在后续页面中，同意所选的任何开源组件（如 Microsoft R open 或 Python Anaconda 分发版）的其他许可条件。

1. 在“即将完成”页面上，记下安装文件夹  。 默认文件夹为 \Program Files\Microsoft\ML Server。

    如果要更改安装文件夹，请选择“高级”以返回向导首页。 但是，必须重复选择前面的所有选择内容。

如果升级失败，请检查 [SqlBindR 错误代码](#sqlbindr-error-codes)以获取详细信息。

## <a name="offline-binding-no-internet-access"></a>脱机绑定（无 internet 访问）

对于没有 Internet 连接的系统，可以将安装程序和 .cab 文件下载到已连接 Internet 的计算机上，然后将文件传输到独立服务器。

安装程序 (ServerSetup.exe) 包括 Microsoft 包（RevoScaleR、MicrosoftML、olapR、sqlRUtils）。 .cab 文件提供其他核心组件。 例如，“SRO”cab 提供了 R Open，这是 Microsoft 的开放源代码 R 分发版。

以下说明解释如何放置文件以进行脱机安装。

1. 下载 MLSWIN93 安装程序。 它以单个压缩文件的形式下载。 我们推荐[最新版本](/machine-learning-server/install/machine-learning-server-windows-install#download-machine-learning-server-installer)，但也可以安装[早期版本](/machine-learning-server/install/r-server-install-windows-offline#download-required-components)。

1. 下载 .cab 文件。 以下链接适用于 9.3 版本。 如果需要早期版本，可以在 [R Server 9.1](/machine-learning-server/install/r-server-install-windows-offline#download-required-components) 中找到其他链接。 请记住，Python/Anaconda 只能添加到 SQL Server 机器学习服务实例中。 对于 Python 和 R，都有预先训练的模型；.cab 提供你正在使用的语言的模型。

    | Feature | 下载 |
    |---------|----------|
    | R       | [SRO_3.4.3.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=867186&clcid=1033) |
    | Python  | [SPO_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859054) | 
    | 预定型模型 | [MLM_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859053) |

1. 将 .zip 和 .cab 文件传输到目标服务器。

1. 在服务器上，在 Run 命令中键入 `%temp%` 以获取临时目录的物理位置。 物理路径因计算机而异，但通常是 `C:\Users\<your-user-name>\AppData\Local\Temp`。

1. 将 .cab 文件放在 %temp% 文件夹中。

1. 解压缩安装程序。

1. 运行 ServerSetup.exe 并按照屏幕提示完成安装。

## <a name="command-line-operations"></a><a name="bkmk_BindCmd"></a>命令行操作


> [!TIP]
> 找不到 SqlBindR？ 可能尚未运行安装程序。
> 只有在运行 Machine Learning Server 安装程序之后，SqlBindR 才可用。

1. 以管理员身份打开命令提示符并导航到包含 sqlbindr.exe 的文件夹。 默认位置为 C:\Program Files\Microsoft\MLServer\Setup

2. 键入以下命令，查看可用实例列表： `SqlBindR.exe /list`
  
   记下所列出的完整实例名称。 例如，对于默认实例，实例名称可能是 MSSQL14.MSSQLSERVER，或者类似于 SERVERNAME.MYNAMEDINSTANCE。

3. 使用 /bind  参数运行 SqlBindR.exe  命令。 使用上一步中返回的实例名称，指定要升级的实例的名称。

   例如，若要升级默认实例，请键入：`SqlBindR.exe /bind MSSQL14.MSSQLSERVER`

4. 在升级完成后，重启与已修改的任何实例关联的启动板服务。

## <a name="revert-or-unbind-an-instance"></a><a name="bkmk_Unbind"></a>还原或取消绑定实例

可以将绑定实例还原为 Python 和 R 组件的初始安装（由 SQL Server 安装程序建立）。 要还原到 SQL Server 服务，需执行三个步骤。

+ [步骤 1：从 Microsoft Machine Learning Server 取消绑定](#step-1-unbind)
+ [步骤 2：将实例还原为原始状态](#step-2-restore)
+ [步骤 3：重新安装添加到安装的所有包](#step-3-reinstall-packages)

<a name="step-1-unbind"></a> 

### <a name="step-1-unbind"></a>步骤 1：取消绑定

回滚绑定有以下两种方法：再次重新运行安装程序，或使用 SqlBindR 命令行实用工具。

#### <a name="unbind-using-setup"></a><a name="bkmk_wizunbind"></a> 使用安装程序取消绑定

1. 找到 Machine Learning Server 的安装程序。 如果已删除安装程序，可能需要重新下载它，或从另一台计算机复制它。
2. 请确保在包含要取消绑定的实例的计算机上运行安装程序。
2. 安装程序将标识要取消绑定的本地实例。
3. 取消选中要还原为原始配置的实例旁边的复选框。
4. 接受所有许可协议。
5. 选择“完成”  。 此过程需要花费一点时间。

#### <a name="unbind-using-the-command-line"></a><a name="bkmk_cmdunbind"></a> 使用命令行取消绑定

1. 如上节所述，打开命令提示符并导航到包含 **sqlbindr.exe** 的文件夹。

2. 在使用 */unbind* 参数的情况下运行 **SqlBindR.exe** 命令，并指定实例。

   例如，以下命令将还原默认实例：
   
    `SqlBindR.exe /unbind MSSQL14.MSSQLSERVER`

<a name="step-2-restore"></a> 

###  <a name="step-2-repair-the-sql-server-instance"></a>步骤 2：修复 SQL Server 实例

运行 SQL Server 安装程序，以修复包含 Python 和 R 功能的数据库引擎实例。 预先存在的更新会被保留。 如果 Python 和 R 包的服务更新缺少更新，则执行下一步。

备用解决方案：完全卸载并重新安装数据库引擎实例，然后应用所有服务更新。

<a name="step-3-reinstall-packages"></a> 

### <a name="step-3-add-any-third-party-packages"></a>步骤 3：添加任何第三方包

你可能已将其他开放源代码或第三方包添加到包库中。 由于反向绑定会切换默认包库的位置，因此必须将包重新安装到 Python 和 R 正在使用的库中。 有关详细信息，请参阅 [R 包信息](../package-management/r-package-information.md)和[安装](../package-management/install-additional-r-packages-on-sql-server.md)，以及 [Python 包信息](../package-management/python-package-information.md)和[安装](../package-management/install-additional-python-packages-on-sql-server.md)。

## <a name="sqlbindrexe-command-syntax"></a>SqlBindR.exe 命令语法

### <a name="usage"></a>使用情况

`sqlbindr [/list] [/bind <SQL_instance_ID>] [/unbind <SQL_instance_ID>]`

### <a name="parameters"></a>参数

|名称|说明|
|------|------|
|*list*| 显示当前计算机上所有 SQL Server 实例 ID 的列表|
|*bind*| 将指定 SQL Server 实例升级到最新版 R Server，并确保实例会自动获取 R Server 的未来升级|
|*unbind*|从指定 SQL Server 实例中卸载最新版 R Server，并防止未来 R Server 升级影响此实例|

<a name="sqlbindr-error-codes"><a/>

## <a name="binding-errors"></a>绑定错误

Machine Learning Server 安装程序和 SqlBindR 都返回以下错误代码和消息。

|错误代码  | 消息           | 详细信息               |
|------------|-------------------|-----------------------|
|绑定错误 0 | 确定（成功） | 绑定已传递，未出错。 |
|绑定错误 1 | 参数无效 | 语法错误。 |
|绑定错误 2 | 操作无效 | 语法错误。 |
|绑定错误 3 | 无效实例 | 实例存在，但对于绑定无效。 |
|绑定错误 4 | 不可绑定 | |
|绑定错误 5 | 已绑定 | 已运行 *bind* 命令，但指定的实例已绑定。 |
|绑定错误 6 | 绑定失败 | 取消绑定实例时出错。 如果在未选择任何功能的情况下运行 Machine Learning Server 安装程序，可能会看到此错误消息。 绑定要求同时选择 MSSQL 实例、Python 和 R（假设实例为 SQL Server 2017）。 如果 SqlBindR 无法写入到“程序文件”文件夹，也会发生此错误。 打开 SQL Server 的会话或句柄将导致发生此错误。 如果出现此错误，请重启计算机并在启动任何新会话之前重新执行绑定步骤。|
|绑定错误 7 | 未绑定 | 数据库引擎实例具有 R 服务或 SQL Server 机器学习服务。 实例未绑定到 Microsoft Machine Learning Server。 |
|绑定错误 8 | 取消绑定失败 | 取消绑定实例时出错。 |
|绑定错误 9 | 未找到任何实例 | 在此计算机上找不到数据库引擎实例。 |

## <a name="known-issues"></a>已知问题

本部分列出特定于使用 SqlBindR.exe 实用工具或升级可能影响 SQL Server 实例的 Machine Learning Server 的已知问题。

### <a name="restoring-packages-that-were-previously-installed"></a>还原以前安装的包

升级到 Microsoft R Server 9.0.1 后，SqlBindR.exe 无法还原原始包或 R 组件。 对实例使用 SQL Server 修复，并应用所有服务发布。 重启实例。

更高版本的 SqlBindR 会自动还原原始 R 功能，无需重新安装 R 组件或重新修补服务器。 但是，必须安装在初始安装之后可能添加的任何 R 包更新。

使用 R 命令和数据库中的记录将已安装的包同步到文件系统。 有关详细信息，请参阅 [SQL Server 的 R 包管理](../package-management/install-additional-r-packages-on-sql-server.md)。

### <a name="problems-with-overwritten-sqlbinrini-file-in-sql-server"></a>SQL Server 中覆盖的 sqlbinr 文件出现问题

方案：将 Machine Learning Server 9.4.7 绑定到 SQL Server 2017 时出现此问题。  当 Python 经过更新和绑定时，或者当你更新到新的 CU 时，它不了解 Python 已绑定并且会覆盖文件。 R 没有已知问题。

一种解决方法是在 PYTHON_SERVICES 目录中创建不为空的 `sqlbindr.ini` 文件。 内容不会影响文件的工作方式。

创建 `sqlbindr.ini` 文件（其中包含 9.4.7.82），保存到此位置：  

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES`

### <a name="problems-with-multiple-upgrades-from-sql-server"></a>从 SQL Server 进行多次升级时出现的问题

方案：先前已将 SQL Server 2016 R Services 的实例升级到 9.0.1。 执行了 Microsoft R Server 9.1.0 的新安装程序。 此安装程序列出了全部有效实例。
默认情况下，安装程序选择以前绑定的实例。 如果继续操作，将取消绑定以前绑定的实例。 因此，先前的 9.0.1 安装以及任何相关包会遭删除，但不会安装新版 Microsoft R Server (9.1.0)。

作为解决方案，可以修改现有的 R Server 安装，如下所示：
1. 在“控制面板”中，打开“添加或删除程序”  。
2. 找到 Microsoft R Server，然后选择“更改/修改”。
3. 当安装程序启动时，选择要绑定到 9.1.0 的实例。

Microsoft Machine Learning Server 9.2.1 和 9.3 没有此问题。

### <a name="binding-or-unbinding-leaves-multiple-temporary-folders"></a>绑定或取消绑定会保留多个临时文件夹

安装完成后，删除临时文件夹。

> [!NOTE]
> 请确保等待安装完成。 删除与某个版本关联的 R 库，然后添加新的 R 库可能需要很长时间。 操作完成后，将删除临时文件夹。

## <a name="see-also"></a>另请参阅

+ [更改默认 R 或 Python 语言运行时版本](./change-default-language-runtime-version.md)
+ [安装适用于 Windows 的 Machine Learning Server（已连接 Internet）](/machine-learning-server/install/machine-learning-server-windows-install)
+ [安装适用于 Windows 的 Machine Learning Server（脱机）](/machine-learning-server/install/machine-learning-server-windows-offline)
+ [Machine Learning Server 中的已知问题](/machine-learning-server/resources-known-issues)
+ [R Server 早期版本的功能公告](/r-server/whats-new-in-r-server)
+ [已弃用、不再支持或已更改的功能](/machine-learning-server/resources-deprecated-features)