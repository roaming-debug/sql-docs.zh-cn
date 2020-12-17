---
title: 脱机安装（无 Internet 访问）
description: 在隔离在网络防火墙之后的计算机上，脱机安装具有 R 和 Python 的 SQL Server 机器学习服务。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/01/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 5190cc4e8525daf79354005132705ae1a27b32a3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471108"
---
# <a name="offline-install-sql-server-machine-learning-services-python-and-r-on-computers-with-no-internet-access"></a>在没有 Internet 访问的情况下，在计算机上安装 SQL Server 机器学习服务 Python 和 R
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文介绍如何在隔离在网络防火墙之后且无 internet 访问的的计算机上，脱机安装 SQL Server 机器学习服务。

默认情况下，安装程序会连接到 Microsoft 下载站点以获取在 SQL Server 上进行机器学习所需的组件和更新的组件。 如果防火墙约束阻止安装程序访问这些站点，你可以使用连接到 Internet 的设备下载文件、将文件传输到脱机服务器，然后进行安装设置。

数据库内分析包含数据库引擎实例以及适用于 R 和 Python 集成的其他组件，具体因 SQL Server 的版本而异。 

+ SQL Server 2019 包括 R、Python 和 Java
+ SQL Server 2017 包括 R 和 Python
+ SQL Server 2016 仅适用于 R

在独立服务器上，已通过 CAB 文件添加了机器学习和 R/Python 特定语言功能。 

::: moniker range="=sql-server-ver15"
## <a name="sql-server-2019-offline-install"></a>SQL Server 2019 脱机安装

若要在独立服务器上安装 SQL Server 机器学习服务（R 和 Python），首先下载 SQL Server 的初始版本以及用于支持 R 和 Python 的相应 CAB 文件。 即使你计划立即更新服务器以使用最新的累积更新，也必须先安装初始版本。

> [!Note]
> SQL Server 2019 没有服务包。 初始版本是唯一的基线，仅通过累积更新提供服务。

### <a name="1---download-2019-cabs"></a>1 - 下载 2019 CAB

在连接到 Internet 的计算机上，下载提供 R 和 Python 功能的 CAB 文件以获取 SQL Server 2019 的初始版本和安装介质。

