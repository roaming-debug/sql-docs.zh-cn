---
title: 用于 Microsoft SQL 数据库的连接库 | Microsoft Docs
description: 提供模块的下载链接，这些模块支持使用各种客户端编程语言连接到 Microsoft SQL Server 和 Azure SQL 数据库。
author: David-Engel
ms.prod: sql
ms.technology: connectivity
ms.custom: ''
ms.topic: article
ms.date: 03/06/2020
ms.author: v-daenge
ms.openlocfilehash: b64d5d6d20e7d83385b040a49516cb9728b5f048
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058142"
---
# <a name="connection-modules-for-microsoft-sql-database"></a>Microsoft SQL 数据库的连接模块

本文提供了连接模块或驱动程序  的下载链接，客户端程序可以使用这些链接与 [Microsoft SQL Server](../relational-databases/databases/databases.md) 及其在云中的孪生平台 [Azure SQL 数据库](/azure/sql-database/)进行交互。 驱动程序适用于多种编程语言，在以下操作系统上运行：

- Linux
- macOS
- Windows

**OOP 与关系不匹配：**

*关系*：使用面向对象的编程 (OOP) 语言编写的客户端程序通常使用 SQL 驱动程序，这些驱动程序返回的查询数据的格式更倾向于关系，而不是面向对象。 使用 ADO.NET 的 C# 就是一个示例。 OOP 关系格式不匹配有时会使 OOP 代码更难以编写和理解。

*ORM*：其他驱动程序或框架以 OOP 格式返回查询的数据，避免了不匹配。 这些驱动程序的工作原理是：使定义的类与特定 SQL 表的数据列相匹配。 然后，该驱动程序将执行对象关系映射  (ORM)，以将查询的数据作为类的实例返回。 如以下两个示例：适用于 C# 的 Microsoft 实体框架 (EF) 和适用于 Java 的 Hibernate。

本文将分别讨论这两种连接驱动程序。

<a name="anchor-20-drivers-relational-access" />

## <a name="drivers-for-relational-access"></a>用于关系访问的驱动程序

| 语言 | 下载 SQL 驱动程序 |
| :------- | :---------------------- |
| C# | [ADO.NET](https://www.microsoft.com/net/download/)<br />[Microsoft.Data.SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient/)<br /><br />[适用于以下系统的 .NET Core：Linux-Ubuntu、macOS、Windows](https://dotnet.microsoft.com/download) |
| C++ | [ODBC](./odbc/download-odbc-driver-for-sql-server.md)<br /><br />[OLE DB](./oledb/download-oledb-driver-for-sql-server.md) |
| Java | [JDBC](./jdbc/download-microsoft-jdbc-driver-for-sql-server.md) |
| Node.js | [Node.js 驱动程序，安装说明](./node-js/step-1-configure-development-environment-for-node-js-development.md) |
| PHP | [PHP](./php/download-drivers-php-sql-server.md) |
| Python | [pyodbc 安装说明](./python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development.md)<br />[下载 ODBC](./odbc/download-odbc-driver-for-sql-server.md) |
| Ruby | [Ruby 驱动程序，安装说明](./ruby/step-1-configure-development-environment-for-ruby-development.md)<br />[Ruby 下载页](https://rubyinstaller.org/downloads/) |
| &nbsp; | &nbsp; |

<a name="anchor-40-drivers-orm-access" />

## <a name="drivers-for-orm-access"></a>用于 ORM 访问的驱动程序

下表列出了客户端应用程序用于连接到 Microsoft SQL 数据库的对象关系映射 (ORM) 框架的示例。

| 语言 | ORM 驱动程序下载 |
| :------- | :------------------ |
| C# | [Entity Framework Core](/ef/core/)<br />[实体框架（6.x 或更高版本）](/ef/) |
| Java | [Hibernate ORM](https://hibernate.org/orm)|
| PHP | [Eloquent ORM，包含在 Laravel 安装中](https://laravel.com/docs/) |
| Node.js | [Sequelize ORM](https://sequelize.org/) |
| Python | [Django](https://www.djangoproject.com/) |
| Ruby | [Ruby on Rails](https://rubyonrails.org/) |
| &nbsp; | &nbsp; |

<a name="anchor-60-build-an-app-webpages" />

## <a name="build-an-app-webpages"></a>“生成应用程序”网页

可以通过 [https://aka.ms/sqldev](https://aka.ms/sqldev)  转到一组“生成应用程序”  网页。 网页提供有关编程语言、操作系统和 SQL 连接驱动程序的多种组合的信息。 “生成应用程序”网页提供的信息包括以下各项：

- 有关语言 + 操作系统 + 驱动程序的各组合的入门方式的详细信息。
  - 安装最新 SQL 连接驱动程序的说明。
- 以下各项的代码示例：
  - 对象关系代码示例。
  - ORM 代码示例。
  - 列存储索引演示如何实现更快的性能。

**“生成应用程序”网页的第一页：**  
![“生成应用程序”网页的第一页屏幕截图](media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-choose-language-g21.png)

**“生成应用程序”网页的“Java - Ubuntu”菜单**  
![“生成应用程序”网页的 Java Ubuntu 菜单](media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-java-ubuntu-c31.png)

&nbsp;

## <a name="related-links"></a>相关链接

- [使用 Java 和其他语言连接到云中的 Azure SQL 数据库的代码示例](/azure/sql-database/sql-database-connect-query-java)。

<!--
Image references, **obsolete** markdown syntax alternative:

![Build-an-app webpages, first page screenshot][image-ref-163-buildanapp-webpages-first-page]
![Build-an-app webpages, menu Java Ubuntu][image-ref-167-buildanapp-webpages-menu-java-ubuntu]

[image-ref-163-buildanapp-webpages-first-page]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-choose-language-g21.png
[image-ref-167-buildanapp-webpages-menu-java-ubuntu]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-java-ubuntu-c31.png
-->