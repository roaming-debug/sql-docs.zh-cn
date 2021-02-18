---
title: 获取 Python 包信息
description: 了解如何获取关于 SQL Server 机器学习服务上安装的 Python 包的信息（包括版本和安装位置）。
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning
ms.date: 06/03/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 29ed535b25d364ffa552aba20b10b446e0561cf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352518"
---
# <a name="get-python-package-information"></a>获取 Python 包信息

[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
本文介绍如何获取 [SQL Server 的机器学习服务上](../sql-server-machine-learning-services.md)以及[大数据群集](../../big-data-cluster/machine-learning-services.md)上安装的 Python 包的相关信息（包括版本和安装位置）。 示例 Python 脚本显示如何列出安装路径和版本等包信息。
::: moniker-end
::: moniker range="=sql-server-2017"
本文介绍如何获取关于 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)上安装的 Python 包的信息（包括版本和安装位置）。 示例 Python 脚本显示如何列出安装路径和版本等包信息。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
本文介绍如何获取关于 [Azure SQL 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)上安装的 Python 包的信息（包括版本和安装位置）。 示例 Python 脚本显示如何列出安装路径和版本等包信息。
::: moniker-end

## <a name="default-python-library-location"></a>Python 库默认位置

使用 SQL Server 安装机器学习时，会在实例级别为安装的每种语言创建一个包库。 实例库是注册到 SQL Server 中的安全文件夹。

在 SQL Server 上的数据库内运行的所有脚本或代码都必须加载实例库中的函数。 SQL Server 无法访问安装到其他库的包。 这也适用于远程客户端：在服务器计算上下文中运行的任何 Python 代码只能使用实例库中安装的包。
若要保护服务器资产，只能由计算机管理员修改默认实例库。

::: moniker range="=sql-server-2017"
Python 的二进制文件的默认路径为：

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES`

这里假设默认 SQL 实例为 MSSQLSERVER。 如果将 SQL Server 安装作为用户定义的命名实例，则改用给定名称。
::: moniker-end

::: moniker range=">=sql-server-ver15"
Python 的二进制文件的默认路径为：

`C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\PYTHON_SERVICES`

这里假设默认 SQL 实例为 MSSQLSERVER。 如果将 SQL Server 安装作为用户定义的命名实例，则改用给定名称。
::: moniker-end

通过运行以下 SQL 命令来启用外部脚本：

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH override;
```

::: moniker range="=azuresqldb-mi-current"
> [!IMPORTANT]
> 在 Azure SQL 托管实例上，运行 sp_configure 和 RECONFIGURE 命令会触发 SQL Server 重启，以使 RG 设置生效。 这可能会导致几秒钟不可用。
::: moniker-end

如果想验证当前实例的默认库，请运行以下 SQL 语句。 此示例返回 Python `sys.path` 变量中包含的文件夹列表。 此列表包括当前目录和标准库路径。

```sql
EXECUTE sp_execute_external_script
  @language =N'Python',
  @script=N'import sys; print("\n".join(sys.path))'
```

有关变量 `sys.path` 以及如何使用它为模块设置解释器搜索路径的详细信息，请参阅[模块搜索路径](https://docs.python.org/2/tutorial/modules.html#the-module-search-path)。

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
> [!NOTE]
> 请勿尝试使用 pip 或类似方法直接在 SQL 包库中安装 Python 包。 相反，请使用 sqlmlutils 在 SQL 实例中安装包。 有关详细信息，请参阅[使用 sqlmlutils 安装 Python 包](install-additional-python-packages-on-sql-server.md)。
::: moniker-end

## <a name="default-microsoft-python-packages"></a>默认 Microsoft Python 包

在安装过程中选择了 Python 功能时，将随 SQL Server 机器学习服务一起安装以下 Microsoft Python 包。

| 包 | 版本 |  说明 |
| ---------|---------|--------------|
| [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) | 9.4.7 | 用于远程计算上下文、流式处理、并行执行进行数据导入和转换的 rx 函数、建模、可视化和分析。 |
| [microsoftml](/machine-learning-server/python-reference/microsoftml/microsoftml-package) | 9.4.7 | 添加 Python 机器学习算法。 |

若要了解包含了哪个 Python 版本，请参阅 [Python 和 R 版本](../sql-server-machine-learning-services.md#versions)。

### <a name="component-upgrades"></a>组件升级

默认情况下，通过服务包和累积更新刷新 Python 包。 只能通过产品升级或将 Python 支持绑定到 Microsoft Machine Learning Server 来实现核心 Python 组件的其他包和完整版本升级。

有关详细信息，请参阅[升级 SQL Server 中的 R 和 Python 组件](../install/upgrade-r-and-python.md)。

## <a name="default-open-source-python-packages"></a>默认开放源代码 Python 包

如果安装过程中选择 Python 语言选项，则会安装 Anaconda 4.2 发行版 (Python 3.5)。 除 Python 代码库之外，标准安装还包括示例数据、单元测试和示例脚本。

> [!IMPORTANT]
> 切勿手动使用 Web 上的新版本覆盖 SQL Server 安装程序安装的 Python 版本。 Microsoft Python 包基于特定的 Anaconda 版本。 修改安装可能会使其不稳定。

## <a name="list-all-installed-python-packages"></a>列出所有已安装的 Python 包

以下示例脚本显示了 SQL Server 实例中安装的所有 Python 包的列表。

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import pkg_resources
import pandas
OutputDataSet = pandas.DataFrame(sorted([(i.key, i.version) for i in pkg_resources.working_set]))'
WITH result sets((Package NVARCHAR(128), Version NVARCHAR(128)));
```

## <a name="find-a-single-python-package"></a>查找单个 Python 包

如果已安装一个 Python 包并要确保它可用于特定的 SQL Server 实例，则可执行存储过程来查找该包并返回消息。

例如，以下代码将查找 `scikit-learn` 包。
如果找到了该包，则代码会输出包版本。

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import pkg_resources
pkg_name = "scikit-learn"
try:
    version = pkg_resources.get_distribution(pkg_name).version
    print("Package " + pkg_name + " is version " + version)
except:
    print("Package " + pkg_name + " not found")
'
```

结果：

```text
STDOUT message(s) from external script: Package scikit-learn is version 0.20.2
```

<a name="bkmk_SQLPythonVersion"></a>
## <a name="view-the-version-of-python"></a>查看 Python 版本

以下示例代码返回 SQL Server 的实例中安装的 Python 版本。

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import sys
print(sys.version)
'
```

## <a name="next-steps"></a>后续步骤

::: moniker range="=sql-server-2017"
+ [使用 Python 工具来安装包](install-python-packages-standard-tools.md)
::: moniker-end
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [使用 sqlmlutils 来安装新的 Python 包](install-additional-r-packages-on-sql-server.md)
::: moniker-end