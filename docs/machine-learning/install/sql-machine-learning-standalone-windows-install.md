---
title: 安装 Machine Learning Server（独立版）
description: 为 Python 和 R 设置一个独立的机器学习服务器。由 SQL Server 安装程序安装的独立服务器在功能上等同于 Microsoft Machine Learning Server 的非 SQL 品牌版本。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/03/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: d446416076160642f86c035082481d318479d594
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471178"
---
# <a name="install-machine-learning-server-standalone-or-r-server-standalone-using-sql-server-setup"></a>使用 SQL Server 安装程序安装 Machine Learning Server（独立版）或 R Server（独立版）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

::: moniker range=">=sql-server-2017"
SQL Server 安装程序包含一个“共享功能”  选项，用于安装在 SQL Server 外部运行的独立机器学习服务器。 它称为 Machine Learning Server（独立版）  ，包括 Python 和 R。 
::: moniker-end
::: moniker range="=sql-server-2016"
SQL Server 安装程序包含一个“共享功能”  选项，用于安装在 SQL Server 外部运行的独立机器学习服务器。 在 SQL Server 2016 中，此功能称为 **R Server（独立版）** 。  
::: moniker-end

由 SQL Server 安装程序安装的独立服务器在功能上等同于 [Microsoft Machine Learning Server](/machine-learning-server/what-is-machine-learning-server) 的非 SQL 品牌版本，并且支持相同的用例和应用场景，其中包括：

+ 远程执行，可在同一控制台中在本地会话与远程会话之间进行切换
+ 通过 Web 节点和计算节点实现操作化
+ Web 服务部署：能够将 R 和 Python 脚本打包到 Web 服务中
+ 完整的 R 和 Python 函数库集合

作为与 SQL Server 分离的独立服务器，其 R 和 Python 环境是使用独立服务器（而非 SQL Server）中提供的基础操作系统和工具进行配置、保护和访问的。

若要开发可以将远程计算上下文用于所有受支持数据平台的高性能机器学习解决方案，作为 SQL Server 附属服务器的独立服务器将非常有用。 可以将执行任务从本地服务器转移到 Spark 群集或另一个 SQL Server 实例上的远程 Machine Learning Server。

<a name="bkmk_prereqs"> </a>

## <a name="pre-install-checklist"></a>安装前清单

如果安装了以前的版本，例如 SQL Server 2016 R Server（独立版）或 Microsoft R Server，请先卸载现有安装，然后再继续操作。

通常，我们建议将独立服务器和识别数据库引擎实例的安装视为互斥项，以免出现资源争用情况，但如果有足够的资源，则不禁止将它们安装在同一台物理计算机上。

计算机上只能有一台独立服务器：SQL Server Machine Learning Server（独立版）或 SQL Server R Server（独立版）。 在添加新版本之前，请确保先卸载以前的版本。

::: moniker range="=sql-server-2016"
<a name="bkmk_ga_instalpatch"></a> 

 ###  <a name="install-patch-requirement"></a>安装修补程序要求 

仅适用于 SQL Server 2016：Microsoft 已发现特定版本的 Microsoft VC++ 2013 运行时二进制文件存在问题，这些二进制文件是作为 SQL Server 系统必备进行安装的。 如果未安装适用于该 VC 运行时二进制文件的更新，则在某些情况下 SQL Server 可能会遇到稳定性问题。 在安装 SQL Server 之前，请按照 [SQL Server 发行说明](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch)中的说明来确定你的电脑是否需要 VC 运行时二进制文件的修补程序。  
::: moniker-end

## <a name="get-the-installation-media"></a>获取安装介质

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

::: moniker range=">=sql-server-2017"
## <a name="run-setup"></a>运行安装程序

对于本地安装，必须以管理员身份运行安装程序。 如果从远程共享安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则必须使用对远程共享具有读取和执行权限的域帐户。

1. 启动安装向导。

2. 单击“安装”选项卡，选择“安装新 Machine Learning Server (独立版)”   。
    
   ::: moniker-end
   ::: moniker range="=sql-server-2017"
   ![安装 Machine Learning Server 独立版](media/2017setup-installation-page-mlsvr.png "开始安装 Machine Learning Server 独立版")
   ::: moniker-end
   ::: moniker range="=sql-server-ver15"
   ![安装 Machine Learning Server 独立版](media/2019setup-installation-page-mlsvr.png "开始安装 Machine Learning Server 独立版")
   ::: moniker-end
   ::: moniker range=">=sql-server-2017"

3. 完成规则检查后，接受 SQL Server 许可条款，并选择新的安装。

