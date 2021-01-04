---
title: 使用 sqlmlutils 安装 R 包
description: 了解如何使用 sqlmlutils 将新的 R 包安装到 SQL Server 机器学习服务的实例。
ms.prod: sql
ms.technology: machine-learning
ms.date: 12/15/2020
ms.topic: how-to
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 9db282708c8f2e9bbd4ee44d45bac0b0d25dc5b9
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97617556"
---
# <a name="install-r-packages-with-sqlmlutils"></a>使用 sqlmlutils 安装 R 包

[!INCLUDE [SQL Server 2019 SQL MI](../../includes/applies-to-version/sqlserver2019-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
本文介绍如何使用 [sqlmlutils](https://github.com/Microsoft/sqlmlutils) 包中的函数将 R 包安装到 [SQL Server 上的机器学习服务](../sql-server-machine-learning-services.md)的实例以及[大数据群集](../../big-data-cluster/machine-learning-services.md)上。 安装的包可用于使用 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) T-SQL 语句在数据库中运行的 R 脚本。

> [!NOTE]
> 本文中所述的 sqlmlutils 包用于将 R 包添加到 SQL Server 2019 或更高版本。 对于 SQL Server 2017 及更早版本，请参阅[使用 R 工具安装包](./install-r-packages-standard-tools.md?view=sql-server-2017&preserve-view=true)。
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
本文介绍如果使用 [sqlmlutils](https://github.com/Microsoft/sqlmlutils) 包中的函数将 R 包安装到 [Azure SQL 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)的实例上。 安装的包可用于使用 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) T-SQL 语句在数据库中运行的 R 脚本。
::: moniker-end

## <a name="prerequisites"></a>先决条件

- 在用于连接到 SQL Server 的客户端计算机上安装 [R](https://www.r-project.org) 和 [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/)。 可以使用任何 R IDE 来运行脚本，但本文假定使用 RStudio。

  客户端计算机上的 R 版本必须与服务器上的 R 版本匹配，并且安装的包必须与你拥有的 R 版本一致。
  若要了解每个 SQL Server 版本包含哪个 R 版本，请参阅 [Python 和 R 版本](../sql-server-machine-learning-services.md#versions)。
  
  若要验证特定 SQL Server 上的 R 版本，请使用以下 T-SQL 命令。

  ```sql
  EXECUTE sp_execute_external_script @language = N'R'
   , @script = N'print(R.version)'
  ```

- 在用于连接到 SQL Server 的客户端计算机上安装 [Azure Data Studio](../../azure-data-studio/what-is.md)。 你可以使用其他数据库管理或查询工具，但本文采用 Azure Data Studio。

### <a name="other-considerations"></a>其他注意事项

- 包安装特定于在向 sqlmlutil 提供的连接信息中指定的 SQL 实例、数据库和用户。 若要在多个 SQL 实例或数据库中使用此包，或供不同的用户使用，则需要分别为它们安装此包。 在以下情况下例外：`dbo` 的成员安装了此包，此包是公共的，并且是与所有用户共享的。 如果用户安装了较新版本的公共包，则不会影响此公共包，但该用户将有权访问较新的版本。

- 在 SQL Server 中运行的 R 脚本只能使用在默认实例库中安装的包。 SQL Server 无法从外部库加载包，即使该库位于同一台计算机上也是如此。 这包括与其他 Microsoft 产品一起安装的 R 库。

- 在强化的 SQL Server 环境中，可能希望避免使用以下包：
  - 需要网络访问权限的包
  - 需要提升的文件系统访问权限的包
  - 用于 Web 开发或不受益于在 SQL Server 内部运行的其他任务的包

## <a name="install-sqlmlutils-on-the-client-computer"></a>在客户端计算机上安装 sqlmlutils

若要使用 sqlmlutils，首先需要将其安装在用于连接到 SQL Server 的客户端计算机上。

sqlmlutils 包依赖于 odbc 包，odbc 依赖于许多其他包  。 请参照以下过程，按正确顺序安装所有这些包。

### <a name="install-sqlmlutils-online"></a>联机安装 sqlmlutils

如果客户端计算机可以访问 Internet，则可以联机下载并安装 sqlmlutils 及其依赖的包。

1. 从 https://github.com/Microsoft/sqlmlutils/tree/master/R/dist 将最新的 sqlmlutils 文件（对于 Windows 为 `.zip` ，对于 Linux 为 `.tar.gz` ）下载到客户端计算机。 不要展开该文件。

1. 打开“命令提示符”并运行以下命令，以安装 odbc 和 sqlmlutils 包  。 替换下载的 sqlmlutils 文件的路径。 已联机查找到 odbc 包，并进行了安装。

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```console
   R.exe -e "install.packages('odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.zip
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```console
   R.exe -e "install.packages('odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.tar.gz
   ```
   ::: moniker-end

### <a name="install-sqlmlutils-offline"></a>脱机安装 sqlmlutils

如果客户端计算机没有 Internet 连接，则需要使用能够访问 Internet 的计算机预先下载 odbc 和 sqlmlutils 包 。 然后，可以将这些文件复制到客户端计算机上的一个文件夹中，并脱机安装这些包。

odbc 包具备许多存在依赖关系的包，并且识别包的所有依赖关系很复杂。 建议使用 [miniCRAN](https://andrie.github.io/miniCRAN/) 来为包含所有依赖包的包创建本地存储库文件夹。
有关详细信息，请参阅[使用 miniCRAN 创建本地 R 包存储库](create-a-local-package-repository-using-minicran.md)。

sqlmlutils 包中包含一个文件，可以将该文件复制到客户端计算机并进行安装。

在能够访问 Internet 的计算机上执行以下操作：

1. 安装 miniCRAN。 有关详细信息，请参阅[安装 miniCRAN](create-a-local-package-repository-using-minicran.md#install-minicran)。

1. 在 RStudio 中，运行以下 R 脚本，创建 odbc 包的本地存储库。 本示例假定在文件夹 `odbc` 中创建存储库。

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "odbc"
   pkgs_needed <- "odbc"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "odbc"
   pkgs_needed <- "odbc"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```
   ::: moniker-end

   至于 `Rversion` 值，请使用安装在 SQL Server 上的 R 版本。 若要验证已安装的版本，请使用以下 T-SQL 命令。

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. 从 [https://github.com/Microsoft/sqlmlutils/tree/master/R/dist](https://github.com/Microsoft/sqlmlutils/tree/master/R/dist) 下载最新的 sqlmlutils 文件（对于 Windows 为 `.zip` ，对于 Linux 为 `.tar.gz` ）。 不要展开该文件。

1. 将整个 odbc 存储库文件夹和 sqlmlutils 文件复制到客户端计算机 。

在用于连接到 SQL Server 的客户端计算机上：

1. 打开命令提示符。

1. 运行以下命令，依次安装 odbc 和 sqlmlutils 。 替换 odbc 存储库文件夹和复制到此计算机的 sqlmlutils 文件的完整路径 。

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```console
   R.exe -e "install.packages('odbc', repos='odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.zip
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```console
   R.exe -e "install.packages('odbc', repos='odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.tar.gz
   ```
   ::: moniker-end

## <a name="add-an-r-package-on-sql-server"></a>在 SQL Server 上添加 R 包

在下面的示例中，将 [glue](https://cran.r-project.org/web/packages/glue/) 包添加到 SQL Server。

### <a name="add-the-package-online"></a>联机添加包

如果用于连接到 SQL Server 的客户端计算机具有 Internet 访问权限，则可以使用 sqlmlutils 在 Internet 上查找 glue 包及任何依赖项，然后将该包远程安装到 SQL Server 实例 。

1. 在客户端计算机上，打开 RStudio 并创建一个新 R 脚本文件。

1. 通过 sqlmlutils 使用以下 R 脚本安装 glue 包 。 请将相应部分替换为自己的 SQL Server 数据库连接信息。

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server   = "server",
     database = "database",
     uid      = "username",
     pwd      = "password")

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
   ```

   > [!TIP]
   > 范围可为 PUBLIC（公共）或 PRIVATE（私有）  。 当数据库管理员需要安装所有用户都能使用的包时，公共范围就非常有用。 若采用私有范围，则只有安装这个包的用户才能使用它。 如果未指定范围，则默认范围为 PRIVATE（私有）。

### <a name="add-the-package-offline"></a>脱机添加包

如果客户端计算机没有 Internet 连接，则需要通过能够访问 Internet 的计算机使用 miniCRAN 下载 glue 包 。 然后将包复制到客户端计算机，可以在其中脱机安装该包。
有关安装 miniCRAN 的信息，请参阅[安装 miniCRAN](create-a-local-package-repository-using-minicran.md#install-minicran)。

在能够访问 Internet 的计算机上执行以下操作：

1. 运行以下 R 脚本，为 glue 创建本地存储库。 此示例在 `c:\downloads\glue` 中创建存储库文件夹。

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```
   ::: moniker-end

   至于 `Rversion` 值，请使用安装在 SQL Server 上的 R 版本。 若要验证已安装的版本，请使用以下 T-SQL 命令。

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. 将整个 glue 存储库文件夹 (`c:\downloads\glue`) 复制到客户端计算机。 例如，将它复制到 `c:\temp\packages\glue` 文件夹。

在客户端计算机上执行以下操作：

1. 打开 RStudio 并创建一个新 R Script 文件。

1. 通过 sqlmlutils 使用以下 R 脚本安装 glue 包 。 将自己的 SQL Server 数据库连接信息替换进去（如果不使用 Windows 身份验证，请添加 `uid` 和 `pwd` 参数）。

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server= "yourserver",
     database = "yourdatabase")
   localRepo = "c:/temp/packages/glue"

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC", repos=paste0("file:///",localRepo))
   ```

   > [!TIP]
   > 范围可为 PUBLIC（公共）或 PRIVATE（私有）  。 当数据库管理员需要安装所有用户都能使用的包时，公共范围就非常有用。 若采用私有范围，则只有安装这个包的用户才能使用它。 如果未指定范围，则默认范围为 PRIVATE（私有）。

## <a name="use-the-package"></a>使用包

安装 glue 包后，可以通过 T-SQL sp_execute_external_script 命令在 SQL Server 中的 R 脚本中使用它 。

1. 打开 Azure Data Studio 并连接到 SQL Server 数据库。

1. 运行以下命令：

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
       , @script = N'
   library(glue)

   name <- "Fred"
   birthday <- as.Date("2020-06-14")
   text <- glue(''My name is {name} '',
   ''and my birthday is {format(birthday, "%A, %B %d, %Y")}.'')

   print(text)
         ';
   ```

    **结果**

    ```text
    My name is Fred and my birthday is Sunday, June 14, 2020.
    ```

## <a name="remove-the-package"></a>删除包

如果要删除 glue 包，请运行以下 R 脚本。 使用与之前的定义一致的 connection 变量。

```R
sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC")
```

## <a name="more-sqlmlutils-functions"></a>更多 sqlmlutils 函数

Sqlmlutils 包包含许多用于管理 R 包以及用于在 SQL Server 中创建、管理和运行存储过程和查询的函数。 有关详细信息，请参阅 [sqlmlutils R README 文件](https://github.com/microsoft/sqlmlutils/tree/master/R)。

有关任何 sqlmlutils 函数的信息，请使用 R 帮助函数或“?”   运算符。 例如：

```R
library(sqlmlutils)
help("sql_install.packages")
```

## <a name="next-steps"></a>后续步骤

- 有关已安装的 R 包的信息，请参阅[获取 R 包信息](r-package-information.md)
- 有关使用 R 包的帮助信息，请参阅[使用 R 包的技巧](tips-for-using-r-packages.md)
- 有关安装 Python 包的信息，请参阅[通过 pip 安装 Python 包](install-additional-python-packages-on-sql-server.md)
- 有关 SQL Server 机器学习服务的详细信息，请参阅[什么是 SQL Server 机器学习服务（Python 和 R）？](../sql-server-machine-learning-services.md)
