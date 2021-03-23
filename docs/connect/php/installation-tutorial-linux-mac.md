---
title: 在 Linux 和 macOS 上安装 Drivers for PHP
description: 这些说明介绍了如何在 Linux 或 macOS 上安装 Microsoft Drivers for PHP for SQL Server。
ms.date: 03/15/2021
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
manager: v-mabarw
ms.openlocfilehash: 33cb8913e240b83e1ada40ebf262acca85472e9b
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2021
ms.locfileid: "103575268"
---
# <a name="linux-and-macos-installation-tutorial-for-the-microsoft-drivers-for-php-for-sql-server"></a>Microsoft Drivers for PHP for SQL Server 的 Linux 和 macOS 安装教程

以下说明假定环境是干净的，它展示了如何在 Ubuntu、Red Hat、Debian、Suse、Alpine 和 macOS 上安装 PHP 8.0、Microsoft ODBC 驱动程序、Apache Web 服务器和 Microsoft Drivers for PHP for SQL Server。 这些说明建议使用 PECL 安装驱动程序，但也可以从 [Microsoft Drivers for PHP for SQL Server](https://github.com/Microsoft/msphpsql/releases) GitHub 项目页下载预生成的二进制文件，并按照[下载 Microsoft Drivers for PHP for SQL Server](../../connect/php/loading-the-php-sql-driver.md) 中的说明安装它们。 有关扩展加载以及为什么不将扩展添加到 php.ini 的说明，请参阅[加载驱动程序](../../connect/php/loading-the-php-sql-driver.md#loading-the-driver-at-php-startup)部分。

如果 PHP 8.0 包可用，以下说明默认情况下使用 `pecl install` 安装 PHP 8.0。 你可能需要先运行 `pecl channel-update pecl.php.net`。 一些受支持的 Linux 发行版默认为 PHP 7.1 或更早版本，而 SQL Server 的 PHP 驱动程序的最新版本不支持这些版本。 请参阅每一节开头的说明，以便安装 PHP 7.4 或 7.3。

还包括有关在 Ubuntu 上安装 PHP FastCGI 进程管理器 (PHP-FPM) 的说明。 如果使用的是 nginx Web 服务器而不是 Apache，则需要 PHP-FPM。

尽管这些说明包含用于安装 SQLSRV 和 PDO_SQLSRV 驱动程序的命令，但可以独立安装和运行驱动程序。 熟悉自定义配置的用户可以调整这些说明，使其特定于 SQLSRV 或 PDO_SQLSRV。 这两个驱动程序具有相同的依赖项，但以下各项除外。

## <a name="installing-on-ubuntu"></a>在 Ubuntu 上安装

支持 Ubuntu 版本 16.04、18.04 和 20.04。

> [!NOTE]
> 若要安装 PHP 7.4 或 7.3，请在以下命令中将 8.0 替换为 7.4 或 7.3。

### <a name="step-1-install-php-ubuntu"></a>步骤 1。 安装 PHP (Ubuntu)

```bash
sudo su
add-apt-repository ppa:ondrej/php -y
apt-get update
apt-get install php8.0 php8.0-dev php8.0-xml -y --allow-unauthenticated
```

### <a name="step-2-install-prerequisites-ubuntu"></a>步骤 2. 安装必备组件 (Ubuntu)

按照[安装 Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md) 上的说明安装适用于 Ubuntu 的 ODBC 驱动程序。 确保也安装可选的 `unixodbc-dev` 包。 `pecl` 命令用它来安装 PHP 驱动程序。

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server-ubuntu"></a>步骤 3. 安装适用于 Microsoft SQL Server 的 PHP 驱动程序 (Ubuntu)

```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/8.0/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/8.0/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 8.0 sqlsrv pdo_sqlsrv
```

如果系统中只有一个 PHP 版本，则可以将最后一个步骤简化为 `phpenmod sqlsrv pdo_sqlsrv`。

### <a name="step-4-install-apache-and-configure-driver-loading-ubuntu"></a>步骤 4. 安装 Apache 并配置驱动程序加载 (Ubuntu)

```bash
sudo su
apt-get install libapache2-mod-php8.0 apache2
a2dismod mpm_event
a2enmod mpm_prefork
a2enmod php8.0
exit
```

### <a name="step-5-restart-apache-and-test-the-sample-script-ubuntu"></a>步骤 5。 重启 Apache 并测试示例脚本 (Ubuntu)

```bash
sudo service apache2 restart
```

若要测试安装，请参阅本文档末尾的[测试安装](#testing-your-installation)。

## <a name="installing-on-ubuntu-with-php-fpm"></a>安装在带 PHP-FPM 的 Ubuntu 上

支持 Ubuntu 版本 16.04、18.04 和 20.04。

> [!NOTE]
> 若要安装 PHP 7.4 或 7.3，请在以下命令中将 8.0 替换为 7.4 或 7.3。

### <a name="step-1-install-php-ubuntu-with-php-fpm"></a>步骤 1。 安装 PHP（带 PHP-FPM 的 Ubuntu）

```bash
sudo su
add-apt-repository ppa:ondrej/php -y
apt-get update
apt-get install php8.0 php8.0-dev php8.0-fpm php8.0-xml -y --allow-unauthenticated
```

运行以下命令来验证 PHP-FPM 服务的状态：

```bash
systemctl status php8.0-fpm
```

### <a name="step-2-install-prerequisites-ubuntu-with-php-fpm"></a>步骤 2. 安装必备组件（带 PHP-FPM 的 Ubuntu）

按照[安装 Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md) 上的说明安装适用于 Ubuntu 的 ODBC 驱动程序。 确保也安装可选的 `unixodbc-dev` 包。 `pecl` 命令用它来安装 PHP 驱动程序。

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server-ubuntu-with-php-fpm"></a>步骤 3. 安装适用于 Microsoft SQL Server 的 PHP 驱动程序（带 PHP-FPM 的 Ubuntu）

```bash
sudo pecl config-set php_ini /etc/php/8.0/fpm/php.ini
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/8.0/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/8.0/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 8.0 sqlsrv pdo_sqlsrv
```

如果系统中只有一个 PHP 版本，则可以将最后一个步骤简化为 `phpenmod sqlsrv pdo_sqlsrv`。

验证 `sqlsrv.ini` 和 `pdo_sqlsrv.ini` 是否位于 `/etc/php/8.0/fpm/conf.d/`：

```bash
ls /etc/php/8.0/fpm/conf.d/*sqlsrv.ini
```

重新启动 PHP-FPM 服务：

```bash
sudo systemctl restart php8.0-fpm
```

### <a name="step-4-install-and-configure-nginx-ubuntu-with-php-fpm"></a>步骤 4. 安装并配置 nginx（带 PHP-FPM 的 Ubuntu）

```bash
sudo apt-get update
sudo apt-get install nginx
sudo systemctl status nginx
```

若要配置 nginx，必须编辑 `/etc/nginx/sites-available/default` 文件。 将 `index.php` 添加到指示 `# Add index.php to the list if you are using PHP` 的部分下的列表中：

```config
# Add index.php to the list if you are using PHP
index index.html index.htm index.nginx-debian.html index.php;
```

接下来，取消注释并修改 `# pass PHP scripts to FastCGI server` 后面的部分，如下所示：

```config
# pass PHP scripts to FastCGI server
#
location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.0-fpm.sock;
}
```

### <a name="step-5-restart-nginx-and-test-the-sample-script-ubuntu-with-php-fpm"></a>步骤 5。 重启 nginx 并测试示例脚本（带 PHP-FPM 的 Ubuntu）

```bash
sudo systemctl restart nginx.service
```

若要测试安装，请参阅本文档末尾的[测试安装](#testing-your-installation)。

## <a name="installing-on-red-hat"></a>在 Red Hat 上安装

支持 Red Hat 版本 7 和 8。

### <a name="step-1-install-php-red-hat"></a>步骤 1。 安装 PHP (Red Hat)

若要在 Red Hat 7 上安装 PHP，请运行以下命令：
> [!NOTE]
> 若要安装 PHP 7.4 或 7.3，请在以下命令中分别用 remi- php74 或 remi-php73 替换 remi-php80。

```bash
sudo su
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install https://rpms.remirepo.net/enterprise/remi-release-7.rpm
subscription-manager repos --enable=rhel-7-server-optional-rpms
yum install yum-utils
yum-config-manager --enable remi-php80
yum update
# Note: The php-pdo package is required only for the PDO_SQLSRV driver
yum install php php-pdo php-xml php-pear php-devel re2c gcc-c++ gcc
```

若要在 Red Hat 8 上安装 PHP，请运行以下命令：
> [!NOTE]
> 若要安装 PHP 7.4 或7.3，请在以下命令中分别用 remi-7.4 或 remi-7.3 替换 remi-8.0。

```bash
sudo su
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm
dnf install yum-utils
dnf module reset php
dnf module install php:remi-8.0
subscription-manager repos --enable codeready-builder-for-rhel-8-x86_64-rpms
dnf update
# Note: The php-pdo package is required only for the PDO_SQLSRV driver
dnf install php-pdo php-pear php-devel
```

### <a name="step-2-install-prerequisites-red-hat"></a>步骤 2. 安装必备组件 (Red Hat)

按照[安装 Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md) 上的说明安装适用于 Red Hat 7 或 8 的 ODBC 驱动程序。 确保也安装可选的 `unixodbc-dev` 包。 `pecl` 命令用它来安装 PHP 驱动程序。

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server-red-hat"></a>步骤 3. 安装适用于 Microsoft SQL Server 的 PHP 驱动程序 (Red Hat)

```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/30-pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/20-sqlsrv.ini
exit
```

也可以从 Remi 存储库进行安装：

```bash
sudo yum install php-sqlsrv
```

### <a name="step-4-install-apache-red-hat"></a>步骤 4. 安装 Apache (Red Hat)

```bash
sudo yum install httpd
```

默认安装 SELinux，并在强制模式下运行。 若要允许 Apache SELinux 通过连接到数据库，请运行以下命令：

```bash
sudo setsebool -P httpd_can_network_connect_db 1
```

### <a name="step-5-restart-apache-and-test-the-sample-script-red-hat"></a>步骤 5。 重启 Apache 并测试示例脚本 (Red Hat)

```bash
sudo apachectl restart
```

若要测试安装，请参阅本文档末尾的[测试安装](#testing-your-installation)。

## <a name="installing-on-debian"></a>在 Debian 上安装

仅支持 Debian 版本 9 和 10。

> [!NOTE]
> 若要安装 PHP 7.4 或 7.3，请在以下命令中将 8.0 替换为 7.4 或 7.3。

### <a name="step-1-install-php-debian"></a>步骤 1。 安装 PHP (Debian)

```bash
sudo su
apt-get install curl apt-transport-https
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list
apt-get update
apt-get install -y php8.0 php8.0-dev php8.0-xml php8.0-intl
```

### <a name="step-2-install-prerequisites-debian"></a>步骤 2. 安装必备组件 (Debian)

按照[安装 Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md) 上的说明安装适用于 Debian 的 ODBC 驱动程序。  确保也安装可选的 `unixodbc-dev` 包。 `pecl` 命令用它来安装 PHP 驱动程序。

可能还需要生成正确的区域设置，以使 PHP 输出在浏览器中正确显示。 例如，对于 en_US UTF-8 区域设置，运行以下命令：

```bash
sudo su
sed -i 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g' /etc/locale.gen
locale-gen
```

可能需要将 `/usr/sbin` 添加到 `$PATH` 中，因为 `locale-gen` 可执行文件位于该位置。

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server-debian"></a>步骤 3. 安装适用于 Microsoft SQL Server 的 PHP 驱动程序 (Debian)

```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/8.0/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/8.0/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 8.0 sqlsrv pdo_sqlsrv
```

如果系统中只有一个 PHP 版本，则可以将最后一个步骤简化为 `phpenmod sqlsrv pdo_sqlsrv`。 与 `locale-gen` 一样，`phpenmod` 位于 `/usr/sbin` 中，因此可能需要将此目录添加到 `$PATH`。

### <a name="step-4-install-apache-and-configure-driver-loading-debian"></a>步骤 4. 安装 Apache 并配置驱动程序加载 (Debian)

```bash
sudo su
apt-get install libapache2-mod-php8.0 apache2
a2dismod mpm_event
a2enmod mpm_prefork
a2enmod php8.0
```

### <a name="step-5-restart-apache-and-test-the-sample-script-debian"></a>步骤 5。 重启 Apache 并测试示例脚本 (Debian)

```bash
sudo service apache2 restart
```

若要测试安装，请参阅本文档末尾的[测试安装](#testing-your-installation)。

## <a name="installing-on-suse"></a>在 Suse 上安装

支持 Suse Enterprise Linux 版本 12 和 15。

> [!NOTE]
> 在以下说明中，请将 `<SuseVersion>` 替换为你的 Suse 版本。 如果使用的是 Suse Enterprise Linux 15，则将为 SLE_15_SP1 或 SLE_15_SP2。 对于 Suse 12，请使用 SLE_12_SP4（或更高版本，如果适用）。 并不是所有版本的 PHP 都适用于所有版本的 Suse Linux。 请参阅 `http://download.opensuse.org/repositories/devel:/languages:/php` 以查看哪些版本的 Suse 具有可用的默认版本 PHP，或者参阅 `http://download.opensuse.org/repositories/devel:/languages:/php:/` 查看哪些其他版本的 PHP 可用于哪些版本的 Suse。

> [!NOTE]
> 适用于 PHP 7.4 或更高版本的包不适用于 Suse 12，适用于 PHP 8.0 的包尚不适用于 Suse 15。
> 若要安装 PHP 7.3，用以下 URL 替换下面的存储库 URL：`https://download.opensuse.org/repositories/devel:/languages:/php:/php73/<SuseVersion>/devel:languages:php:php73.repo`。

### <a name="step-1-install-php-suse"></a>步骤 1。 安装 PHP (Suse)

```bash
sudo su
zypper -n ar -f https://download.opensuse.org/repositories/devel:languages:php/<SuseVersion>/devel:languages:php.repo
zypper --gpg-auto-import-keys refresh
zypper -n install php7 php7-devel php7-openssl
```

### <a name="step-2-install-prerequisites-suse"></a>步骤 2. 安装必备组件 (Suse)

按照[安装 Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md) 上的说明安装适用于 Suse 的 ODBC 驱动程序。 确保也安装可选的 `unixodbc-dev` 包。 `pecl` 命令用它来安装 PHP 驱动程序。

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server-suse"></a>步骤 3. 安装适用于 Microsoft SQL Server 的 PHP 驱动程序 (Suse)

> [!NOTE]
> 如果收到错误消息 `Connection to 'pecl.php.net:443' failed: Unable to find the socket transport "ssl"`，请编辑 /usr/bin/pecl 中的 pecl 脚本并删除最后一行的 `-n` 开关。 此开关可防止 PECL 在调用 PHP 时加载 ini 文件，从而防止加载 OpenSSL 扩展。

```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/sqlsrv.ini
exit
```

### <a name="step-4-install-apache-and-configure-driver-loading-suse"></a>步骤 4. 安装 Apache 并配置驱动程序加载 (Suse)

```bash
sudo su
zypper install apache2 apache2-mod_php7
a2enmod php7
echo "extension=sqlsrv.so" >> /etc/php7/apache2/php.ini
echo "extension=pdo_sqlsrv.so" >> /etc/php7/apache2/php.ini
exit
```

### <a name="step-5-restart-apache-and-test-the-sample-script-suse"></a>步骤 5。 重启 Apache 并测试示例脚本 (Suse)

```bash
sudo systemctl restart apache2
```

若要测试安装，请参阅本文档末尾的[测试安装](#testing-your-installation)。

## <a name="installing-on-alpine"></a>在 Alpine 上安装

仅支持 Alpine 版本 3.11 和 3.12。

> [!NOTE]
> PHP 的默认版本为 7.3。 可从 Alpine 的测试或边缘存储库中获取 PHP 7.4 或更高版本。 可以改为从源编译 PHP。

### <a name="step-1-install-php-alpine"></a>步骤 1。 安装 PHP (Alpine)

可以在 `edge/community` 存储库中找到用于 Alpine 的 PHP 包。 请查看 Wiki 页上的[启用社区存储库](https://wiki.alpinelinux.org/wiki/Enable_Community_Repository)。 将以下行添加到 `/etc/apk/repositories`，并将 `<mirror>` 替换为 Alpine 存储库镜像的 URL：

```bash
http://<mirror>/alpine/edge/community
```

然后运行：

```bash
sudo su
apk update
# Note: The php7-pdo package is required only for the PDO_SQLSRV driver
apk add php7 php7-dev php7-pear php7-pdo php7-openssl autoconf make g++
```

### <a name="step-2-install-prerequisites-alpine"></a>步骤 2. 安装必备组件 (Alpine)

按照[安装 Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md) 上的说明安装适用于 Alpine 的 ODBC 驱动程序。  确保也安装 `unixodbc-dev` 包 (`sudo apk add unixodbc-dev`)。 `pecl` 命令用它来安装 PHP 驱动程序。

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server-alpine"></a>步骤 3. 安装适用于 Microsoft SQL Server 的 PHP 驱动程序 (Alpine)

```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/10_pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/00_sqlsrv.ini
```

### <a name="step-4-install-apache-and-configure-driver-loading-alpine"></a>步骤 4. 安装 Apache 并配置驱动程序加载 (Alpine)

```bash
sudo apk add php7-apache2 apache2
```

### <a name="step-5-restart-apache-and-test-the-sample-script-alpine"></a>步骤 5。 重启 Apache 并测试示例脚本 (Alpine)

```bash
sudo rc-service apache2 restart
```

若要测试安装，请参阅本文档末尾的[测试安装](#testing-your-installation)。

## <a name="installing-on-macos"></a>在 macOS 上安装

支持 MacOS 版本 10.14 (Mojave)、10.15 (Catalina) 和 11.0 (Big Sur)。

如果还没有安装 brew，请按照以下步骤安装：

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

> [!NOTE]
> 若要安装 PHP 7.4 或 7.3，请在以下命令中分别用 php@7.4 或 php@7.3 替换 php@8.0。

### <a name="step-1-install-php-macos"></a>步骤 1。 安装 PHP (macOS)

```bash
brew tap
brew tap homebrew/core
brew install php@8.0
```

PHP 现应已在路径中。 请运行 `php -v` 以验证当前正在运行正确版本的 PHP。 如果 PHP 不在路径中或版本不正确，请运行以下命令：

```bash
brew link --force --overwrite php@8.0
```

### <a name="step-2-install-prerequisites-macos"></a>步骤 2. 安装必备组件 (macOS)

按照[安装 Microsoft ODBC Driver for SQL Server (macOS)](../../connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos.md) 上的说明安装适用于 macOS 的 ODBC 驱动程序。

此外，可能需要安装 GNU make 工具：

```bash
brew install autoconf automake libtool
```

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server-macos"></a>步骤 3. 安装适用于 Microsoft SQL Server 的 PHP 驱动程序 (macOS)

```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
```

### <a name="step-4-install-apache-and-configure-driver-loading-macos"></a>步骤 4. 安装 Apache 并配置驱动程序加载 (macOS)

```bash
brew install apache2
```

若要查找用于 Apache 安装的 Apache 配置文件 `httpd.conf`，请运行：

```bash
/usr/local/bin/apachectl -V | grep SERVER_CONFIG_FILE
```

以下命令将所需的配置追加到 `httpd.conf`。 请确保用上一个命令返回的路径替换 `/usr/local/etc/httpd/httpd.conf`：

```bash
echo "LoadModule php7_module /usr/local/opt/php@8.0/lib/httpd/modules/libphp7.so" >> /usr/local/etc/httpd/httpd.conf
(echo "<FilesMatch .php$>"; echo "SetHandler application/x-httpd-php"; echo "</FilesMatch>";) >> /usr/local/etc/httpd/httpd.conf
```

### <a name="step-5-restart-apache-and-test-the-sample-script-macos"></a>步骤 5。 重启 Apache 并测试示例脚本 (macOS)

```bash
sudo apachectl restart
```

若要测试安装，请参阅本文档末尾的[测试安装](#testing-your-installation)。

## <a name="testing-your-installation"></a>测试安装

若要测试此示例脚本，请在系统的文档根目录中创建名为“testsql.php”的文件。 该路径在 Ubuntu、Debian 和 Red Hat 上是 `/var/www/html/`，在 SUSE 上是 `/srv/www/htdocs`，在 Alpine 上是 `/var/www/localhost/htdocs`，而在 macOS 上是 `/usr/local/var/www`。 将以下脚本复制到其中，酌情替换服务器、数据库、用户名和密码。

### <a name="sqlsrv-example"></a>SQLSRV 示例

```php
<?php
$serverName = "yourServername";
$connectionOptions = array(
    "database" => "yourDatabase",
    "uid" => "yourUsername",
    "pwd" => "yourPassword"
);

function exception_handler($exception) {
    echo "<h1>Failure</h1>";
    echo "Uncaught exception: " , $exception->getMessage();
    echo "<h1>PHP Info for troubleshooting</h1>";
    phpinfo();
}

set_exception_handler('exception_handler');

// Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
if ($conn === false) {
    die(formatErrors(sqlsrv_errors()));
}

// Select Query
$tsql = "SELECT @@Version AS SQL_VERSION";

// Executes the query
$stmt = sqlsrv_query($conn, $tsql);

// Error handling
if ($stmt === false) {
    die(formatErrors(sqlsrv_errors()));
}
?>

<h1> Success Results : </h1>

<?php
while ($row = sqlsrv_fetch_array($stmt, SQLSRV_FETCH_ASSOC)) {
    echo $row['SQL_VERSION'] . PHP_EOL;
}

sqlsrv_free_stmt($stmt);
sqlsrv_close($conn);

function formatErrors($errors)
{
    // Display errors
    echo "<h1>SQL Error:</h1>";
    echo "Error information: <br/>";
    foreach ($errors as $error) {
        echo "SQLSTATE: ". $error['SQLSTATE'] . "<br/>";
        echo "Code: ". $error['code'] . "<br/>";
        echo "Message: ". $error['message'] . "<br/>";
    }
}
?>
```

### <a name="pdo_sqlsrv-example"></a>PDO_SQLSRV 示例

```php
<?php
try {
    $serverName = "yourServername";
    $databaseName = "yourDatabase";
    $uid = "yourUsername";
    $pwd = "yourPassword";
    
    $conn = new PDO("sqlsrv:server = $serverName; Database = $databaseName;", $uid, $pwd);

    // Select Query
    $tsql = "SELECT @@Version AS SQL_VERSION";

    // Executes the query
    $stmt = $conn->query($tsql);
} catch (PDOException $exception1) {
    echo "<h1>Caught PDO exception:</h1>";
    echo $exception1->getMessage() . PHP_EOL;
    echo "<h1>PHP Info for troubleshooting</h1>";
    phpinfo();
}

?>

<h1> Success Results : </h1>

<?php
try {
    while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
        echo $row['SQL_VERSION'] . PHP_EOL;
    }
} catch (PDOException $exception2) {
    // Display errors
    echo "<h1>Caught PDO exception:</h1>";
    echo $exception2->getMessage() . PHP_EOL;
}

unset($stmt);
unset($conn);
?>
```

将浏览器指向 `https://localhost/testsql.php` （macOS 上的 `https://localhost:8080/testsql.php` ）。 现在应能够连接到 SQL Server/Azure SQL 数据库。 如果看不到显示 SQL 版本信息的成功消息，可以通过从命令行运行脚本来执行一些基本的故障排除：

```bash
php testsql.php
```

如果从命令行运行成功但在浏览器中未显示任何消息，请查看 [Apache 日志文件](https://linuxize.com/post/apache-log-files/#location-of-the-log-files)。 如需更多帮助，请查看[支持资源](support-resources-for-the-php-sql-driver.md)。

## <a name="see-also"></a>另请参阅

[开始使用 Microsoft Drivers for PHP for SQL Server](../../connect/php/getting-started-with-the-php-sql-driver.md)

[加载 Microsoft Drivers for PHP for SQL Server](../../connect/php/loading-the-php-sql-driver.md)

[Microsoft Drivers for PHP for SQL Server 系统要求](../../connect/php/system-requirements-for-the-php-sql-driver.md)