4. 在“功能选择”页上，以下选项应该已处于选中状态  ：

    - **Microsoft Machine Learning Server（独立版）**

    - “R”和“Python”默认情况下均处于选中状态   。 你可以取消选择任一语言，但我们建议你至少安装一种受支持的语言。

   ::: moniker-end
   ::: moniker range="=sql-server-2017"
   ![选择 R 或 Python 功能](media/2017setup-features-page-mlsvr-rpy.png "开始安装 Machine Learning Server 独立版")
   ::: moniker-end
   ::: moniker range="=sql-server-ver15"
   ![选择 R 或 Python 功能](media/2019setup-features-page-mlsvr-rpy.png "开始安装 Machine Learning Server 独立版")
   ::: moniker-end
   ::: moniker range=">=sql-server-2017"
    
   应忽略所有其他选项。 
    
   > [!NOTE]
   > 如果计算机已经为 SQL Server 数据库内分析安装了机器学习服务，请避免安装“共享功能”  。 这会创建重复的库。
   > 
   > 此外，在 SQL Server 中运行的 R 或 Python 脚本由 SQL Server 进行管理，因此不会与其他数据库引擎服务使用的内存发生冲突，而独立的 Machine Learning Server 没有此类约束，因此可能会干扰其他数据库操作。 最后，数据库管理员通常会阻止通过 RDP 会话进行远程访问（通常用于操作化）。
   > 
   > 鉴于以上原因，我们一般建议你将 Machine Learning Server（独立版）与 SQL Server 机器学习服务分别安装在不同的计算机上。

5. 接受下载和安装基本语言发行版的许可条款。 当“接受”  按钮变为不可用时，可以单击“下一步”  。 

6. 在“准备安装”  页上，验证选择，并单击“安装”  。
::: moniker-end

::: moniker range="=sql-server-2016"
## <a name="run-setup"></a>运行安装程序

对于本地安装，必须以管理员身份运行安装程序。 如果从远程共享安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则必须使用对远程共享具有读取和执行权限的域帐户。

1. 启动安装向导。

2. 在“安装”选项卡上，单击“安装新的 R Server (独立版)”   。
    
   ![开始安装 R Server 独立版](media/2016-setup-installation-rsvr.png "开始安装 R Server 独立版")

3. 完成规则检查后，接受 SQL Server 许可条款，并选择新的安装。

4. 在“功能选择”  页上，应已选择以下选项：
    
   - **R Server（独立版）**  
    
   ![R Server 独立版的功能选择](media/2016setup-rserver-features.png "R Server 独立版的功能选择")
    
   应忽略所有其他选项。 
    
   > [!NOTE]
   > 如果在已安装 R Services 进行 SQL Server 数据库内分析的计算机上运行安装程序，请避免安装“共享功能”  。 这会创建重复的库。
   > 
   > 在 SQL Server 中运行的 R 脚本由 SQL Server 进行管理，因此不会与其他数据库引擎服务使用的内存发生冲突，而独立的 R Server 没有此类约束，因此可能会干扰其他数据库操作。
   > 
   > 我们通常建议将 R Server（独立版）与 SQL Server R Services（数据库内）分别安装在不同的计算机上。

5. 接受下载和安装基本语言发行版的许可条款。 当“接受”  按钮变为不可用时，可以单击“下一步”  。 

6. 在“准备安装”  页上，验证选择，并单击“安装”  。
::: moniker-end

## <a name="set-environment-variables"></a>设置环境变量

（仅适用于 R 功能集成）应设置“MKL_CBWR”环境变量，以确保从 Intel 数学核心函数库 (MKL) 计算得到[一致的输出结果](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr)  。

1. 在“控制面板”中，单击“系统和安全” > “系统” > “高级系统设置” > “环境变量”     。

2. 创建新的用户或系统变量。 

  + 将变量名称设置为 `MKL_CBWR`
  + 将变量值设置为 `AUTO`

3. 重新启动服务器。

<a name="install-path"></a>

### <a name="default-installation-folders"></a>默认安装文件夹

进行 R 和 Python 开发时，同一台计算机上通常有多个版本。 由 SQL Server 安装程序安装时，基本发行版安装在与用于安装的 SQL Server 版本关联的文件夹中。

下表列出了由 Microsoft 安装程序创建的 R 和 Python 发行版的路径。 为完整起见，该表包括由 SQL Server 安装程序以及 Microsoft Machine Learning Server 的独立安装程序生成的路径。

|版本| 安装方法 | 默认文件夹|
|----|----|----|
|SQL Server 2019 Machine Learning Server（独立版） |  SQL Server 2019 安装向导 |`C:\Program Files\Microsoft SQL Server\150\R_SERVER` <br/>`C:\Program Files\Microsoft SQL Server\150\PYTHON_SERVER`|
|SQL Server 2017 Machine Learning Server（独立版） |  SQL Server 2017 安装向导 |`C:\Program Files\Microsoft SQL Server\140\R_SERVER` <br/>`C:\Program Files\Microsoft SQL Server\140\PYTHON_SERVER`|
|Microsoft Machine Learning Server（独立版） |  Windows 独立安装程序 |`C:\Program Files\Microsoft\ML Server\R_SERVER`<br/>`C:\Program Files\Microsoft\ML Server\PYTHON_SERVER`|
|SQL Server 机器学习服务（数据库内） |SQL Server 2019 安装向导，带 R 语言选项|`C:\Program Files\Microsoft SQL Server\MSSQL15.<instance_name>\R_SERVICES`  <br/>`C:\Program Files\Microsoft SQL Server\MSSQL15.<instance_name>\PYTHON_SERVICES` |
|SQL Server 机器学习服务（数据库内） |SQL Server 2017 安装向导，带 R 语言选项|`C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\R_SERVICES`  <br/>`C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\PYTHON_SERVICES` |
|SQL Server 2016 R Server（独立版） |  SQL Server 2016 安装向导 |`C:\Program Files\Microsoft SQL Server\130\R_SERVER`|
|SQL Server 2016 R Services（数据库内） |SQL Server 2016 安装向导|`C:\Program Files\Microsoft SQL Server\MSSQL13.<instance_name>\R_SERVICES`|

