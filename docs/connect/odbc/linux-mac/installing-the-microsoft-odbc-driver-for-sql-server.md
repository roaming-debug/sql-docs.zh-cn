---
title: 安装 Microsoft ODBC Driver for SQL Server (Linux)
description: 了解如何在 Linux 客户端上安装 Microsoft ODBC Driver for SQL Server 来启用数据库连接。
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver, installing
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d4e2ee02544955cab007d542bca75b8f14cfeeee
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770455"
---
# <a name="install-the-microsoft-odbc-driver-for-sql-server-linux"></a>安装 Microsoft ODBC Driver for SQL Server (Linux)

本文介绍如何在 Linux 上安装 Microsoft ODBC Driver for SQL Server。 本文还包括有关 SQL Server 的可选命令行工具（`bcp` 和 `sqlcmd`）以及 unixODBC 开发标头的说明。

本文提供了用于从 bash shell 安装 ODBC 驱动程序的命令。 如果要直接下载包，请参阅[下载 ODBC Driver for SQL Server](../download-odbc-driver-for-sql-server.md)。

## <a name="microsoft-odbc-17"></a><a id="17"></a> Microsoft ODBC 17

以下部分介绍了如何从用于不同 Linux 分发版本的 bash shell 安装 Microsoft ODBC 驱动程序 17。

