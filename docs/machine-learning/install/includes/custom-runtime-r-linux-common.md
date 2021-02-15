---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 07bd68eaee05893174813ed43dc5358104016e4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072714"
---
## <a name="custom-installation-of-r"></a>R 的自定义安装

> [!NOTE]
> 如果已在默认位置 /usr/lib/R 安装了 R，则可以跳过此部分，并转到[安装 Rcpp 包](#install-rcpp-package-linux)部分。

### <a name="update-the-environment-variables"></a>更新环境变量

首先，编辑 mssql-launchpadd 服务，将 R_HOME 环境变量添加到 `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` 文件 

1. 用 systemctl 打开文件

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. 在打开的 `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` 文件中插入以下文本。 将 R_HOME 的值设置为自定义 R 安装路径。

    ```text
    [Service]
    Environment="R_HOME=/path/to/installation/of/R"
    ```

1. 保存并关闭。

接下来，确保可加载 `libR.so`。

1. 在 /etc/ld.so.conf.d 中创建 custom-r.conf 文件。

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-r.conf
    ```

1. 在打开的文件中，添加从自定义 R 安装到 libR.so 的路径。

    ```
    /path/to/installation/of/R/lib
    ```

1. 保存新文件并关闭编辑器。

1. 运行 `ldconfig`，然后运行以下命令，并检查是否可以找到所有依赖库，来验证是否可以加载 libR.so。

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/R/lib/libR.so
    ```

### <a name="grant-access-to-the-custom-r-installation-folder"></a>授权访问自定义 R 安装文件夹

将 `/var/opt/mssql/mssql.conf` 文件的扩展性部分中的 `datadirectories` 选项设置为自定义 R 安装。

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/R
```

### <a name="restart-mssql-launchpadd-service"></a>重启 mssql-launchpadd 服务

运行以下命令重启 mssql-launchpadd。

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="install-rcpp-package-linux"></a>

## <a name="install-rcpp-package"></a>安装 Rcpp 包

执行以下步骤安装 Rcpp 包。

1. 从 shell 启动 R：

    ```bash
    sudo ${R_HOME}/bin/R
    ```

1. 运行以下脚本以将 Rcpp 包安装到 ${R_HOME}\library 文件夹中。

  ```R
  install.packages("Rcpp", lib = "${R_HOME}/library");
  ```

## <a name="register-language-extension"></a>注册语言扩展

按照以下步骤下载并注册 R 语言扩展，该扩展用于 R 自定义运行时。

1. 从 [SQL Server 语言扩展 GitHub 存储库](https://github.com/microsoft/sql-server-language-extensions/releases)下载 R-lang-extension-linux-release.zip 文件。

    或者，可以在开发或测试环境中使用调试版本 (R-lang-extension-linux-debug.zip)。 调试版本提供详细的日志记录信息来调查任何错误，不建议用于生产环境。

1. 使用 [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) 连接到 SQL Server 实例，并运行以下 T-SQL 命令，以便使用 [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md) 注册 R 语言扩展。 

    修改此语句中的路径，以反映下载的语言扩展 zip 文件 (R-lang-extension-linux-release.zip) 的位置。

    ```sql
    CREATE EXTERNAL LANGUAGE [myR]
    FROM (CONTENT = N'/path/to/R-lang-extension-linux-release.zip', FILE_NAME = 'libRExtension.so.1.0');
    GO
    ```

    为要使用 R 语言扩展的每个数据库执行该语句。

    > [!NOTE]
    > R 是保留字，不能用作新的外部语言名称。 请改用其他名称。 例如，上面的语句使用 myR。
