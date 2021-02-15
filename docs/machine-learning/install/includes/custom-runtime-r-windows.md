---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 2a156ab122f0eadce71da9f4fcaf9e99584c8e32
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072713"
---
## <a name="prerequisites"></a>必备条件

安装 R 自定义运行时之前，请安装以下各项：

+ 如果使用现有 SQL Server 实例，请安装适用于 SQL Server 2019 的[累积更新 (CU) 3 或更高版本](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)。

## <a name="install-language-extensions"></a>安装语言扩展

> [!NOTE]
> 如果已在 SQL Server 2019 上安装[机器学习服务](../../sql-server-machine-learning-services.md)，则已安装语言扩展，可以跳过此步骤。

按照以下步骤安装 [SQL Server 语言扩展](../../../language-extensions/language-extensions-overview.md)，该扩展用于 R 自定义运行时。

1. 启动 SQL Server 2019 的安装向导。
  
1. 在“安装”选项卡上，选择“全新 SQL Server 独立安装或向现有安装添加功能”   。

1. 在“功能选择”  页上，选择以下选项：
  
    + **数据库引擎服务**
  
        要将语言扩展与 SQL Server 结合使用，必须安装数据库引擎的实例。 可以使用新的或现有实例。
  
    + **机器学习服务和语言扩展**

        选择“机器学习服务和语言扩展”。 请勿选择 R，因为稍后将安装自定义 R 运行时。

        :::image type="content" source="../media/2019-setup-language-extensions.png" alt-text="SQL Server 2019 语言扩展安装。":::

1. 在“准备安装”页面上，验证是否已包括这些选择，然后选择“安装”   。
  
    + 数据库引擎服务
    + 机器学习服务和语言扩展

1. 安装完成后，重新启动计算机（如果要求这样做）。

> [!IMPORTANT]
> 如果使用语言扩展安装 SQL Server 2019 的新实例，则在继续下一步之前，请安装[累积更新 (CU) 3 或更高版本](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)。

## <a name="install-r"></a>安装 R

下载并安装将用作自定义运行时的 R 版本。 R 版本 3.3 或更高版本受支持。

1. 下载 [R 版本 3.3 或更高版本](https://cran.r-project.org/bin/windows/base/)。

1. 运行 R 安装程序。

1. 请记下 R 的安装路径。 例如，在本文中，该路径为 `C:\Program Files\R\R-4.0.3`。

    :::image type="content" source="../media/r-setup-path.png" alt-text="R 安装程序":::

## <a name="update-system-environment-variable"></a>更新系统环境变量

请按照以下步骤修改 PATH 系统环境变量。

1. 在 Windows 搜索框中，搜索“编辑系统环境变量”，然后打开它。

1. 在“高级”选项卡中，选择“环境变量” 。

1. 修改“PATH”系统环境变量。

    选择“PATH”并单击“编辑”。 

    选择“新建”并添加指向 R 安装路径中的 `\bin\x64` 文件夹的路径。 例如，`C:\Program Files\R\R-4.0.3\bin\x64` 。

## <a name="install-rcpp-package"></a>安装 Rcpp 包

执行以下步骤安装 Rcpp 包。

1. 启动权限已提升的命令提示符（以管理员身份运行）。

1. 从命令提示符下启动 R。 运行 R 安装路径中的文件夹下的 `\bin\R.exe`。 例如，`C:\Program Files\R\R-4.0.3\bin\R.exe` 。

    ```CMD
    "C:\Program Files\R\R-4.0.3\bin\R.exe"
    ```

1. 运行以下脚本，将 Rcpp 包安装到 R 安装路径中的 `\library` 文件夹下。 例如，`C:\Program Files\R\R-4.0.3\library` 。

    ```R
    install.packages("Rcpp", lib="C:\Program Files\R\R-4.0.3\library");
    ```

## <a name="grant-access-to-r-folder"></a>授予对 R 文件夹的访问权限

> [!NOTE]
> 如果已在默认位置 `C:\Program Files\R\R-version`（例如 `C:\Program Files\R\R-4.0.3`）安装 R，则可以跳过此步骤。

从权限已提升的新命令提示符下运行以下 icacls 命令，以向 SQL Server Launchpad 服务用户名和 SID S-1-15-2-1（所有应用程序包）授予读取和执行访问权限。    Launchpad 服务用户名的格式为 `NT Service\MSSQLLAUNCHPAD$INSTANCENAME`，其中 `INSTANCENAME` 是 SQL Server 的实例名称。

这些命令将以递归方式授予给定目录路径下的所有文件和文件夹的访问权限。

1. 向 SQL Server Launchpad 服务用户名授予对 R 安装路径的权限。 例如，`C:\Program Files\R\R-4.0.3` 。

    ```cmd
    icacls "C:\Program Files\R\R-4.0.3" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    对于命名实例，针对名为 SQL01 的实例的命令将为 `icacls "C:\Program Files\R\R-4.0.3" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T`。

2. 向 SID S-1-15-2-1 授予对 R 安装路径的权限。 例如，`C:\Program Files\R\R-4.0.3` 。

    ```cmd
    icacls "C:\Program Files\R\R-4.0.3" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    上述命令向计算机 SID S-1-15-2-1 授予权限，这等同于英语版本的 Windows 上的 ALL APPLICATION PACKAGES。 此外，也可以在英语版本的 Windows 上使用 `icacls "C:\Program Files\R\R-4.0.3" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T`。

## <a name="restart-sql-server-launchpad"></a>重新启动 SQL Server Launchpad

请按照以下步骤重新启动 SQL Server Launchpad 服务。

1. 打开“SQL Server 配置管理器”。

1. 在“SQL Server 服务”下，右键单击“SQL Server Launchpad (MSSQLSERVER)，然后选择“重启”。 如果使用命名实例，则将显示实例名称，而不是 (MSSQLSERVER)。

## <a name="register-language-extension"></a>注册语言扩展

按照以下步骤下载并注册 R 语言扩展，该扩展用于 R 自定义运行时。

1. 从 [SQL Server 语言扩展 GitHub 存储库](https://github.com/microsoft/sql-server-language-extensions/releases)下载 R-lang-extension-windows-release.zip 文件。

    或者，可以在开发或测试环境中使用调试版本 (R-lang-extension-windows-debug.zip)。 调试版本提供详细的日志记录信息来调查任何错误，不建议用于生产环境。

1. 使用 [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) 连接到 SQL Server 实例，并运行以下 T-SQL 命令，以便使用 [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md) 注册 R 语言扩展。

    修改此语句中的路径，以反映下载的语言扩展 zip 文件 (R-lang-extension-windows-release.zip) 的位置和 R 安装的位置 (`C:\\Program Files\\R\\R-4.0.3`)。

    ```sql
    CREATE EXTERNAL LANGUAGE [myR]
    FROM (CONTENT = N'C:\path\to\R-lang-extension-windows-release.zip', 
        FILE_NAME = 'libRExtension.dll',
        ENVIRONMENT_VARIABLES = N'{"R_HOME": "C:\\Program Files\\R\\R-4.0.3"}'););
    GO
    ```

    为要使用 R 语言扩展的每个数据库执行该语句。

    > [!NOTE]
    > R 是保留字，不能用作新的外部语言名称。 请改用其他名称。 例如，上面的语句使用 myR。
