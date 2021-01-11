---
title: 安装 Python 自定义运行时
description: 了解如何使用语言扩展为 SQL Server 安装 Python 自定义运行时。 Python 自定义运行时可用于机器学习。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
zone_pivot_groups: python-custom-runtime-platform
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 91aace4333b4496338b782344e64cdfea2b886bd
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804325"
---
# <a name="install-a-python-custom-runtime-for-sql-server"></a>为 SQL Server 安装 Python 自定义运行时
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

本文介绍如何安装 Python 自定义运行时，以便使用 SQL Server 运行外部 Python 脚本。 自定义运行时使用 [SQL Server 语言扩展](../../language-extensions/language-extensions-overview.md)，并可用于执行机器学习脚本。

使用 Python 自定义运行时，你可以将自己的 Python 运行时版本与 SQL Server 一起使用，而不是随 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)安装的默认运行时版本。

::: zone pivot="python-custom-runtime-windows"

## <a name="prerequisites"></a>先决条件

安装 Python 自定义运行时之前，请安装以下各项：

+ 安装 SQL Server 2019 的[累积更新 (CU) 3 或更高版本](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)。

+ 在服务器上安装 [Python 3.7](https://www.python.org/downloads/)。

    用于自定义 Python 运行时的 Python 语言扩展目前仅支持 Python 3.7。 如果要使用不同版本的 Python，请按照 [Python 语言扩展 GitHub 存储库](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python)中的说明来修改和重新生成扩展。

    > [!IMPORTANT]
    > 在安装 Python 期间，查看“将 Python 3.7 添加到 PATH”。

## <a name="install-language-extensions"></a>安装语言扩展

> [!NOTE]
> 如果已在 SQL Server 2019 上安装[机器学习服务](../sql-server-machine-learning-services.md)，则已安装语言扩展，可以跳过此步骤。

按照以下步骤安装 [SQL Server 语言扩展](../../language-extensions/language-extensions-overview.md)，该扩展用于 Python 自定义运行时。

1. 启动 SQL Server 2019 的安装向导。
  
1. 在“安装”选项卡上，选择“全新 SQL Server 独立安装或向现有安装添加功能”   。

1. 在“功能选择”  页上，选择以下选项：
  
    + **数据库引擎服务**
  
        要将语言扩展与 SQL Server 结合使用，必须安装数据库引擎的实例。 可以使用新的或现有实例。
  
    + **机器学习服务和语言扩展**

        选择“机器学习服务和语言扩展”。 请勿选择 Python，因为稍后将安装自定义 Python 运行时。

        :::image type="content" source="media/2019-setup-language-extensions.png" alt-text="SQL Server 2019 语言扩展安装。":::

1. 在“准备安装”页面上，验证是否已包括这些选择，然后选择“安装”   。
  
    + 数据库引擎服务
    + 机器学习服务和语言扩展

1. 安装完成后，重新启动计算机（如果要求这样做）。

> [!IMPORTANT]
> 如果使用语言扩展安装 SQL Server 2019 的新实例，则在继续下一步之前，请安装[累积更新 (CU) 3 或更高版本](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)。

## <a name="install-pandas"></a>安装 pandas

从提升的命令提示符安装适用于 Python 的 [pandas](https://pandas.pydata.org/) 包：

```bash
python.exe -m pip install pandas
```

## <a name="add-environment-variable"></a>添加环境变量

添加或修改系统环境变量 PYTHONHOME。

1. 在 Windows 搜索框中，键入“环境”，然后选择“编辑系统环境变量”。
1. 在“高级”选项卡中，选择“环境变量” 。
1. 在“系统变量”下，选择“新建”以创建指向 Python 3.7 安装位置的 PYTHONHOME。 如果 PYTHONHOME 已存在，请选择“编辑”将其指向 Python 3.7 安装位置。
1. 选择“确定”以关闭所有窗口。

    :::image type="content" source="media/pythonhome-env-variable.png" alt-text="PYTHONHOME 环境变量。":::

## <a name="grant-access-to-python-folder"></a>授权访问 Python 文件夹

从新的提升的命令提示符运行以下 icacls 命令，以向 PYTHONHOME 授予对 SQL Server Launchpad 服务和 SID S-1-15-2-1 (ALL_APPLICATION_PACKAGES) 的读取和执行访问权限。

1. 授予对 SQL Server Launchpad 服务用户名的权限。

    ```cmd
    icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    对于命名实例，针对名为 SQL01 的实例的命令将为 `icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T`。

2. 授予对 SID S-1-15-2-1 的权限。

    ```cmd
    icacls "%PYTHONHOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    上述命令向计算机 SID S-1-15-2-1 授予权限，这等同于英语版本的 Windows 上的 ALL APPLICATION PACKAGES。 此外，也可以在英语版本的 Windows 上使用 `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T`。

## <a name="restart-sql-server-launchpad"></a>重新启动 SQL Server Launchpad

请按照以下步骤重新启动 SQL Server Launchpad 服务。

1. 打开“SQL Server 配置管理器”。

1. 在“SQL Server 服务”下，右键单击“SQL Server Launchpad (MSSQLSERVER)，然后选择“重启”。 如果使用命名实例，则将显示实例名称，而不是 (MSSQLSERVER)。

## <a name="register-language-extension"></a>注册语言扩展

按照以下步骤下载并注册 Python 语言扩展，该扩展用于 Python 自定义运行时。

1. 从 [SQL Server 语言扩展 GitHub 存储库](https://github.com/microsoft/sql-server-language-extensions/releases)下载 python-lang-extension-windows.zip 文件。

    或者，你可以在开发或测试环境中使用调试版本 (python-lang-extension-windows-debug.zip)。 调试版本提供详细的日志记录信息来调查任何错误，不建议用于生产环境。

1. 使用 [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) 连接到 SQL Server 实例，并运行以下 T-SQL 命令，使用 [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) 注册 Python 语言扩展。 

    修改此语句中的路径，以反映下载的语言扩展 zip 文件 (python-lang-extension-windows.zip) 的位置。

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-windows.zip', FILE_NAME = 'pythonextension.dll');
    GO
    ```

    为要在其中使用 Python 语言扩展的每个数据库执行语句。

    > [!NOTE]
    > Python 是保留字，不能用作新外部语言名称的名称。 请改用其他名称。 例如，上面的语句使用 myPython。

::: zone-end

::: zone pivot="python-custom-runtime-linux"

## <a name="prerequisites"></a>先决条件

安装 Python 自定义运行时之前，请安装以下各项：

+ 安装适用于 Linux 的 SQL Server 2019。 你可以安装 SQL Server on Red Hat Enterprise Linux (RHEL)、SUSE Linux Enterprise Server (SLES) 和 Ubuntu。 有关更多信息，请参阅 [Linux 上的 SQL Server 的安装指南](../../linux/sql-server-linux-setup.md)。

+ 升级到 SQL Server 2019 的累积更新 (CU) 3 或更高版本。 执行以下步骤：
    1. 配置用于累积更新的存储库。 有关详细信息，请参阅[配置存储库以便安装和升级 Linux 上的 SQL Server](../../linux/sql-server-linux-change-repo.md)。

    1. 将 mssql-server 包更新为最新的累积更新。 有关详细信息，请参阅 [Linux 上的 SQL Server 安装指南中的“更新或升级 SQL Server”部分](../../linux/sql-server-linux-setup.md#upgrade)。

+ 在服务器上安装 [Python 3.7](https://www.python.org/downloads/)。

    用于自定义 Python 运行时的 Python 语言扩展目前仅支持 Python 3.7。 如果要使用不同版本的 Python，请按照 [Python 语言扩展 GitHub 存储库](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python)中的说明来修改和重新生成扩展。

## <a name="install-language-extensions"></a>安装语言扩展

> [!NOTE]
> 如果已在 SQL Server 2019 上安装 [机器学习服务](../sql-server-machine-learning-services.md)，则已安装用于语言扩展的 **mssql-server-extensibility** 包，你可以跳过此步骤。

按照以下命令在 Linux 上安装 [SQL Server 语言扩展](../../language-extensions/language-extensions-overview.md)，该扩展用于 Python 自定义运行时。

#### <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

1. 如果可以，请在安装之前运行此命令以刷新系统中的包。

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. Ubuntu 可能没有 https apt 传输选项。 若要安装，请运行此命令。

    ```bash
    # Install as root or sudo
    apt-get install apt-transport-https
    ```

1. 使用此命令安装 mssql-server-extensibility。

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

#### <a name="red-hat-enterprise-linux-rhel"></a>[Red Hat Enterprise Linux (RHEL)](#tab/rhel)

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

#### <a name="suse-linux-enterprise-server-sles"></a>[SUSE Linux Enterprise Server (SLES)](#tab/sles)

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

---

## <a name="install-python-37-and-pandas"></a>安装 Python 3.7 和 pandas

安装 Python 3.7、libpython 3.7 库和 pandas 包。 

下面是 Ubuntu 的示例命令：

```bash
# Install python3.7 and the corresponding library:
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.7 python3-pip libpython3.7

# Install pandas to /usr/lib:
sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
```

## <a name="custom-installation-of-python"></a>自定义 Python 安装

> [!NOTE]
> 如果已在 `/usr/lib/python3.7` 的默认位置安装了 Python 3.7，则可以跳过此部分，并转到[注册语言扩展](#register-language-extension-linux)部分。

如果你构建了自己的 Python 3.7 版本，请使用以下命令，使 SQL Server 知道你的自定义安装。

### <a name="add-environment-variable"></a>添加环境变量

首先，编辑 mssql-launchpadd 服务，将 PYTHONHOME 环境变量添加到 `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` 文件

1. 用 systemctl 打开文件

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. 在打开的 `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` 文件中插入以下文本。 将 PYTHONHOME 的值设置为自定义 Python 安装路径。

    ```
    [Service]
    Environment="PYTHONHOME=/path/to/installation/of/python3.7"
    ```

1. 保存文件并关闭编辑器。

接下来，确保可加载 `libpython3.7m.so.1.0`。

1. 在 `/etc/ld.so.conf.d` 中创建 custom-python.conf 文件。

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-python.conf
    ```

1. 在打开的文件中，添加从自定义 Python 安装到 libpython3.7m.so.1.0 的路径。

    ```
    /path/to/installation/of/python3.7/lib
    ```

1. 保存新文件并关闭编辑器。

1. 运行 `ldconfig`，然后运行以下命令，并检查是否可以找到所有依赖库，来验证是否可以加载 `libpython3.7m.so.1.0`。

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/python3.7/lib/libpython3.7m.so.1.0
    ```

### <a name="grant-access-to-python-folder"></a>授权访问 Python 文件夹

将 /var/opt/mssql/mssql.conf 文件扩展性部分中的 `datadirectories` 选项设置为自定义 python 安装。

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/python3.7
```

### <a name="restart-mssql-launchpadd"></a>重启 mssql-launchpadd

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="register-language-extension-linux"></a>

## <a name="register-language-extension"></a>注册语言扩展

按照以下步骤下载并注册 Python 语言扩展，该扩展用于 Python 自定义运行时。

1. 从 [SQL Server 语言扩展 GitHub 存储库](https://github.com/microsoft/sql-server-language-extensions/releases)下载 python-lang-extension-linux.zip 文件。

    或者，你可以在开发或测试环境中使用调试版本 (python-lang-extension-linux-debug.zip)。 调试版本提供详细的日志记录信息来调查任何错误，不建议用于生产环境。

1. 使用 [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) 连接到 SQL Server 实例，并运行以下 T-SQL 命令，使用 [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) 注册 Python 语言扩展。 

    修改此语句中的路径，以反映下载的语言扩展 zip 文件 (python-lang-extension-linux.zip) 的位置。

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-linux.zip', FILE_NAME = 'libPythonExtension.so.1.0');
    GO
    ```

    为要在其中使用 Python 语言扩展的每个数据库执行语句。

    > [!NOTE]
    > Python 是保留字，不能用作新外部语言名称的名称。 请改用其他名称。 例如，上面的语句使用 myPython。

::: zone-end

## <a name="enable-external-script"></a>启用外部脚本

可以通过存储过程 [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 执行 Python 外部脚本。

若要启用外部脚本，请使用 [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) 执行下面的语句。

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-installation"></a>验证安装

使用以下 SQL 脚本验证 Python 自定义运行时的安装和功能。

```sql
EXEC sp_execute_external_script
@language =N'myPython',
@script=N'
import sys
print(sys.path)
print(sys.version)
print(sys.executable)'
```

## <a name="next-steps"></a>后续步骤

+ [为 SQL Server 安装 R 自定义运行时](custom-runtime-r.md)
+ [SQL Server 中的扩展性框架](../concepts/extensibility-framework.md)
+ [语言扩展概述](../../language-extensions/language-extensions-overview.md)
