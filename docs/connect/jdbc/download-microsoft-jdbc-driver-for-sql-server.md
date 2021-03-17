---
title: 下载 Microsoft SQL Server JDBC 驱动程序
description: 下载 Microsoft JDBC Driver for SQL Server，以开发连接到 SQL Server 和 Azure SQL 数据库的 Java 应用程序。
ms.date: 03/02/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 451181b8-11e6-4d01-b547-9ac5aada8238
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bfb200c1b29f15489f8500ccb02d5bf066bf6937
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2021
ms.locfileid: "103575217"
---
# <a name="download-microsoft-jdbc-driver-for-sql-server"></a>下载 Microsoft SQL Server JDBC 驱动程序

Microsoft JDBC Driver for SQL Server 是一个 Type 4 JDBC 驱动程序，它通过 Java 平台中可用的标准 JDBC 应用程序编程接口 (API) 提供数据库连接。 所有用户都可以免费下载驱动程序。 通过这些程序，用户可以从任何 Java 应用程序、应用程序服务器或支持 Java 的小程序访问 SQL Server。

## <a name="download"></a>下载

版本 9.2 是最新正式发布 (GA) 版本。 它支持 Java 8、11 和 15。 如果需要在更低的 Java 运行时版本上运行，请参阅 [Java 和 JDBC 规范支持矩阵](microsoft-jdbc-driver-for-sql-server-support-matrix.md#java-and-jdbc-specification-support)，查看是否存在可以使用的受支持的驱动程序版本。 我们在不断改进 Java 连接支持。 因此，强烈建议使用最新版 Microsoft JDBC Driver。

[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 9.2 for SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2155948)  
[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 9.2 for SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2155949)  

### <a name="version-information"></a>版本信息

- 版本号：9.2.1
- 发布日期：2021 年 3 月 2 日

下载此驱动程序时，有多个 JAR 文件。 JAR 文件名表示它支持的 Java 版本。

> [!Note]
> 如果你正从一个非英语的语言版本访问此页面并想要查看最新内容，请访问[此网站的英语（美国）版本](https://aka.ms/downloadmssqljdbcenglish)。 可以通过选择[可用语言](#available-languages)从英语（美国）版本站点下载不同的语言。

## <a name="available-languages"></a>可用语言

此版本的 Microsoft JDBC Driver for SQL Server 提供以下语言版本：

Microsoft JDBC Driver 9.2.1 for SQL Server (zip)：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x40a)

Microsoft JDBC Driver 9.2.1 for SQL Server (tar.gz)：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x40a)

### <a name="release-notes"></a>发行说明

有关此版本的详细信息，请参阅[发行说明](release-notes-for-the-jdbc-driver.md)和[系统要求](system-requirements-for-the-jdbc-driver.md)。

### <a name="previous-releases"></a>以前的版本

若要下载早期版本，请参阅 [Microsoft JDBC Driver for SQL Server 的早期版本](release-notes-for-the-jdbc-driver.md#previous-releases)。

## <a name="using-the-jdbc-driver-with-maven-central"></a>结合使用 JDBC 驱动程序与 Maven Central

JDBC 驱动程序可以添加到 Maven 项目，具体方法是通过使用以下代码将它作为依赖项添加到 POM.xml 文件中：

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>9.2.1.jre11</version>
</dependency>
```  

## <a name="unsupported-drivers"></a>不受支持的驱动程序

无法从此处下载不受支持的驱动程序版本。 我们在不断改善 Java 连接支持。 因此，强烈建议使用最新版 Microsoft JDBC Driver。  
  
## <a name="next-steps"></a>后续步骤

若要详细了解 Microsoft JDBC Driver for SQL Server，请参阅 [JDBC 驱动程序概述](overview-of-the-jdbc-driver.md)和 [JDBC 驱动程序 GitHub 存储库](https://github.com/microsoft/mssql-jdbc/blob/dev/README.md)。
