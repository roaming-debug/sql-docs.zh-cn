---
title: 在 Linux 上安装
titleSuffix: SQL Server Machine Learning Services
description: 了解如何在 Linux 上安装 SQL Server 机器学习服务（Python 和 R）：Red Hat、Ubuntu 和 SUSE。
author: dphansen
ms.author: davidph
manager: cgronlun
ms.date: 11/24/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 462857bd0a9aac7b63659bad809dcdc9a577f3b0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031489"
---
# <a name="install-sql-server-machine-learning-services-python-and-r-on-linux"></a>在 Linux 上安装 SQL Server 机器学习服务（Python 和 R）

[!INCLUDE [SQL Server 2019 - Linux](../includes/applies-to-version/sqlserver2019-linux.md)]

本文指导你在 Linux 上安装 [SQL Server 机器学习服务](../machine-learning//sql-server-machine-learning-services.md)。 可使用机器学习服务在数据库中执行 Python 和 R 脚本。

可在 Red Hat Enterprise Linux (RHEL)、SUSE Linux Enterprise Server (SLES) 和 Ubuntu 上安装机器学习服务。 有关详细信息，请参阅 [Linux 上的 SQL Server 安装指南中的“受支持的平台”部分](sql-server-linux-setup.md#supportedplatforms)。

> [!NOTE]
> 默认情况下，机器学习服务安装在 SQL Server 大数据群集上。 有关详细信息，请参阅[在大数据群集上使用机器学习服务（Python 和 R）](../big-data-cluster/machine-learning-services.md)

<a name="mro"></a>

## <a name="pre-install-checklist"></a>安装前清单

* [安装 Linux 上的 SQL Server](sql-server-linux-setup.md)，然后验证安装。

* 查看 SQL Server Linux 存储库中的 Python 和 R 扩展。 
  如果已为数据库引擎安装配置了源存储库，则可以使用相同的存储库注册运行 mssql-mlservices 包安装命令  。

* （仅限 R）Microsoft R Open (MRO) 为 SQL Server 中的 R 功能提供 R 基础发行版，要使用通过机器学习服务安装的 RevoScaleR、MicrosoftML 和其他 R 包，则该基础发行版是一种先决条件。
    * 所需版本为 MRO 3.5.2。
    * 从以下两种安装 MRO 的方法中进行选择：
        * 从 MRAN 下载 MRO tarball，解压缩并运行其 install.sh 脚本。 如果要使用此方法，可以按照 [MRAN 上的安装说明](https://mran.microsoft.com/releases/3.5.2)进行操作。
        * 如下所述注册 **packages.microsoft.com** 存储库以安装 MRO 发行版：microsoft-r-open-mro 和 microsoft-r-open-mkl。 
    * 若要了解如何安装 MRO，请参阅下面的安装部分。

* 应安装用于运行 T-SQL 命令的工具。 

  * 你可使用免费的 [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) 数据库工具，该工具在 Linux、Windows 和 macOS 上运行。

## <a name="package-list"></a>包列表

在已连接 Internet 的设备上，将使用针对每个操作系统的包安装程序独立于数据库引擎下载和安装相应的包。 下表介绍了所有可用包，但对于 R 和 Python，需要指定提供完整功能安装或最小功能安装的包。

可用安装包：

| 包名称 | 适用对象 | 说明 |
|--------------|----------|-------------|
|mssql-server-extensibility  | All | 用于运行 Python 和 R 的扩展性框架。 |
| microsoft-openmpi  | Python、R | Rev* 库在 Linux 上进行并行化所使用的消息传递接口。 |
| mssql-mlservices-python | Python | Anaconda 和 Python 的开放源代码发行版。 |
|mssql-mlservices-mlm-py  | Python | 完全安装  。 提供用于图像特征化和文本情绪分析的 revoscalepy、microsoftml、预先训练的模型。| 
|mssql-mlservices-packages-py  | Python | 最小安装  。 提供 revoscalepy 和 microsoftml。 <br/>不包括预先训练的模型。 | 
| [microsoft-r-open*](#mro) | R | 由三个包组成的 R 的开放源代码发行版。 |
|mssql-mlservices-mlm-r  | R | 完全安装  。 提供：用于图像特征化和文本情绪分析的 RevoScaleR、MicrosoftML、sqlRUtils、olapR、预先训练的模型。| 
|mssql-mlservices-packages-r  | R | 最小安装  。 提供 RevoScaleR、sqlRUtils、MicrosoftML、olapR。 <br/>不包括预先训练的模型。 |

<a name="RHEL"></a>

## <a name="install-on-rhel"></a>在 RHEL 上安装

请按照以下步骤在 Red Hat Enterprise Linux (RHEL) 上安装 SQL Server 机器学习服务。

### <a name="install-mro-on-rhel"></a>在 RHEL 上安装 MRO

以下命令可注册提供 MRO 的存储库。 注册后，用于安装其他 R 包的命令（如 mssql-mlservices-mml-r）将自动包含作为包依赖项的 MRO。
```bash
# Import the Microsoft repository key

sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

# Set the location of the package repo at the "prod" directory
# The following command is for version 7.x
# For 6.x, replace 7 with 6 to get that version
rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm

# Update packages on your system (optional)
yum update
```

Python 和 R 的安装选项：

*  根据你的要求安装语言支持（一种或多种语言）。
*  完全安装  提供了所有可用功能，包括经过预先训练的机器学习模型。
*  最小安装  不包括模型，但仍具备所有功能。

> [!Tip]
> 如果可以，请在安装之前运行 `yum clean all` 以刷新系统中的包。

### <a name="full-installation"></a>完全安装

包括：
*  开放源代码 Python
*  开放源代码 R
*  可扩展性框架
*  Microsoft-openmpi
*  扩展（Python、R）
*  机器学习库
*  用于 Python 和 R 的预训练模型

```bash
# Install as root or sudo
# Add everything (all R, Python)
# Be sure to include -9.4.7* in mlsservices package names
sudo yum install mssql-mlservices-mlm-py-9.4.7*
sudo yum install mssql-mlservices-mlm-r-9.4.7*
```

### <a name="minimum-installation"></a>最小安装

包括：
*  开放源代码 Python
*  开放源代码 R
*  可扩展性框架
*  Microsoft-openmpi
*  核心 Revo* 库
*  机器学习库

```bash
# Install as root or sudo
# Minimum install of R, Python extensions
# Be sure to include -9.4.6* in mlsservices package names
sudo yum install mssql-mlservices-packages-py-9.4.7*
sudo yum install mssql-mlservices-packages-r-9.4.7*
```
<a name="ubuntu"></a>

## <a name="install-on-ubuntu"></a>在 Ubuntu 上安装

请按照以下步骤在 Ubuntu 上安装 SQL Server 机器学习服务。

### <a name="install-mro-on-ubuntu"></a>在 Ubuntu 上安装 MRO

以下命令可注册提供 MRO 的存储库。 注册后，用于安装其他 R 包的命令（如 mssql-mlservices-mml-r）将自动包含作为包依赖项的 MRO。

```bash
# Install as root
sudo su

# Optionally, if your system does not have the https apt transport option
apt-get install apt-transport-https

# Set the location of the package repo the "prod" directory containing the distribution.
# This example specifies 16.04. Replace with 14.04 if you want that version
wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb

# Register the repo
dpkg -i packages-microsoft-prod.deb

# Update packages on your system (required), including MRO installation
sudo apt-get update
```

Python 和 R 的安装选项：

*  根据你的要求安装语言支持（一种或多种语言）。
*  完全安装  提供了所有可用功能，包括经过预先训练的机器学习模型。
*  最小安装  不包括模型，但仍具备所有功能。

> [!Tip]
> 如果可以，请在安装之前运行 `apt-get update` 以刷新系统中的包。 

### <a name="full-installation"></a>完全安装 

包括：
*  开放源代码 Python
*  开放源代码 R
*  可扩展性框架
*  Microsoft-openmpi
*  Python 扩展
*  R 扩展
*  机器学习库
*  用于 Python 和 R 的预训练模型

```bash
# Install as root or sudo
# Add everything (all R, Python)
# There is no asterisk in this full install
sudo apt-get install mssql-mlservices-mlm-py 
sudo apt-get install mssql-mlservices-mlm-r 
```

### <a name="minimum-installation"></a>最小安装 

包括：
*  开放源代码 Python
*  开放源代码 R
*  可扩展性框架
*  Microsoft-openmpi
*  核心 Revo* 库
*  机器学习库

```bash
# Install as root or sudo
# Minimum install of R, Python
# No asterisk
sudo apt-get install mssql-mlservices-packages-py
sudo apt-get install mssql-mlservices-packages-r
```

<a name="SLES"></a>

## <a name="install-on-sles"></a>在 SLES 上安装

请按照以下步骤在 SUSE Linux Enterprise Server (SLES) 上安装 SQL Server 机器学习服务。

### <a name="install-mro-on-sles"></a>在 SLES 上安装 MRO

以下命令可注册提供 MRO 的存储库。 注册后，用于安装其他 R 包的命令（如 mssql-mlservices-mml-r）将自动包含作为包依赖项的 MRO。

```bash
# Install as root
sudo su

# Set the location of the package repo at the "prod" directory containing the distribution
# This example is for SLES12, the only supported version of SLES in Machine Learning Server
zypper ar -f https://packages.microsoft.com/sles/12/prod packages-microsoft-com

# Update packages on your system (optional)
zypper update
```

Python 和 R 的安装选项：

*  根据你的要求安装语言支持（一种或多种语言）。
*  完全安装  提供了所有可用功能，包括经过预先训练的机器学习模型。
*  最小安装  不包括模型，但仍具备所有功能。

### <a name="full-installation"></a>完全安装 

包括：
*  开放源代码 Python
*  开放源代码 R
*  可扩展性框架
*  Microsoft-openmpi
*  Python 和 R 的扩展
*  机器学习库
*  用于 Python 和 R 的预训练模型

```bash
# Install as root or sudo
# Add everything (all R, Python)
sudo zypper install mssql-mlservices-mlm-py
sudo zypper install mssql-mlservices-mlm-r
```

### <a name="minimum-installation"></a>最小安装 

包括：
*  开放源代码 Python
*  开放源代码 R
*  可扩展性框架
*  Microsoft-openmpi
*  核心 Revo* 库
*  机器学习库 

```bash
# Install as root or sudo
# Minimum install of R, Python extensions
sudo zypper install mssql-mlservices-packages-py
sudo zypper install mssql-mlservices-packages-r
```

## <a name="post-install-config-required"></a>安装后配置（必需）

其他配置主要通过 [mssql-conf 工具](sql-server-linux-configure-mssql-conf.md)完成。

1. 包安装完成后，运行 mssql-conf 安装程序，按照提示设置 SA 密码并选择版本。 仅在尚未在 Linux 上配置 SQL Server 的情况下执行此步骤。 

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

2. 接受开放源代码 Python 和 R 扩展的许可协议。 使用以下命令：

   ```bash
   # Run as SUDO or root
   # Use set + EULA 
   sudo /opt/mssql/bin/mssql-conf set EULA accepteulaml Y
   ```
   安装程序检测到 mssql-mlservices 包，并在运行 `mssql-conf setup` 时提示接受 EULA（如果以前未接受）。 有关 EULA 参数的详细信息，请参阅[使用 mssql-conf 工具配置 SQL Server](sql-server-linux-configure-mssql-conf.md#mlservices-eula)。

3. 启用出站网络访问。 默认情况下，出站网络访问处于禁用状态。 若要启用出站请求，请使用 mssql-conf 工具设置“outboundnetworkaccess”布尔属性。 有关详细信息，请参阅[使用 mssql-conf 配置 Linux 上的 SQL Server](sql-server-linux-configure-mssql-conf.md#mlservices-outbound-access)。

   ```bash
   # Run as SUDO or root
   # Enable outbound requests over the network
   sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 1
   ```

4. （仅适用于 R 功能集成）请设置“MKL_CBWR”环境变量，以确保从 Intel 数学核心函数库 (MKL) 计算得到[一致的输出结果](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr)。

   + 在用户的主目录中编辑或创建文件 `.bash_profile`，并将行 `export MKL_CBWR="AUTO"` 添加到该文件中。

   + 通过在 bash 命令提示符处键入 `source .bash_profile` 来执行此文件。

5. 重启 SQL Server Launchpad 服务和数据库引擎实例，以从 INI 文件中读取更新后的值。 修改与扩展性有关的设置时，系统将显示一条通知消息。  

   ```bash
   systemctl restart mssql-launchpadd

   systemctl restart mssql-server.service
   ```

6. 使用 Azure Data Studio 或使用运行 Transact-SQL 的其他工具（如 SQL Server Management Studio，仅限 Windows）来启用外部脚本执行。

   ```sql
   EXEC sp_configure 'external scripts enabled', 1 
   RECONFIGURE WITH OVERRIDE 
   ```

7. 再次重启 Launchpad 服务。

## <a name="verify-installation"></a>验证安装

可在 `/opt/mssql/mlservices/libraries/RServer` 中找到 R 库（MicrosoftML、RevoScaleR 及其他库）。

可在 `/opt/mssql/mlservices/libraries/PythonServer` 中找到 Python 库（microsoftml 和 revoscalepy）。

验证安装：

* 运行 T-SQL 脚本，该脚本执行使用查询工具调用 Python 或 R 的系统存储过程。 

* 请执行以下 SQL 命令以测试 SQL Server 中的 R 执行。 遇到错误？ 尝试重启服务 `sudo systemctl restart mssql-server.service`。
  ```sql
  EXEC sp_execute_external_script   
  @language =N'R', 
  @script=N' 
  OutputDataSet <- InputDataSet', 
  @input_data_1 =N'SELECT 1 AS hello' 
  WITH RESULT SETS (([hello] int not null)); 
  GO 
  ```
 
* 请执行以下 SQL 命令以测试 SQL Server 中的 Python 执行。 
 
  ```sql
  EXEC sp_execute_external_script  
  @language =N'Python', 
  @script=N' 
  OutputDataSet = InputDataSet; 
  ', 
  @input_data_1 =N'SELECT 1 AS hello' 
  WITH RESULT SETS (([hello] int not null)); 
  GO 
  ```

<a name="install-all"></a>

## <a name="unattended-installation"></a>无人参与的安装

对数据库引擎使用[无人参与安装](sql-server-linux-setup.md#unattended)，添加 mssql-mlservices 和 EULA 的包。

 对开放源代码 R 和 Python 发行版使用其中某个 mlservices 特定的 EULA 参数：

```bash
sudo /opt/mssql/bin/mssql-conf setup accept-eula-ml
```

[使用 mssql-con 工具配置 Linux 上的 SQL Server](sql-server-linux-configure-mssql-conf.md#mlservices-eula) 中记录了完整的 EULA。

## <a name="offline-installation"></a>脱机安装

有关安装包的步骤，请按照[脱机安装](sql-server-linux-setup.md#offline)说明进行操作。 找到下载网站，然后使用下面的包列表下载特定的包。

> [!Tip]
> 一些包管理工具提供可帮助确定包依赖项的命令。 对于 yum，请使用 `sudo yum deplist [package]`。 对于 Ubuntu，请在使用 `sudo apt-get install --reinstall --download-only [package name]` 之后使用 `dpkg -I [package name].deb`。

 
### <a name="download-site"></a>下载站点

通过 [https://packages.microsoft.com/](https://packages.microsoft.com/) 下载包。 Python 和 R 的所有 mlservices 包都与数据库引擎包位于相同位置。 mlservices 包的基础版本为 9.4.6。 回想一下，microsoft-r-open 包位于[其他存储库](#mro)中。

### <a name="rhel7-paths"></a>RHEL/7 路径

|程序包|下载位置|
|--|----|
| mssql/mlservices 包 | [https://packages.microsoft.com/rhel/7/mssql-server-2019/](https://packages.microsoft.com/rhel/7/mssql-server-2019/) |
| microsoft-r-open 包 | [https://packages.microsoft.com/rhel/7/prod/](https://packages.microsoft.com/rhel/7/prod/) | 

### <a name="ubuntu1604-paths"></a>Ubuntu/16.04 路径

|程序包|下载位置|
|--|----|
| mssql/mlservices 包 | [https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/) |
| microsoft-r-open 包 | [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/) | 

### <a name="sles12-paths"></a>SLES/12 路径

|程序包|下载位置|
|--|----|
| mssql/mlservices 包 | [https://packages.microsoft.com/sles/12/mssql-server-2019/](https://packages.microsoft.com/sles/12/mssql-server-2019/) |
| microsoft-r-open 包 | [https://packages.microsoft.com/sles/12/prod/](https://packages.microsoft.com/sles/12/prod/) | 

选择要使用的扩展，下载特定语言所需的包。 文件名在后缀中包含平台信息。

### <a name="package-list"></a>包列表

根据想要使用的扩展，下载特定语言所需的包。 精确的文件名在后缀中包含平台信息，但以下文件名应足够接近，可用于确定要获取的文件。

```
# Core packages 
mssql-server-15.0.1000
mssql-server-extensibility-15.0.1000

# R
microsoft-openmpi-3.0.0
microsoft-r-open-mkl-3.5.2
microsoft-r-open-mro-3.5.2
mssql-mlservices-packages-r-9.4.7.64
mssql-mlservices-mlm-r-9.4.7.64


# Python
microsoft-openmpi-3.0.0
mssql-mlservices-python-9.4.7.64
mssql-mlservices-packages-py-9.4.7.64
mssql-mlservices-mlm-py-9.4.7.64
```

## <a name="next-steps"></a>后续步骤

Python 开发人员可以通过以下教程了解如何将 Python 与 SQL Server 一起使用：

+ [Python 教程：在 SQL Server 机器学习服务中使用线性回归来预测雪橇租赁](../machine-learning/tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Python 教程：配合使用 K-Means 群集和 SQL Server 机器学习服务对客户进行分类](../machine-learning/tutorials/python-clustering-model.md)

R 开发人员可以开始使用一些简单的示例，并了解 R 如何与 SQL Server 协同工作的基础知识。 有关下一步，请参阅以下链接：

+ [快速入门：在 T-SQL 中运行 R](../machine-learning/tutorials/quickstart-r-create-script.md)
+ [教程：适用于 R 开发人员的数据库内分析](../machine-learning/tutorials/r-taxi-classification-introduction.md)
