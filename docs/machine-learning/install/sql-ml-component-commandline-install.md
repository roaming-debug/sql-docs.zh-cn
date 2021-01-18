---
title: 通过命令提示符安装
description: 运行 SQL Server 命令行安装程序，将具有 Python 和 R 的机器学习服务添加到 SQL Server 数据库引擎实例。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/07/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 9b0bf3444ab09cbbbb4aa8eca225dfa9964898c3
ms.sourcegitcommit: d681796e8c012eca2d9629d3b816749e9f50f868
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98005292"
---
# <a name="install-sql-server-machine-learning-services-with-r-and-python-from-the-command-line"></a>从命令行安装具有 R 和 Python 的 SQL Server 机器学习服务
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文说明如何从命令行安装具有 Python 和 R 的 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)。

可以指定与安装程序用户界面是进行无提示交互、基本交互还是完全交互。 本文对[从命令提示符安装 SQL Server](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md) 进行了补充，介绍了 R 和 Python 机器学习组件的唯一参数。

## <a name="pre-install-checklist"></a>安装前清单

+ 从提升的命令提示符运行命令。 

+ 数据库内安装需要数据库引擎实例。 不能只安装 R 或 Python 功能，但可以[将它们逐渐添加到现有实例](#add-existing)。 如果只需要 R 和 Python，而不需要数据库引擎，请安装[独立服务器](#shared-feature)。

+ 请勿在故障转移群集上安装。 用于隔离 R 和 Python 进程的安全机制与 Windows Server 故障转移群集环境不兼容。

+ 请勿在域控制器上安装。 安装程序的机器学习服务部分将失败。

+ 避免在同一台计算机上安装独立实例和数据库内实例。 一个独立服务器将争夺相同的资源，从而降低这两个安装的性能。

## <a name="command-line-arguments"></a>命令行参数

需要 /FEATURES 参数，就如许可条款协议时一样。 

通过命令提示符安装时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持完全静默模式（通过使用 /Q 参数）或简单静默模式（通过使用 /QS 参数）。 /QS 开关仅显示进度，不接受任何输入，也不显示错误消息（如果遇到）。 仅当指定 /Action=install 时才支持 /QS 参数。

::: moniker range="=sql-server-2016"
| 参数 | 说明 |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | 安装数据库内版本：SQL Server R Services（数据库内）。  |
| /FEATURES = SQL_SHARED_MR | 为独立版本安装 R 功能：SQL Server R Server（独立版）。 独立服务器是未绑定到数据库引擎实例的“共享功能”。|
| /IACCEPTROPENLICENSETERMS  | 指示你已接受使用开放源代码 R 组件的许可条款。 |
| /IACCEPTPYTHONLICENSETERMS | 指示你已接受使用 Python 组件的许可条款。 |
| /IACCEPTSQLSERVERLICENSETERMS | 指示你已接受使用 SQL Server 的许可条款。|
| /MRCACHEDIRECTORY | 对于脱机安装，设置包含 R 组件 CAB 文件的文件夹。 |
::: moniker-end

::: moniker range="=sql-server-2017"
| 参数 | 说明 |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | 安装数据库内版本：SQL Server 机器学习服务（数据库内）。  |
| /FEATURES = SQL_INST_MR | 将此与 AdvancedAnalytics 配对。 安装（数据库内）R 功能，包括 Microsoft R Open 和专用 R 包。 |
| /FEATURES = SQL_INST_MPY | 将此与 AdvancedAnalytics 配对。 安装（数据库内）Python 功能，包括 Anaconda 和专用 Python 包。 |
| /FEATURES = SQL_SHARED_MR | 为独立版本安装 R 功能：SQL Server Machine Learning Server（独立版）。 独立服务器是未绑定到数据库引擎实例的“共享功能”。|
| /FEATURES = SQL_SHARED_MPY | 为独立版本安装 Python 功能：SQL Server Machine Learning Server（独立版）。 独立服务器是未绑定到数据库引擎实例的“共享功能”。|
| /IACCEPTROPENLICENSETERMS  | 指示你已接受使用开放源代码 R 组件的许可条款。 |
| /IACCEPTPYTHONLICENSETERMS | 指示你已接受使用 Python 组件的许可条款。 |
| /IACCEPTSQLSERVERLICENSETERMS | 指示你已接受使用 SQL Server 的许可条款。|
| /MRCACHEDIRECTORY | 对于脱机安装，设置包含 R 组件 CAB 文件的文件夹。 |
| /MPYCACHEDIRECTORY | 保留供将来使用。 使用 %TEMP% 存储 Python 组件 CAB 文件，以便在没有 Internet 连接的计算机上安装。 |
::: moniker-end

::: moniker range=">=sql-server-ver15"
| 参数 | 说明 |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | 安装数据库内版本：SQL Server 机器学习服务（数据库内）。  |
| /FEATURES = SQL_INST_MR | 将此与 AdvancedAnalytics 配对。 安装（数据库内）R 功能，包括 Microsoft R Open 和专用 R 包。 |
| /FEATURES = SQL_INST_MPY | 将此与 AdvancedAnalytics 配对。 安装（数据库内）Python 功能，包括 Anaconda 和专用 Python 包。 |
| /FEATURES = SQL_INST_MJAVA | 将此与 AdvancedAnalytics 配对。 安装（数据库内）Java 功能，包括 Open JRE。 适用于 [SQL Server Java 语言扩展](../../language-extensions/install/windows-java.md)。|
| /FEATURES = SQL_SHARED_MR | 为独立版本安装 R 功能：SQL Server Machine Learning Server（独立版）。 独立服务器是未绑定到数据库引擎实例的“共享功能”。|
| /FEATURES = SQL_SHARED_MPY | 为独立版本安装 Python 功能：SQL Server Machine Learning Server（独立版）。 独立服务器是未绑定到数据库引擎实例的“共享功能”。|
| /IACCEPTROPENLICENSETERMS  | 指示你已接受使用开放源代码 R 组件的许可条款。 |
| /IACCEPTPYTHONLICENSETERMS | 指示你已接受使用 Python 组件的许可条款。 |
| /IACCEPTSQLSERVERLICENSETERMS | 指示你已接受使用 SQL Server 的许可条款。|
| /MRCACHEDIRECTORY | 对于脱机安装，设置包含 R 组件 CAB 文件的文件夹。 |
| /MPYCACHEDIRECTORY | 保留供将来使用。 使用 %TEMP% 存储 Python 组件 CAB 文件，以便在没有 Internet 连接的计算机上安装。 |
::: moniker-end

## <a name="in-database-instance-installations"></a><a name="indb"></a> 数据库内实例安装

数据库内分析可用于数据库引擎实例，这是向安装添加 AdvancedAnalytics 功能所需的  。 可以安装带有高级分析的数据库引擎实例，或[将它添加到现有实例](#add-existing)。 

若要在不与屏幕提示交互的情况下查看进度信息，请使用 /qs 参数。

> [!IMPORTANT]
> 安装完成后，还需要进行两个额外的配置步骤。 执行这些任务之后才能完成集成。 有关说明，请参阅[安装后任务](#post-install)。

::: moniker range=">=sql-server-2017"
### <a name="sql-server-machine-learning-services-database-engine-advanced-analytics-with-python-and-r"></a>SQL Server 机器学习服务：数据库引擎、具有 Python 和 R 的高级分析

对于数据库引擎实例的并发安装，请提供实例名称和管理员 (Windows) 登录名。 包括用于安装核心和语言组件的功能，以及接受所有许可条款。

```cmd
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

这是相同的命令，但在使用混合身份验证的数据库引擎上使用了 SQL Server 登录。

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SECURITYMODE=SQL /SAPWD="%password%" /SQLSYSADMINACCOUNTS="<sql-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

此示例仅限 Python，表示你可以通过省略一项功能来添加一种语言。

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTPYTHONLICENSETERMS
```
::: moniker-end

::: moniker range="=sql-server-2016"
### <a name="sql-server-r-services-database-engine-and-advanced-analytics-with-r"></a>SQL Server R Services：数据库引擎和具有 R 的高级分析

对于数据库引擎实例的并发安装，请提供实例名称和管理员 (Windows) 登录名。 包括用于安装核心和语言组件的功能，以及接受所有许可条款。

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS 
```
::: moniker-end

## <a name="post-install-configuration-required"></a><a name="post-install"></a> 安装后配置（必需）

仅适用于数据库内安装。

安装完成后，你就有了一个具有 R 和 Python 的数据库引擎实例、Microsoft R 和 Python 包、Microsoft R Open、Anaconda、工具、示例以及作为分发一部分的脚本。 

需要再执行两个任务才能完成安装：


::: moniker range=">=sql-server-2017"
1. 重启数据库引擎服务。

1. SQL Server 机器学习服务：启用外部脚本后才能使用此功能。 按照[安装 SQL Server 机器学习服务（数据库内）](sql-machine-learning-services-windows-install.md)中的说明执行下一步。 
::: moniker-end

::: moniker range="=sql-server-2016"
1. 重启数据库引擎服务。

1. SQL Server R Services：启用外部脚本后才能使用此功能。 按照[安装 SQL Server R Services（数据库内）](sql-r-services-windows-install.md)中的说明执行下一步。 
::: moniker-end

## <a name="add-advanced-analytics-to-an-existing-database-engine-instance"></a><a name="add-existing"></a> 向现有数据库引擎实例添加高级分析

向现有数据库引擎实例添加数据库内高级分析时，请提供实例名称。 例如，如果之前安装了 SQL Server 2017 或更高版本的数据库引擎和 Python，则可以使用此命令添加 R。

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQL_INST_MR /INSTANCENAME=MSSQLSERVER 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTROPENLICENSETERMS
```

## <a name="silent-install"></a><a name="silent"></a> 无提示安装

无提示安装会阻止检查 .cab 文件位置。 因此，必须指定要在其中解压缩 .cab 文件的位置。 对于 Python，CAB 文件必须位于 %TEMP* 中。 对于 R，你可以使用临时目录来设置文件夹路径。
 
```cmd  
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS 
/MRCACHEDIRECTORY=%temp% 
```

## <a name="standalone-server-installations"></a><a name="shared-feature"></a> 独立服务器安装

独立服务器是未绑定到数据库引擎实例的“共享功能”。 以下示例显示了用于安装独立服务器的有效语法。

::: moniker range=">=sql-server-2017"
SQL Server Machine Learning Server 支持独立服务器上的 Python 和 R：

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR,SQL_SHARED_MPY  
/IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```
::: moniker-end
::: moniker range="=sql-server-2016"
SQL Server R Server 仅适用于 R：

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR 
/IACCEPTROPENLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```
::: moniker-end

安装完成后，你就有了一个服务器、Microsoft 包、开放源代码分发版的 R 和 Python、工具、示例以及作为分发一部分的脚本。 

若要打开 R 控制台窗口，请转到 `\Program files\Microsoft SQL Server\150 (or 140/130)\R_SERVER\bin\x64` 并双击“RGui.exe”  。 不熟悉 R？ 试用本教程：[基本 R 命令和 RevoScaleR 函数：25 个常见示例](/machine-learning-server/r/tutorial-r-to-revoscaler)。

若要打开 Python 命令，请转到 `\Program files\Microsoft SQL Server\150 (or 140)\PYTHON_SERVER\bin\x64` 并双击“python.exe”  。

## <a name="next-steps"></a>后续步骤

Python 开发人员可以通过以下教程了解如何将 Python 与 SQL Server 一起使用：

+ [Python 教程：在 SQL Server 机器学习服务中使用线性回归来预测雪橇租赁](../tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Python 教程：配合使用 K-Means 群集和 SQL Server 机器学习服务对客户进行分类](../tutorials/python-clustering-model.md)

R 开发人员可以开始使用一些简单的示例，并了解 R 如何与 SQL Server 协同工作的基础知识。 有关下一步，请参阅以下链接：

+ [快速入门：在 T-SQL 中运行 R](../tutorials/quickstart-r-create-script.md)
+ [教程：适用于 R 开发人员的数据库内分析](../tutorials/r-taxi-classification-introduction.md)
