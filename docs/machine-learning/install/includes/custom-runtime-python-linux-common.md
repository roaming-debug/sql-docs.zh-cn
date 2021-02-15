---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 1303df98c60212c13a233d1e386c60109308e981
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072857"
---
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

将 `/var/opt/mssql/mssql.conf` 文件的扩展性部分中的 `datadirectories` 选项设置为自定义 python 安装。

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/python3.7
```

### <a name="restart-mssql-launchpadd"></a>重启 mssql-launchpadd

运行以下命令重启 mssql-launchpadd。

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="register-language-extension-linux"></a>

## <a name="register-language-extension"></a>注册语言扩展

按照以下步骤下载并注册 Python 语言扩展，该扩展用于 Python 自定义运行时。

1. 从 [SQL Server 语言扩展 GitHub 存储库](https://github.com/microsoft/sql-server-language-extensions/releases)下载 python-lang-extension-linux-release.zip 文件。

    或者，你可以在开发或测试环境中使用调试版本 (python-lang-extension-linux-debug.zip)。 调试版本提供详细的日志记录信息来调查任何错误，不建议用于生产环境。

1. 使用 [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) 连接到 SQL Server 实例，并运行以下 T-SQL 命令，使用 [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md) 注册 Python 语言扩展。 

    修改此语句中的路径，以反映下载的语言扩展 zip 文件 (python-lang-extension-linux-release.zip) 的位置。

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-linux-release.zip', FILE_NAME = 'libPythonExtension.so.1.1');
    GO
    ```

    为要在其中使用 Python 语言扩展的每个数据库执行语句。

    > [!NOTE]
    > Python 是保留字，不能用作新外部语言名称的名称。 请改用其他名称。 例如，上面的语句使用 myPython。
