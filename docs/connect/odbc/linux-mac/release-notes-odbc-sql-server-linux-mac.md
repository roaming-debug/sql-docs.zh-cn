---
title: Linux 和 macOS 上的 ODBC Driver for SQL Server 的发行说明
description: 了解 Microsoft ODBC Driver for SQL Server 的发布版本中的新增功能和已更新内容。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-jizho2
ms.technology: connectivity
ms.topic: conceptual
author: v-chojas
ms.author: v-jizho2
manager: kenvh
ms.openlocfilehash: 43f6be9103f000150d8069888b6129b36ccf29e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194605"
---
# <a name="release-notes-for-the-microsoft-odbc-driver-for-sql-server-on-linux-and-macos"></a>Linux 和 macOS 上的 Microsoft ODBC Driver for SQL Server 的发行说明

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

本文列出并介绍了 Linux 和 macOS 上的 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的版本控制版本中的新增功能。

<!--
Going forward, please use the new 2-column markdown table for each new H2 version section.
And we are keeping the H2 titles much shorter, partly by removing redundant or obvious info.
We want to track the Month YYYY each new version H2 section is added, at the end of the H2 title.
This is the new standard format for Release Notes article content.

OLD     FILE NAME:    linux-mac/release-notes.md
NOW NEW FILE NAME:    linux-mac/release-notes-odbc-sql-server-linux-mac.md

Thank you.
GeneMi.  2019/04/03.
-->

## <a name="177-january-2021"></a>17.7，2021 年 1 月