- [Alpine Linux](#alpine17)
- [Debian](#debian17)
- [Red Hat Enterprise Linux 和 Oracle](#redhat17)
- [SUSE Linux Enterprise Server](#suse17)
- [Ubuntu](#ubuntu17)

> [!IMPORTANT]
> 如果安装了暂时可用的 v17 `msodbcsql` 包，应先删除它，再安装 `msodbcsql17` 包。 这样可避免冲突。 `msodbcsql17` 包可以与 `msodbcsql` v13 包并行安装。

### <a name="alpine-linux"></a><a id="alpine17"></a> Alpine Linux

```bash
#Download the desired package(s)
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.2.1-1_amd64.apk
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/mssql-tools_17.7.1.1-1_amd64.apk


#(Optional) Verify signature, if 'gpg' is missing install it using 'apk add gnupg':
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.2.1-1_amd64.sig
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/mssql-tools_17.7.1.1-1_amd64.sig

curl https://packages.microsoft.com/keys/microsoft.asc  | gpg --import -
gpg --verify msodbcsql17_17.7.2.1-1_amd64.sig msodbcsql17_17.7.2.1-1_amd64.apk
gpg --verify mssql-tools_17.7.1.1-1_amd64.sig mssql-tools_17.7.1.1-1_amd64.apk


#Install the package(s)
sudo apk add --allow-untrusted msodbcsql17_17.7.2.1-1_amd64.apk
sudo apk add --allow-untrusted mssql-tools_17.7.1.1-1_amd64.apk
```

> [!NOTE]
> 必须有驱动程序版本 17.5 或更高版本，才能获得 Alpine 支持。

### <a name="debian"></a><a id="debian17"></a> Debian

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Debian 8
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 9
curl https://packages.microsoft.com/config/debian/9/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 10
curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
# optional: kerberos library for debian-slim distributions
sudo apt-get install libgssapi-krb5-2
```

> [!NOTE]
> 可以将设置环境变量“ACCEPT_EULA”替换为设置 debconf 变量“msodbcsql/ACCEPT_EULA”：`echo msodbcsql17 msodbcsql/ACCEPT_EULA boolean true | sudo debconf-set-selections`

### <a name="red-hat-enterprise-server-and-oracle-linux"></a><a id="redhat17"></a> Red Hat Enterprise Server 和 Oracle Linux

```bash
sudo su

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#RedHat Enterprise Server 6
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo

#RedHat Enterprise Server 7 and Oracle Linux 7
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo

#RedHat Enterprise Server 8 and Oracle Linux 8
curl https://packages.microsoft.com/config/rhel/8/prod.repo > /etc/yum.repos.d/mssql-release.repo

exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server"></a><a id="suse17"></a> SUSE Linux Enterprise Server

```bash
sudo su
curl -O https://packages.microsoft.com/keys/microsoft.asc
rpm --import microsoft.asc

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#SUSE Linux Enterprise Server 11 SP4
#Ensure SUSE Linux Enterprise 11 Security Module has been installed 
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo

#SUSE Linux Enterprise Server 12
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo

#SUSE Linux Enterprise Server 15
zypper ar https://packages.microsoft.com/config/sles/15/prod.repo
#(Only for driver 17.3 and below)
SUSEConnect -p sle-module-legacy/15/x86_64

exit
sudo ACCEPT_EULA=Y zypper install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="ubuntu"></a><a id="ubuntu17"></a> Ubuntu

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Ubuntu 16.04
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 18.04
curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 20.04
curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 20.10
curl https://packages.microsoft.com/config/ubuntu/20.10/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

> [!NOTE]
> - Ubuntu 18.04 支持需要 17.2 或更高版本的驱动程序。
> - Ubuntu 18.10 支持需要 17.3 或更高版本的驱动程序。

> [!NOTE]
> 可以将设置环境变量“ACCEPT_EULA”替换为设置 debconf 变量“msodbcsql/ACCEPT_EULA”：`echo msodbcsql17 msodbcsql/ACCEPT_EULA boolean true | sudo debconf-set-selections`

## <a name="previous-versions"></a>以前的版本

以下部分提供了有关在 Linux 上安装以前版本的的 Microsoft ODBC 驱动程序的说明。 涵盖以下驱动程序版本：

- [Microsoft ODBC Driver 13.1 for SQL Server](#13.1)
- [Microsoft ODBC Driver 13 for SQL Server](#13)
- [Microsoft ODBC Driver 11 for SQL Server](#11)

## <a name="odbc-131"></a><a id="13.1"></a> ODBC 13.1

以下部分介绍了如何从用于不同 Linux 分发版本的 bash shell 安装 Microsoft ODBC 驱动程序 13.1。

### <a name="debian-8"></a>Debian 8

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-11"></a>SUSE Linux Enterprise Server 11

```bash
sudo su
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```bash
sudo su
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="ubuntu-1510"></a>Ubuntu 15.10

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1610"></a>Ubuntu 16.10

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

## <a name="odbc-13"></a><a id="13"></a> ODBC 13

以下部分介绍了如何从用于不同 Linux 分发版本的 bash shell 安装 Microsoft ODBC 驱动程序 13。

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1510"></a>Ubuntu 15.10

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```bash
sudo su 
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo 
zypper update 
sudo ACCEPT_EULA=Y zypper install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
zypper install unixODBC-utf16-devel
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="offline-installation"></a>脱机安装

如果希望/需要在未连接 Internet 的计算机上安装 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13，则需要手动解析包依赖项。 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 具有以下直接依赖项：
- Ubuntu：libc6 (>= 2.21), libstdc++6 (>= 4.9), libkrb5-3, libcurl3, openssl, debconf (>= 0.5), unixodbc (>= 2.3.1-1)
- Red Hat：```glibc, e2fsprogs, krb5-libs, openssl, unixODBC```
- SUSE: ```glibc, libuuid1, krb5, openssl, unixODBC```

所有这些包都具有自己的依赖项，这些依赖性可能会显示在系统上，也可能不会显示。 有关此问题的常规解决方案，请参阅发行版对应的包管理器文档：[Redhat](https://wiki.centos.org/HowTos/CreateLocalRepos)、[Ubuntu](https://unix.stackexchange.com/questions/87130/how-to-quickly-create-a-local-apt-repository-for-random-packages-using-a-debian) 和 [SUSE](https://en.opensuse.org/Portal:Zypper)

还有一种常见做法是，手动下载所有相关包并将其一起放置在安装计算机上，然后依次手动安装每个包，最后安装 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 包。

#### <a name="redhat-linux-enterprise-server-7"></a>Redhat Linux Enterprise Server 7

- 从 [https://packages.microsoft.com/rhel/7/prod/](https://packages.microsoft.com/rhel/7/prod/) 下载最新的 `msodbcsql` `.rpm` 。
- 安装依赖项和驱动程序
  
```bash
yum install glibc e2fsprogs krb5-libs openssl unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

#### <a name="ubuntu-1604"></a>Ubuntu 16.04

- 从 [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/) 下载最新的 `msodbcsql` `.deb` 。
- 安装依赖项和驱动程序

```bash
sudo apt-get install libc6 libstdc++6 libkrb5-3 libcurl3 openssl debconf unixodbc unixodbc-dev #install dependencies
sudo dpkg -i msodbcsql_13.1.X.X-X_amd64.deb #install the Driver
```

#### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

- 从 [https://packages.microsoft.com/sles/12/prod/](https://packages.microsoft.com/sles/12/prod/) 下载最新的 `msodbcsql` `.rpm` 。
- 安装依赖项和驱动程序。

```bash
zypper install glibc, libuuid1, krb5, openssl, unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

完成包安装后，可以通过运行 ldd 并检查其输出是否缺少库来验证 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 是否可以找到其所有依赖项：

```bash
ldd /opt/microsoft/msodbcsql/lib64/libmsodbcsql-*
```

## <a name="odbc-11"></a><a id="11"></a> ODBC 11

以下部分介绍如何在 Linux 安装 Microsoft ODBC 驱动程序 11。 安装 unixODBC 驱动程序管理器后才能开始使用该驱动程序。 有关详细信息，请参阅[安装驱动程序管理器](../../../connect/odbc/linux-mac/installing-the-driver-manager.md)。

### <a name="installation-steps"></a>安装步骤  

> [!IMPORTANT]  
> 这些说明参考了 Red Hat Linux 的安装文件 `msodbcsql-11.0.2270.0.tar.gz`。 如果要安装适用于 SUSE Linux 的预览版，则文件名为 `msodbcsql-11.0.2260.0.tar.gz`。  
  
若要安装该驱动程序：

1. 请确保具有根权限。  

2. 转到下载放置文件 `msodbcsql-11.0.2270.0.tar.gz` 的目录。 确保所拥有的 \*.tar.gz 文件与你的 Linux 版本匹配。 若要提取文件，请执行以下命令：`tar xvzf msodbcsql-11.0.2270.0.tar.gz`。  
  
3. 转到 `msodbcsql-11.0.2270.0` 目录，你应在此处看到一个名为 install.sh 的文件  。  
  
4. 若要查看可用安装选项的列表，请执行以下命令： **./install.sh**。  
  
5. 创建 **odbcinst.ini** 的备份。 驱动程序安装会更新 **odbcinst.ini**。 odbcinst.ini 包含在 unixODBC 驱动程序管理器中注册的驱动程序列表。 若要在计算机上发现 odbcinst.ini 的位置，请执行以下命令：```odbc_config --odbcinstini```。  
  
6. 安装该驱动程序之前，请执行以下命令：`./install.sh verify`。 `./install.sh verify` 告的输出会报计算机中是否具有在 Linux 上支持 ODBC 驱动程序所需的软件。  
  
7. 当你准备好在 Linux 上安装 ODBC 驱动程序时，请执行命令：`./install.sh install`。 如果需要指定安装命令（`bin-dir` 或 `lib-dir`），请在“安装”选项后指定该命令  。  
  
8. 查看许可协议之后，键入 **YES** 以继续安装。  
  
安装会将驱动程序放在 `/opt/microsoft/msodbcsql/11.0.2270.0` 中。 驱动程序及其支持文件必须位于 `/opt/microsoft/msodbcsql/11.0.2270.0` 中。  
  
若要验证 Linux 上的 Microsoft ODBC 驱动程序是否已成功注册，请执行以下命令：```odbcinst -q -d -n "ODBC Driver 11 for SQL Server"```。  
  
### <a name="uninstall"></a>卸载  
  
通过执行以下命令，可以卸载 Linux 上的 ODBC 驱动程序 11：  
  
1. `rm -f /usr/bin/sqlcmd`
  
2. `rm -f /usr/bin/bcp`  
  
3. `rm -rf /opt/microsoft/msodbcsql`  
  
4. `odbcinst -u -d -n "ODBC Driver 11 for SQL Server"`
  
## <a name="driver-files"></a>驱动程序文件

Linux 上的 ODBC 驱动程序由以下组件构成：

|组件|说明|  
|---------------|-----------------|  
|libmsodbcsql-17.X.so.X.X 或 libmsodbcsql-13.X.so.X.X|包含该驱动程序所有功能的共享对象 (`so`) 动态库文件。 此文件安装在 ODBC Driver 17 的 `/opt/microsoft/msodbcsql17/lib64/` 中和 ODBC Driver 13 的 `/opt/microsoft/msodbcsql/lib64/` 中。|  
|`msodbcsqlr17.rll` 或 `msodbcsqlr13.rll`|驱动程序库的附带资源文件。 此文件安装在 `[driver .so directory]../share/resources/en_US/` 中| 
|msodbcsql.h|头文件，它包含使用驱动程序所需的所有新定义。<br /><br /> **注意：** 无法在同一个程序中引用 msodbcsql.h 和 odbcss.h。<br /><br /> msodbcsql.h 安装在 ODBC Driver 17 的 `/opt/microsoft/msodbcsql17/include/` 中和 ODBC Driver 13 的 `/opt/microsoft/msodbcsql/include/` 中。 |
|LICENSE.txt|包含最终用户许可协议条款的文本文件。 此文件位于 ODBC Driver 17 的 `/usr/share/doc/msodbcsql17/` 中和 ODBC Driver 13 的 `/usr/share/doc/msodbcsql/` 中。|
|RELEASE_NOTES|包含发行说明的文本文件。 此文件位于 ODBC Driver 17 的 `/usr/share/doc/msodbcsql17/` 中和 ODBC Driver 13 的 `/usr/share/doc/msodbcsql/` 中。|

## <a name="resource-file-loading"></a>资源文件加载

驱动程序需要加载资源文件才能正常运行。 此文件称为 `msodbcsqlr17.rll` 或 `msodbcsqlr13.rll`，具体取决于驱动程序版本。 `.rll` 文件的位置与驱动程序本身的位置（`so` 或 `dylib`）相对，如上表中所述。 自版本 17.1 开始，如果从相对路径加载失败，驱动程序还将尝试从默认目录加载 `.rll`。 Linux 上的默认资源文件路径是 `/opt/microsoft/msodbcsql17/share/resources/en_US/`。

## <a name="troubleshooting"></a>疑难解答

如果无法使用 ODBC 驱动程序建立与 SQL Server 的连接，请参阅[解决连接问题](known-issues-in-this-version-of-the-driver.md#connectivity)上的“已知问题”一文。

## <a name="next-steps"></a>后续步骤

安装驱动程序后，可以尝试 [C++ ODBC 示例应用程序](../../odbc/cpp-code-example-app-connect-access-sql-db.md)。 有关开发 ODBC 应用程序的详细信息，请参阅[开发应用程序](../../../odbc/reference/develop-app/developing-applications.md)。

有关详细信息，请参阅 ODBC 驱动程序[发行说明](release-notes-odbc-sql-server-linux-mac.md)和[系统要求](system-requirements.md)。