发布  |下载链接  |
---------|---------------|
Microsoft R Open        | [SRO_3.5.2.125_1033.cab](https://go.microsoft.com/fwlink/?linkid=2085686) |
Microsoft R Server      | [SRS_9.4.7.25_1033.cab](https://go.microsoft.com/fwlink/?linkid=2085792) |
Microsoft Python Open   | [SPO_4.5.12.120_1033.cab](https://go.microsoft.com/fwlink/?linkid=2085793) |
Microsoft Python Server | [SPS_9.4.7.25_1033.cab](https://go.microsoft.com/fwlink/?linkid=2085685) |

> [!NOTE]
> Java 功能包含在 SQL Server 安装介质中，不需要单独的 CAB 文件。

###  <a name="2---get-sql-server-2019-installation-media"></a>2 - 获取 SQL Server 2019 安装介质

1. 在连接到 Internet 的计算机上，下载 [SQL Server 2019 安装程序](https://www.microsoft.com/sql-server/sql-server-downloads)。 

2. 双击“安装”，然后选择“下载介质”安装类型  。 通过使用此选项，安装程序将创建包含安装介质的本地 .iso（或 .cab）文件。

   ![选择“下载介质”安装类型](media/2019offline-download-tile.png "下载介质")

::: moniker-end

::: moniker range="=sql-server-2017"
## <a name="sql-server-2017-offline-install"></a>SQL Server 2017 脱机安装

若要在独立服务器上安装 SQL Server 机器学习服务（R 和 Python），首先下载 SQL Server 的初始版本以及用于支持 R 和 Python 的相应 CAB 文件。 即使你计划立即更新服务器以使用最新的累积更新，也必须先安装初始版本。

> [!Note]
> SQL Server 2017 没有服务包。 首次发布的 SQL Server 将初始版本用作唯一的基线，仅通过累积更新提供服务。 

### <a name="1---download-2017-cabs"></a>1 - 下载 2017 CAB

在连接到 Internet 的计算机上，下载提供 R 和 Python 功能的 CAB 文件以获取 SQL Server 2017 的初始版本和安装介质。 

发布  |下载链接  |
---------|---------------|
Microsoft R Open     |[SRO_3.3.3.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851496)|
Microsoft R Server      |[SRS_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851507)|
Microsoft Python Open     |[SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502) |
Microsoft Python Server    |[SPS_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851508) |

###  <a name="2---get-sql-server-2017-installation-media"></a>2 - 获取 SQL Server 2017 安装介质

1. 在连接到 Internet 的计算机上，下载 [SQL Server 2017 安装程序](https://www.microsoft.com/sql-server/sql-server-downloads)。 

2. 双击“安装”，然后选择“下载介质”安装类型  。 通过使用此选项，安装程序将创建包含安装介质的本地 .iso（或 .cab）文件。

   ![选择“下载介质”安装类型](media/offline-download-tile.png "下载介质")

::: moniker-end

::: moniker range="=sql-server-2016"

## <a name="sql-server-2016-offline-install"></a>SQL Server 2016 脱机安装

SQL Server 2016 数据库内分析仅适用于 R，仅包含分别用于产品包和 Microsoft 的开放源代码 R 分发的两个 CAB 文件。 首先安装以下任一版本：RTM、SP 1 和 SP 2。 基本安装完成后，接下来即可应用累积更新。

在连接到 Internet 的计算机上，下载由安装程序用于在 SQL Server 2016 上安装数据库内分析的 CAB 文件。 

### <a name="1---download-2016-cabs"></a>1 - 下载 2016 CAB

发布  | Microsoft R Open | Microsoft R Server |
---------|-----------------|---------------------|
**SQL Server 2016 RTM**     | [SRO_3.2.2.803_1033.cab](https://go.microsoft.com/fwlink/?LinkId=761266) |[SRS_8.0.3.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=735051) |
**SQL Server 2016 SP 1**     | [SRO_3.2.2.15000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=824879) |[SRS_8.0.3.15000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=824881) | 
**SQL Server 2016 SP 2**  |[SRO_3.2.2.16000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=866039) |[SRS_8.0.3.17000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=850317) |

### <a name="2---get-sql-server-2016-installation-media"></a>2 - 获取 SQL Server 2016 安装介质

在目标计算机上进行首次安装时，可以安装 SQL Server 2016 RTM、SP 1 或 SP 2。 这些版本中的任何版本都可以接受累积更新。

获取包含安装介质的 .iso 文件的一种方法是通过 [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) 获取。 登录后使用下载链接查找要安装的 SQL Server 2016 版本  。 下载内容采用的是 .iso 文件的形式，你可以将该文件复制到目标计算机以进行脱机安装。

::: moniker-end

## <a name="transfer-files"></a>传输文件

将 SQL Server 安装介质（.iso 或 .cab）和数据库内分析 CAB 文件复制到目标计算机。 将 CAB 文件和安装介质文件放置在目标计算机上的同一文件夹中，例如安装用户的 %TEMP% 文件夹。

Python CAB 文件需要放置在 %TEMP% 文件夹中。 对于 R，你可以使用 %TEMP%，或将 `myrcachedirectory` 参数设置为 CAB 路径。

## <a name="run-setup"></a>运行安装程序

在断开 Internet 连接的计算机上运行 SQL Server 安装程序时，安装程序会将“脱机安装”页面添加到向导，使你可以指定上一步中复制的 CAB 文件的位置  。

1. 若要开始安装，请双击 .iso 或 .cab 文件以访问安装介质。 随后你应会看到 setup.exe 文件  。

2. 右键单击“setup.exe”并以管理员身份运行  。

3. 当安装向导显示开放源代码 R 或 Python 组件的许可页面时，请单击“接受”  。 接受许可条款后，可继续执行下一步。

4. 在转至“脱机安装”页面后，在“安装路径”中指定包含之前复制的 CAB 文件的文件夹   。

5. 继续按照屏幕上的提示完成安装。

<a name="apply-cu"></a>

## <a name="apply-cumulative-updates"></a>应用累积更新

建议将最新的累积更新应用于数据库引擎和机器学习组件。 累积更新通过安装程序进行安装。 

::: moniker range="=sql-server-ver15"
1. 从基线实例开始。 只能将累积更新应用于初始版本的 SQL Server 的现有安装。

2. 在连接到 Internet 的设备上，转到你的 SQL Server 版本的累积更新列表：

   + SQL Server 2019 更新（更新目前尚不可用于 2019 版） 
::: moniker-end

::: moniker range="=sql-server-2017"
1. 从基线实例开始。 只能将累积更新应用于初始版本的 SQL Server 的现有安装。

2. 在连接到 Internet 的设备上，转到你的 SQL Server 版本的累积更新列表：

   + [SQL Server 2017 更新](https://sqlserverupdates.com/sql-server-2017-updates/)
::: moniker-end

::: moniker range="=sql-server-2016"
1. 从基线实例开始。 只能将累积更新应用于 SQL Server 2016 初始版本、SQL Server 2016 SP 1 或 SQL Server 2016 SP 2 的现有安装。

2. 在连接到 Internet 的设备上，转到你的 SQL Server 版本的累积更新列表：

   + [SQL Server 2016 更新](https://sqlserverupdates.com/sql-server-2016-updates/)
::: moniker-end

3. 选择最新的累积更新以下载可执行文件。

4. 获取 R 和 Python 的相应 CAB 文件。 有关下载链接，请参阅 [SQL Server 数据库内分析实例上的累积更新的 CAB 下载](sql-ml-cab-downloads.md)。

5. 将所有文件、可执行文件和 CAB 文件传输到脱机计算机上的同一文件夹中。

6. 运行安装程序。 接受许可条款，然后在“功能选择”页上，查看应用了累积更新的功能。 应看到为当前实例安装的每个功能，包括机器学习功能。

   ![从功能树中选择功能](media/cumulative-update-feature-selection.png "功能列表")

5. 继续执行向导，接受 R 和 Python 分发的许可条款。 在安装过程中，系统会提示你选择包含更新的 CAB 文件的文件夹位置。

## <a name="set-environment-variables"></a>设置环境变量

（仅适用于 R 功能集成）应设置“MKL_CBWR”环境变量，以确保从 Intel 数学核心函数库 (MKL) 计算得到[一致的输出结果](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr)  。

1. 在“控制面板”中，单击“系统和安全” > “系统” > “高级系统设置” > “环境变量”     。

2. 创建新的用户或系统变量。 

   + 将变量名称设置为 `MKL_CBWR`
   + 将变量值设置为 `AUTO`

此步骤需要重启服务器。 如果要启用脚本执行，则可以在完成所有配置工作之前暂停重启。

## <a name="post-install-configuration"></a>安装后配置

::: moniker range=">=sql-server-2017"
安装完成后，请重启服务，然后将服务器配置为启用脚本执行：

+ [启用外部脚本执行](sql-machine-learning-services-windows-install.md#bkmk_enableFeature)

SQL Server 机器学习服务的初始脱机安装需要与联机安装相同的配置：

+ [验证安装](sql-machine-learning-services-windows-install.md#verify-installation)
+ [按需进行其他配置](sql-machine-learning-services-windows-install.md#additional-configuration)
::: moniker-end

::: moniker range="=sql-server-2016"
安装完成后，请重启服务，然后将服务器配置为启用脚本执行：

+ [启用外部脚本执行](sql-r-services-windows-install.md#bkmk_enableFeature)

SQL Server R Services 的初始脱机安装需要与联机安装相同的配置：

+ [验证安装](sql-r-services-windows-install.md#verify-installation)
+ [按需进行其他配置](sql-r-services-windows-install.md#bkmk_FollowUp)
::: moniker-end

## <a name="next-steps"></a>后续步骤

若要使用机器学习服务在数据库中执行 Python 和 R 脚本，请参阅[安装 SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md)。
