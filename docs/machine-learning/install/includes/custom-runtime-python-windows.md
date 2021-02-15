---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 9f58ddf6b58e32d40b2962b47255aba2e553acca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072863"
---
## <a name="prerequisites"></a>必备条件

安装 Python 自定义运行时之前，请安装：

+ 如果使用现有 SQL Server 实例，请安装适用于 SQL Server 2019 的[累积更新 (CU) 3 或更高版本](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)。

## <a name="install-language-extensions"></a>安装语言扩展

> [!NOTE]
> 如果已在 SQL Server 2019 上安装[机器学习服务](../../sql-server-machine-learning-services.md)，则已安装语言扩展，可以跳过此步骤。

按照以下步骤安装 [SQL Server 语言扩展](../../../language-extensions/language-extensions-overview.md)，该扩展用于 Python 自定义运行时。

1. 启动 SQL Server 2019 的安装向导。
  
1. 在“安装”选项卡上，选择“全新 SQL Server 独立安装或向现有安装添加功能”   。

1. 在“功能选择”  页上，选择以下选项：
  
    + **数据库引擎服务**
  
        要将语言扩展与 SQL Server 结合使用，必须安装数据库引擎的实例。 可以使用新的或现有实例。
  
    + **机器学习服务和语言扩展**

        选择“机器学习服务和语言扩展”。 请勿选择 Python，因为稍后将安装自定义 Python 运行时。

        :::image type="content" source="../media/2019-setup-language-extensions.png" alt-text="SQL Server 2019 语言扩展安装。":::

1. 在“准备安装”页面上，验证是否已包括这些选择，然后选择“安装”   。
  
    + 数据库引擎服务
    + 机器学习服务和语言扩展

1. 安装完成后，重新启动计算机（如果要求这样做）。

> [!IMPORTANT]
> 如果使用语言扩展安装 SQL Server 2019 的新实例，则在继续下一步之前，请安装[累积更新 (CU) 3 或更高版本](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)。

## <a name="install-python"></a>安装 Python

用于自定义 Python 运行时的 Python 语言扩展目前仅支持 Python 3.7。 如果要使用不同版本的 Python，请按照 [Python 语言扩展 GitHub 存储库](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python)中的说明来修改和重新生成扩展。

1. 下载适用于 Windows 的 [Python 3.7](https://www.python.org/downloads/windows/)，并在服务器上运行安装程序。

1. 选择“将 Python 3.7 添加到 PATH”，然后选择“自定义安装”。

    :::image type="content" source="../media/python-install-add-to-path.png" alt-text="Python 3.7 安装 - 将 Python 3.7 添加到 PATH":::

1. 在“可选功能”下，保留默认值并选择“下一步”。

1. 选择“为所有用户安装”并记下安装位置。

    :::image type="content" source="../media/python-install-for-all-users.png" alt-text="Python 3.7 安装 - 为所有用户安装":::

1. 选择“安装”  。

## <a name="install-pandas"></a>安装 pandas

从权限已提升的命令提示符下安装适用于 Python 的 [pandas](https://pandas.pydata.org/) 包（以管理员身份运行）：

```bash
python.exe -m pip install pandas
```

## <a name="grant-access-to-python-folder"></a>授权访问 Python 文件夹

从权限已提升的新命令提示符下运行以下 icacls 命令，以向 SQL Server Launchpad 服务和 SID S-1-15-2-1 (ALL_APPLICATION_PACKAGES) 授予对 Python 安装位置的读取和执行访问权限。

下面的示例使用 `C:\Program Files\Python37` 作为 Python 安装位置。 如果你的位置不同，请在命令中进行更改。

1. 授予对 SQL Server Launchpad 服务用户名的权限。

    ```cmd
    icacls "C:\Program Files\Python37" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    对于命名实例，针对名为 SQL01 的实例的命令将为 `icacls "C:\Program Files\Python37" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T`。

2. 授予对 SID S-1-15-2-1 的权限。

    ```cmd
    icacls "C:\Program Files\Python37" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    上述命令向计算机 SID S-1-15-2-1 授予权限，这等同于英语版本的 Windows 上的 ALL APPLICATION PACKAGES。 此外，也可以在英语版本的 Windows 上使用 `icacls "C:\Program Files\Python37" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T`。

## <a name="restart-sql-server-launchpad"></a>重新启动 SQL Server Launchpad

请按照以下步骤重新启动 SQL Server Launchpad 服务。

1. 打开“SQL Server 配置管理器”。

1. 在“SQL Server 服务”下，右键单击“SQL Server Launchpad (MSSQLSERVER)，然后选择“重启”。 如果使用命名实例，则将显示实例名称，而不是 (MSSQLSERVER)。

## <a name="register-language-extension"></a>注册语言扩展

按照以下步骤下载并注册 Python 语言扩展，该扩展用于 Python 自定义运行时。

1. 从 [SQL Server 语言扩展 GitHub 存储库](https://github.com/microsoft/sql-server-language-extensions/releases)下载 python-lang-extension-windows-release.zip 文件。

    或者，你可以在开发或测试环境中使用调试版本 (python-lang-extension-windows-debug.zip)。 调试版本提供详细的日志记录信息来调查任何错误，不建议用于生产环境。

1. 使用 [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) 连接到 SQL Server 实例，并运行以下 T-SQL 命令，使用 [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md) 注册 Python 语言扩展。

    修改此语句中的路径，以反映下载的语言扩展 zip 文件 (python-lang-extension-windows-release.zip) 的位置和 Python 安装的位置 (`C:\\Program Files\\Python3.7`)。

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'C:\path\to\python-lang-extension-windows-release.zip', 
        FILE_NAME = 'pythonextension.dll', 
        ENVIRONMENT_VARIABLES = N'{"PYTHONHOME": "C:\\Program Files\\Python3.7"}');
    GO
    ```

    为要在其中使用 Python 语言扩展的每个数据库执行语句。

    > [!NOTE]
    > Python 是保留字，不能用作新外部语言名称的名称。 请改用其他名称。 例如，上面的语句使用 myPython。
