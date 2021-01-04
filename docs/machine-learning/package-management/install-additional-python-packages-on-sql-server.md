---
title: 使用 sqlmlutils 安装 Python 包
description: 了解如何使用 Python pip 在 SQL Server 机器学习服务的实例上安装新的 Python 包。
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/26/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: cd0528125a4bd74b259fd02facb0589f4e123aad
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97617546"
---
# <a name="install-python-packages-with-sqlmlutils"></a>使用 sqlmlutils 安装 Python 包

[!INCLUDE [SQL Server 2019 SQL MI](../../includes/applies-to-version/sqlserver2019-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
本文介绍如何使用 [sqlmlutils](https://github.com/Microsoft/sqlmlutils) 包中的函数将新的 Python 包安装到 [SQL Server 上的机器学习服务](../sql-server-machine-learning-services.md)实例以及[大数据群集](../../big-data-cluster/machine-learning-services.md)。 安装的包可用于使用 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) T-SQL 语句在数据库内运行的 Python 脚本。
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
本文介绍如何使用 [sqlmlutils](https://github.com/Microsoft/sqlmlutils) 包中的函数将新的 Python 包安装到 [Azure SQL 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)实例。 安装的包可用于使用 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) T-SQL 语句在数据库内运行的 Python 脚本。
::: moniker-end

有关包位置和安装路径的详细信息，请参阅[获取 Python 包信息](../package-management/python-package-information.md)。

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> 本文中所述的 sqlmlutils 包用于将 Python 包添加到 SQL Server 2019 或更高版本。 对于 SQL Server 2017 及更早版本，请参阅[使用 Python 工具安装包](./install-python-packages-standard-tools.md?view=sql-server-2017&preserve-view=true)。
::: moniker-end

## <a name="prerequisites"></a>先决条件

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
+ 必须使用 Python 语言选项安装 [SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md)。
::: moniker-end

+ 在用于连接到 SQL Server 的客户端计算机上安装 [Azure Data Studio](../../azure-data-studio/what-is.md)。 你可以使用其他数据库管理或查询工具，但本文采用 Azure Data Studio。

+ 在 Azure Data Studio 中安装 Python 内核。 此外，还可以通过命令行安装和使用 Python，并且可以使用其他 Python 开发环境，例如具有 [Python 扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python)的 [Visual Studio Code](https://code.visualstudio.com/download)。

  客户端计算机上的 Python 版本必须与服务器上的 Python 版本匹配，并且安装的包必须与你拥有的 Python 版本一致。
  若要了解每个 SQL Server 版本包含哪个 Python 版本，请参阅 [Python 和 R 版本](../sql-server-machine-learning-services.md#versions)。
  
  若要验证特定 SQL Server 实例上的 Python 版本，请使用以下 T-SQL 命令。

  ```sql
  EXECUTE sp_execute_external_script
    @language = N'Python',
    @script = N'
  import sys
  print(sys.version)
  '
  ```

### <a name="other-considerations"></a>其他注意事项

+ Python 包库位于 SQL Server 实例的“程序文件”文件夹中，默认情况下，在此文件夹中安装需要管理员权限。 有关详细信息，请参阅[包库位置](../package-management/python-package-information.md#default-python-library-location)。

+ 包安装特定于在向 sqlmlutil 提供的连接信息中指定的 SQL 实例、数据库和用户。 若要在多个 SQL 实例或数据库中使用此包，或供不同的用户使用，则需要分别为它们安装此包。 在以下情况下例外：`dbo` 的成员安装了此包，此包是公共的，并且是与所有用户共享的。 如果用户安装了较新版本的公共包，则不会影响此公共包，但该用户将有权访问较新的版本。

+ 在添加包之前，请考虑该包是否适合 SQL Server 环境。

  + 建议将数据库内的 Python 用于那些受益于与数据库引擎紧密集成的任务（如机器学习），而不是简单地查询数据库的任务。

  + 如果添加的包给服务器带来了太多的计算压力，那么性能将受到影响。

  + 在强化的 SQL Server 环境中，可能希望避免使用以下包：
    + 需要网络访问权限的包
    + 需要提升的文件系统访问权限的包
    + 用于 Web 开发或不受益于在 SQL Server 内部运行的其他任务的包

  + 无法使用 sqlmlutils 安装 Python 包 tensorflow。 有关详细信息和解决方法，请参阅 [SQL Server 机器学习服务中的已知问题](../troubleshooting/known-issues-for-sql-server-machine-learning-services.md#9-cannot-install-tensorflow-package-using-sqlmlutils)。

## <a name="install-sqlmlutils-on-the-client-computer"></a>在客户端计算机上安装 sqlmlutils

若要使用 sqlmlutils，首先需要将其安装在用于连接到 SQL Server 的客户端计算机上。

### <a name="in-azure-data-studio"></a>在 Azure Data Studio 中

如果要在 Azure Data Studio 中使用 sqlmlutils，可以使用 Python 内核笔记本中的“管理包”功能来安装它。

1. 在 [Azure Data Studio 中的 Python 内核笔记本](../../azure-data-studio/notebooks/notebooks-python-kernel.md)中，单击“管理包”。
1. 单击“新增” 。
1. 在“搜索 Pip 包”字段中输入“sqlmlutils”，然后单击“搜索” 。
1. 选择要安装的“包版本”（建议选择最新版本）。
1. 单击“安装”，然后单击“关闭” 。

### <a name="from-python-command-line"></a>在 Python 命令行中

如果要在 Python 命令提示符或 IDE 中使用 sqlmlutils，可以使用简单的 pip 命令安装 sqlmlutils ：

```console
pip install sqlmlutils
```

此外，还可以从 zip 文件安装 sqlmlutils：

1. 确保已安装 pip。 有关详细信息，请参阅 [pip 安装](https://pip.pypa.io/en/stable/installing/)。
1. 从 https://github.com/Microsoft/sqlmlutils/tree/master/Python/dist 将最新的 sqlmlutils zip 文件下载到客户端计算机。 请勿解压缩文件。
1. 打开命令提示符并运行以下命令，安装 sqlmlutils 包 。 替换下载的 sqlmlutils zip 文件的完整路径（此示例假定下载的文件为 `c:\temp\sqlmlutils-1.0.0.zip`）。
   ```console
   pip install --upgrade --upgrade-strategy only-if-needed c:\temp\sqlmlutils-1.0.0.zip
   ```

## <a name="add-a-python-package-on-sql-server"></a>在 SQL Server 上添加 Python 包

使用 sqlmlutils 可以将 Python 包添加到 SQL 实例。 然后，可以在 SQL 实例中运行的 Python 代码中使用这些包。 sqlmlutils 使用 [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) 安装此包及其各个依赖项。

在下面的示例中，将向 SQL Server 添加 [text-tools](https://pypi.org/project/text-tools/) 包。

### <a name="add-the-package-online"></a>联机添加包

如果用于连接到 SQL Server 的客户端计算机具有 Internet 访问权限，则可以通过 Internet 使用 sqlmlutils 查找 text-tools 包和任何依赖项，然后将该包远程安装到 SQL Server 实例 。

::: moniker range=">=sql-server-ver15"

1. 在客户端计算机上，打开 Python 或 Python 环境。

1. 使用以下命令安装 text-tools 包。 替换你自己的 SQL Server 数据库连接信息（如果使用 Windows 身份验证，无需 `uid` 和 `pwd` 参数）。

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=azuresqldb-mi-current"

1. 在客户端计算机上，打开 Python 或 Python 环境。

1. 使用以下命令安装 text-tools 包。 请将相应部分替换为自己的 SQL Server 数据库连接信息。

::: moniker-end

   ```python
   import sqlmlutils
   connection = sqlmlutils.ConnectionInfo(server="server", database="database", uid="username", pwd="password")
   sqlmlutils.SQLPackageManager(connection).install("text-tools")
   ```

### <a name="add-the-package-offline"></a>脱机添加包

如果用于连接到 SQL Server 的客户端计算机没有 Internet 连接，则可以在具有 Internet 访问权限的计算机上使用 pip 将包和所有依赖包下载到本地文件夹。 然后，将该文件夹复制到可在其中脱机安装该包的客户端计算机。

#### <a name="on-a-computer-with-internet-access"></a>在能够访问 Internet 的计算机上

1. 打开“命令提示符”，运行以下命令以创建包含 text-tools 包的本地文件夹 。 此示例将创建文件夹 `c:\temp\text-tools`。

   ```console
   pip download text-tools -d c:\temp\text-tools
   ```

1. 将 `text-tools` 文件夹复制到客户端计算机。 以下示例假定已将其复制到 `c:\temp\packages\text-tools`。

#### <a name="on-the-client-computer"></a>在客户端计算机上

使用 sqlmlutils 安装在 pip 创建的本地文件夹中找到的每个包（WHL 文件） 。 安装包的顺序并不重要。

在本例中，text-tools 没有依赖项，因此 `text-tools` 文件夹中只有一个文件可供安装。 而类似于 scikit-plot 的包有 11 个依赖项，因此你会在文件夹中找到 12 个文件（即 scikit-plot 包及其 11 个依赖包），然后可安装每个文件 。

::: moniker range=">=sql-server-ver15"

运行以下 Python 脚本。 替换包的实际文件路径和名称，以及你自己的 SQL Server 数据库连接信息（如果使用 Windows 身份验证，无需 `uid` 和 `pwd` 参数）。 对文件夹中的每个包文件重复 `sqlmlutils.SQLPackageManager` 语句。

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=azuresqldb-mi-current"

运行以下 Python 脚本。 替换包的实际文件路径和名称，以及你自己的 SQL Server 数据库连接信息。 对文件夹中的每个包文件重复 `sqlmlutils.SQLPackageManager` 语句。

::: moniker-end

```python
import sqlmlutils
connection = sqlmlutils.ConnectionInfo(server="yourserver", database="yourdatabase", uid="username", pwd="password"))
sqlmlutils.SQLPackageManager(connection).install("text_tools-1.0.0-py3-none-any.whl")
```

## <a name="use-the-package"></a>使用包

现在可以在 SQL Server 的 Python 脚本中使用该包。 例如：

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
from text_tools.finders import find_best_string
corpus = "Lorem Ipsum text"
query = "Ipsum"
first_match = find_best_string(query, corpus)
print(first_match)
  '
```

## <a name="remove-the-package-from-sql-server"></a>从 SQL Server 删除包

如果要删除 text-tools 包，请在客户端计算机上使用以下 Python 命令，并使用之前定义的相同连接变量。

```python
sqlmlutils.SQLPackageManager(connection).uninstall("text-tools")
```

## <a name="more-sqlmlutils-functions"></a>更多 sqlmlutils 函数

Sqlmlutils 包包含许多用于管理 Python 包以及用于在 SQL Server 中创建、管理和运行存储过程和查询的函数。 有关详细信息，请参阅 [sqlmlutils Python README 文件](https://github.com/microsoft/sqlmlutils/tree/master/Python)。

有关任何 sqlmlutils 函数的信息，请使用 Python 帮助函数 。 例如：

```Python
import sqlmlutils
help(SQLPackageManager.install)
```

## <a name="next-steps"></a>后续步骤

+ 有关 SQL Server 机器学习服务中安装的 Python 包的信息，请参阅[获取 Python 包信息](../package-management/python-package-information.md)。

+ 有关在 SQL Server 机器学习服务中安装 R 包的信息，请参阅[在 SQL Server 上安装新的 R 包](install-additional-r-packages-on-sql-server.md)。