<a name="apply-cu"></a>

## <a name="apply-updates"></a>应用更新

建议将最新的累积更新应用于数据库引擎和机器学习组件。 累积更新通过安装程序进行安装。 

在连接 Internet 的设备上，可以下载一个自解压缩的可执行文件。 为数据库引擎应用更新会自动为现有 R 和 Python 功能引入累积更新。 

在断开连接的服务器上，需要执行额外的步骤。 你必须获取数据库引擎的累积更新以及机器学习功能的 CAB 文件。 必须将所有文件传输到被隔离的服务器并手动应用。

1. 从基线实例开始。 只能将累积更新应用于现有安装：

  + SQL Server 2019 初始版本中的 Machine Learning Server（独立版）
  + SQL Server 2017 初始版本中的 Machine Learning Server（独立版）
  + SQL Server 2016 初始版本、SQL Server 2016 SP 1 或 SQL Server 2016 SP 2 中的 R Server（独立版）

2. 关闭已打开的所有 R 或 Python 会话，并停止系统上仍在运行的所有进程。

3. 如果为 Web 服务部署启用了操作化以作为 Web 节点和计算节点运行，请备份 **AppSettings.json** 文件，以防万一。 应用 SQL Server 2017 CU13 或更高版本会修改此文件，因此你可能需要一个备份副本来保留原始版本。

4. 在连接 Internet 的计算机上，从 [Microsoft SQL Server 的最新更新](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)下载你的版本的最新累积更新。

5. 下载最新的累积更新。 它是一个可执行文件。

6. 在连接 Internet 的设备上，双击 .exe 运行安装程序，逐步完成向导以接受许可条款，查看受影响的功能，并监视进度直到完成。

7. 在未连接 Internet 的服务器上：

   + 获取 R 和 Python 的相应 CAB 文件。 有关下载链接，请参阅 [SQL Server 数据库内分析实例上的累积更新的 CAB 下载](sql-ml-cab-downloads.md)。

   + 将所有文件（主要可执行文件和 CAB 文件）传输到脱机计算机上的文件夹中。

   + 双击 .exe 运行安装程序。 在未连接 Internet 的服务器上安装累积更新时，系统会提示你选择 R 和 Python 的 .cab 文件的位置。

8. 安装后，在通过 Web 节点和计算节点实现了部署的服务器上，编辑 AppSettings.json  （直接在“MMLNativePath”下添加“MMLResourcePath”条目）。 例如：

    ```json
    "ScorerParameters": {
        "MMLNativePath": "C:\Program Files\Microsoft SQL Server\140\R_SERVER\library\MicrosoftML\mxLibs\x64\",
        "MMLResourcePath": "C:\Program Files\Microsoft SQL Server\140\R_SERVER\library\MicrosoftML\mxLibs\x64\"
    }
    ```

9. [运行管理 CLI 实用工具](/machine-learning-server/operationalize/configure-admin-cli-launch)以重新启动 Web 节点和计算节点。 有关步骤和语法，请参阅[监视、启动和停止 Web 节点与计算节点](/machine-learning-server/operationalize/configure-admin-cli-stop-start)。

## <a name="development-tools"></a>开发工具

安装过程中不会安装开发 IDE。 有关配置开发环境的详细信息，请参阅[设置 R 工具](../r/set-up-a-data-science-client.md)和[设置 Python 工具](../python/setup-python-client-tools-sql.md)。

## <a name="next-steps"></a>后续步骤

R 开发人员可以开始使用一些简单的示例，并了解 R 如何与 SQL Server 协同工作的基础知识。 有关下一步，请参阅以下链接：

+ [快速入门：在 T-SQL 中运行 R](../tutorials/quickstart-r-create-script.md)
+ [教程：适用于 R 开发人员的数据库内分析](../tutorials/r-taxi-classification-introduction.md)

::: moniker range=">=sql-server-2017"
Python 开发人员可以通过以下教程了解如何将 Python 与 SQL Server 一起使用：

+ [Python 教程：在 SQL Server 机器学习服务中使用线性回归来预测雪橇租赁](../tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Python 教程：配合使用 K-Means 群集和 SQL Server 机器学习服务对客户进行分类](../tutorials/python-clustering-model.md)
::: moniker-end