---
title: Microsoft ODBC Driver for SQL Server
description: Microsoft ODBC Driver for SQL Server 支持通过标准 ODBC API 连接到 SQL Server 和 Azure SQL 数据库。
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9f2ae91b-06af-4c9a-9d24-062df7bc4662
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b107155d9672b2a1d7b905a387e48373905dab55
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464167"
---
# <a name="microsoft-odbc-driver-for-sql-server"></a>Microsoft ODBC Driver for SQL Server

[!INCLUDE[ODBC_Current_Version](../../includes/odbc-latest-release.md)]

[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

ODBC 是以 C 和 C++ for SQL Server 编写的用于应用程序的主要本机数据访问 API。 大多数数据源都有适用的 ODBC 驱动程序。 其他可以使用 ODBC 的语言包括 COBOL、Perl、PHP 和 Python。 ODBC 广泛用于数据集成方案。

ODBC 驱动程序附带 [sqlcmd](../../tools/sqlcmd-utility.md)  和 [bcp](../../tools/bcp-utility.md)  等工具。 使用 sqlcmd  实用工具可以运行 Transact-SQL 语句、系统过程和 SQL 脚本。 bcp  实用工具可以在 Microsoft SQL Server 实例和用户指定格式的数据文件间大批量复制数据。 使用 bcp  实用工具可以将大量新行导入 SQL Server 表，或将表数据导出到数据文件。  

## <a name="code-example-in-c"></a>C++ 中的代码示例

下面 C++ 示例演示了如何使用 ODBC API 连接和访问数据库：

- [使用 ODBC 的 C++ 代码示例](../../odbc/reference/sample-odbc-program.md)

## <a name="download"></a>下载

- ![Download-DownArrow-Circled](../../ssms/media/download-icon.png)[下载 ODBC 驱动程序](download-odbc-driver-for-sql-server.md)

## <a name="documentation"></a>文档

### <a name="features"></a>功能

- [自定义密钥存储提供程序](../../connect/odbc/custom-keystore-providers.md)
- [数据分类](../../connect/odbc/data-classification.md)
- [DSN 和连接字符串关键字和属性](dsn-connection-string-attribute.md)
- [SQL Server Native Client](../../relational-databases/native-client/features/sql-server-native-client-features.md)（没有 OLEDB，提供的功能也适用于 ODBC Driver for SQL Server）
- [使用 Always Encrypted](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)
- [使用 Azure Active Directory](../../connect/odbc/using-azure-active-directory.md)
- [使用透明网络 IP 解析](../../connect/odbc/using-transparent-network-ip-resolution.md)
- [使用 XA 事务](../../connect/odbc/use-xa-with-dtc.md)

### <a name="linux-and-macos"></a>Linux 和 macOS

- [在 Linux 上安装驱动程序](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)
- [在 macOS 上安装驱动程序](../../connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos.md)
- [连接到 SQL Server](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md)
- [使用 bcp 连接  ](../../connect/odbc/linux-mac/connecting-with-bcp.md)
- [使用 sqlcmd 进行连接  ](../../connect/odbc/linux-mac/connecting-with-sqlcmd.md)
- [数据访问跟踪](../../connect/odbc/linux-mac/data-access-tracing-with-the-odbc-driver-on-linux.md)
- [常见问题解答](../../connect/odbc/linux-mac/frequently-asked-questions-faq-for-odbc-linux.yml)
- [安装驱动程序管理器](../../connect/odbc/linux-mac/installing-the-driver-manager.md)
- [已知问题](../../connect/odbc/linux-mac/known-issues-in-this-version-of-the-driver.md)
- [编程指南](../../connect/odbc/linux-mac/programming-guidelines.md)
- [发行说明](../../connect/odbc/linux-mac/release-notes-odbc-sql-server-linux-mac.md)
- [发行说明 (mssql-tools)](../../connect/odbc/linux-mac/release-notes-tools.md)
- [支持高可用性和灾难恢复](../../connect/odbc/linux-mac/odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md)
- [使用集成身份验证 (Kerberos)](../../connect/odbc/linux-mac/using-integrated-authentication.md)

### <a name="windows"></a>Windows

- [异步执行（通知方法）示例](../../connect/odbc/windows/asynchronous-execution-notification-method-sample.md)
- [Windows ODBC 驱动程序中的连接弹性](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)
- [识别驱动程序的连接池](../../connect/odbc/windows/driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md)
- [功能和行为变更](../../connect/odbc/windows/features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md)
- [适用于 Windows 上 SQL Server 的 ODBC 的发行说明](windows/release-notes-odbc-sql-server-windows.md)
- [系统要求、安装和驱动程序文件](../../connect/odbc/windows/system-requirements-installation-and-driver-files.md)

## <a name="community"></a>社区

- [SQL Server 驱动程序博客](https://techcommunity.microsoft.com/t5/SQL-Server/bg-p/SQLServer/label-name/SQLServerDrivers)  
- [SQL Server 数据访问论坛](https://social.technet.microsoft.com/Forums/en/sqldataaccess/threads)  
