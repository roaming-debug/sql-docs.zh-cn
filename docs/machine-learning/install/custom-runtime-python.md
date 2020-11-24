---
title: 安装 Python 自定义运行时
description: 了解如何为 SQL Server 安装 Python 自定义运行时。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/20/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 2a37b086804a8fabe3719db0744b49345d69e6b8
ms.sourcegitcommit: 2bf83972036bdbe6a039fb2d1fc7b5f9ca9589d3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94674135"
---
# <a name="install-a-python-custom-runtime-for-sql-server"></a>为 SQL Server 安装 Python 自定义运行时
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

本文介绍如何安装自定义运行时，以便使用 SQL Server 运行 Python 脚本。 自定义运行时使用在可扩展性框架上生成的语言扩展来执行外部代码。 适用于 Python 的自定义运行时可用于以下方案：

+ 使用扩展性框架安装 SQL Server。

+ 使用 SQL Server 2019 安装机器学习服务。 完成一些其他配置步骤后，可将语言扩展与用于 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)。

::: moniker range=">=sql-server-ver15||=sqlallproducts-allversions"

> [!NOTE]
> 本文介绍如何在 Windows 上安装适用于 Python 的自定义运行时。 若要在 Linux 上安装，请参阅[在 Linux 上安装适用于 SQL Server 的 Python 自定义运行时](custom-runtime-python.md?view=sql-server-linux-ver15&preserve-view=true)。

## <a name="pre-install-checklist"></a>安装前清单

安装 Python 自定义运行时之前，请安装以下各项：

+ [适用于 Windows 的 SQL Server 2019 累积更新 (CU) 3](../../database-engine/install-windows/install-sql-server.md)。

+ [通过扩展性框架在 Windows 上安装 SQL Server 语言扩展](../../language-extensions/install/windows-java.md)。

