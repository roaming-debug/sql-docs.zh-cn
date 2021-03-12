---
title: Microsoft Drivers for PHP 系统要求
description: Microsoft Drivers for PHP for SQL Server 支持各种 PHP 版本、操作系统和 SQL Server 版本。
ms.custom: ''
ms.date: 03/09/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- requirements
ms.assetid: 5db4b75f-c605-4785-9560-399a533c0fc9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b52bb4597b76ca831b94899e040a814b11b2a903
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622653"
---
# <a name="system-requirements-for-the-microsoft-drivers-for-php-for-sql-server"></a>Microsoft Drivers for PHP for SQL Server 系统要求

[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

本文列出了必须在系统上安装才能使用 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 访问 SQL Server 或 Azure SQL 数据库中的数据的组件。

正式支持 Microsoft PHP Drivers for SQL Server 版本 4.0 及更高版本。 有关支持 PHP 驱动程序生命周期和要求的完整详细信息，请参阅[支持矩阵](microsoft-php-drivers-for-sql-server-support-matrix.md)。

## <a name="php"></a>PHP

要了解如何下载并安装最新的稳定 PHP 二进制文件，请参阅 [PHP 网站](https://php.net)。  Microsoft Drivers for PHP for SQL Server 需要正确版本的 PHP，如 [PHP 版本支持](microsoft-php-drivers-for-sql-server-support-matrix.md#php-version-support)中所述。

-   必须通过相应的 PHP 版本启用驱动程序文件的正确版本。 有关不同驱动程序文件的信息，请参阅[驱动程序版本](#driver-versions)。 若要下载驱动程序，请参阅[下载 Microsoft Drivers for PHP for SQL Server](download-drivers-php-sql-server.md)。 要了解如何配置适用于 PHP 的驱动程序，请参阅[加载 Microsoft Drivers for PHP for SQL Server](loading-the-php-sql-driver.md)。

-   Web 服务器是必需的。 必须将 Web 服务器配置为运行 PHP。 有关使用 IIS 托管 PHP 应用程序的信息，请参阅 [PHP 网站上的教程](http://docs.php.net/manual/da/install.windows.iis7.php)。

    已通过结合使用 IIS 10 和 FastCGI 对 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 进行了测试。

> [!NOTE]
> Microsoft 仅提供对 IIS 的支持。

## <a name="odbc-driver"></a>ODBC 驱动程序

运行 PHP 的计算机需要正确版本的 Microsoft ODBC Driver for SQL Server。 [可以在此页](../odbc/download-odbc-driver-for-sql-server.md)下载支持平台的所有支持的驱动程序版本。

如果要在 64 位版本的 Windows 上下载 Windows 版本的驱动程序，ODBC 64 位安装程序将同时安装 32 位和 64 位 ODBC 驱动程序。 如果使用的是 32 位版本的 Windows，请使用 ODBC x86 安装程序。 在非 Windows 平台上，仅 64 位版本的驱动程序可用。

|PHP 驱动程序版本 &#8594;<br />&#8595; ODBC 驱动程序版本|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|ODBC 驱动程序 17+ |是|是|是|是|是|   |   |   |
|ODBC 驱动程序 13.1|是|是|是|是|是|是|是|   |
|ODBC 驱动程序 13  |   |   |   |   |   |   |是|   |
|ODBC 驱动程序 11  |   |是|是|是|是|是|是|是|

如果使用的是 SQLSRV 驱动程序，[sqlsrv_client_info](sqlsrv-client-info.md) 将返回有关 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 正在使用哪个版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Microsoft ODBC Driver for SQL Server 的信息。 如果使用的是 PDO_SQLSRV 驱动程序，可以使用 [PDO::getAttribute](pdo-getattribute.md) 来发现版本。

## <a name="sql-server"></a>SQL Server

若要详细了解哪些 SQL Server 版本受支持，请参阅[受支持的数据库版本](microsoft-php-drivers-for-sql-server-support-matrix.md#sql-server-version-certified-compatibility)。

## <a name="operating-systems"></a>操作系统

有关支持的操作系统的详细信息，请参阅[支持的操作系统](microsoft-php-drivers-for-sql-server-support-matrix.md#supported-operating-systems)。

## <a name="driver-versions"></a>驱动程序版本

本部分列出各版本的 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 所附带的驱动程序文件。 每个安装包都包含线程和非线程变体中的 SQLSRV 和 PDO_SQLSRV 驱动程序文件。 在 Windows 上，它们也可用于 32 位和 64 位变体。 若要配置可供与 PHP 运行时一起使用的驱动程序，请按照 [加载 Microsoft Drivers for PHP for SQL Server](loading-the-php-sql-driver.md) 中的安装说明进行操作。

在支持的 Linux 和 macOS 版本上，可以按照 [Linux 和 macOS 安装说明](installation-tutorial-linux-mac.md)，使用 PHP 的 PECL 包系统安装适当的驱动程序。 或者，可以从 [Microsoft Drivers for PHP for SQL Server](https://github.com/Microsoft/msphpsql/releases) GitHub 项目页中为平台下载预生成的二进制文件 -- 下表列出了在预生成的二进制包中找到的文件。

Microsoft Drivers 5.9 for PHP for SQL Server：

在 Windows 上，提供以下驱动程序文件：

|驱动程序文件|PHP 版本|线程是否安全？|与 PHP .dll 一起使用|
|---------------|:---------------:|:----------------:|---------------------|
|32 位 php_sqlsrv_73_nts.dll<br />32 位 php_pdo_sqlsrv_73_nts.dll|7.3|否 |32 位 php7.dll|
|32 位 php_sqlsrv_73_ts.dll <br />32 位 php_pdo_sqlsrv_73_ts.dll |7.3|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_73_nts.dll<br />64 位 php_pdo_sqlsrv_73_nts.dll|7.3|否 |64 位 php7.dll|
|64 位 php_sqlsrv_73_ts.dll <br />64 位 php_pdo_sqlsrv_73_ts.dll |7.3|是|64 位 php7ts.dll|
|32 位 php_sqlsrv_74_nts.dll<br />32 位 php_pdo_sqlsrv_74_nts.dll|7.4|否 |32 位 php7.dll|
|32 位 php_sqlsrv_74_ts.dll <br />32 位 php_pdo_sqlsrv_74_ts.dll |7.4|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_74_nts.dll<br />64 位 php_pdo_sqlsrv_74_nts.dll|7.4|否 |64 位 php7.dll|
|64 位 php_sqlsrv_74_ts.dll <br />64 位 php_pdo_sqlsrv_74_ts.dll |7.4|是|64 位 php7ts.dll|
|32-bit php_sqlsrv_80_nts.dll<br />32-bit php_pdo_sqlsrv_80_nts.dll|8.0|否 |32-bit php8.dll|
|32-bit php_sqlsrv_80_ts.dll <br />32-bit php_pdo_sqlsrv_80_ts.dll |8.0|是|32-bit php8ts.dll|
|64-bit php_sqlsrv_80_nts.dll<br />64-bit php_pdo_sqlsrv_80_nts.dll|8.0|否 |64-bit php8.dll|
|64-bit php_sqlsrv_80_ts.dll <br />64-bit php_pdo_sqlsrv_80_ts.dll |8.0|是|64-bit php8ts.dll|

在 Linux 上，提供以下驱动程序文件：

|驱动程序文件|PHP 版本|线程是否安全？|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_73_nts.so<br />php_pdo_sqlsrv_73_nts.so|7.3|否 |
|php_sqlsrv_73_ts.so <br />php_pdo_sqlsrv_73_ts.so |7.3|是|
|php_sqlsrv_74_nts.so<br />php_pdo_sqlsrv_74_nts.so|7.4|否 |
|php_sqlsrv_74_ts.so <br />php_pdo_sqlsrv_74_ts.so |7.4|是|
|php_sqlsrv_80_nts.so<br />php_pdo_sqlsrv_80_nts.so|8.0|否 |
|php_sqlsrv_80_ts.so <br />php_pdo_sqlsrv_80_ts.so |8.0|是|

**Microsoft Drivers 5.8 for PHP for SQL Server：**

在 Windows 上，包括以下版本的驱动程序：

|驱动程序文件|PHP 版本|线程是否安全？|与 PHP .dll 一起使用|
|---------------|:---------------:|:----------------:|---------------------|
|32 位 php_sqlsrv_72_nts.dll<br />32 位 php_pdo_sqlsrv_72_nts.dll|7.2|否 |32 位 php7.dll|
|32 位 php_sqlsrv_72_ts.dll <br />32 位 php_pdo_sqlsrv_72_ts.dll |7.2|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_72_nts.dll<br />64 位 php_pdo_sqlsrv_72_nts.dll|7.2|否 |64 位 php7.dll|
|64 位 php_sqlsrv_72_ts.dll <br />64 位 php_pdo_sqlsrv_72_ts.dll |7.2|是|64 位 php7ts.dll|
|32 位 php_sqlsrv_73_nts.dll<br />32 位 php_pdo_sqlsrv_73_nts.dll|7.3|否 |32 位 php7.dll|
|32 位 php_sqlsrv_73_ts.dll <br />32 位 php_pdo_sqlsrv_73_ts.dll |7.3|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_73_nts.dll<br />64 位 php_pdo_sqlsrv_73_nts.dll|7.3|否 |64 位 php7.dll|
|64 位 php_sqlsrv_73_ts.dll <br />64 位 php_pdo_sqlsrv_73_ts.dll |7.3|是|64 位 php7ts.dll|
|32 位 php_sqlsrv_74_nts.dll<br />32 位 php_pdo_sqlsrv_74_nts.dll|7.4|否 |32 位 php7.dll|
|32 位 php_sqlsrv_74_ts.dll <br />32 位 php_pdo_sqlsrv_74_ts.dll |7.4|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_74_nts.dll<br />64 位 php_pdo_sqlsrv_74_nts.dll|7.4|否 |64 位 php7.dll|
|64 位 php_sqlsrv_74_ts.dll <br />64 位 php_pdo_sqlsrv_74_ts.dll |7.4|是|64 位 php7ts.dll|

在 Linux 上，包括以下版本的驱动程序：

|驱动程序文件|PHP 版本|线程是否安全？|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|否 |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|是|
|php_sqlsrv_73_nts.so<br />php_pdo_sqlsrv_73_nts.so|7.3|否 |
|php_sqlsrv_73_ts.so <br />php_pdo_sqlsrv_73_ts.so |7.3|是|
|php_sqlsrv_74_nts.so<br />php_pdo_sqlsrv_74_nts.so|7.4|否 |
|php_sqlsrv_74_ts.so <br />php_pdo_sqlsrv_74_ts.so |7.4|是|

**Microsoft Drivers 5.6 for PHP for SQL Server：**

在 Windows 上，包括以下版本的驱动程序：

|驱动程序文件|PHP 版本|线程是否安全？|与 PHP .dll 一起使用|
|---------------|:---------------:|:----------------:|---------------------|
|32 位 php_sqlsrv_71_nts.dll<br />32 位 php_pdo_sqlsrv_71_nts.dll|7.1|否 |32 位 php7.dll|
|32 位 php_sqlsrv_71_ts.dll <br />32 位 php_pdo_sqlsrv_71_ts.dll |7.1|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_71_nts.dll<br />64 位 php_pdo_sqlsrv_71_nts.dll|7.1|否 |64 位 php7.dll|
|64 位 php_sqlsrv_71_ts.dll <br />64 位 php_pdo_sqlsrv_71_ts.dll |7.1|是|64 位 php7ts.dll|
|32 位 php_sqlsrv_72_nts.dll<br />32 位 php_pdo_sqlsrv_72_nts.dll|7.2|否 |32 位 php7.dll|
|32 位 php_sqlsrv_72_ts.dll <br />32 位 php_pdo_sqlsrv_72_ts.dll |7.2|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_72_nts.dll<br />64 位 php_pdo_sqlsrv_72_nts.dll|7.2|否 |64 位 php7.dll|
|64 位 php_sqlsrv_72_ts.dll <br />64 位 php_pdo_sqlsrv_72_ts.dll |7.2|是|64 位 php7ts.dll|
|32 位 php_sqlsrv_73_nts.dll<br />32 位 php_pdo_sqlsrv_73_nts.dll|7.3|否 |32 位 php7.dll|
|32 位 php_sqlsrv_73_ts.dll <br />32 位 php_pdo_sqlsrv_73_ts.dll |7.3|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_73_nts.dll<br />64 位 php_pdo_sqlsrv_73_nts.dll|7.3|否 |64 位 php7.dll|
|64 位 php_sqlsrv_73_ts.dll <br />64 位 php_pdo_sqlsrv_73_ts.dll |7.3|是|64 位 php7ts.dll|

在 Linux 上，包括以下版本的驱动程序：

|驱动程序文件|PHP 版本|线程是否安全？|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|否 |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|是|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|否 |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|是|
|php_sqlsrv_73_nts.so<br />php_pdo_sqlsrv_73_nts.so|7.3|否 |
|php_sqlsrv_73_ts.so <br />php_pdo_sqlsrv_73_ts.so |7.3|是|

**Microsoft Drivers 5.3 for PHP for SQL Server：**

在 Windows 上，包括以下版本的驱动程序：

|驱动程序文件|PHP 版本|线程是否安全？|与 PHP .dll 一起使用|
|---------------|:---------------:|:----------------:|---------------------|
|32 位 php_sqlsrv_7_nts.dll <br />32 位 php_pdo_sqlsrv_7_nts.dll |7.0|否 |32 位 php7.dll|
|32 位 php_sqlsrv_7_ts.dll  <br />32 位 php_pdo_sqlsrv_7_ts.dll  |7.0|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_7_nts.dll <br />64 位 php_pdo_sqlsrv_7_nts.dll |7.0|否 |64 位 php7.dll|
|64 位 php_sqlsrv_7_ts.dll  <br />64 位 php_pdo_sqlsrv_7_ts.dll  |7.0|是|64 位 php7ts.dll|
|32 位 php_sqlsrv_71_nts.dll<br />32 位 php_pdo_sqlsrv_71_nts.dll|7.1|否 |32 位 php7.dll|
|32 位 php_sqlsrv_71_ts.dll <br />32 位 php_pdo_sqlsrv_71_ts.dll |7.1|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_71_nts.dll<br />64 位 php_pdo_sqlsrv_71_nts.dll|7.1|否 |64 位 php7.dll|
|64 位 php_sqlsrv_71_ts.dll <br />64 位 php_pdo_sqlsrv_71_ts.dll |7.1|是|64 位 php7ts.dll|
|32 位 php_sqlsrv_72_nts.dll<br />32 位 php_pdo_sqlsrv_72_nts.dll|7.2|否 |32 位 php7.dll|
|32 位 php_sqlsrv_72_ts.dll <br />32 位 php_pdo_sqlsrv_72_ts.dll |7.2|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_72_nts.dll<br />64 位 php_pdo_sqlsrv_72_nts.dll|7.2|否 |64 位 php7.dll|
|64 位 php_sqlsrv_72_ts.dll <br />64 位 php_pdo_sqlsrv_72_ts.dll |7.2|是|64 位 php7ts.dll|

在 Linux 上，包括以下版本的驱动程序：

|驱动程序文件|PHP 版本|线程是否安全？|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|否 |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|是|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|否 |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|是|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|否 |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|是|

**Microsoft Drivers 5.2 for PHP for SQL Server：**

在 Windows 上，包括以下版本的驱动程序：

|驱动程序文件|PHP 版本|线程是否安全？|与 PHP .dll 一起使用|
|---------------|:---------------:|:----------------:|---------------------|
|32 位 php_sqlsrv_7_nts.dll <br />32 位 php_pdo_sqlsrv_7_nts.dll |7.0|否 |32 位 php7.dll|
|32 位 php_sqlsrv_7_ts.dll  <br />32 位 php_pdo_sqlsrv_7_ts.dll  |7.0|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_7_nts.dll <br />64 位 php_pdo_sqlsrv_7_nts.dll |7.0|否 |64 位 php7.dll|
|64 位 php_sqlsrv_7_ts.dll  <br />64 位 php_pdo_sqlsrv_7_ts.dll  |7.0|是|64 位 php7ts.dll|
|32 位 php_sqlsrv_71_nts.dll<br />32 位 php_pdo_sqlsrv_71_nts.dll|7.1|否 |32 位 php7.dll|
|32 位 php_sqlsrv_71_ts.dll <br />32 位 php_pdo_sqlsrv_71_ts.dll |7.1|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_71_nts.dll<br />64 位 php_pdo_sqlsrv_71_nts.dll|7.1|否 |64 位 php7.dll|
|64 位 php_sqlsrv_71_ts.dll <br />64 位 php_pdo_sqlsrv_71_ts.dll |7.1|是|64 位 php7ts.dll|
|32 位 php_sqlsrv_72_nts.dll<br />32 位 php_pdo_sqlsrv_72_nts.dll|7.2|否 |32 位 php7.dll|
|32 位 php_sqlsrv_72_ts.dll <br />32 位 php_pdo_sqlsrv_72_ts.dll |7.2|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_72_nts.dll<br />64 位 php_pdo_sqlsrv_72_nts.dll|7.2|否 |64 位 php7.dll|
|64 位 php_sqlsrv_72_ts.dll <br />64 位 php_pdo_sqlsrv_72_ts.dll |7.2|是|64 位 php7ts.dll|

在 Linux 上，包括以下版本的驱动程序：

|驱动程序文件|PHP 版本|线程是否安全？|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|否 |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|是|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|否 |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|是|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|否 |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|是|

**Microsoft Drivers 4.3 for PHP for SQL Server：**

在 Windows 上，包括以下版本的驱动程序：

|驱动程序文件|PHP 版本|线程是否安全？|与 PHP .dll 一起使用|
|---------------|:---------------:|:----------------:|---------------------|
|32 位 php_sqlsrv_7_nts.dll <br />32 位 php_pdo_sqlsrv_7_nts.dll |7.0|否 |32 位 php7.dll|
|32 位 php_sqlsrv_7_ts.dll  <br />32 位 php_pdo_sqlsrv_7_ts.dll  |7.0|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_7_nts.dll <br />64 位 php_pdo_sqlsrv_7_nts.dll |7.0|否 |64 位 php7.dll|
|64 位 php_sqlsrv_7_ts.dll  <br />64 位 php_pdo_sqlsrv_7_ts.dll  |7.0|是|64 位 php7ts.dll|
|32 位 php_sqlsrv_71_nts.dll<br />32 位 php_pdo_sqlsrv_71_nts.dll|7.1|否 |32 位 php7.dll|
|32 位 php_sqlsrv_71_ts.dll <br />32 位 php_pdo_sqlsrv_71_ts.dll |7.1|是|32 位 php7ts.dll|
|64 位 php_sqlsrv_71_nts.dll<br />64 位 php_pdo_sqlsrv_71_nts.dll|7.1|否 |64 位 php7.dll|
|64 位 php_sqlsrv_71_ts.dll <br />64 位 php_pdo_sqlsrv_71_ts.dll |7.1|是|64 位 php7ts.dll|

在 Linux 上，包括以下版本的驱动程序：

|驱动程序文件|PHP 版本|线程是否安全？|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|否 |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|是|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|否 |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|是|

**Microsoft Drivers 4.0 for PHP for SQL Server：**

在 Windows 上，包括以下版本的驱动程序：

|驱动程序文件|PHP 版本|线程是否安全？|与 PHP .dll 一起使用|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_7_nts_x86.dll<br />php_pdo_sqlsrv_7_nts_x86.dll|7.0|否|32 位 php7.dll|
|php_sqlsrv_7_ts_x86.dll<br />php_pdo_sqlsrv_7_ts_x86.dll|7.0|是|32 位 php7ts.dll|
|php_sqlsrv_7_nts_x64.dll<br />php_pdo_sqlsrv_7_nts_x64.dll|7.0|否|64 位 php7.dll|
|php_sqlsrv_7_ts_x64.dll<br />php_pdo_sqlsrv_7_ts_x64.dll|7.0|是|64 位 php7ts.dll|

在 Linux 上，包括以下版本的驱动程序：

|驱动程序文件|PHP 版本|线程是否安全？|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|否 |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|是|

**Microsoft Drivers 3.2 for PHP for SQL Server：**

在 Windows 上，包括以下版本的驱动程序：

|驱动程序文件|PHP 版本|线程是否安全？|与 PHP .dll 一起使用|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_54_nts.dll<br />php_pdo_sqlsrv_54_nts.dll|5.4|否|php5.dll|
|php_sqlsrv_54_ts.dll<br />php_pdo_sqlsrv_54_ts.dll|5.4|是|php5ts.dll|
|php_sqlsrv_55_nts.dll<br />php_pdo_sqlsrv_55_nts.dll|5.5|否|php5.dll|
|php_sqlsrv_55_ts.dll<br />php_pdo_sqlsrv_55_ts.dll|5.5|是|php5ts.dll|
|php_sqlsrv_56_nts.dll<br />php_pdo_sqlsrv_56_nts.dll|5.6|否|php5.dll|
|php_sqlsrv_56_ts.dll<br />php_pdo_sqlsrv_56_ts.dll|5.6|是|php5ts.dll|

## <a name="see-also"></a>另请参阅

- [开始使用 Microsoft Drivers for PHP for SQL Server](getting-started-with-the-php-sql-driver.md)
- [Microsoft Drivers for PHP for SQL Server 编程指南](programming-guide-for-php-sql-driver.md)
- [SQLSRV 驱动程序 API 参考](sqlsrv-driver-api-reference.md)
- [PDO_SQLSRV 驱动程序 API 参考](pdo-sqlsrv-driver-reference.md)
