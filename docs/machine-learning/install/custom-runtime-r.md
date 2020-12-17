---
title: 安装 R 自定义运行时
description: 了解如何为 SQL Server 安装 R 自定义运行时。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/20/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 554c3a08cc29cfbc6addef598698c40df31f9990
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471228"
---
# <a name="install-an-r-custom-runtime-for-sql-server"></a>为 SQL Server 安装 R 自定义运行时

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

本文介绍如何安装自定义运行时，以便使用 SQL Server 运行 R 脚本。 自定义运行时使用在可扩展性框架上生成的语言扩展来执行外部代码。 适用于 R 的自定义运行时可用于以下方案：

+ 使用扩展性框架安装 SQL Server。

+ 使用 SQL Server 2019 安装机器学习服务。 完成一些其他配置步骤后，可将语言扩展与用于 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)。

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> 本文介绍如何在 Windows 上安装适用于 R 的自定义运行时。 若要在 Linux 上安装，请参阅[在 Linux 上安装适用于 SQL Server 的 R 自定义运行时](custom-runtime-r.md?view=sql-server-linux-ver15&preserve-view=true)

## <a name="pre-install-checklist"></a>安装前清单

安装 R 自定义运行时之前，请安装以下各项：

+ [适用于 Windows 的 SQL Server 2019（累积更新 3 或更高版本）](../../database-engine/install-windows/install-sql-server.md)。

+ [通过扩展性框架在 Windows 上安装 SQL Server 语言扩展](../../language-extensions/install/windows-java.md)。

