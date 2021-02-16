---
title: 连接库和框架
description: 列出客户端应用可通过多种语言使用的连接驱动程序，它们可连接到本地或云中、在 Linux、Windows 或 Docker 上运行的 Microsoft SQL Server，也可连接到 Azure SQL 数据库和 Azure Synapse Analytics。
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 80efe5ff-09ba-48a0-ac93-a91d62cff47c
ms.openlocfilehash: bd3e6740bf460eda42a1448bcbe9745a41e1a387
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273118"
---
# <a name="connectivity-libraries-and-frameworks-for-microsoft-sql-server"></a>适用于 Microsoft SQL Server 的连接库和框架

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

查阅[入门教程](https://aka.ms/sqldev)，通过编程语言（如 C#、Java、Node.js、PHP 和 Python）获得快速入门，并通过 Linux/Windows 上的 SQL Server 或 macOS 上的 Docker 生成应用。

下表列出了客户端应用程序可通过多种语言使用的连接库或驱动程序，它们可连接到本地或云中、在 Linux、Windows 或 Docker 上运行的 Microsoft SQL Server 并使用该服务器，也可连接到 Azure SQL 数据库和 Azure Synapse Analytics。 

| 语言 | 平台 | 其他资源 | 下载 | 入门 |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows、Linux、macOS | [用于 SQL Server 的 Microsoft ADO.NET](../connect/ado-net/microsoft-ado-net-sql-server.md) | [下载](https://msdn.microsoft.com/vstudio/aa496123.aspx) | [入门](https://www.microsoft.com/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows、Linux、macOS | [用于 SQL Server 的 Microsoft JDBC 驱动程序](../connect/jdbc/microsoft-jdbc-driver-for-sql-server.md) | [下载](https://go.microsoft.com/fwlink/?LinkId=245496) |  [入门](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows、Linux、macOS| [适用于 SQL Server 的 PHP SQL 驱动程序](../connect/php/microsoft-php-driver-for-sql-server.md) | 操作系统： <br/> \* [Windows](https://www.microsoft.com/download/details.aspx?id=20098) <br/> \* [Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \* [macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [入门](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu)
| Node.js | Windows、Linux、macOS | [适用于 SQL Server 的 Node.js 驱动程序](../connect/node-js/node-js-driver-for-sql-server.md) |  [入门](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu)
| Python | Windows、Linux、macOS | [Python SQL 驱动程序](../connect/python/python-driver-for-sql-server.md) <br/> \* [pyodbc](../connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development.md) |  [入门](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows、Linux、macOS | [适用于 SQL Server 的 Ruby 驱动程序](../connect/ruby/ruby-driver-for-sql-server.md) | [入门](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows、Linux、macOS | [Microsoft ODBC Driver for SQL Server](../connect/odbc/microsoft-odbc-driver-for-sql-server.md) | [下载](../connect/odbc/microsoft-odbc-driver-for-sql-server.md) |  

下表列出了对象关系映射 (ORM) 框架和 Web 框架的几个示例，客户端应用程序可将这些框架与本地或云中、在 Linux、Windows 或 Docker 上运行的 Microsoft SQL Server 以及 Azure SQL 数据库和 Azure Synapse Analytics 结合使用。 

| 语言 | 平台 | ORM(s) |
| :-- | :-- | :-- |
| C# | Windows、Linux、macOS | [实体框架](/ef)<br>[Entity Framework Core](/ef/core/index) |
| Java | Windows、Linux、macOS |[Hibernate ORM](https://hibernate.org/orm)|
| PHP | Windows、Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Windows、Linux、macOS | [Sequelize ORM](http://sequelize.org/) |
| Python | Windows、Linux、macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows、Linux、macOS | [Ruby on Rails](https://rubyonrails.org/) |

## <a name="related-links"></a>相关链接
- 用于从客户端应用程序进行连接的 [SQL Server 驱动程序](../connect/sql-connection-libraries.md)