+ [Python 3.7]( https://www.python.org/downloads/release/python-379/).

## <a name="add-sql-server-language-extensions-for-windows"></a>添加适用于 Windows 的 SQL Server 语言扩展

> [!NOTE]
> 如果已在 SQL Server 2019 上安装机器学习服务，则已安装扩展性框架，你可以跳过此步骤。

语言扩展使用扩展性框架来执行外部代码。 代码执行与核心引擎进程隔离，但与 SQL Server 查询执行完全集成。

1. 启动 SQL Server 2019 的安装向导。
  
1. 在“安装”选项卡上，选择“全新 SQL Server 独立安装或向现有安装添加功能”   。
    
    ![SQL Server 2019 安装 CU3 或更高版本](../install/media/2019setup-installation-page-mlsvcs.png) 

1. 在“功能选择”  页上，选择以下选项：
  
    - **数据库引擎服务**
  
        要将语言扩展与 SQL Server 结合使用，必须安装数据库引擎的实例。 可使用默认实例或命名实例。
  
    - **机器学习服务和语言扩展**
   
       选择“机器学习服务和语言扩展”。 无需选择 Python。

    ![SQL Server 2019 CU3 或更高版本的安装功能](../install/media/sql-feature-selection.png) 

1. 在“准备安装”页面上，验证是否已包括这些选择，然后选择“安装”   。
  
    + 数据库引擎服务
    + 机器学习服务和语言扩展

1. 安装完成后，如果收到重启计算机的指示，请立即重启。 安装完成后，请务必阅读来自安装向导的消息。 有关详细信息，请参阅 [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。


## <a name="install-python-37"></a>安装 Python 3.7 

安装 [Python 3.7]( https://www.python.org/downloads/release/python-379/) 并将其添加到 PATH。

![将 Python 3.7 添加到路径。](../install/media/python-379.png) 


#### <a name="install-pandas"></a>安装 pandas

从提升的命令提示符安装适用于 Python 的 [pandas](https://pandas.pydata.org/) 包：

```bash
python.exe -m pip install pandas
```

## <a name="update-the-system-environment-variables"></a>更新系统环境变量

将 PYTHONHOME 添加或修改为系统环境变量。

+ 在 Windows 搜索框中，键入“环境”，然后选择“编辑系统环境变量”。
+ 在“高级”选项卡中，选择“环境变量” 。
+ 在“系统变量”下，选择“新建”以创建指向 Python 3.7 安装位置的 PYTHONHOME。 
如果 PYTHONHOME 已存在，请选择“编辑”将其指向 Python 3.7 安装位置。
+ 选择“确定”关闭剩余的窗口。

![创建 PYTHONHOME 系统变量。](../install/media/sys-pythonhome.png)

## <a name="grant-access-to-the-custom-python-installation-folder"></a>授权访问自定义 Python 安装文件夹

从新的提升的命令提示符运行以下 icacls 命令，以向 PYTHONHOME 授予对 SQL Server Launchpad 服务和 SID S-1-15-2-1 (ALL_APPLICATION_PACKAGES) 的读取和执行访问权限。   Launchpad 服务用户名为：`NT Service\MSSQLLAUNCHPAD$INSTANCENAME* where INSTANCENAME` 是 SQL Server 的实例名称。 这些命令将以递归方式授予给定目录路径下的所有文件和文件夹的访问权限。

将实例名称追加到 `MSSQLLAUNCHPAD` (`MSSQLLAUNCHPAD$INSTANCENAME`)。 在此示例中，INSTANCENAME 是默认实例 `MSSQLSERVER`。

1. 授予对 SQL Server Launchpad 服务用户名的权限。

    ```cmd
    icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD$MSSQLSERVER":(OI)(CI)RX /T

2. Give permissions to **SID S-1-15-2-1**.
    ```cmd
    icacls "%PYTHONHOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T

>[!NOTE]
>The preceding command grants permissions to the computer **SID S-1-15-2-1**, which is equivalent to ALL APPLICATION PACKAGES on an English version of Windows. Alternatively, you can use `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` on an English version of Windows.

## Restart SQL Server Launchpad service

From an *elevated* command prompt, run the following commands. Replace "MSSQLSERVER" with the name of your SQL Server instance.

```CMD
net stop MSSQLLAUNCHPAD$MSSQLSERVER
net start MSSQLLAUNCHPAD$MSSQLSERVER
```

或者，右键单击系统的“服务”应用中的 SQL Server Launchpad 服务，然后单击“重启”命令。  或者使用 [SQL Server 配置管理器](../../relational-databases/sql-server-configuration-manager.md)重启服务。

## <a name="download-python-language-extension"></a>下载 Python 语言扩展

下载[包含用于 Windows 的 Python 语言扩展的 zip 文件](https://github.com/microsoft/sql-server-language-extensions/releases)。 建议在生产环境中使用发行版。 在开发或测试中使用调试版本，因为它提供了详细的日志记录信息，可用于调查任何错误。

## <a name="register-external-language"></a>注册外部语言

将此 Python 语言扩展注册到你要在其中使用此扩展的每个数据库的 [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md)。 使用 [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) 连接到 SQL Server 并运行以下 T-SQL 命令。 修改此语句中的路径，以反映下载的语言扩展 zip 文件 (python-lang-extension) 的位置。

> [!NOTE]
> Python 是保留字。 为外部语言使用不同的名称，例如“myPython”。

```sql
CREATE EXTERNAL LANGUAGE [myPython]
FROM (CONTENT = N'/path/to/python-lang-extension.zip', FILE_NAME = 'pythonextension.dll');
GO
```

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=sqlallproducts-allversions"

你可以安装 SQL Server on Red Hat Enterprise Linux (RHEL)、SUSE Linux Enterprise Server (SLES) 和 Ubuntu。 有关详细信息，请参阅 [Linux 上的 SQL Server 安装指南中的“受支持的平台”部分](../../linux/sql-server-linux-setup.md)。

> [!NOTE]
> 本文介绍如何在 Linux 上安装适用于 Python 的自定义运行时。 若要在 Windows 上安装，请参阅[在 Windows 上安装适用于 SQL Server 的 Python 自定义运行时](custom-runtime-python.md?view=sql-server-ver15&preserve-view=true)

## <a name="pre-install-checklist"></a>安装前清单

安装 Python 自定义运行时之前，请安装以下各项：

+ [适用于 Linux 的 SQL Server 2019（累积更新 3 或更高版本）](../../linux/sql-server-linux-setup.md)。
在 Linux 上安装 SQL Server 时，须配置 Microsoft 存储库。 有关详细信息，请参阅[配置存储库](../../linux/sql-server-linux-change-repo.md)

  > [!NOTE]
  > Python 自定义运行时需要 SQL Server 2019 的累积更新 (CU) 3 或更高版本。

+ [通过扩展性框架在 Linux 上安装 SQL Server 语言扩展](../../linux/sql-server-linux-setup-language-extensions-java.md)。

+ [Python 3.7](https://www.python.org/downloads/release/python-379/).

## <a name="add-sql-server-language-extensions-for-linux"></a>添加适用于 Linux 的 SQL Server 语言扩展

> [!NOTE]
> 如果已在 SQL Server 2019 上安装机器学习服务，则已安装用于语言扩展的 **mssql-server-extensibility** 包，你可以跳过此步骤。

语言扩展使用扩展性框架来执行外部代码。 代码执行与核心引擎进程隔离，但与 SQL Server 查询执行完全集成。

根据你的 Linux 版本，使用以下命令安装语言扩展。

### <a name="ubuntu"></a>Ubuntu
> [!TIP]
> 如果可以，请在安装之前运行 `update` 以刷新系统中的包。 Ubuntu 可能没有 https apt 传输选项。 若要进行安装，请使用 `apt-get install apt-transport-https`。

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

## <a name="using-a-custom-installation-of-python-37"></a>使用 Python 3.7 的自定义安装

> [!NOTE]
> 如果已在默认位置 `/usr/lib/python3.7` 安装 Python，可以跳到[下一节](#download-python-linux)。

如果你构建了自己的 Python 3.7 版本，请使用以下命令，使 SQL Server 能够找到并加载你的自定义安装。

### <a name="update-the-environment-variables"></a>更新环境变量

1. 编辑 mssql-launchpadd 服务，将 PYTHONHOME 环境变量添加到 `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` 文件

      ```bash
      sudo systemctl edit mssql-launchpadd
      ```

    + 在打开的 `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` 文件中插入以下文本。 将 PYTHONHOME 的值设置为自定义 Python 安装路径。

      ```vi
      [Service]
      Environment="PYTHONHOME=/path/to/installation/of/python3.7"
      ```

    + 保存并关闭。

2. 请确保可加载 `libpython3.7m.so.1.0`。

    + 在 `/etc/ld.so.conf.d` 中创建 custom-python.conf 文件。

      ```bash
      sudo vi /etc/ld.so.conf.d/custom-python.conf
      ```

    + 在打开的文件中，添加从自定义 Python 安装到 libpython3.7m.so.1.0 的路径。

      ```vi
      /path/to/installation/of/python3.7/lib
      ```

    + 保存并关闭新文件。

    + 运行 `ldconfig`，然后运行以下命令，并检查是否可以找到所有依赖库，来验证是否可以加载 `libpython3.7m.so.1.0`。

      ```bash
      sudo ldconfig
      ldd /path/to/installation/of/python3.7/lib/libpython3.7m.so.1.0
      ```

### <a name="grant-access-to-the-custom-python-folder"></a>授权访问自定义 Python 文件夹

将 /var/opt/mssql/mssql.conf 文件扩展性部分中的 `datadirectories` 选项设置为自定义 python 安装。

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/python3.7
```

### <a name="restart-the-mssql-launchpadd-service"></a>重启 mssql-launchpadd 服务

```bash
sudo systemctl restart mssql-launchpadd
```
## <a name="download-python-language-extension"></a><a name="download-python-linux"></a> 下载 Python 语言扩展

下载[包含用于 Linux 的 Python 语言扩展的 zip 文件](https://github.com/microsoft/sql-server-language-extensions/releases)。 建议在生产环境中使用发行版。 在开发或测试中使用调试版本，因为它提供了详细的日志记录信息，可用于调查任何错误。

## <a name="register-external-language"></a>注册外部语言

将此 Python 语言扩展注册到你要在其中使用此扩展的每个数据库的 [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md)。 使用 [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) 连接到 SQL Server 并运行以下 T-SQL 命令。 
修改此语句中的路径，以反映下载的语言扩展 zip 文件 (python-lang-extension) 的位置。

> [!NOTE]
>Python 是保留字。 为外部语言使用不同的名称，例如“myPython”。

```sql
CREATE EXTERNAL LANGUAGE myPython 
FROM (CONTENT = N'/PATH/TO/python-lang-extension.zip', FILE_NAME = 'libPythonExtension.so.1.0');
GO
```

::: moniker-end

## <a name="enable-external-script-execution-in-sql-server"></a>在 SQL Server 中启用外部脚本执行

可以通过针对 SQL Server 运行的存储过程 [sp_execute_external 脚本](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)，来执行以 Python 语言编写的外部脚本。 

若要启用外部脚本，请使用连接到 SQL Server 的 [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) 执行以下 SQL 命令。

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-language-extension-installation"></a>验证语言扩展安装

此 SQL 脚本将测试已安装的语言扩展的功能。

```sql
EXEC sp_execute_external_script
@language =N'myPython',
@script=N'
import sys
print(sys.path)
print(sys.version)
print(sys.executable)'
```

## <a name="verify-parameters-and-datasets-of-different-data-types"></a>验证不同数据类型的参数和数据集

此脚本为输入/输出参数和数据集测试不同的数据类型。

```sql
DECLARE @sumVal int = 12;
DECLARE @charVal VARCHAR(30) = N'Hello'

EXEC sp_execute_external_script
@language =N'myPython',
@script=N'
print(sumVal)
print(charVal)
sumVal = sumVal + 300
OutputDataSet = InputDataSet'
,@input_data_1 = N'SELECT 1, CAST(1.4 as real), ''Hi'', CAST(''1'' as bit)'
,@params = N'@sumVal int OUTPUT, @charVal VARCHAR(30)'
,@sumVal = @sumVal OUTPUT
,@charVal = @charVal
WITH RESULT SETS ((intCol int, doubleCol real, charCol char(2), logicalCol bit));

PRINT @sumVal
```

## <a name="next-steps"></a>后续步骤

+ [SQL Server 中的扩展性框架](../concepts/extensibility-framework.md)
+ [语言扩展概述](../../language-extensions/language-extensions-overview.md)
