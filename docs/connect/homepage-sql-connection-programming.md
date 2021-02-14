---
title: SQL 客户端编程的主页 | Microsoft Docs
description: 此页面收录了指向不同语言和操作系统的下载内容和文档的附注链接，可便于你连接到 SQL Server 或 Azure SQL 数据库。
author: David-Engel
ms.date: 11/07/2018
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-daveng
ms.author: v-daenge
ms.openlocfilehash: 10d04306621722abd9c9a170028ba632a8d95c9e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045497"
---
# <a name="homepage-for-client-programming-to-microsoft-sql-server"></a>Microsoft SQL Server 客户端编程的主页


欢迎访问与 Microsoft SQL Server 和云中的 Azure SQL 数据库进行交互的客户端编程的主页。 本文提供了以下信息：

- 列出并介绍了可用的语言和驱动程序组合。
  - 介绍了操作系统 Linux（Ubuntu 及其他）、macOS 和 Windows。
- 收录了每个组合的详细文档的链接。
- 适当时，显示某些语言的分层文档的区域和子区域。


#### <a name="azure-sql-database"></a>Azure SQL Database

在任何给定语言中，用于连接到 SQL Server 和 Azure SQL 数据库的代码几乎完全相同。

若要详细了解用于连接到 Azure SQL 数据库的连接字符串，请参阅：

