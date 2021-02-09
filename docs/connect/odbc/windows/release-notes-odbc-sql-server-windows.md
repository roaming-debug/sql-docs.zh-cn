---
title: Windows 上的 ODBC Driver for SQL Server 发行说明
description: 本发行说明文章介绍了 Windows 上每个发行版 Microsoft ODBC Driver for SQL Server 的变更内容。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: b8459ed8-625e-4d8b-891c-e7e78c9977cc
ms.reviewer: v-chojas
author: v-makouz
ms.author: v-chojas
manager: kenvh
ms.openlocfilehash: e587d8938cd5c63172f9e27622b680de62cb7cea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168006"
---
# <a name="release-notes-for-microsoft-odbc-driver-for-sql-server-on-windows"></a>Windows 上的 Microsoft ODBC Driver for SQL Server 发行说明

本发行说明文章介绍适用于 Windows 上 SQL Server 的 Microsoft ODBC 驱动程序的新增功能。

<!--
PLEASE USE THE STANDARD 2-COLUMN TABLE FORMAT!

For all our Release Notes articles (What's New too?), we are standardizing on the 2-column format that you see here for version "## 17.3".

Going forward, all new additions to this article must use the 2-column format.

Also, use the shorter ## H2 title format, which eliminates all the redundant constants, and appends the date-added.
One beneift of shortness is the avoidance of the annoying wrapping of unnecessarily long H2 titles in the rightNav.
- OLD H2:  ## What's New in the [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 17.3 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] on Windows
- NEW H2:  ## 17.3, February 2019

By the way, in GitHub, the file name is changing today 2019/03/30:
- FROM:  docs/connect/odbc/windows/release-notes.md
- TO  :  docs/connect/odbc/windows/release-notes-odbc-sql-server-windows.md

Thank you.
GeneMi (and CraigG).  2019/03/30.
-->

## <a name="177"></a>17.7

![下载](../../../ssms/media/download-icon.png)[下载 x64 安装程序](https://go.microsoft.com/fwlink/?linkid=2153471)  
![下载](../../../ssms/media/download-icon.png)[下载 x86 安装程序](https://go.microsoft.com/fwlink/?linkid=2153469)  

版本号：17.7.1.1  
发布日期：2021 年 1 月 29 日

如果想要安装的语言版本不包含在检测到的语言中，可以使用以下直接链接。  
对于 x64 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40a)  
对于 x86 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40a)

| 新增功能 | 详细信息 |
| :------- | :------ |
| Azure Key Vault 交互式身份验证 | 请参阅[使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| 服务主体身份验证 | 请参阅 [DSN 和连接字符串属性及关键字](../dsn-connection-string-attribute.md)。 |
| 能够插入已加密的 money 和 smallmoney 列 | 请参阅[使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| bug 修复。 | [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="previous-releases"></a>以前的版本

## <a name="176"></a>17.6

![下载](../../../ssms/media/download-icon.png)[下载 x64 安装程序](https://go.microsoft.com/fwlink/?linkid=2137027)  
![下载](../../../ssms/media/download-icon.png)[下载 x86 安装程序](https://go.microsoft.com/fwlink/?linkid=2137028)  

版本号：17.6.1.1  
发布日期：2020 年 7 月 31 日

如果想要安装的语言版本不包含在检测到的语言中，可以使用以下直接链接。  
对于 x64 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x40a)  
对于 x86 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x40a)

| 新增功能 | 详细信息 |
| :------- | :------ |
| 预定义语句的元数据缓存 | 请参阅[使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| SQL_COPT_SS_AUTOBEGINTXN 连接属性用于控制自动 BEGIN TRANSACTION 是在 ROLLBACK 后还是 COMMIT 之后发生 | 请参阅 [DSN 和连接字符串属性及关键字](../dsn-connection-string-attribute.md)。 |
| bug 修复。 | [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="1752"></a>17.5.2

![下载](../../../ssms/media/download-icon.png)[下载 x64 安装程序](https://go.microsoft.com/fwlink/?linkid=2120137)  
![下载](../../../ssms/media/download-icon.png)[下载 x86 安装程序](https://go.microsoft.com/fwlink/?linkid=2120140)  

版本号：17.5.2.1  
发布日期：2020 年 3 月 6 日

如果想要安装的语言版本不包含在检测到的语言中，可以使用以下直接链接。  
对于 x64 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x40a)  
对于 x86 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x40a)  

### <a name="features-added-in-1752"></a>17.5.2 中的新增功能

| 新增功能 | 详细信息 |
| :------------ | :------ |
| 支持使用托管标识进行 Azure Key Vault 身份验证 | 请参阅[对 ODBC 驱动程序使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| 支持其他 Azure Key Vault 终结点 | 请参阅[对 ODBC 驱动程序使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| bug 修复。 | 请参阅 [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

通过单击以下部分中的下载链接，下载 ODBC Driver 的早期版本：

## <a name="175"></a>17.5

![下载](../../../ssms/media/download-icon.png)[下载 x64 安装程序](https://go.microsoft.com/fwlink/?linkid=2120248)  
![下载](../../../ssms/media/download-icon.png)[下载 x86 安装程序](https://go.microsoft.com/fwlink/?linkid=2120353)  

版本号：17.5.1.1  
发布日期：2020 年 1 月 31 日

如果想要安装的语言版本不包含在检测到的语言中，可以使用以下直接链接。  
对于 x64 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x40a)  
对于 x86 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x40a)  

### <a name="features-added-in-175"></a>17.5 中的新增功能

| 新增功能 | 详细信息 |
| :------------ | :------ |
| SQL_COPT_SS_SPID 连接属性，用于在不往返服务器的情况下检索 SPID | 请参阅 [DSN 和连接字符串属性及关键字](../dsn-connection-string-attribute.md)。 |
| bug 修复。 | 请参阅 [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="1742"></a>17.4.2

![下载](../../../ssms/media/download-icon.png)[下载 x64 安装程序](https://go.microsoft.com/fwlink/?linkid=2120354)  
![下载](../../../ssms/media/download-icon.png)[下载 x86 安装程序](https://go.microsoft.com/fwlink/?linkid=2120249)  

版本号：17.4.2.1  
发布日期：2019 年 10 月

如果想要安装的语言版本不包含在检测到的语言中，可以使用以下直接链接。  
对于 x64 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x40a)  
对于 x86 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x40a)  

### <a name="features-added-in-1742"></a>17.4.2 中的新增功能

| 新增功能 | 详细信息 |
| :------------ | :------ |
| 支持其他 Azure Key Vault 终结点 | 请参阅[对 ODBC 驱动程序使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| 支持设置数据分类版本 | 请参阅[数据分类](../data-classification.md#bkmk-version)。 |
| 在安装程序中添加 Azure Active Directory 身份验证库 (adal.dll) | 现已包含在基础驱动程序安装中，ODBC 驱动程序将升级适用于 SQL Server 的 Microsoft Active Directory 身份验证库的现有安装，同时从 Windows 的已安装应用程序列表中将其删除。 |
| bug 修复。 | 请参阅 [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="174"></a>17.4

![下载](../../../ssms/media/download-icon.png)[下载 x64 安装程序](https://go.microsoft.com/fwlink/?linkid=2120149)  
![下载](../../../ssms/media/download-icon.png)[下载 x86 安装程序](https://go.microsoft.com/fwlink/?linkid=2120150)  

版本号：17.4.1.1  
发布日期：2019 年 7 月

如果想要安装的语言版本不包含在检测到的语言中，可以使用以下直接链接。  
对于 x64 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x40a)  
对于 x86 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x40a)  

### <a name="features-added-in-174"></a>17.4 中的新增功能

| 新增功能 | 详细信息 |
| :------------ | :------ |
| 具有安全 Enclave 的 Always Encrypted。 | 请参阅[对 ODBC 驱动程序使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| 可配置的 TCP 保持活动状态设置。 | 请参阅[连接到 SQL Server](../linux-mac/connection-string-keywords-and-data-source-names-dsns.md)。 |
| bug 修复。 | 请参阅 [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="173"></a>17.3

![下载](../../../ssms/media/download-icon.png)[下载 x64 安装程序](https://go.microsoft.com/fwlink/?linkid=2120355)  
![下载](../../../ssms/media/download-icon.png)[下载 x86 安装程序](https://go.microsoft.com/fwlink/?linkid=2120356)  

版本号：17.3.1.1  
发布日期：2019 年 2 月

如果想要安装的语言版本不包含在检测到的语言中，可以使用以下直接链接。  
对于 x64 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x40a)  
对于 x86 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x40a)  

### <a name="features-added-in-173"></a>17.3 中的新增功能

| 新增功能 | 详细信息 |
| :------------ | :------ |
| Azure Active Directory 托管标识（系统和用户分配）身份验证模式。 | 请参阅[结合使用 Azure Active Directory 和 ODBC Driver](../using-azure-active-directory.md)。 |
| 能够针对 Always Encrypted 列流式传输输入参数。 | 请参阅[使用 Always Encrypted 时的 ODBC 驱动程序限制](../using-always-encrypted-with-the-odbc-driver.md#limitations-of-the-odbc-driver-when-using-always-encrypted)。 |
| XA 分布式事务。 | [使用 XA 事务](../use-xa-with-dtc.md)。 |
| 已更新的 Visual C++ 可再发行程序包 | 将运行时依赖项升级到 Visual C++ 2017 可再发行程序包（[X64 下载](https://aka.ms/vs/15/release/vc_redist.x64.exe)、[X86 下载](https://aka.ms/vs/15/release/vc_redist.x86.exe)） |
| bug 修复。 | 请参阅 [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="172"></a>17.2

![下载](../../../ssms/media/download-icon.png)[下载 x64 安装程序](https://go.microsoft.com/fwlink/?linkid=2120250)  
![下载](../../../ssms/media/download-icon.png)[下载 x86 安装程序](https://go.microsoft.com/fwlink/?linkid=2120357)  

版本号：17.2.0.1  
发布日期：2018 年 7 月

如果想要安装的语言版本不包含在检测到的语言中，可以使用以下直接链接。  
对于 x64 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x40a)  
对于 x86 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x40a)  

### <a name="features-added-in-172"></a>17.2 中的新增功能

| 新增功能 | 详细信息 |
| :------------ | :------ |
| 对 Azure SQL 数据库和 SQL Server 进行数据分类。 | 请参阅[数据分类](../data-classification.md)。 |
| 支持 UTF-8 服务器编码。 | &nbsp; |
| bug 修复。 | 请参阅 [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="171"></a>17.1

![下载](../../../ssms/media/download-icon.png)[下载 x64 安装程序](https://go.microsoft.com/fwlink/?linkid=2120151)  
![下载](../../../ssms/media/download-icon.png)[下载 x86 安装程序](https://go.microsoft.com/fwlink/?linkid=2120443)  

版本号：17.1.0.1  
发布日期：2018 年 3月

如果想要安装的语言版本不包含在检测到的语言中，可以使用以下直接链接。  
对于 x64 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x40a)  
对于 x86 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x40a)  

### <a name="features-added-in-171"></a>17.1 中的新增功能

| 新增功能 | 详细信息 |
| :------------ | :------ |
| 支持 `SQL_COPT_SS_CEKCACHETTL` 和 `SQL_COPT_SS_TRUSTEDCMKPATHS` 连接属性。 | &bull; &nbsp; `SQL_COPT_SS_CEKCACHETTL`<br/>允许控制列加密密钥的本地缓存的保留时间以及刷新该时间。<br/><br/>&bull; &nbsp; `SQL_COPT_SS_TRUSTEDCMKPATHS`<br/>允许应用程序将 AE 操作限制为仅使用指定的列主密钥列表。<br/><br/> 有关详细信息，请参阅[在 ODBC Driver for SQL Server 中使用 Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md)。 |
| Azure Active Directory 交互式身份验证支持 | &nbsp; |
| bug 修复。 | 请参阅 [bug 修复](../bug-fixes.md)。 |
| &nbsp; | &nbsp; |

## <a name="170"></a>17.0

![下载](../../../ssms/media/download-icon.png)[下载 x64 安装程序](https://go.microsoft.com/fwlink/?linkid=2120444)  
![下载](../../../ssms/media/download-icon.png)[下载 x86 安装程序](https://go.microsoft.com/fwlink/?linkid=2120152)  

版本号：17.0.1.1  
发布日期：2018 年 2 月

如果想要安装的语言版本不包含在检测到的语言中，可以使用以下直接链接。  
对于 x64 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x40a)  
对于 x86 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x40a)  

### <a name="features-added-in-170"></a>17.0 中的新增功能

| 新增功能 | 详细信息 |
| :------------ | :------ |
| 对 BCP API 的 Always Encrypted 支持。 | &nbsp; |
| 新连接字符串属性 `UseFMTOnly`。 | 使驱动程序在需要临时表的特殊情况下使用旧的元数据。 |
| 支持 Azure SQL 托管实例。 | 请参阅下面的[使用托管实例（ODBC 版本 17）时的差异](#diffs-managed-instance-17)列表。 |
| &nbsp; | &nbsp; |

| 更改的依赖项 | 详细信息 |
| :------------ | :------ |
| 删除了 Microsoft 联机服务登录助手 | 该依赖项已删除。 |
| &nbsp; | &nbsp; |

### <a name="differences-when-using-managed-instance-odbc-version-17"></a><a name="diffs-managed-instance-17"></a> 使用托管实例（ODBC 版本 17）时的差异

此版本的 ODBC 包含对 Azure SQL 托管实例的支持。 请参阅如下列出的使用托管实例时的差异列表。

> [!NOTE]
> 使用托管实例时存在许多差异：
>
> - 不支持 FILESTREAM。
> - 不支持本地文件系统访问，但这对于跟踪文件等类似内容是必需的。
> - 不支持从本地路径创建 UDT。
> - 不支持 Windows 集成身份验证。
> - 不支持 DTC。
> - `sa` 帐户不存在（默认帐户称为 `cloudSA`）。
> - TDS 令牌错误 (0xAA) 返回不正确的服务器名称。
> - 不支持数据库名称中的特殊字符。
> - 不支持 ALTER DATABASE [dbname1] MODIFY NAME = [dbname2]。
> - 无论语言设置如何，错误消息始终以英语显示（与 Azure 相同）。

## <a name="131"></a>13.1

![下载](../../../ssms/media/download-icon.png)[下载 x64 安装程序](https://go.microsoft.com/fwlink/?linkid=2121020)  
![下载](../../../ssms/media/download-icon.png)[下载 x86 安装程序](https://go.microsoft.com/fwlink/?linkid=2120923)  

版本号：13.1  

如果想要安装的语言版本不包含在检测到的语言中，可以使用以下直接链接。  
对于 x64 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x40a)  
对于 x86 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x40a)  

[下载 Microsoft Command Line Utilities 13.1 for SQL Server](https://www.microsoft.com/download/details.aspx?id=53591)

### <a name="features-added-in-131"></a>13.1 中的新增功能

| 新增功能 | 详细信息 |
| :------------ | :------ |
| ODBC Driver 13.1 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 添加了对 [Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md) 和 [Azure Active Directory](../using-azure-active-directory.md) 的支持。 | 连接到 Microsoft SQL Server 2016 或更高版本时即可使用这些新增支持。 |
| 存在与对 Always Encrypted 和 Azure Active Directory 的支持相对应的连接池关键字和属性。 | [ODBC Driver for SQL Server 中识别驱动程序的连接池](driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md)中介绍了这些关键字和属性。 |
| &nbsp; | &nbsp; |

## <a name="13"></a>13

![下载](../../../ssms/media/download-icon.png)[下载 x64 安装程序](https://go.microsoft.com/fwlink/?linkid=2121118)  
![下载](../../../ssms/media/download-icon.png)[下载 x86 安装程序](https://go.microsoft.com/fwlink/?linkid=2120924)  

版本号：13  

如果想要安装的语言版本不包含在检测到的语言中，可以使用以下直接链接。  
对于 x64 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x40a)  
对于 x86 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x40a)  

[下载 Microsoft Command Line Utilities 13 for SQL Server](https://www.microsoft.com/download/details.aspx?id=52680)

### <a name="features-added-in-13"></a>13 中的新增功能

| 新增功能 | 详细信息 |
| :------------ | :------ |
| 添加对 Microsoft SQL Server 2016 的支持。 | 保留 ODBC 驱动程序版本 11 的功能。 |
| &nbsp; | &nbsp; |

## <a name="11"></a>11

![下载](../../../ssms/media/download-icon.png)[下载 x64 安装程序](https://go.microsoft.com/fwlink/?linkid=2121206)  
![下载](../../../ssms/media/download-icon.png)[下载 x86 安装程序](https://go.microsoft.com/fwlink/?linkid=2121021)  

版本号：11  

如果想要安装的语言版本不包含在检测到的语言中，可以使用以下直接链接。  
对于 x64 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x40a)  
对于 x86 驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x40a)  

[下载 Microsoft Command Line Utilities 11 for SQL Server](https://www.microsoft.com/download/details.aspx?id=36433)  

### <a name="features-added-in-11"></a>11 中的新增功能

| 新增功能 | 详细信息 |
| :------------ | :------ |
| 包含新功能。 | 请参阅 [Windows 上 Microsoft ODBC Driver for SQL Server 的功能](features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md)。 |
| 包含 SQL Server 2012 Native Client 中 ODBC 附带的所有功能。 | &nbsp; |
| &nbsp; | &nbsp; |
