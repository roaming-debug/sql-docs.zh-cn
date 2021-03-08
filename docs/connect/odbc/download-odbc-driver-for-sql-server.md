---
title: 下载 ODBC Driver for SQL Server
description: 下载 Microsoft ODBC Driver for SQL Server，以开发连接到 SQL Server 和 Azure SQL 数据库的本机代码应用程序。
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 53b09784-bb9d-4fd4-99d3-0492b3308ac4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 778d8f2f53ba56436047eee53d17dd827c58f3eb
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837564"
---
# <a name="download-odbc-driver-for-sql-server"></a>下载 ODBC Driver for SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Microsoft ODBC Driver for SQL Server 是单个动态链接库 (DLL)，它包含对使用本机代码 API 连接到 SQL Server 的应用程序的运行时支持。 使用 Microsoft ODBC Driver 17 for SQL Server 创建新的应用程序或增强需利用 SQL Server 新增功能的现有应用程序。

> [!Important]
> 如果向 Microsoft ODBC Driver for SQL Server 使用 Azure Active Directory 托管标识 (`authentication=ActiveDirectoryMSI`) 身份验证，请不要升级到版本 17.7。 17.7 中有一个问题，会在使用该身份验证模式时导致错误。

## <a name="download-for-windows"></a>适用于 Windows 的下载项

适用于 Microsoft ODBC Driver 17 for SQL Server 的可再发行安装程序安装客户端组件，在运行时利用 SQL Server 的新功能需要这些组件。 它可选择安装开发使用 ODBC API 的应用程序所需的头文件。 从版本 17.4.2 开始，安装程序还包括并安装 Microsoft Active Directory 身份验证库 (ADAL.dll)。

版本 17.7.1 是最新正式发布 (GA) 版本。 如果安装了低于 Microsoft ODBC Driver 17 for SQL Server 的版本，安装 17.7.1 版会使其升级到 17.7.1 版。