- [使用 .NET Core (C#) 查询 Azure SQL 数据库](/azure/sql-database/sql-database-connect-query-dotnet-core)。
- 目录中前一篇文章附近关于其他语言的其他 Azure SQL 数据库文章。 例如，请参阅[使用 PHP 查询 Azure SQL 数据库](/azure/sql-database/sql-database-connect-query-php)。


#### <a name="build-an-app-webpages"></a>“生成应用程序”网页

“生成应用程序”  网页以另一种格式提供代码示例和配置信息。 有关详细信息，请参阅本文稍后将介绍的[标记为“‘生成应用程序’网站”  的部分](#an-204-aka-ms-sqldev)。



<a name="an-050-languages-clients" />

## <a name="languages-and-drivers-for-client-programs"></a>用于客户端程序的语言和驱动程序


下表中的每个语言图像都是一个链接，可便于详细了解如何结合使用相应语言与 SQL Server。 每个链接都可跳转到本文稍后将介绍的部分。

:::row:::
    :::column:::
        [![C# 徽标][image-ref-320-csharp]](#an-110-ado-net-docu)  

        [![Node.js 徽标][image-ref-340-node]](#an-140-node-js-docu)  

        [![Python 徽标][image-ref-370-python]](#an-180-python-docu)  
    :::column-end:::
    :::column:::
        [![.NET Framework 的 ORM 实体框架][image-ref-333-ef]](#an-116-csharp-ef-orm)  

        [**`ODBC for C++`**](#an-160-odbc-cpp-docu)<br/>[![cpp-big-plus][image-ref-322-cpp]](#an-160-odbc-cpp-docu)  

        [![Ruby 徽标][image-ref-380-ruby]](#an-190-ruby-docu)  
    :::column-end:::
    :::column:::
        [![Java 徽标][image-ref-330-java]](#an-130-jdbc-docu)  

        [![PHP 徽标][image-ref-360-php]](#an-170-php-docu)  
    :::column-end:::
:::row-end:::

#### <a name="downloads-and-installs"></a>下载和安装

下面的文章专门介绍如何下载和安装各种 SQL 连接驱动程序，以供编程语言使用：

- [SQL Server 驱动程序](./sql-connection-libraries.md)



<a name="an-110-ado-net-docu" />

## <a name="c-logoimage-ref-320-csharp-c-using-adonet"></a>![C# 徽标][image-ref-320-csharp] 使用 ADO.NET 的 C#

.NET 托管语言（如 C# 和 Visual Basic）最常使用 ADO.NET。 ADO.NET  是 .NET Framework 类子集的临时名称。

#### <a name="code-examples"></a>代码示例

| 示例 | 描述 |
| :-- | :-- |
| [使用 ADO.NET 连接到 SQL 的概念证明](./ado-net/step-3-connect-sql-ado-net.md) | 专注于如何连接和查询 SQL Server 的小型代码示例。 |
| [使用 ADO.NET 实现对 SQL 的弹性连接](./ado-net/step-4-connect-resiliently-sql-ado-net.md) | 代码示例中的重试逻辑，因为连接有时可能会断开。<br /><br />重试逻辑非常适用于通过 Internet 维持连接到任何云数据库（如 Azure SQL 数据库）的连接。 |
| [Azure SQL 数据库：展示了如何在 Windows/Linux/macOS 上使用 .NET Core 创建 C# 程序并进行连接和查询](/azure/sql-database/sql-database-connect-query-dotnet-core) | Azure SQL 数据库示例。 |
| [生成应用程序：C#、ADO.NET、Windows](https://www.microsoft.com/sql-server/developer-get-started/csharp/win/) | 配置信息和代码示例。 |
| &nbsp; | <br /> |

#### <a name="documentation"></a>文档

| 区域 | 说明 |
| :-- | :-- |
| [使用 ADO.NET 的 C#](./ado-net/microsoft-ado-net-sql-server.md)| 文档的根目录。 |
| [命名空间：System.Data](/dotnet/api/system.data) | 用于 ADO.NET 的类集。 |
| [命名空间：Microsoft.Data.SqlClient](/dotnet/api/microsoft.data.SqlClient) | 用于 Microsoft .NET Data Provider for SQL Server 的类集 |
| &nbsp; | <br /> |



<a name="an-116-csharp-ef-orm" />

## <a name="entity-framework-logoimage-ref-333-ef-entity-framework-ef-with-cx23"></a>![实体框架徽标][image-ref-333-ef] 使用 C&#x23; 的 Entity Framework (EF)

Entity Framework (EF) 提供对象关系映射 (ORM)。 通过 ORM，面向对象的编程 (OOP) 源代码可以更轻松地操作从关系 SQL 数据库中检索到的数据。

EF 与以下技术有直接或间接关系：

- .NET Framework
- [LINQ to SQL](/dotnet/framework/data/adonet/sql/linq/) 或 [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities)
- 语言语法增强功能，如 C# 中的 =>  运算符。
- 为映射到 SQL 数据库中表的类生成源代码的便捷程序。 例如，[EdmGen.exe](/dotnet/framework/data/adonet/ef/edm-generator-edmgen-exe)。


#### <a name="original-ef-and-new-ef"></a>原始 EF 和新 EF

[Entity Framework 的入门页](/ef/)介绍了 EF，说明如下所示：

- Entity Framework 是一种对象关系映射程序 (O/RM)，可方便 .NET 开发人员使用 .NET 对象处理数据库。 开发人员无需再像往常一样编写大部分数据访问源代码。

Entity Framework  是由两个单独的源代码分支共用的名称。 一个 EF 分支较旧，它的源代码现在采用公共维护。 另一个 EF 分支是新的。 下面介绍了这两个 EF 分支：

| 版本 | 说明 |
| :-- | :-- |
| [EF 6.x](/ef/ef6/) | Microsoft 第一次发布 EF 是在 2008 年 8 月。 2015 年 3 月，Microsoft 宣布了 EF 6.x 是 Microsoft 开发的最终版本。 Microsoft 将源代码发布到了公共域。<br /><br />EF 最初属于 .NET Framework。 但后来，EF 6.x 从 .NET Framework 中脱离。<br /><br />[GitHub 存储库 aspnet/EntityFramework6 中的 EF 6.x 源代码  ](https://github.com/aspnet/EntityFramework6) |
| [EF Core](/ef/core/) | 2016 年 6 月，Microsoft 发布了新开发的 EF Core。 EF Core 旨在提升灵活性和可移植性。 可运行 EF Core 的操作系统不止 Microsoft Windows。 EF Core 可以与之交互的数据库不止 Microsoft SQL Server 和其他关系数据库。<br /><br />**C&#x23; 代码示例：**<br />[Entity Framework Core 入门](/ef/core/get-started/index)<br />[开始对现有数据库使用 .NET Framework 上的 EF Core](/ef/core/get-started/full-dotnet/existing-db) |
| &nbsp; | <br /> |

EF 及其相关技术非常强大，对于想要掌握整个领域的开发人员来说，有很多东西需要学习。

&nbsp;



<a name="an-130-jdbc-docu" />

## <a name="java-logoimage-ref-330-java-java-and-jdbc"></a>![Java 徽标][image-ref-330-java] Java 和 JDBC

Microsoft 提供了 Java Database Connectivity (JDBC) 驱动程序，可用于 SQL Server（或 Azure SQL 数据库）。 它是 Type 4 JDBC 驱动程序，通过标准 JDBC 应用程序编程接口 (API) 提供数据库连接。

#### <a name="code-examples"></a>代码示例

| 示例 | 描述 |
| :-- | :-- |
| [代码示例](./jdbc/sample-jdbc-driver-applications.md) | 介绍数据类型、结果集和大型数据的代码示例。 |
| [连接 URL 示例](./jdbc/connection-url-sample.md) | 介绍了如何使用连接 URL 连接到 SQL Server。 然后介绍了如何使用 SQL 语句来检索数据。 |
| [数据源示例](./jdbc/data-source-sample.md) | 介绍了如何使用数据源连接到 SQL Server。 然后介绍了如何使用存储过程来检索数据。 |
| [使用 Java 查询 Azure SQL 数据库](/azure/sql-database/sql-database-connect-query-java) | Azure SQL 数据库示例。 |
| [创建在 Ubuntu 上使用 SQL Server 的 Java 应用程序](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/) | 配置信息和代码示例。 |
| &nbsp; | <br /> |

#### <a name="documentation"></a>文档

JDBC 文档包括以下主要方面：

| 领域 | 说明 |
| :-- | :-- |
| [Java Database Connectivity (JDBC)](./jdbc/microsoft-jdbc-driver-for-sql-server.md) | JDBC 文档的根目录。 |
| [参考](./jdbc/reference/jdbc-driver-api-reference.md) | 接口、类和成员。 |
| [JDBC SQL 驱动程序编程指南](./jdbc/programming-guide-for-jdbc-sql-driver.md) | 配置信息和代码示例。 |
| &nbsp; | <br /> |



<a name="an-140-node-js-docu" />

## <a name="nodejs-logoimage-ref-340-node-nodejs"></a>![Node.js 徽标][image-ref-340-node] Node.js

利用 Node.js，可以从 Windows、Linux 或 macOS 连接到 SQL Server。 Node.js 文档的根目录位于[此处](./node-js/node-js-driver-for-sql-server.md)。

用于 SQL Server 的 Node.js 连接驱动程序是用 JavaScript 实现的。 此驱动程序使用 TDS 协议，所有新式版 SQL Server 都支持这种协议。 此驱动程序是 [GitHub 上的](https://tediousjs.github.io/tedious/)开放源代码项目。

#### <a name="code-examples"></a>代码示例

| 示例 | 描述 |
| :-- | :-- |
| [使用 Node.js 连接到 SQL 的概念证明](./node-js/step-3-proof-of-concept-connecting-to-sql-using-node-js.md) | 用于连接到 SQL Server 和执行查询的基本功能源代码。 |
| [Azure SQL 数据库：使用 Node.js 进行查询](/azure/sql-database/sql-database-connect-query-nodejs) | 云中的 Azure SQL 数据库的示例。 |
| [创建在 macOS 上使用 SQL Server 的 Node.js 应用程序](https://www.microsoft.com/sql-server/developer-get-started/node/mac/) | 配置信息和代码示例。 |
| &nbsp; | <br /> |



<a name="an-160-odbc-cpp-docu" />

## <a name="odbc-for-c"></a>ODBC for C++

![ODBC 徽标][image-ref-350-odbc] ![cpp-big-plus][image-ref-322-cpp]

开放式数据库连接 (ODBC) 是在 20 世纪 90 年代开发的，比 .NET Framework 更早。 ODBC 旨在独立于任何特定数据库系统和操作系统。

多年来，Microsoft 内外的团队已经创建和发布了许多 ODBC 驱动程序。 驱动程序的范围涉及几种客户端编程语言。 数据目标列表远不止包含 SQL Server。

其他一些连接驱动程序在内部使用 ODBC。

#### <a name="code-example"></a>代码示例

- [使用 ODBC 的 C++ 代码示例](../odbc/reference/sample-odbc-program.md)

#### <a name="documentation-outline"></a>文档大纲

本部分中的 ODBC 内容重点介绍了如何从 C++ 访问 SQL Server 或 Azure SQL 数据库。 下表列出了主要 ODBC 文档的大纲。


| 区域 | 子区域 | 说明 |
| :--- | :------ | :---------- |
| [ODBC for C++](./odbc/microsoft-odbc-driver-for-sql-server.md) | 文档的根目录。 |
| [Linux-macOS](./odbc/linux-mac/system-requirements.md) | &nbsp; | 介绍了如何在 Linux 或 macOS 操作系统上使用 ODBC。 |
| [Windows](./odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)     | &nbsp; | 介绍了在 Windows 操作系统上使用 ODBC。 |
| [管理](../odbc/admin/odbc-data-source-administrator.md) | &nbsp; | 用于管理 ODBC 数据源的管理工具。 |
| [Microsoft](../odbc/microsoft/microsoft-supplied-odbc-drivers.md)  | &nbsp; | Microsoft 创建和提供的各种 ODBC 驱动程序。 |
| [概念和参考](../odbc/reference/introduction-to-odbc.md) | &nbsp; | 除了传统参考之外，还介绍了 ODBC 接口的概念性信息。 |
| &nbsp; " | [附录](../odbc/reference/appendixes/odbc-appendixes.md)    | 状态转换表、ODBC 游标库等。 |
| &nbsp; " | [开发应用程序](../odbc/reference/develop-app/checking-feature-support-and-variability.md)  | 函数、句柄等。 |
| &nbsp; " | [开发驱动程序](../odbc/reference/develop-driver/developing-an-odbc-driver.md) | 如何开发你自己的 ODBC 驱动程序（如果有特殊化数据源的话）。 |
| &nbsp; " | [安装](../odbc/reference/install/odbc-subkey.md) | ODBC 安装、子项等。 |
| &nbsp; " | [语法](../odbc/reference/syntax/odbc-reference.md)   | 用于安装、安装程序、转换和数据访问的 API。 |
| &nbsp; | &nbsp; | <br /> |



<a name="an-170-php-docu" />

## <a name="php-logoimage-ref-360-php-php"></a>![PHP 徽标][image-ref-360-php] PHP

可以使用 PHP 与 SQL Server 进行交互。 PHP 文档的根目录位于[此处](./php/microsoft-php-driver-for-sql-server.md)。

#### <a name="code-examples"></a>代码示例

| 示例 | 描述 |
| :-- | :-- |
| [使用 PHP 连接到 SQL 的概念验证](./php/step-3-proof-of-concept-connecting-to-sql-using-php.md) | 专注于如何连接和查询 SQL Server 的小型代码示例。 |
| [使用 PHP 实现对 SQL 的弹性连接](./php/step-4-connect-resiliently-to-sql-with-php.md) | 代码示例中的重试逻辑，因为连接是通过 Internet 实现，且云有时可能会断开连接。 |
| [Azure SQL 数据库：使用 PHP 进行查询](/azure/sql-database/sql-database-connect-query-php) | Azure SQL 数据库示例。 |
| [创建在 RHEL 上使用 SQL Server 的 PHP 应用程序](https://www.microsoft.com/sql-server/developer-get-started/php/rhel/) | 配置信息和代码示例。 |
| &nbsp; | <br /> |



<a name="an-180-python-docu" />

## <a name="python-logoimage-ref-370-python-python"></a>![Python 徽标][image-ref-370-python] Python


可以使用 Python 与 SQL Server 进行交互。

#### <a name="code-examples"></a>代码示例

| 示例 | 描述 |
| :-- | :-- |
| [使用 pyodbc Python 连接到 SQL 的概念证明](./python/pyodbc/step-3-proof-of-concept-connecting-to-sql-using-pyodbc.md) | 专注于如何连接和查询 SQL Server 的小型代码示例。 |
| [Azure SQL 数据库：使用 Python 进行查询](/azure/sql-database/sql-database-connect-query-python) | Azure SQL 数据库示例。 |
| [创建在 SLES 上使用 SQL Server 的 PHP 应用程序](https://www.microsoft.com/sql-server/developer-get-started/python/sles/) | 配置信息和代码示例。 |
| &nbsp; | <br /> |

#### <a name="documentation"></a>文档

| 区域 | 说明 |
| :--- | :---------- |
| [使用 Python 连接到 SQL Server](./python/python-driver-for-sql-server.md) | 文档的根目录。 |
| [pymssql 驱动程序](./python/pymssql/python-sql-driver-pymssql.md) | Microsoft 不维护或测试 pymssql 驱动程序。<br /><br />pymssql 连接驱动程序是连接到 SQL 数据库的简单接口，以供在 Python 程序中使用。 pymssql 在 FreeTDS 的基础之上构建，以提供连接到 Microsoft SQL Server 的 Python DB-API (PEP-249) 接口。 |
| [pyodbc 驱动程序](./python/pyodbc/python-sql-driver-pyodbc.md)   | pyodbc 连接驱动程序是开放源代码 Python 模块，它简化了访问 ODBC 数据库的过程。 它虽实现了 DB API 2.0 规范，但提供了更多 Pythonic 便利。 |
| &nbsp; | <br /> |


<a name="an-190-ruby-docu" />

## <a name="ruby-logoimage-ref-380-ruby-ruby"></a>![Ruby 徽标][image-ref-380-ruby] Ruby

可以使用 Ruby 与 SQL Server 进行交互。 Ruby 文档的根目录位于[此处](./ruby/ruby-driver-for-sql-server.md)。

#### <a name="code-examples"></a>代码示例

| 示例 | 描述 |
| :-- | :-- |
| [使用 Ruby 连接到 SQL 的概念证明](./ruby/step-3-proof-of-concept-connecting-to-sql-using-ruby.md) | 专注于如何连接和查询 SQL Server 的小型代码示例。 |
| [Azure SQL 数据库：使用 Ruby 进行查询](/azure/sql-database/sql-database-connect-query-ruby) | Azure SQL 数据库示例。 |
| [创建在 macOS 上使用 SQL Server 的 Ruby 应用程序](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/) | 配置信息和代码示例。 |
| &nbsp; | <br /> |



<a name="an-204-aka-ms-sqldev" />

## <a name="build-an-app-website-for-sql-client-development"></a>[用于 SQL 客户端开发的“生成应用程序”网站](https://www.microsoft.com/sql-server/developer-get-started/)


在[“生成应用程序”  ](https://www.microsoft.com/sql-server/developer-get-started/)网页上，可以从用于连接到 SQL Server 的编程语言长列表中进行选择。 客户端程序可以运行各种操作系统。

“生成应用程序”  着重为刚开始接触的开发人员提供了简单、完整的体验。 这些步骤解释了以下任务：

1. 如何安装 Microsoft SQL Server
2. 如何下载并安装工具和驱动程序。
3. 如何根据选定操作系统进行任何必要的配置。
4. 如何编译提供的源代码。
5. 如何运行该程序。

下面是网站上的详细信息的一些大纲式要点：

#### <a name="java-on-ubuntu"></a>Ubuntu 上的 Java

1. 设置你的环境
    - 步骤 1.1：安装 SQL Server
    - 步骤 1.2：安装 Java
    - 步骤 1.3：安装 Java 开发工具包 (JDK)
    - 步骤 1.4：安装 Maven
2. 创建使用 SQL Server 的 Java 应用程序
    - 步骤 2.1：创建连接到 SQL Server 并执行查询的 Java 应用程序
    - 步骤 2.2：使用热门框架 Hibernate 创建连接到 SQL Server 的 Java 应用程序
3. 让 Java 应用程序的速度提高 100 倍
    - 步骤 3.1：创建用于展示列存储索引的 Java 应用程序

#### <a name="python-on-windows"></a>Windows 上的 Python

1. 设置你的环境
    - 步骤 1.1：安装 SQL Server
    - 步骤 1.2：安装 Python
    - 步骤 1.3：安装用于 SQL Server 的 ODBC 驱动程序和 SQL 命令行实用工具
2. 创建使用 SQL Server 的 Python 应用程序
    - 步骤 2.1：安装 Python Driver for SQL Server
    - 步骤 2.2：创建应用程序的数据库
    - 步骤 2.3：创建连接到 SQL Server 并执行查询的 Python 应用程序
3. 让 Python 应用程序的速度提高 100 倍
    - 步骤 3.1：使用 sqlcmd 创建包含 500 万条数据的新表
    - 步骤 3.2：创建查询此表并度量耗时的 Python 应用程序
    - 步骤 3.3：度量查询运行时长
    - 步骤 3.4：向表中添加列存储索引
    - 步骤 3.5：度量使用列存储索引运行查询所需的时长

下面的屏幕截图展示了 SQL 开发文档网站的外观。

#### <a name="choose-a-language"></a>选择语言

![SQL 开发网站 - 开始使用][image-ref-390-aka-ms-sqldev-choose-language]

&nbsp;

#### <a name="choose-an-operating-system"></a>选择操作系统

![SQL 开发网站 - Java Ubuntu][image-ref-400-aka-ms-sqldev-java-ubuntu]

&nbsp;



## <a name="other-development"></a>其他开发


本部分收录了其他开发选项的链接。 其中包括通常在 Azure 开发中使用这些相同的语言。 这些信息不仅仅针对 Azure SQL 数据库和 Microsoft SQL Server。

#### <a name="developer-hub-for-azure"></a>适用于 Azure 的开发人员中心

- [适用于 Azure 的开发人员中心](/azure/)
- [面向 .NET 开发人员的 Azure](/dotnet/azure/)
- [面向 Java 开发人员的 Azure](/java/azure/)
- [面向 Node.js 开发人员的 Azure](/nodejs/azure/)
- [面向 Python 开发人员的 Azure](/python/azure/)
- [在 Azure 中创建 PHP Web 应用](/azure/app-service-web/app-service-web-get-started-php)

#### <a name="other-languages"></a>其他语言

- [创建在 Windows 上使用 SQL Server 的 Go 应用程序](https://www.microsoft.com/sql-server/developer-get-started/go/windows/)



<!-- Image references. -->

[image-ref-322-cpp]: ./media/homepage-sql-connection-drivers/gm-cpp-4point-p61f.png
[image-ref-320-csharp]: ./media/homepage-sql-connection-drivers/gm-csharp-c10c.png
[image-ref-333-ef]: ./media/homepage-sql-connection-drivers/gm-entity-framework-ef20d.png
[image-ref-330-java]: ./media/homepage-sql-connection-drivers/gm-java-j18c.png
[image-ref-340-node]: ./media/homepage-sql-connection-drivers/gm-node-n30.png
[image-ref-350-odbc]: ./media/homepage-sql-connection-drivers/gm-odbc-ic55826-o35.png
[image-ref-360-php]: ./media/homepage-sql-connection-drivers/gm-php-php60.png
[image-ref-370-python]: ./media/homepage-sql-connection-drivers/gm-python-py72.png
[image-ref-380-ruby]: ./media/homepage-sql-connection-drivers/gm-ruby-un-r82.png
[image-ref-390-aka-ms-sqldev-choose-language]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-choose-language-g21.png
[image-ref-400-aka-ms-sqldev-java-ubuntu]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-java-ubuntu-c31.png