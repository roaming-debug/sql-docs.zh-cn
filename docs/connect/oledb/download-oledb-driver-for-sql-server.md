---
title: 下载适用于 SQL Server 的 Microsoft OLE DB 驱动程序 | Microsoft Docs
description: 下载 Microsoft OLE DB Driver for SQL Server，以开发连接到 SQL Server 和 Azure SQL 数据库的本机 Windows 应用程序。
ms.date: 09/30/2020
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ea4fcf0ed6fdab9a9580d8f13227fb5bd079f3bc
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506401"
---
# <a name="download-microsoft-ole-db-driver-for-sql-server"></a>下载适用于 SQL Server 的 Microsoft OLE DB 驱动程序

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

OLE DB Driver for SQL Server 是独立的数据访问应用程序编程接口 (API)，用于 OLE DB。 Windows 上提供了 OLE DB Driver for SQL Server，并在一个动态链接库 (DLL) 中提供了 SQL OLE DB 驱动程序。

## <a name="download"></a>下载

适用于 Microsoft OLE DB Driver for SQL Server 的可再发行安装程序安装客户端组件，在运行时利用 SQL Server 的新功能需要这些组件。 从版本 18.3 开始，安装程序还包括并安装了 Microsoft Active Directory 身份验证库 (ADAL.dll)。

Microsoft OLE DB Driver 18.5 for SQL Server 是最新正式发行版 (GA)。 如果安装了 Microsoft OLE DB Driver 18 for SQL Server 的早期版本，安装 18.5 版会使其升级到 18.5 版。

[![下载](../../ssms/media/download-icon.png)下载 Microsoft OLE DB Driver for SQL Server (x64)](https://go.microsoft.com/fwlink/?linkid=2135577)   
[![下载](../../ssms/media/download-icon.png)下载 Microsoft OLE DB Driver for SQL Server (x86)](https://go.microsoft.com/fwlink/?linkid=2135722)   

### <a name="version-information"></a>版本信息

- 版本号：18.5.0
- 发布日期：2020 年 9 月 30 日

> [!Note]
> 如果你正从一个非英语的语言版本访问此页面并想要查看最新内容，请访问[此网站的英语（美国）版本]()。 可以通过选择[可用语言](#available-languages)从英语（美国）版本站点下载不同的语言。

## <a name="available-languages"></a>可用语言

可以安装以下语言的此版本 Microsoft OLE DB Driver for SQL Server：

Microsoft OLE DB Driver 18.5 for SQL Server (x64)：  
[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x40a)

Microsoft OLE DB Driver 18.5 for SQL Server (x86)：  
[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x40a)

## <a name="release-notes"></a>发行说明

有关此版本的详细信息，请参阅[发行说明](release-notes-for-oledb-driver-for-sql-server.md)。

## <a name="previous-releases"></a>以前的版本

[以前的 Microsoft OLE DB Driver for SQL Server 版本](release-notes-for-oledb-driver-for-sql-server.md#previous-releases)

## <a name="see-also"></a>另请参阅

[Microsoft OLE DB Driver for SQL Server 发行说明](release-notes-for-oledb-driver-for-sql-server.md)  
[适用于 SQL Server 的 OLE DB 驱动程序的系统要求](system-requirements-for-oledb-driver-for-sql-server.md)  
[适用于 SQL Server 的 OLE DB 驱动程序的支持策略](applications\support-policies-for-oledb-driver-for-sql-server.md)  
[何时使用适用于 SQL Server 的 OLE DB 驱动程序](when-to-use-oledb-driver-for-sql-server.md)  
[安装适用于 SQL Server 的 OLE DB 驱动程序](applications/installing-oledb-driver-for-sql-server.md)