[![下载](../../ssms/media/download-icon.png)下载 Microsoft ODBC Driver 17 for SQL Server (x64)](https://go.microsoft.com/fwlink/?linkid=2153471)   
[![下载](../../ssms/media/download-icon.png)下载 Microsoft ODBC Driver 17 for SQL Server (x86)](https://go.microsoft.com/fwlink/?linkid=2153469)   

### <a name="version-information"></a>版本信息

- 版本号：17.7.1.1
- 发布日期：2021 年 1 月 29 日

> [!Note]
> 如果你正从一个非英语的语言版本访问此页面并想要查看最新内容，请访问[此网站的英语（美国）版本]()。 可以通过选择[可用语言](#available-languages)从英语（美国）版本站点下载不同的语言。

## <a name="available-languages"></a>可用语言

可以安装以下语言的此版本 Microsoft ODBC Driver for SQL Server：

Microsoft ODBC Driver 17.7.1 for SQL Server (x64)：  
[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40a)

Microsoft ODBC Driver 17.7.1 for SQL Server (x86)：  
[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40a)

### <a name="release-notes-for-windows"></a>Windows 的发行说明

有关 Windows 上此版本的详细信息，请参阅 [Windows 发行说明](windows\release-notes-odbc-sql-server-windows.md)。

### <a name="previous-releases-for-windows"></a>Windows 的早期版本

若要下载 Windows 的早期版本，请参阅 [Microsoft ODBC Driver for SQL Server 的早期版本](windows\release-notes-odbc-sql-server-windows.md#previous-releases)。

## <a name="download-for-linux-and-macos"></a>适用于 Linux 和 macOS 的下载

可以根据相关安装说明，使用适用于 Linux 和 macOS 的包管理器下载和安装 Microsoft ODBC Driver for SQL Server：  
[安装 ODBC for SQL Server (Linux)](linux-mac\installing-the-microsoft-odbc-driver-for-sql-server.md)  
[安装 ODBC for SQL Server (macOS)](linux-mac\install-microsoft-odbc-driver-sql-server-macos.md)  

如果需要下载包以进行脱机安装，可通过以下链接获得所有版本。

> [!Note]
> 名为 `msodbcsql17-*` 的包是最新版本。 名为 `msodbcsql-*` 的包是驱动程序的 13 版本。

### <a name="alpine"></a>Alpine

- [17.7.1.1 Alpine 包](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.1.1-1_amd64.apk)（[PGP 签名](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.1.1-1_amd64.sig)）
- [17.6.1.1 Alpine 包](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.6.1.1-1_amd64.apk)（[PGP 签名](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.6.1.1-1_amd64.sig)）
- [17.5.2.2 Alpine 包](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.2-1_amd64.apk)（[PGP 签名](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.2-1_amd64.sig)）
- [17.5.2.1 Alpine 包](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.1-1_amd64.apk)（[PGP 签名](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.1-1_amd64.sig)）
- [17.5.1.1 Alpine 包](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.1.1-1_amd64.apk)（[PGP 签名](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.1.1-1_amd64.sig)）

### <a name="debian"></a>Debian

- [Debian 10 .deb 包](https://packages.microsoft.com/debian/10/prod/pool/main/m/msodbcsql17/)
- [Debian 9 .deb 包](https://packages.microsoft.com/debian/9/prod/pool/main/m/msodbcsql17/)
- [Debian 8 .deb 包](https://packages.microsoft.com/debian/8/prod/pool/main/m/msodbcsql17/)
- [Debian 8 .deb 包 (msodbcsql 13.x)](https://packages.microsoft.com/debian/8/prod/pool/main/m/msodbcsql/)

### <a name="redhat"></a>RedHat

- [RedHat 8 .rpm 包](https://packages.microsoft.com/rhel/8/prod/)
- [RedHat 7 .rpm 包](https://packages.microsoft.com/rhel/7/prod/)
- [RedHat 6 .rpm 包](https://packages.microsoft.com/rhel/6/prod/)

### <a name="suse"></a>Suse

- [SuSE 15 .rpm 包](https://packages.microsoft.com/sles/15/prod/)
- [SuSE 12 .rpm 包](https://packages.microsoft.com/sles/12/prod/)
- [SuSE 11 .rpm 包](https://packages.microsoft.com/sles/11/prod/)

### <a name="ubuntu"></a>Ubuntu

- [Ubuntu 20.10 .deb 包](https://packages.microsoft.com/ubuntu/20.10/prod/pool/main/m/msodbcsql17/)
- [Ubuntu 20.04 .deb 包](https://packages.microsoft.com/ubuntu/20.04/prod/pool/main/m/msodbcsql17/)
- [Ubuntu 18.04 .deb 包](https://packages.microsoft.com/ubuntu/18.04/prod/pool/main/m/msodbcsql17/)
- [Ubuntu 16.04 .deb 包](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql17/)
- [Ubuntu 14.04 .deb 包](https://packages.microsoft.com/ubuntu/14.04/prod/pool/main/m/msodbcsql17/)
- [Ubuntu 16.04 .deb 包 (msodbcsql 13.x)](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/)
- [Ubuntu 14.04 .deb 包 (msodbcsql 13.x)](https://packages.microsoft.com/ubuntu/14.04/prod/pool/main/m/msodbcsql/)

另请参阅[安装 Linux 驱动程序](linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)。

### <a name="macos"></a>macOS

- 有关详细信息，请参阅 [Homebrew formulae](https://github.com/Microsoft/homebrew-mssql-release)。

另请参阅[安装 macOS 驱动程序](linux-mac/install-microsoft-odbc-driver-sql-server-macos.md)。

### <a name="older-linux-releases"></a>旧版 Linux

- **Red Hat Enterprise Linux 5 和 6（64 位）**  - [下载 Microsoft ODBC Driver 11 for SQL Server - Red Hat Linux](https://go.microsoft.com/fwlink/?LinkId=267321)  
- **SUSE Linux Enterprise 11 Service Pack 2（64 位）**  - [下载 Microsoft ODBC Driver 11（预览版）for SQL Server - SUSE Linux](https://go.microsoft.com/fwlink/?LinkId=264916)

### <a name="release-notes-for-linux-and-macos"></a>Linux 和 macOS 的发行说明

有关 Linux 和 macOS 的各个版本的详细信息，请参阅 [Linux 和 macOS 的发行说明](linux-mac\release-notes-odbc-sql-server-linux-mac.md)。