| 新建项 | 详细信息 |
| :------- | :------ |
| 支持新分发。 | Ubuntu 20.10、macOS Big Sur (11.0)、Oracle Linux 7 |
| 服务主体身份验证 | 请参阅 [DSN 和连接字符串属性及关键字](../dsn-connection-string-attribute.md)。 |
| 能够插入已加密的 money 和 smallmoney 列 | 请参阅[使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| bug 修复。 | [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="176-july-2020"></a>17.6，2020 年 7 月

| 新建项 | 详细信息 |
| :------- | :------ |
| 支持新分发。 | Ubuntu 20.04 |
| 联合身份验证支持 | 请参阅[使用 Azure Active Directory](../using-azure-active-directory.md)。 |
| 预定义语句的元数据缓存 | 请参阅[使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| SQL_COPT_SS_AUTOBEGINTXN 连接属性用于控制自动 BEGIN TRANSACTION 是在 ROLLBACK 后还是 COMMIT 之后发生 | 请参阅 [DSN 和连接字符串属性及关键字](../dsn-connection-string-attribute.md)。 |
| bug 修复。 | [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="17522-april-2020-alpine-linux-only"></a>17.5.2.2，2020 年 4 月（仅限 Alpine Linux）

| 新增功能 | 详细信息 |
| :------------ | :------ |
| 已修复 bug。 | 请参阅 [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="1752-march-2020"></a>17.5.2，2020 年 3 月

| 新增功能 | 详细信息 |
| :------------ | :------ |
| 支持使用托管标识进行 Azure Key Vault 身份验证 | 请参阅[对 ODBC 驱动程序使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| 支持其他 Azure Key Vault 终结点 | 请参阅[对 ODBC 驱动程序使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| bug 修复。 | 请参阅 [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="175-january-2020"></a>17.5，2020 年 1 月

| 新增功能 | 详细信息 |
| :------------ | :------ |
| SQL_COPT_SS_SPID 连接属性，用于在不往返服务器的情况下检索 SPID | 请参阅 [DSN 和连接字符串属性及关键字](../dsn-connection-string-attribute.md)。 |
| 支持通过 Debian 和 Ubuntu 上的 `debconf` 指示 EULA 接受情况 | 请参阅[安装驱动程序](./installing-the-microsoft-odbc-driver-for-sql-server.md)。 |
| 支持新分发。 | &bull; &nbsp; &nbsp; Alpine Linux（3.10、3.11）<br/>&bull; &nbsp; &nbsp; Oracle Linux 8<br/>&bull; &nbsp; &nbsp; Ubuntu 19.10<br/>&bull; &nbsp; &nbsp; macOS 10.15 |
| bug 修复。 | 请参阅 [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="1742-october-2019"></a>2019 年 10 月 17.4.2 版

| 新增功能 | 详细信息 |
| :------------ | :------ |
| 支持其他 Azure Key Vault 终结点 | 请参阅[对 ODBC 驱动程序使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| 支持设置数据分类版本 | 请参阅[数据分类](../data-classification.md#bkmk-version)。 |
| bug 修复。 | 请参阅 [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

**已知问题：**

使用具有安全 Enclave 和 Azure Key Vault 的 Always Encrypted 时，奇数密钥路径长度可能导致 CMK 签名验证错误。 如果遇到此问题，请尝试通过重命名 AKV 键，将密钥路径的长度更改为一个字符。

## <a name="174-august-2019"></a>17.4，2019 年 8 月

| 新增功能 | 详细信息 |
| :------------ | :------ |
| 具有安全 Enclave 的 Always Encrypted。 | 请参阅[对 ODBC 驱动程序使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| 动态加载 OpenSSL | 请参阅[编程指南](programming-guidelines.md#bkmk-openssl)。 |
| 可配置的 TCP 保持活动状态设置。 | 请参阅[连接到 SQL Server](connection-string-keywords-and-data-source-names-dsns.md)。 |
| bug 修复。 | 请参阅 [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="173-february-2019"></a>17.3 版，2019 年 2 月

| 新建项 | 详细信息 |
| :------- | :------ |
| 支持新分发。 | &bull; &nbsp; &nbsp; SUSE 15<br/>&bull; &nbsp; &nbsp; Ubuntu 18.10<br/>&bull; &nbsp; &nbsp; macOS 10.14 |
| Azure Active Directory 托管标识（系统和用户分配）身份验证模式。 | 请参阅[结合使用 Azure Active Directory 和 ODBC Driver](../using-azure-active-directory.md)。 |
| 能够针对 Always Encrypted 列流式传输输入参数。 | 有关详细信息，请参阅[使用 Always Encrypted 时的 ODBC 驱动程序限制](../using-always-encrypted-with-the-odbc-driver.md#limitations-of-the-odbc-driver-when-using-always-encrypted)。 |
| XA 分布式事务。 | 请参阅[使用 XA 事务](../use-xa-with-dtc.md)。<br/><br/>XA 是扩展体系结构 (eXtended Architecture) 的词首字母缩略词，它是执行访问多个服务器端数据存储系统的全局事务的标准  。 |
| &nbsp; | &nbsp; |

## <a name="172-july-2018"></a>17.2 版，2018 年 7 月

| 新建项 | 详细信息 |
| :------- | :------ |
| 支持新分发。 | &bull; &nbsp; &nbsp; Ubuntu 18.04 |
| 对 Azure SQL 数据库和 SQL Server 进行数据分类。 | 请参阅[数据分类](../data-classification.md)。 |
| 支持 UTF-8 服务器编码。 | &nbsp; |
| `SQLBrowseConnect` | &nbsp; |
| `libcurl` 上的动态依赖项。 | 从此版本开始，`libcurl` 包不再是显式依赖项。<br/>使用 Azure Key Vault 或 Azure Active Directory 身份验证时，需要使用 OpenSSL 或 NSS 的 `libcurl` 包。<br/>如果遇到有关 `libcurl` 的错误，请确保它已安装。 |
| 在连接字符串中使用 ConnectRetryCount 和 ConnectRetryInterval 关键字进行空闲连接复原。 | &bull; &nbsp; &nbsp; 使用 `SQL_COPT_SS_CONNECT_RETRY_COUNT`（只读）检索连接重试尝试的次数。<br/><br/>&bull; &nbsp; &nbsp; 使用 `SQL_COPT_SS_CONNECT_RETRY_INTERVAL`（只读）检索连接重试间隔的时长。<br/><br/>请参阅 [Windows ODBC Driver 中的连接复原](../windows/connection-resiliency-in-the-windows-odbc-driver.md)。 |
| bug 修复。 | [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="171-march-2018"></a>17.1 版，2018 年 3 月

| 新建项 | 详细信息 |
| :------- | :------ |
| 支持 `SQL_COPT_SS_CEKCACHETTL` 和 `SQL_COPT_SS_TRUSTEDCMKPATHS` 连接属性。 | &bull; &nbsp; &nbsp; `SQL_COPT_SS_CEKCACHETTL` 允许控制列加密密钥的本地缓存的保留时间以及刷新该时间。<br/><br/>&bull; &nbsp; &nbsp; `SQL_COPT_SS_TRUSTEDCMKPATHS` 允许应用程序将 Always Encrypted 操作限制为仅使用指定的列主密钥列表。<br/><br/>请参阅[结合使用 Always Encrypted 和 ODBC Driver for SQL Server](../using-always-encrypted-with-the-odbc-driver.md)。 |
| 支持从默认位置加载 `.rll`。 | 请参阅[安装文档中的“资源文件加载”部分](installing-the-microsoft-odbc-driver-for-sql-server.md#resource-file-loading)。 |
| bug 修复。 | [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="17"></a>17

**支持新分发**：macOS High Sierra 和 Ubuntu 17.10 

**性能改进**：驱动程序在 UTF-8/16 之间转换时，性能提升超过 10 倍。

**新增功能**：

对 BCP API 的 Always Encrypted 支持

新连接字符串属性 UseFMTOnly 使驱动程序在需要临时表的特殊情况下使用旧的元数据。

支持 Azure SQL 托管实例。 
> [!NOTE]
> 使用托管实例时存在许多差异：
> -   不支持 FILESTREAM 
> -   不支持本地文件系统访问，但这对于跟踪文件等类似内容是必需的 
> -   不支持从本地路径创建 UDT 
> -   不支持 Windows 集成身份验证 
> -   不支持 DTC 
> -   “sa”帐户不存在（默认帐户称为“cloudSA”）
> -   TDS 令牌错误 (0xAA) 返回不正确的服务器名称
> -   不支持数据库名称中的特殊字符 
> -   不支持 ALTER DATABASE [dbname1] MODIFY NAME = [dbname2]
> -   无论语言设置如何，错误消息始终以英语显示（与 Azure 相同） 

## <a name="131-for-ssnoversion-on-linux-and-macos-may-2017"></a>Linux 和 macOS 上的 13.1 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，2017 年 5 月

ODBC Driver 13.1 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 添加了对将 Always Encrypted 和 Azure Active Directory 与 Microsoft SQL Server 2016 一起使用的支持。

**支持新分发**：在 macOS 上的 ODBC Driver 的第一个版本中支持 OS X 10.11 和 macOS 10.12。 现在还支持 Ubuntu 16.10、Red Hat 6、7 和 SUSE 12。 每个平台都有与平台相关的包（RPM 或 DEB），用于简化安装和配置。 有关详细信息，请参阅 [Linux ](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md) 和 [macOS](../../../connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos.md) 的 ODBC 驱动程序安装说明。

**unixODBC 驱动程序管理器 2.3.1 支持更改**：ODBC 驱动程序不再依赖于 unixODBC 驱动程序管理器的自定义打包（RedHat 6 除外），而是依赖于分发包管理器来解析分发程序库中的 UnixODBC 依赖项。

**BCP API 支持**：Linux 和 macOS ODBC 驱动程序现支持使用 [BCP API 函数（bcp_init  等）](../../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)

## <a name="130-for-ssnoversion-on-linux"></a>Linux 上的 13.0 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

借助 Microsoft ODBC Driver 13.0 for SQL Server，SQL Server 2014 和 SQL Server 2016 现在也受支持。  

**支持新分发**：

Ubuntu 以及 Red Hat 和 SUSE 现在受支持。 每个平台都有与平台相关的包（RPM 或 DEB），用于简化安装和配置。  有关安装说明，请参阅[安装 ODBC Driver](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)。

**unixODBC 驱动程序管理器 2.3.1 支持**：除了较新的驱动程序管理器外，还有用于安装此依赖项的包，可以简化安装和配置。  

**透明网络 IP 解析**：透明网络 IP 解析是现有多子网故障转移功能的修订版，在第一个解析的主机名 IP 未响应且存在多个与主机名关联的 IP 的情况下，该功能会影响驱动程序的连接顺序。

**TLS 1.2 支持**：使用与 SQL Server 的安全通信时，Linux 上的 Microsoft ODBC Driver 13.0 for SQL Server 现支持 TLS 1.2。

## <a name="11-for-ssnoversion-on-linux"></a>Linux 上的 11 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

SUSE Linux 上的 ODBC 驱动程序（预览版）支持 64 位 SUSE Linux Enterprise 11 Service Pack 2。 有关详细信息，请参阅[系统需求](../../../connect/odbc/linux-mac/system-requirements.md)。  

Linux 上的 ODBC 驱动程序支持 [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)]。 有关详细信息，请参阅 [Linux 上的 ODBC Driver 对高可用性、灾难恢复的支持](../../../connect/odbc/linux-mac/odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md)。  

Linux 上的 ODBC 驱动程序支持与 Azure SQL 数据库的连接。

`-l` 选项（登录超时）已添加到 `bcp` 中。 有关详细信息，请参阅[使用 bcp 连接](../../../connect/odbc/linux-mac/connecting-with-bcp.md)  。