+ [R 3.3 或更高版本](https://cran.r-project.org/)。

## <a name="add-sql-server-language-extensions-for-windows"></a>添加适用于 Windows 的 SQL Server 语言扩展

> [!NOTE]
> 如果已在 SQL Server 2019 上安装机器学习服务，则已安装 Launchpad 服务的语言扩展的扩展性框架，你可以跳过此步骤。

语言扩展使用扩展性框架来执行外部代码。 代码执行与核心引擎进程隔离，但与 SQL Server 查询执行完全集成。

1. 启动 SQL Server 2019 的安装向导。

1. 在“安装”选项卡上，选择“全新 SQL Server 独立安装或向现有安装添加功能”   。

    ![SQL Server 2019 安装 CU3 或更高版本](../install/media/2019setup-installation-page-mlsvcs.png)

1. 在“功能选择”  页上，选择以下选项：

    - **数据库引擎服务**

        要将语言扩展与 SQL Server 结合使用，必须安装数据库引擎的实例。 可使用默认实例或命名实例。

    - **机器学习服务和语言扩展**

       选择“机器学习服务和语言扩展”。 无需选择 R。

    ![SQL Server 2019 CU3 或更高版本的安装功能](../install/media/sql-feature-selection.png)

1. 在“准备安装”页面上，验证是否已包括这些选择，然后选择“安装”   。

    + 数据库引擎服务
    + 机器学习服务和语言扩展

1. 安装完成后，如果收到重启计算机的指示，请立即重启。 安装完成后，请务必阅读来自安装向导的消息。 有关详细信息，请参阅 [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。

## <a name="install-r"></a>安装 R

> [!NOTE]
>对于 SQL 机器学习服务，R 已安装在 SQL Server 实例的 R_SERVICES 文件夹中。 例如“C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES”。 如果要继续使用此路径作为 R_HOME，请跳到安装 Rcpp 的下一步。 否则，如果要使用 R 的其他运行时，请继续安装。

安装 [R（3.3 或更高版本）](https://cran.r-project.org/bin/windows/base/)并记下其安装路径。 此路径是 R_HOME。 例如像这里显示的，R_HOME 为“C:\Program Files\R\R-4.0.2”

![安装自定义 R](../install/media/custom-r-installation.png)

> [!NOTE]
>在以下说明中，%R_HOME% 是 R 安装的路径，应将其替换为该值。

## <a name="install-rcpp-package"></a>安装 Rcpp 包

+ 在 %R_HOME%\bin 中找到 R 可执行文件。 默认情况下，它位于 `C:\Program Files\R\R-4.0.2\bin` 中。

+ 从提升的命令提示符启动 R：

```CMD
%R_HOME%\bin\R.exe
```

在此提升的 R 提示符 (%R_HOME%\bin\R.exe) 中，运行以下脚本，以将 Rcpp 包安装到 %R_HOME%\library 文件夹中。

```R
install.packages("Rcpp", lib="%R_HOME%/library");
```

## <a name="update-the-system-environment-variables"></a>更新系统环境变量

1. 将 R_HOME 添加或修改为系统环境变量。
    + 在 Windows 搜索框中，键入“环境”，然后选择“编辑系统环境变量”。
    + 在“高级”选项卡中，选择“环境变量” 。

    + 在“系统变量”下，选择“新建”以创建 R_HOME。 
    若要进行修改，请选择“编辑”以进行更改。 修改其值以指向自定义 R 安装路径。

    ![创建 R_HOME 系统环境变量。](../install/media/sys-env-r-home.png)

2. 更新 PATH 环境变量。
    将 R.dll 的路径追加到系统 PATH 环境变量。  选择“PATH”，然后选择“编辑”，并将  `%R_HOME%\bin\x64` 添加到路径列表。

    ![追加到 PATH 系统环境变量。](../install/media/sys-env-path-r.png)

3. 选择“确定”关闭剩余的窗口。

    作为替代方法，若要从提升的命令提示符设置这些环境变量，请运行以下命令。 请确保使用自定义 R 安装路径。

```CMD
setx /m R_HOME "path\to\installation\of\R"
setx /m PATH "path\to\installation\of\R\bin\x64;%PATH%"
```

## <a name="grant-access-to-the-custom-r-installation-folder"></a>授权访问自定义 R 安装文件夹

> [!NOTE]
>如果已在默认位置 C:\Program Files\R\R-version 安装 R，则可以跳过此步骤。

从新的提升的命令提示符运行 icacls 命令，以授予对 SQL Server Launchpad 服务用户名和 SID S-1-15-2-1 (ALL APPLICATION PACKAGES) 的读取和执行访问权限。   Launchpad 服务用户名的格式为 `NT Service\MSSQLLAUNCHPAD$INSTANCENAME`，其中 `INSTANCENAME` 是 SQL Server 的实例名称。

这些命令将以递归方式授予给定目录路径下的所有文件和文件夹的访问权限。

将实例名称追加到 `MSSQLLAUNCHPAD` (`MSSQLLAUNCHPAD$INSTANCENAME`)。 在此示例中，`INSTANCENAME` 是 `MSSQLSERVER` 的默认实例。

1. 授予对 SQL Server Launchpad 服务用户名的权限

    ```cmd
    icacls "%R_HOME%" /grant "NT Service\MSSQLLAUNCHPAD$MSSQLSERVER":(OI)(CI)RX /T
    ```
2. 授予对 SID S-1-15-2-1 的权限

    ```cmd
    icacls "%R_HOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T

>[!NOTE]
>The preceding command grants permissions to the computer **SID S-1-15-2-1**, which is equivalent to ALL APPLICATION PACKAGES on an English version of Windows. Alternatively, you can use `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` on an English version of Windows.

## Restart SQL Server Launchpad service

From an *elevated* command prompt, run the following commands. Replace "MSSQLSERVER" with the name of your SQL Server instance.


```CMD
net stop MSSQLLAUNCHPAD$MSSQLSERVER
net start MSSQLLAUNCHPAD$MSSQLSERVER
```

作为替代方法，请右键单击系统的“服务”应用中的 SQL Server Launchpad 服务，然后选择“重启”命令。  或者使用 [SQL Server 配置管理器](../../relational-databases/sql-server-configuration-manager.md)重启服务。

## <a name="download-r-language-extension"></a>下载 R 语言扩展

下载[包含用于 Windows 的 R 语言扩展的 zip 文件](https://github.com/microsoft/sql-server-language-extensions/releases)。 建议在生产环境中使用发行版。 在开发或测试中使用调试版本，因为它提供了详细的日志记录信息，可用于调查任何错误。

## <a name="register-external-language"></a>注册外部语言

将此 R 语言扩展注册到你要在其中使用此扩展的每个数据库的 [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md)。 使用 [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) 连接到 SQL Server 并运行以下 T-SQL 命令。
修改此语句中的路径，以反映下载的语言扩展 zip 文件 (R-lang-extension) 的位置。

> [!NOTE]
>R 是保留字。 为外部语言使用不同的名称，例如“myR”。

```sql
CREATE EXTERNAL LANGUAGE [myR]
FROM (CONTENT = N'/path/to/R-lang-extension.zip', FILE_NAME = 'libRExtension.dll');
GO
```

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

你可以安装 SQL Server on Red Hat Enterprise Linux (RHEL)、SUSE Linux Enterprise Server (SLES) 和 Ubuntu。 有关详细信息，请参阅 [Linux 上的 SQL Server 安装指南中的“受支持的平台”部分](../../linux/sql-server-linux-setup.md#supportedplatforms)。

> [!NOTE]
> 本文介绍如何在 Linux 上安装适用于 R 的自定义运行时。 若要在 Windows 上安装，请参阅[在 Windows 上安装适用于 SQL Server 的 R 自定义运行时](custom-runtime-r.md?view=sql-server-ver15&preserve-view=true)

## <a name="pre-install-checklist"></a>安装前清单

安装 R 自定义运行时之前，请安装以下各项：

+ [适用于 Linux 的 SQL Server 2019（累积更新 3 或更高版本）](../../linux/sql-server-linux-setup.md)。
在 Linux 上安装 SQL Server 前，须配置 Microsoft 存储库。 有关详细信息，请参阅[配置存储库](../../linux/sql-server-linux-change-repo.md)。

+ [通过扩展性框架在 Linux 上安装 SQL Server 语言扩展](../../linux/sql-server-linux-setup-language-extensions-java.md)。

+ [R 3.3 或更高版本](https://cran.r-project.org/)。

## <a name="add-sql-server-language-extensions-for-linux"></a>添加适用于 Linux 的 SQL Server 语言扩展

> [!NOTE]
> 如果已在 SQL Server 2019 上安装机器学习服务，则已安装用于语言扩展的 **mssql-server-extensibility** 包，你可以跳过此步骤。

语言扩展使用扩展性框架来执行外部代码。 代码执行与核心引擎进程隔离，但与 SQL Server 查询执行完全集成。

根据你的 Linux 版本，使用以下命令安装语言扩展。

### <a name="ubuntu"></a>Ubuntu
> [!Tip]
> 如果可以，请在安装之前运行 `sudo apt-get update` 以刷新系统中的包。 Ubuntu 可能没有 https apt 传输选项。 若要进行安装，请使用 `sudo apt-get install apt-transport-https`。

```bash
# Install as root or sudo
sudo apt-get install mssql-server-extensibility
```

### <a name="red-hat"></a>Red Hat
```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

### <a name="suse"></a>Suse
```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## <a name="install-r"></a>安装 R

>[!NOTE]
>对于 SQL 机器学习服务，已在 `/opt/microsoft/ropen/3.5.2/lib64/R` 中安装 R。 如果要继续使用此路径作为 R_HOME，请跳到安装 Rcpp 的下一步。 

如果要使用 R 的其他运行时，首先需要删除 `microsoft-r-open-mro`，然后再继续安装新版本。 Ubuntu 的示例：

```bash
sudo apt remove microsoft-r-open-mro-3.5.2
```

按照[说明](https://cran.r-project.org/bin/linux/)，为相应的 linux 平台完成 R（3.3 或更高版本）的安装。 默认情况下，R 安装在 /usr/lib/R 中。 此路径是 R_HOME。 如果在其他位置安装 R，请记下该路径作为 R_HOME。

Ubuntu 的示例说明。 为你的 R 版本更改以下存储库 URL。

```bash
export DEBIAN_FRONTEND=noninteractive
sudo apt-get update
sudo apt-get --no-install-recommends -y install curl zip unzip apt-transport-https libstdc++6

# Add R CRAN repository. This repository works for R 4.0.x.
#
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran40/'
sudo apt-get update

# Install R runtime.
#
sudo apt-get -y install r-base-core
```

## <a name="install-rcpp-package"></a>安装 Rcpp 包

在以下说明中，${R_HOME} 是 R 安装的路径。 

+ 在 ${R_HOME}/bin 中找到 R 二进制文件。 默认情况下，它位于 /usr/lib/R/bin 中。

+ 启动 R

  ```bash
  sudo ${R_HOME}/bin/R
  ```

+ 在此提升的 R 提示符 (${R_HOME}/bin/R) 中，运行以下脚本，以将 Rcpp 包安装到 ${R_HOME}/library 文件夹中。

  ```R
  install.packages("Rcpp", lib = "${R_HOME}/library");
  ```

## <a name="using-a-custom-installation-of-r"></a>使用 R 的自定义安装

> [!NOTE]
>如果已在默认位置 /usr/lib/R 安装 R，则可以跳过本节。

### <a name="update-the-environment-variables"></a>更新环境变量

1. 编辑 mssql-launchpadd 服务，将 R_HOME 环境变量添加到 `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` 文件

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

    + 在打开的 `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` 文件中插入以下文本。 将 R_HOME 的值设置为自定义 R 安装路径。

    ```text
    [Service]
    Environment="R_HOME=/path/to/installation/of/R"
    ```

    + 保存并关闭。

2. 确保可以加载 libR.so。

    + 在 /etc/ld.so.conf.d 中创建 custom-r.conf 文件。

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-r.conf
    ```

    + 在打开的文件中，添加从自定义 R 安装到 libR.so 的路径。

    ```vi editor
    /path/to/installation/of/R/lib
    ```

    + 保存并关闭新文件。

    + 运行 `ldconfig`，然后运行以下命令，并检查是否可以找到所有依赖库，来验证是否可以加载 libR.so。

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/R/lib/libR.so
    ```

### <a name="grant-access-to-the-custom-r-installation-folder"></a>授权访问自定义 R 安装文件夹

将 /var/opt/mssql/mssql.conf 文件扩展性部分中的 `datadirectories` 选项设置为自定义 R 安装。

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/R
```

### <a name="restart-mssql-launchpadd-service"></a>重启 mssql-launchpadd 服务

> [!NOTE]
>如果已在默认位置 /usr/lib/R 安装 R，则可以跳过此步骤。

```bash
sudo systemctl restart mssql-launchpadd
```

## <a name="download-r-language-extension"></a>下载 R 语言扩展

下载[包含用于 Linux 的 R 语言扩展的 zip 文件](https://github.com/microsoft/sql-server-language-extensions/releases)。 建议在生产环境中使用发行版。 在开发或测试中使用调试版本，因为它提供了详细的日志记录信息，可用于调查任何错误。

## <a name="register-external-language"></a>注册外部语言

将此 R 语言扩展注册到你要在其中使用此扩展的每个数据库的 [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md)。 使用 [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) 连接到 SQL Server 并运行以下 T-SQL 命令。 
修改此语句中的路径，以反映下载的语言扩展 zip 文件 (r-lang-extension) 的位置。


> [!NOTE]
>R 是保留字。 为外部语言使用不同的名称，例如“myR”。

```sql
CREATE EXTERNAL LANGUAGE [myR]
FROM (CONTENT = N'/path/to/R-lang-extension.zip', FILE_NAME = 'libRExtension.so.1.0');
GO
```

::: moniker-end

## <a name="enable-external-script-execution-in-sql-server"></a>在 SQL Server 中启用外部脚本执行

可以通过针对 SQL Server 运行的存储过程 [sp_execute_external 脚本](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)，来执行以 R 语言编写的外部脚本。 

若要启用外部脚本，请使用连接到 SQL Server 的 [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) 执行以下 SQL 命令。

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;
```

## <a name="verify-language-extension-installation"></a>验证语言扩展安装

此 SQL 脚本验证自定义 R 语言扩展的安装是否成功。 此脚本的输出应显示 R_HOME、R 的路径以及自定义 R 运行时的版本。 它确认脚本使用你的自定义运行时。

```sql
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(R.home());
print(file.path(R.home("bin"), "R"));
print(R.version);
print("Hello RExtension!");'
```

## <a name="verify-parameters-and-datasets-of-different-data-types"></a>验证不同数据类型的参数和数据集

此脚本为输入/输出参数和数据集测试不同的数据类型。

```sql
DECLARE @sumVal INT = 12;
DECLARE @charVal VARCHAR(30) = N'Hello';
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(sumVal);
print(charVal);
sumVal <- sumVal + 300;
OutputDataSet <- InputDataSet;'
    ,@input_data_1 = N'select 1, cast(1.4 as real), ''Hi'', cast(''1'' as bit)'
    ,@params = N'@sumVal int OUTPUT, @charVal VARCHAR(30)'
    ,@sumVal = @sumVal OUTPUT
    ,@charVal =  @charVal
WITH RESULT SETS ((intCol INT, doubleCol REAL, charCol CHAR(2), logicalCol BIT));
PRINT @sumVal;
```

## <a name="see-also"></a>另请参阅

+ [SQL Server 中的扩展性框架](../concepts/extensibility-framework.md)
+ [语言扩展概述](../../language-extensions/language-extensions-overview.md)