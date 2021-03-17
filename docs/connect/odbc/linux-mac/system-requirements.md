---
title: 系统要求（SQL Server ODBC 驱动程序）
description: 本文列出了 Linux 和 macOS 操作系统上的 SQL Server ODBC 驱动程序的系统要求。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- prerequisites
- system requirements
- requirements
ms.assetid: f03b7fdd-0e9d-4e74-958d-e8c87e027348
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9c38bba31650ae495fc4ddb2a09a94d79900c121
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2021
ms.locfileid: "103575213"
---
# <a name="system-requirements-linux-and-macos"></a>系统要求（Linux 和 macOS）

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

本主题列出了使用 Linux 和 macOS 上的 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的要求。

## <a name="sql-version-compatibility"></a>SQL 版本兼容性

Linux 和 macOS 驱动程序 SQL 版本兼容性与 [Windows 驱动程序 SQL 版本兼容性](../windows/system-requirements-installation-and-driver-files.md#sql-version-compatibility)相同。

## <a name="operating-system-support"></a>操作系统支持

以下操作系统的 x64 体系结构支持 Linux 和 macOS 驱动程序的版本 17、13.1 和 13：

|驱动程序版本&nbsp;&#8594;<br />&#8595; 操作系统     |17.7|17.6|17.5|17.4|17.3|17.2|17.1|17.0|13.1|13|
|-------------------------------|----|----|----|----|----|----|----|----|----|---|
|Apple OS X 10.11 (El Capitan)  |    |    |    |是 |是 |是 |是 |是 |是 |是|
|Apple macOS 10.12 (Sierra)     |    |    |    |是 |是 |是 |是 |是 |是 |是|
|Apple macOS 10.13 (High Sierra)|    |是 |是 |是 |是 |是 |是 |是 |是 |是|
|Apple macOS 10.14 (Mojave)     |是 |是 |是 |是 |是 |    |    |    |    |   |
|Apple macOS 10.15 (Catalina)   |是 |是 |是 |    |    |    |    |    |    |   |
|Apple macOS 11.0 (Big Sur)     |是 |    |    |    |    |    |    |    |    |   |
|Alpine Linux 3.11              |是 |是 |是 |    |    |    |    |    |    |   |
|Alpine Linux 3.12              |是 |    |    |    |    |    |    |    |    |   |
|Debian Linux 8                 |    |是 |是 |是 |是 |是 |是 |是 |是 |是|
|Debian Linux 9                 |是 |是 |是 |是 |是 |是 |是 |是 |是 |是|
|Debian Linux 10                |是 |是 |是 |是 |    |    |    |    |    |   |
|Oracle Linux 7                 |是 |    |    |    |    |    |    |    |    |   |
|Oracle Linux 8                 |是 |是 |是 |    |    |    |    |    |    |   |
|Red Hat Enterprise Linux 6     |是 |是 |是 |是 |是 |是 |是 |是 |是 |是|
|Red Hat Enterprise Linux 7     |是 |是 |是 |是 |是 |是 |是 |是 |是 |是|
|Red Hat Enterprise Linux 8     |是 |是 |是 |是 |    |    |    |    |    |   |
|SUSE Linux Enterprise Server 11<sup>1</sup>|是 |是 |是 |是 |是 |是 |是 |是 |是 |是|
|SUSE Linux Enterprise Server 12|是 |是 |是 |是 |是 |是 |是 |是 |是 |是|
|SUSE Linux Enterprise Server 15|是 |是 |是 |是 |是 |    |    |    |    |   |
|Ubuntu Linux 14.04             |    |    |    |是 |是 |是 |是 |是 |是 |是|
|Ubuntu Linux 16.04             |是 |是 |是 |是 |是 |是 |是 |是 |是 |是|
|Ubuntu Linux 18.04             |是 |是 |是 |是 |是 |是 |    |    |    |   |
|Ubuntu Linux 20.04             |是 |是 |    |    |    |    |    |    |    |   |
|Ubuntu Linux 20.10             |是 |    |    |    |    |    |    |    |    |   |

<sup>1</sup> ODBC Driver 17 仅支持 SUSE Linux Enterprise Server 11 SP4

如[安装 ODBC Driver (Linux)](installing-the-microsoft-odbc-driver-for-sql-server.md) 和[安装 ODBC Driver (macOS) ](install-microsoft-odbc-driver-sql-server-macos.md)中所述，使用分发的程序包管理系统安装时，Linux 和 macOS 上的 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 13、13.1 和 17 版的安装包会自动解析驱动程序的依赖项。

## <a name="microsoft-odbc-driver-11-for-sql-server"></a>Microsoft ODBC Driver 11 for SQL Server  
  
* 针对 64 位 SQLLEN/SQLULEN 生成的 64 位 UnixODBC 2.3.0 驱动程序管理器。 Linux 上的 ODBC 驱动程序不支持更高版本的 64 位 UnixODBC 驱动程序管理器。 有关详细信息，请参阅 [Installing the Driver Manager](../../../connect/odbc/linux-mac/installing-the-driver-manager.md) 。  
  
* 用于 Red Hat Enterprise Linux 5（64 位）的 ODBC 驱动程序需要以下程序包，并且可以在此处下载：[Microsoft ODBC Driver 11 for SQL Server - Red Hat Linux](https://go.microsoft.com/fwlink/?LinkId=267321)  
  * `glibc`  
  * `libgcc`  
  * `libstdc++`  
  * `e2fsprogs-libs`  
  * `krb5-libs`  
  * `openssl`  
  
* 用于 Red Hat Enterprise Linux 6（64 位）的 ODBC 驱动程序需要以下程序包，并且可以在此处下载：[Microsoft ODBC Driver 11 for SQL Server - Red Hat Linux](https://go.microsoft.com/fwlink/?LinkId=267321)  
  * `glibc`  
  * `libgcc`  
  * `libstdc++`  
  * `libuuid`  
  * `krb5-libs`  
  * `openssl`  
  
* 用于 SUSE Linux Enterprise 11 Service Pack 2（64 位）的 ODBC 驱动程序需要以下程序包，并且可以在此处下载：[Microsoft ODBC Driver 11（预览版）for SQL Server - SUSE Linux](https://go.microsoft.com/fwlink/?LinkId=264916)  
  * `glibc`  
  * `libstdc++46`  
  * `libgcc46`  
  * `libuuid1`  
  * `krb5`  
  * `libopenssl0_9_8`  
  
## <a name="see-also"></a>另请参阅

[安装驱动程序管理器](../../../connect/odbc/linux-mac/installing-the-driver-manager.md)  
[此版本驱动程序中的已知问题](../../../connect/odbc/linux-mac/known-issues-in-this-version-of-the-driver.md)  
[发行说明](../../../connect/odbc/linux-mac/release-notes-odbc-sql-server-linux-mac.md)  
